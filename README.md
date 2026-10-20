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

`plugin.json` is the portable Agent Plugins manifest; `.cursor-plugin/plugin.json`
is the Cursor-specific copy, and `mcp.json` points the agent at the remote server
`https://rheono.dev/api/mcp`. Nothing runs locally.

The server is unauthenticated and read-only. `check_app` is the only tool that
touches the network, and it refuses private, loopback and metadata addresses, so
it cannot be turned into a probe of the host it runs on.

## Layout

| Path | Purpose |
|---|---|
| `plugin.json` | Portable manifest, plus the OpenAI listing block under `extensions`. |
| `.cursor-plugin/plugin.json` | Cursor marketplace manifest. |
| `mcp.json` | The single remote server. |
| `skills/preflight/SKILL.md` | The pre-deploy gate. |
| `assets/logo.svg` | Square icon, 64×64. |

## Scope

Everything is read-only and shallow. No crawling, no payloads, no auth bypass. The deep probe is a researcher's job and is triggered by `request_pentest`.

MIT.
