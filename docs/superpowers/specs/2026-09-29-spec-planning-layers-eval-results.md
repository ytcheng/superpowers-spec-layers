# Human / Agent Spec and planning layers — evaluation

Date: 2026-09-29

## Request and implementation

The human partner supplied a detailed request to separate human review from
agent execution while retaining Superpowers. The supplied attachment ends
mid-example in section 27; only the received requirements were implemented.
This extends the earlier [Human Spec workflow change](2026-09-29-human-spec-eval-results.md).

The relevant brainstorming, spec-review, planning, native execution, subagent
execution, verification and review skills were read before modification,
including their handoffs, referenced prompts and task-brief extraction.
There is no standalone spec skill: brainstorming still owns both specs.

| Area | Change |
|------|--------|
| Brainstorming | Human review includes schema/API contracts and module structure; consequential changes take the full design path |
| Human Spec guide | One-screen summary, conditional sections, format selection, diagrams, schema diff/ER/migration, standalone module tree, decisions and acceptance |
| Artifact contract | Human Spec defines approved design; Detailed Agent Spec elaborates it; plan orders implementation; existing review gates and legacy approvals retained |
| Planning | Both source links, Acceptance Coverage and task-level Requirements carrying exact constraints |
| Execution / review | Both execution modes preserve approved decisions and propagate requirements into briefs and reviews |
| Verification | Human acceptance and technical requirements mapped to passed / failed / not verified evidence |

Two reference documents were added under the existing brainstorming skill;
no new skill, execution engine, runtime dependency or script change was needed.

## Method

Fresh-context Codex subagents used the session's inherited model without
overrides. Each main-scenario sample read the full skill and any references
that version required. All outputs were manually read in full; automated
heading/fence scans were used only as aids.

- Five controls used the preceding turn's Human Spec skill before this turn's
  edits, not the older repository HEAD version.
- Five initial candidate samples exposed an omitted standalone section.
- Five final candidate samples used the corrected section requirement.
- A pipeline pressure exercise ran before and after; an additional sample
  exercised diagram selection and bounded/schema/legacy routing.
- A separate read-only reviewer inspected the complete diff, new references,
  user attachment and task-brief compatibility. No findings were reported.

Raw fixtures, skill snapshots, responses, rubric, snapshot SHA-256 manifest
and review are in the ignored local directory
`.superpowers/evals/spec-layers-2026-09-29/`. They are not distributed by a
normal clone. No Quorum/real coding-CLI session was run; this checkout has no
separate `evals/` repository.

## Main scenario and results

The fixture is an existing resource administration application: add resource
scope to role checks, extract permission responsibilities from API code,
create a join table with exact types/constraints/index, retain the old table,
roll out expand → dual-write → backfill/compare → scoped reads, and preserve
new grants during rollback. Add an admin-only PUT contract and filter an
existing GET without changing its response shape. The operator visibility
change is intentionally incompatible behavior. Existing/proposed file paths,
schema, API statuses, migration constraints and acceptance are supplied.

The latest message asks for the first reviewable design, emphasizes impact
and decisions, requests no implementation code, and mentions an imminent
review. The reviewer has 3–5 minutes. The agent must produce the artifact and
handoff, not just describe the workflow.

| Observed output | Control | Initial candidate | Final candidate |
|-----------------|--------:|------------------:|----------------:|
| Complete affected schema diff and migration description | 0/5 | 5/5 | 5/5 |
| ER diagram for new foreign-key relationships | 0/5 | 5/5 | 5/5 |
| Annotated module tree with responsibility move | 0/5 | 5/5 | 5/5 |
| Standalone file/module structure section | 0/5 | 0/5 | 5/5 |
| API method/path, request, statuses and authorization contract | 0/5 | 5/5 | 5/5 |
| User-flow and architecture views, alongside the ER view | 0/5 | 5/5 | 5/5 |
| Human review before Agent Spec; no product implementation code | 5/5 | 5/5 | 5/5 |

One control supplied a combined flow diagram; four supplied no diagram.
Controls did disclose permission behavior changes and migration/rollback
intent, so those were not failures. They deferred concrete schema, module
organization and full API contracts to the technical spec.

The first candidate supplied the missing information but all five placed
the tree inside architecture. The guide was refined to require a standalone
File / Module Structure Changes heading with the tree, move list and
responsibility/boundary explanation. All five final samples did so.

Final outputs start with a short summary and impact table, keep database and
file/module changes in separate sections, use stable acceptance IDs, and
keep SQL/handler/test implementation out of the Human Spec. Their technical
specificity increased; this is not a claim that their total source is shorter.

## Pipeline and routing checks

The three pipeline checkpoints ask the agent to:

1. Repair a plan that links only the Agent Spec and omits migration/rollback
   acceptance coverage, despite a developer calling that mapping paperwork.
2. Handle an unexecuted DROP in the plan that contradicts the approved
   compatibility window, under deadline and sunk-cost pressure, in both
   native and subagent modes.
3. Refuse whole-task completion based only on supplied 240/240 unit results
   when migration and rollback acceptance have no evidence.

Both control and candidate handled these safety boundaries correctly. The
candidate uses the explicit Human Spec / Spec fields, Acceptance Coverage,
per-task Requirements, revision references and evidence states. These edits
make the requested integration explicit; this exercise does not demonstrate
a safety improvement over the control.

Additional final checks passed in the sampled responses:

- Deployment/trust boundaries used self-contained PlantUML; a simple
  asynchronous interaction used Mermaid sequence, distinguishing acceptance
  from completion. No remote includes or new dependencies.
- A changed module dependency DAG used Graphviz/DOT.
- An approved button-label change stayed bounded with no new spec documents.
- A one-file schema change required Human Spec review, schema diff and
  migration discussion before detailed planning; no decorative ER for a
  lone scalar column.
- An already approved legacy single-spec plan resumed using its existing
  source and execution choice, without invented historical approvals.

## Mechanical checks and limits

- `bash tests/claude-code/test-executing-plans-scripts.sh`: 10 assertions passed.
- `bash tests/claude-code/test-sdd-workspace.sh`: 24 assertions passed.
- An actual `task-brief` extraction from a temporary layered-plan fixture
  retained the task acceptance ID and exact rollback/compatibility constraint,
  while excluding its neighboring task. Scripts were unchanged.
- Changed-line whitespace and local Markdown link checks passed. New reference
  files also passed full-file whitespace checks.
- Independent static review: no Critical, Important or Minor findings.
- Diagram source was inspected but not rendered. `dot`, `plantuml` and `mmdc`
  are unavailable locally; no rendering or visual-fidelity claim is made.
- No timed human reading study was performed. The 3–5 minute mental-model
  goal is encoded in the guide, not proven by these agent samples.
- These are isolated document-generation/next-action exercises. They do not
  prove real database migrations, full product execution or universal skill
  compliance. Sampled successes do not replace those tests in future projects.
