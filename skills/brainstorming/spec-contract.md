# Design artifacts and authority

Read this contract when writing or consuming the Human Spec, Agent Spec or
implementation plan. It extends the existing Superpowers workflow; it does
not introduce another execution path.

| Artifact | Question answered | Owns |
|----------|-------------------|------|
| Human Spec | What changes, why, and which decisions need approval? | Behavior, scope, architecture, schema changes, module boundaries, API contracts, compatibility, migration strategy, risks and acceptance |
| Detailed Agent Spec | What technical detail is needed to realize the approved design? | Exact interfaces, data rules, algorithms, failure handling, migration mechanics and technical validation requirements |
| Implementation Plan | Which code changes happen in what order, and how are they verified? | Exact files, task dependencies, implementation/test code, commands and expected results |

The Human Spec is organized for review, not made by truncating an Agent Spec.
Database schema diffs, changed API contracts and module trees are reviewable
design information. Full SQL, handler bodies and line-by-line edits belong
in the Agent Spec or plan.

## Files and handoffs

Use the existing directories; a partner's location preference overrides these:

- Human Spec: `docs/superpowers/specs/YYYY-MM-DD-<topic>-human-spec.md`.
- Agent Spec: `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`. This is
  the existing technical/design spec, not a fourth document.
- Plan: `docs/superpowers/plans/YYYY-MM-DD-<topic>.md`.

Brainstorming writes and self-reviews the Human Spec, obtains human approval,
then writes the Agent Spec. The existing Agent Spec review and plan review /
execution-method handoff still apply. A review request states the decisions
or changes requiring attention and links the artifacts; an automated review
is not human approval. Preserve approvals already explicitly given for the
actual artifact/version. Bounded work without these documents retains its
in-chat design and approval; spikes retain their question and probe.

The Agent Spec links its Human Spec near the top and maps each acceptance
criterion to the technical section that realizes it. Give criteria stable
IDs (for example `AC1`) and important decisions stable references. The plan
links both documents and maps criteria to tasks and verification. Record
which revision was approved using a commit, document revision or a dated
approval reference; a prefilled `Approved` label is not approval evidence.

## Authority during planning and execution

Explicit human instructions govern the work. Within the approved design,
the Human Spec governs intent and reviewable decisions, the Agent Spec
elaborates them, and the plan orders the work. Detail does not outrank an
approved decision. If documents disagree, resolve the discrepancy before
performing the affected work; do not silently pick the newer or longer file.

- A plan typo or implementation choice that preserves the approved design
  can be corrected and recorded in the normal ledger/ruling workflow.
- A proposed change to approved behavior, schema, API/permission contracts,
  module boundaries, migration/rollback strategy, compatibility or acceptance
  needs an updated Human Spec and human approval. Explain the impact and
  synchronize the Agent Spec and plan after approval. Continue independent
  work that does not depend on that decision.
- If a required source is missing or approval is unclear, retrieve it from
  the conversation/files first. Do not infer approval or invent the design.

For an already approved legacy plan with a single spec, retain that spec as
its source; do not manufacture a historical Human Spec or repeat completed
approvals. Apply this layered workflow to new design or a material redesign.
A small bounded change need not grow documents just to populate a header.

## Completion

Verification checks both human acceptance and technical requirements.
Report each criterion as passed, failed or not verified with evidence.
Passing unit tests alone does not establish migration, rollback, compatibility
or user-flow acceptance. Preserve the evidence and any design changes in the
final handoff; never silently remove an unverified criterion to claim success.
