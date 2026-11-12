<p align="center"><img src="assets/logo.png" alt="Rebel Tech Labs" width="180"></p>

# Rebel Verified Action

> Don't just build it. Prove it.

Scans your repository with [Rebel Verified](https://github.com/RebelTechLabsInc) — findings become prioritized, actionable fixes, and every scan is persisted so you can **verify** resolution with a rescan-compare loop instead of trusting "should be fixed".

## Quick start

1. Get a free API key:

   ```bash
   curl -X POST https://gxo8latjoa.execute-api.us-east-1.amazonaws.com/v1/signup \
     -H "Content-Type: application/json" -d '{"email":"you@example.com"}'
   ```

2. Add it as a repo secret: `REBEL_API_KEY`

3. Add the workflow:

   ```yaml
   name: rebel-verified
   on: [push, pull_request]
   jobs:
     verify:
       runs-on: ubuntu-latest
       steps:
         - uses: RebelTechLabsInc/rebel-verified-action@v1
           with:
             api-key: ${{ secrets.REBEL_API_KEY }}
   ```

Findings appear as workflow annotations; the job fails when findings exist (`fail-on-findings: false` to warn-only).

## Inputs

| Input | Default | Description |
|---|---|---|
| `api-key` | — | **Required.** `rbk_…` tenant key |
| `repo` | `github.repository` | `owner/name` to scan |
| `api-url` | hosted API | Override for self-hosted installs |
| `repo-token` | `github.token` | Needed only for private repos |
| `no-ai` | `false` | Deterministic rules only |
| `fail-on-findings` | `true` | Exit non-zero on findings |

## Outputs

| Output | Description |
|---|---|
| `scan-id` | Persisted scan ID — feed to compare/rescan |
| `findings` | Finding count |

## Verified badge

`POST /v1/badge` (with your key) returns a token → embed `https://gxo8latjoa.execute-api.us-east-1.amazonaws.com/badge/<token>.svg` in your README. It flips to **verified** when a rescan reaches zero findings.

## Tiers

Free: 3 scans/mo · Developer $29 · Team $99 · Business $299 — upgrade via `rebel upgrade` or the dashboard.
