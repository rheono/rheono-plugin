# Rheono plugin

Answers "is this app safe to ship?" from inside the agent loop.

## Tools

Served by the host `https://rheono.dev/api/mcp`.

| Tool | What it does |
|---|---|
| `check_app` | Shallow read-only pass on a live URL: TLS, headers, cookie flags, exposed paths. |
| `verify_ownership` | Issues and checks the DNS TXT record that proves a domain is yours. |
| `request_pentest` | Hands the domain to a researcher; the report is emailed. |
| `check_status` | Follows a request. |

## Skill

`skills/preflight` runs the check as a pre-deploy gate and reads the findings back into the conversation.

## Install

Hosts that read this manifest load the skill. `.mcp.json` points the agent at the
remote server `https://rheono.dev/api/mcp`; nothing runs locally.

The server is unauthenticated and read-only. `check_app` is the only tool that
touches the network, and it refuses private, loopback and metadata addresses, so
it cannot be turned into a probe of the host it runs on.

## Scope

Everything is read-only and shallow. No crawling, no payloads, no auth bypass. The deep probe is a researcher's job and is triggered by `request_pentest`.

MIT.
