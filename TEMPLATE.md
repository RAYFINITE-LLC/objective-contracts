# Objective — <one-sentence bounded outcome>

Copy this file into the objective's workspace (or inline it into the agent brief for small
tasks). All five contract parts are REQUIRED — an objective missing any of them is not ready to
delegate. See [README](README.md).

## Contract

```
Objective: <one sentence, one outcome — no "and" joining unrelated work>
Done-Condition: <machine-checkable — tests green / count 0 / endpoint returns X / renders Y>
Checker: <who pronounces done — test suite | reviewer agent | rendered verification | CI gate>
Budget: <max attempts per failure + time/token ceiling + "after N failures: stop and report">
Scope: <surfaces the task may touch> · Never-touch: <secrets/auth/payments/migrations/CI + task-specific>
Workspace: <named branch/worktree/clone — never a workspace serving something live>
```

## Done-condition checklist

- [ ] <each independently checkable clause of the done condition>

## Progress log (terse, append-only)

- <date> — <what moved>

## Blockers

- <blocker → what was tried → what's needed>  *(N failed attempts on one blocker = stop, report)*
