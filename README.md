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

## Environment: INT

`harden-runner` is pinned to the `rc-20-int` branch, which sets
`STEPSECURITY_ENV = "int"` (`src/configs.ts`), so the agent reports to:

| | prod (`@v2` / `@rc`) | int (`@rc-20-int`) |
|---|---|---|
| API | `agent.api.stepsecurity.io/v1` | `int.api.stepsecurity.io/v1` |
| Web | `app.stepsecurity.io` | `int1.stepsecurity.io` |

Do NOT use `@rc-20-oss-int` — despite the name it sets `STEPSECURITY_ENV = "agent"`
(prod). `rc-20-int` is the only int-pointing ref.

Insights land at `https://int1.stepsecurity.io/github/test-org-tushar/suspend-403-repro/actions/runs/<run_id>`.
