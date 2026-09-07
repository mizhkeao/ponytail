---
name: ponytail
description: >
  Favor clear algorithms, coherent architecture, and maintainable code with
  little unnecessary machinery. Apply when writing, fixing, refactoring,
  reviewing, or designing code, and when the user invokes Ponytail or asks
  for simplicity. Judge simplicity by how easily the solution can be
  understood, verified, and changed, rather than by line count or diff size.
license: MIT
---

# Ponytail

Choose the simplest model that faithfully represents the problem and satisfies
its requirements. Simplify the machinery without removing distinctions
essential to correctness.

Optimize for the codebase's long-term coherence and maintainability. Minimize
the burden of understanding, verifying, and changing the code. Use diff size
and line count as tie-breakers between otherwise comparable solutions.

## Simplify without losing meaning

A model includes the data representation, algorithm, and assumptions. Make
essential distinctions and assumptions explicit. A simplification is valid
only if it preserves required behavior, correctness guarantees, and applicable
accuracy and performance constraints.

Prefer a representation or algorithm that makes correctness easy to explain.
A larger change is justified when it replaces interacting exceptions with
one coherent rule.

Stop simplifying when the next reduction would hide an assumption, merge
meaningfully different cases, weaken a guarantee, or make correctness harder
to establish. State the model's limits.

Generalize across the supported domain and its governing rules. Extend that
domain only when requirements justify it.

## Understand the problem

Before choosing a solution, establish:

- The required behavior and the rules that must remain true.
- The data representation, algorithm, and component responsible for them.
- The relevant callers, external interfaces, and existing checks.

Investigate enough to explain the actual problem. Expand the investigation
when evidence crosses a boundary; do not turn every task into a whole-repo audit.

## Prefer durable, general corrections

Prefer the correct architectural or algorithmic change over a smaller patch
that leaves the underlying defect in place.

Treat the reported example as evidence of a problem, not as the boundary of
the solution. Identify the general class of inputs, states, and callers
covered by the existing contract. Correct the behavior across that class.

Before choosing a fix:

- Identify the violated rule and the component that should enforce it.
- Determine whether the cause is local logic, the algorithm, the data
  representation, or misplaced responsibility.
- Check whether the proposed fix remains correct as valid inputs and ordinary
  combinations vary, without accumulating special cases.

Prefer representations and algorithms that express the governing rule
directly. If correctness requires changing ownership or a shared model,
make that focused structural change and update its affected callers.

A guard is appropriate when it expresses a real rule; it is not automatically
a root-cause fix. Reject patches that recognize the current example through
hard-coded values, fixture-specific branches, duplicated guards, silent
fallbacks, or another competing implementation unless that distinction is
itself part of the intended behavior.

Support extension through clear responsibilities, explicit assumptions, and
coherent interfaces. Do not add speculative configuration, hooks, or
abstraction layers merely to appear future-proof.

When a local patch and a structural change are both plausible, briefly compare
their concrete costs. A broader change must explain which current structural
problem it removes and why a local patch would leave that problem unresolved.
A local fix is appropriate when the cause is genuinely local and the governing
design already handles the general case.

Use temporary workarounds only when explicitly requested or when a concrete
constraint prevents the durable fix. Disclose the constraint, remaining
limitation, and removal condition; do not present the workaround as complete.
Mark an accepted deliberate shortcut with a `ponytail:` comment naming its
limitation and removal condition. A comment does not justify the shortcut.

## Choose the design

First require correctness, requested behavior, compatibility, and applicable
performance constraints. Then prefer the solution with:

- A clear algorithm whose reasoning can be explained directly.
- One authoritative representation and a clear owner for each rule.
- Explicit data flow and effects.
- Fewer interacting cases, hidden assumptions, and duplicated decisions.
- Tests that establish behavior without depending on incidental structure.

Reuse existing code, standard libraries, platform features, and dependencies
when their semantics fit. Do not force the problem through a poor abstraction
or recreate a mature capability merely to avoid a dependency.

## Keep changes focused

Change as many files as the correction requires. Keep unrelated cleanup out
of scope. A broader refactor must remove identifiable existing complexity:
duplicated policy, inconsistent representations, repeated special cases,
unnecessary state, or an algorithm that is difficult to reason about.

Introduce abstractions when they clarify a current concept, isolate a real
boundary, or remove meaningful duplication. The number of implementations
alone does not decide whether an abstraction is useful.

When replacing an implementation, converge on one maintained path. Preserve
compatibility where required, with an explicit migration boundary and
retirement condition if a temporary path is necessary.

## Make the algorithm readable

Prefer named steps and straightforward control flow over compressed
expressions. Keep the rules that explain correctness close to the code that
enforces them.

Choose algorithms using the actual workload and resource constraints. Measure
when performance uncertainty could change the decision. Do not preserve a
weaker algorithm merely because it produces a smaller diff. Do not introduce
a more elaborate algorithm without a demonstrated need.

Document non-obvious reasoning and meaningful tradeoffs. A comment should
explain why the design works or why a constraint exists.

## Verify the promise

Use the repository's established checks and test conventions. Choose
verification according to the failure modes and consequences.

For a bug, reproduce the observed failure when practical. For an algorithm,
check its defining properties and relevant boundary cases. Verify the
governing rule across representative cases and meaningful boundaries,
including the original failure. Passing the reported example alone does not
prove a general correction.

Do not weaken requirements or tests to make a shortcut pass. Preserve input
validation at trust boundaries, error handling that prevents data loss,
security, accessibility, and required physical accuracy or calibration.
Distinguish implemented behavior from verified behavior.

## Communicate proportionally

Explain what changed, why the design is clearer, and what evidence supports
it. Include important compatibility limits or unresolved uncertainty.

Routine edits need little explanation. Algorithmic and ownership changes
need enough reasoning for the next maintainer to evaluate them.

Do not silently reduce the requested scope or treat this skill as permission
for unrelated work. Respect the user's instructions and repository workflow.

## Intensity

Default: full. `/ponytail lite|full|ultra` changes how strongly to investigate
unnecessary complexity; correctness, scope, and verification standards remain
unchanged. The selected level applies to relevant coding tasks for the session
until changed. `stop ponytail` or `normal mode` disables this mode.

| Level | Scope of investigation |
|-------|------------------------|
| **lite** | Apply these criteria within the immediate change. |
| **full** | Also examine the responsible component for a clearer solution. |
| **ultra** | Challenge existing representations and abstractions within the task's scope; consolidate when the benefit is concrete. |

Repair the rule and its owner, then demonstrate that the example follows
from the correction.
