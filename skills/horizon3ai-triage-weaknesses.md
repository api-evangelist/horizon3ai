---
name: Triage and re-verify NodeZero weaknesses
description: List and inspect proven weaknesses from a pentest, then schedule a 1-click verification to confirm a fix.
api: NodeZero GraphQL API
endpoint: https://api.gateway.horizon3ai.com/v1/graphql
operations: [weaknesses_page, weaknesses_count, weakness, schedule_weakness_1cv_ops]
---

# Triage and re-verify NodeZero weaknesses

## Auth
Exchange your API key for a JWT at `/v1/auth` and send `Authorization: Bearer <jwt>` to `/v1/graphql` (see the run-pentest skill). A `Read-only` key is sufficient to read weaknesses; re-verification requires a `User` role.

## Steps
1. **List** — query `weaknesses_page` (paginated) with `weaknesses_count` for totals, filtered to the pentest/op you care about. Pagination is page-based (`page` / `page_size`).
2. **Inspect** — for each finding, query `weakness` by id for the impact, affected hosts, and proof / attack-path context.
3. **Prioritize** — order by severity and whether the weakness sits on a proven attack path to a critical asset.
4. **Re-verify a fix** — after remediation, call the `schedule_weakness_1cv_ops` mutation to launch a targeted one-click-verify op that re-tests just those weaknesses, then poll the resulting op to confirm the fix holds.

## Rules
- Track findings over time via `weakness_series_page` / `WeaknessSeries`, not by re-reading raw weaknesses each run.
- Errors surface in the GraphQL `errors[]` array; `401` means the JWT expired (re-auth).
