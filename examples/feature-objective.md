# Objective — Add rate limiting to the public search endpoint

## Contract

```
Objective: The public /api/search endpoint rejects clients that exceed 60 requests/minute.
Done-Condition: New tests pass proving (a) request 61 within a minute returns 429 with a
  Retry-After header, (b) requests succeed again once the limit interval elapses, (c) authenticated internal
  callers are exempt — AND the full existing suite stays green.
Checker: CI test job on the pull request (maker cannot edit CI config — see Never-touch).
Budget: 3 attempts per failing test; 90 minutes total; then stop and report with the failing
  output attached.
Scope: api/middleware/, api/routes/search.*, tests/ · Never-touch: auth code, CI workflows,
  deployment config, any credentials or secrets.
Workspace: worktree feature/search-rate-limit (created for this objective).
```

## Done-condition checklist

- [ ] Failing tests written FIRST and observed failing (61st request → 429 + Retry-After)
- [ ] Limit-interval recovery test
- [ ] Internal-caller exemption test
- [ ] Implementation makes all new tests pass
- [ ] Full existing suite green
- [ ] Pull request opened; CI (the checker) is green

## Progress log (terse, append-only)

- day 1 — contract accepted; three failing tests written and observed failing
- day 1 — token-bucket middleware added; two of three new tests green
- day 1 — exemption test green after reading existing auth-context helper; full suite green
- day 1 — PR opened; CI green; objective ends on checker approval

## Blockers

- (none — if the same test had failed three times, this section would hold the report instead
  of attempt four)
