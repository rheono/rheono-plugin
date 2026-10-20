---
name: preflight
description: Check a live URL's security posture before deploying, handing over, or shipping. Use when a deploy is about to happen, when someone asks whether a site or API is secure, or when a client or auditor wants evidence the surface is looked after.
---

# Preflight

Runs the `check_app` tool against a URL and reads the result back as a short
verdict. Read-only and shallow by design: it never changes the target.

## Steps

1. Pick the URL that is actually about to change: the staging or preview host
   for a deploy, the production host for a handover. Ask if ambiguous.
2. Call `check_app` with it.
3. Report in one line: `HTTP status`, `TLS state`, and the severity counts.
   Then list every high and medium finding with its one-line detail. Fold the
   low and info items into a single "also worth fixing" line.
4. If there are no high or medium findings say so plainly and stop — do not
   invent work.

## Reading the findings

- `tls-*` and `no-https` fail the preflight. Say the deploy is not ready.
- `hsts-missing`, `csp-missing` and the cookie findings do not fail it; they
  are fixes to schedule.
- `dotenv-exposed` is treated as a leak. Tell the user to rotate what was
  served, not just to delete the file.

## Going deeper

If the user wants more than the shallow pass, call `request_pentest` with the
domain and an email, then `check_status` to follow it. Before that, call
`verify_ownership` once so the domain is a proven asset rather than an
arbitrary URL, and hand the user the TXT record it returns.

## Boundaries

- One check per URL per turn unless asked again.
- Do not probe the URL with any other tool; the findings come from `check_app`.
- Never claim a verdict the tool did not return.
