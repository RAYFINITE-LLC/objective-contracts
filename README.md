# Objective Contracts

**A discipline for delegating bounded tasks to autonomous AI agents — so they finish, provably,
instead of wandering.**

Prompt engineering shapes one model turn. Agent frameworks shape how turns chain together.
An objective contract shapes the unit most delegated work actually is: **one bounded task an
agent carries to a provable finish** — a feature, a migration, a fix-until-green run.

Most autonomous-agent failure starts at this unit. The first generation of "give the agent a
goal and walk away" tooling burned out in public: vague natural-language goals that the agent
both executed *and graded*, no way to detect that it was repeating itself, no budget, and no
concept of "good enough." The result was agents that re-planned in circles, "improved"
already-finished work forever, and spent hundreds of calls producing nothing. The lesson was
never that agents can't finish things — it's that an un-engineered objective can't be finished,
by anyone.

The cure is not a smarter agent. It is a better-engineered objective.

## The Objective Contract

An objective is not "what I asked for." An objective is a **contract with five parts**, and it
is not ready to delegate until all five exist:

### 1. One sentence of intent
A single bounded outcome — not a theme, not a wish. If unrelated outcomes share the sentence,
you have several contracts wearing one costume (see [Scoping](#scoping-the-omnibus-objective)).

### 2. A verifiable done condition
Machine-checkable, stated up front: *tests green · item count reaches zero · endpoint returns
X · page renders Y*. If you cannot state "done" in a way a machine (or an unambiguous
procedure) can confirm, the task is not ready for autonomous delegation — scope it further or
keep a human in the loop. For feature work, the strongest done condition is a **failing test
written before the implementation**: the test is the objective's executable spec, and watching
it fail first proves it can detect the difference.

### 3. An independent finish line
The agent that implements **never declares its own success**. "Done" is pronounced by an
independent check the doer cannot weaken: a test suite, a reviewer agent that can only
approve or return work with evidence, a rendered-page verification for UI work, a CI gate.
Self-declared completion is among the most reliable predictors of autonomous failure — a doer
judging its own finish will either wave everything through or polish forever.

### 4. A budget
Maximum attempts per failure, a time or token ceiling, and an escalation rule: *after N failed
attempts on the same blocker, stop and report — do not push through.* Persistence past a budget
is not diligence; it is the failure mode with better manners.

### 5. A scope boundary
The surfaces the task may touch, the paths it must never touch (secrets, credentials, payment
code, database migrations, CI definitions — escalate instead), and **where the work happens**:
a named, dedicated workspace (branch, worktree, or clone). Never run an objective inside a
workspace that is serving something live — a dev server, a demo, another agent's session.

## The state file

Any objective that outlives a single sitting externalizes its state to one small file the agent
re-reads on every resumption: the objective sentence, the done-condition checklist, a terse
append-only progress log, and current blockers. Context windows compact and sessions restart;
the state file is what keeps the mission from being re-derived from vibes. An agent that cannot
find its objective state **stops and asks** — it never guesses the mission.

See [`TEMPLATE.md`](TEMPLATE.md) for the file format and
[`examples/feature-objective.md`](examples/feature-objective.md) for a filled-in example.

## Scoping: the omnibus objective

The likeliest failure is not a bad run but a bad objective. *"Rebuild the module, fix the
tests, update the docs, and improve performance"* is four contracts in one sentence, and an
agent given all four will finish none. Before delegating, split until each objective has ONE
verifiable done condition and fits its budget.

Smells that demand a split:

- the word **"and"** joining unrelated outcomes;
- a done condition you can only phrase as *"it's better"*;
- a single concern whose expected change footprint exceeds roughly ten files;
- verification that needs more than one *kind* of independent check just to make sense.

Scoping is the **delegator's** job. Handing an agent an unscoped omnibus objective and blaming the
agent afterward is a management failure, not a model failure.

## Coordinating multiple objectives

- **Sequential is the default.** Each objective starts from the *verified* end-state of the
  previous one.
- **Parallel requires isolation.** One dedicated workspace per objective, no shared mutable
  state, and a delegator who owns the merge order. Two objectives that would edit the same
  files are one objective — or a sequence — never parallel.
- **Know when it stopped being an objective.** "Keep doing this on a schedule, unattended,
  until the metric holds" is not a bounded task anymore — it is a recurring autonomous loop,
  and it needs loop-grade governance (its own declared stop conditions, kill switch, and
  budget) on top of everything here. Objective contracts feed that governance; they do not
  replace it.

## Anti-pattern catalog

| Anti-pattern | Smell | Cure |
|---|---|---|
| Perfectionism loop | "improving" work whose done condition already passed | the checker's approval ends the objective, full stop |
| Silent renegotiation | the done condition quietly shrinks mid-run | only the delegator may amend the contract |
| Self-declared done | "I've completed the task" with no independent evidence | the independent finish line — the checker pronounces done |
| Omnibus objective | unrelated outcomes sharing one brief | split: one verifiable outcome per contract |
| Scope creep | "while I was in there" changes | scope boundary in the contract; out-of-scope = a new objective |
| Unbudgeted persistence | attempt seven on the same failure | attempt cap + escalation — a report beats a retry |
| Vague done condition | "improve", "clean up", "modernize" | a machine-checkable criterion, or don't delegate |
| Missing state file | long-running task, no external objective record | the state file, re-read on every resumption |

## Adopting this in any agent stack

Objective Contracts is deliberately tool-agnostic — it is a discipline, not a dependency:

1. **Put the contract in the brief.** Whatever your delegation mechanism (a sub-agent prompt, a
   ticket, a CLI task), the five parts travel with the task. Inline for small tasks; as a
   committed state file for long ones.
2. **Bind the checker to something real.** A test command, a CI job, a reviewer agent that can
   only approve or return work, a rendered-verification step. The checker must be something the
   doer cannot edit.
3. **Name the workspace.** Delegation without a stated work location invites an agent to work
   wherever it wakes up — including inside something live.
4. **Log the ending.** Every objective ends in exactly one of: checker approval, a budget-stop with
   a report, or a contract the delegator explicitly amended. If you can't say which one happened, the objective
   isn't over.

## License

[MIT](LICENSE) — © 2026 Pradeep Singala Reddy / RAYFINITE LLC.
This document synthesizes widely-shared industry lessons on autonomous-agent task design.
Use it, adapt it, ship better agents.
