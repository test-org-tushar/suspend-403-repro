# suspend-403-repro

Repro repo for: `could not refresh installation id <id>'s token: received non 2xx
response status "403 Forbidden"` when the StepSecurity GitHub App installation is
**suspended** on the org.

## Why this repo exists

The 403 only surfaces when something in the platform actually constructs a GitHub
client for the org. An empty org generates no traffic, so suspension is invisible.
This repo deliberately produces:

1. **Runtime traffic** — `harden-runner` in audit mode on a job that makes outbound
   calls, so runtime insights and build logs exist to analyze.
2. **Policy findings** — unpinned actions, no top-level `permissions`, and a job with
   no `harden-runner`, so the policy-driven-PR pipeline has real remediations to
   generate and will construct an Advanced-App client.

## Triggering

- Push to `main`
- Or: Actions -> "CI" -> Run workflow (`workflow_dispatch`)
