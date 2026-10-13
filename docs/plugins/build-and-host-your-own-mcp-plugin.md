---
sidebar_position: 12
sidebar_label: Build and host your own MCP plugin
title: Build and host your own MCP plugin
---

# Build and host your own MCP plugin

For a developer who wants a plugin whose logic runs on **their own server** and that Rudi calls like an MCP server.
We use **Appointments** (Datability's own hosted plugin; it answers "When is Dr. X free?") as the worked example.
Source: `plugins-hosted/appointments/` in the `rudi-mcp` repo. Reference: `docs/plugins/MANIFEST_REFERENCE.md` and `QUICKSTART.md` there.
If you only wrap an existing REST API, you do not need a server: use `rudi plugin init --from-openapi` (see QUICKSTART).

## How a call flows

1. A user asks the assistant something. Rudi's dispatcher POSTs a JSON-RPC message (`initialize`, `tools/list`, `tools/call`) to your plugin's `http.baseUrl`.
2. Every request carries a **`Rudi-Signature`** header. Your server must verify it with the plugin's signing secret and reject anything else with 401.
3. A `tools/call` also carries a **`Rudi-Token`** header: a short-lived token scoped to this call. Use it to read the team's files or call other plugins' tools through the **Platform API**. Your server never sees the team's other secrets.
4. The tool's config values (what the admin picked in the Add form) arrive inside the call arguments as `config`; the conversation info as `context`. Pop both before treating the rest as tool input.

## 1. Write the manifest (`plugin.yaml`)

Appointments' manifest, trimmed:

```yaml
manifestVersion: 1
slug: appointments
name: Appointments
description: Doctor clinic hours minus leave, for a date range.
category: healthcare
version: 1.0.0
runtime: mcp
permissions: [conversations.send]      # optional extra grants, consented to at install
http:
  baseUrl: "https://<your-server>/mcp"  # your own server
  auth:
    type: none                          # Rudi-Signature authenticates every call instead
  verifiesSignature: true               # see step 3
configSchema:
  type: object
  properties:
    clinicHours:
      type: file                        # admin picks a file from My Documents
      title: Clinic hours file
    leaveSource:
      type: tool                        # admin picks a tool of another installed plugin
      title: Doctor leave source
  required: [clinicHours, leaveSource]
tools:
  - key: get_doctor_schedule
    kind: lookup                        # read only; use change_data for anything that writes
    description: Available clinic-hours windows for a date range, per doctor.
    inputSchema:
      type: object
      properties:
        from: { type: string, description: "ISO date, inclusive" }
        to:   { type: string, description: "ISO date, inclusive, at most 31 days after from" }
      required: [from, to]
```

- `kind: change_data` tools always ask the user to confirm and are shown with a **Mutation** badge.
- `type: file | folder | tool` in `configSchema` become the pickers on the Add page, and the picked value becomes the **scope** of the call token: your plugin can read that file and call that one tool, nothing else.
- Check it: `rudi plugin validate`. Try it against your real server: `rudi plugin test get_doctor_schedule`.

## 2. Build the server

Any language. It must answer three JSON-RPC methods over HTTPS POST, and return 202 for the `notifications/initialized` notification. Appointments is a Python Lambda behind a Function URL; the core is:

```python
from rudi_plugin_sdk import RudiApiError, rudi, verify

def handler(event, context=None):
    headers, raw_body = event["headers"], read_body(event)      # the EXACT bytes received
    rpc = json.loads(raw_body)
    if rpc.get("method") == "notifications/initialized":
        return {"statusCode": 202, "body": ""}
    if not verify(headers, raw_body, SIGNING_SECRET):            # step 3
        return respond(401, error(rpc.get("id"), -32001, "invalid or missing Rudi-Signature"))
    return respond(200, dispatch(rpc, headers))

def dispatch(rpc, headers):
    method, params = rpc["method"], rpc.get("params") or {}
    if method == "initialize":
        return result(rpc["id"], {"protocolVersion": "...", "capabilities": {"tools": {}},
                                  "serverInfo": {"name": "my-plugin", "version": "1.0.0"}})
    if method == "tools/list":
        return result(rpc["id"], {"tools": TOOL_DEFINITIONS})    # the same tools as plugin.yaml
    if method == "tools/call":
        args = params.get("arguments") or {}
        config = args.pop("config", None) or {}                  # what the admin picked
        args.pop("context", None)
        return result(rpc["id"], run_tool(params["name"], args, config, headers))
    return error(rpc["id"], -32601, "method not found")
```

A tool result is `{"content": [{"type": "text", "text": "..."}], "isError": false, "structuredContent": {...}}`. Report a failure as `isError: true` with a plain sentence the assistant can relay, not as an HTTP error.

### Use the Platform API (SDK)

```python
client = rudi(PLATFORM_API_URL, headers["Rudi-Token"])
rows  = client.files.rows(config["clinicHours"])                 # rows of the file the admin picked
leave = client.tools.call(config["leaveSource"], {})             # another plugin's tool, e.g. <id>.get_doctor_leave
# client.conversations.send(conversation_id, text)               # needs permissions: [conversations.send]
```

A failed call raises `RudiApiError` (status, code, message): catch it and return `isError: true`. For background work with no call in flight, mint your own 15-minute token with `installation_token(...)` and your client credentials.

SDKs live in the `rudi-mcp` repo: `sdk/plugin-sdk-py` (`rudi_plugin_sdk`) and `sdk/plugin-sdk-ts` (`@rudi/plugin-sdk`, Node 22+, plain `.ts`). Install from the repo, e.g. `uv pip install -e sdk/plugin-sdk-py`.

## 3. Signing and `verifiesSignature`

- `Rudi-Signature: t=<unix seconds>,v1=<hex HMAC-SHA256>` over `"<t>." + raw body bytes`. `verify()` checks the HMAC (constant time) and rejects a timestamp more than 5 minutes off. Always verify the **raw bytes**, before any JSON re-encoding.
- Rudi mints the **signing secret** when a team adds your plugin and returns it **once**, base64-encoded (32 bytes). Pass that exact string to `verify()`; do not decode it yourself. Store it as a secret on your server (Appointments reads it from an SSM SecureString).
- Because your server verifies every request, it cannot pass Add's pre-install connection check with a secret it has not received yet. Setting **`verifiesSignature: true`** tells Rudi to skip that check: the install is saved as **Waiting for verification** with the tools off. You store the secret, then the team presses **Check again**; Rudi sends a signed test call and flips the plugin to **Connected**.
- Today that hand-off is manual for Datability-hosted plugins (an operator stores the secret and redeploys). A plugin you run yourself does the same with your own secret store. Leave `verifiesSignature` off only if your server does not verify signatures, which we do not recommend.

## 4. Deploy

Host it anywhere reachable over HTTPS. Appointments is one CDK stack (Lambda + Function URL, a keep-warm schedule, the SSM parameter for the secret): `plugins-hosted/appointments/stack.py`. Put the deployed URL in `http.baseUrl`.

## 5. Publish and share

```
rudi login                              # once, opens the dashboard
rudi plugin publish                     # private to your team
rudi plugin share <slug> --team <teamID or invite email>
rudi plugin publish --public            # submit for Datability review
rudi plugin status <slug>
```

The receiving team opens **Plugins → Marketplace → Shared with you**, fills the Add form (the pickers from step 1), and adds it. A published version is immutable: ship changes as a new `version` with a `changelog`.

## Checklist

- `rudi plugin validate` passes; `rudi plugin test <tool>` works against the real server.
- A request without a valid `Rudi-Signature` gets 401; a replay older than 5 minutes does too.
- Every tool failure is returned as `isError: true` with a readable message.
- `change_data` is used for anything that writes; the tool itself never reports a pending request as done.
- No secret, token or patient data is logged.
