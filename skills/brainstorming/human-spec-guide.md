# Writing a Human Spec

**Required for architectural design:** use this guide with
[spec-contract.md](spec-contract.md) before writing the first design artifact.
The reader should form an accurate mental model and identify the decisions
in roughly 3–5 minutes. This is a review target, not permission to omit a
schema change or hide a risk. For work too large to review coherently,
decompose the scope into the existing sub-project design cycles.

Use the human partner's language. Prefer a useful diagram, then a tree,
diff or table, then bullets, then short prose. Choose by information value:
a trivial relationship needs no decorative diagram. Explain unfamiliar
terms briefly. Keep only context relevant to this change and use actual
code/module names, marking proposed names as new.

## Opening and section selection

Start with a one-screen **Executive Summary**: what, why, the largest changes,
and a compact impact line or table explicitly covering database, architecture,
file/module organization, API, permissions, data migration and breaking
changes. Say which change and which do not; distinguish HTTP shape compatibility
from changed behavior. Unknown impact is an open investigation, not “none.”

Then select applicable sections in this order:

| Section | Review content |
|---------|----------------|
| Goals and scope | Problem, goal, in scope, out of scope; no functions, classes, SQL or test implementation |
| User / business flow | Who acts, what the system does, the result and relevant failure/recovery branches |
| System architecture | Relevant current context, changed components, calls and data movement |
| Functional changes | NEW / MODIFY / DELETE / UNCHANGED behavior; include unchanged behavior only when it matters |
| Database Changes | The complete affected schema diff, relationships and migration strategy |
| File / Module Structure Changes | Annotated tree, moves, responsibilities and boundaries |
| API / data flow / interactions | Changed contracts and synchronous/asynchronous exchanges |
| Key Technical Decisions | Decision, reason, impact, alternative considered, reason rejected |
| Impact | Affected users, callers, services, deployment or operations |
| Risks / compatibility | Failure modes, compatibility window, breaking behavior, mitigations and rollback risks |
| Acceptance criteria | Stable IDs, observable action/condition and expected result |

Omit unchanged sections rather than filling them with N/A. The compact
summary still states impact status. Combine overlapping impact/risk sections
when that improves scanning. Separate agreed requirements, proposals and
open decisions; resolve approval-relevant unknowns before approval.

## Diagrams as code

Use a fenced format that the target reader can render. Choose by the
relationship being explained, not by a requirement to use one format:

| Need | Preferred format |
|------|------------------|
| Simple user/business flow, architecture, sequence, state or small dependency graph | Mermaid |
| Ordinary entity relationships | Mermaid ER |
| Complex process | Mermaid or PlantUML activity |
| Complex sequence, component/service boundaries, package/class model, C4-style view | PlantUML |
| Deployment nodes and environments | PlantUML deployment |
| Complex data model | Mermaid ER or PlantUML |
| Dependency network, module/package/file dependencies, DAG or pipeline | Graphviz / DOT |

For PlantUML use `plantuml` or `puml`; for Graphviz use `dot` or `graphviz`.
Mermaid is sufficient for a clear simple diagram; added syntax complexity
is not a benefit. Use self-contained diagram source rather than requiring
remote includes or a new plugin dependency. If the reader cannot render the
best format, offer a supported equivalent or local preview and retain the
source. Disclose rendering limitations; do not claim an unrendered diagram
was visually checked. The browser companion remains optional.

Apply these triggers:

- A meaningful user/business journey gets a flow diagram. Keep function,
  class and test names out of the user flow.
- New services, workers, queues, databases or external systems, or changes
  to module boundaries, calls or data flow get an architecture diagram.
- Interactions among three or more important components normally get at
  least one architecture, sequence or data-flow diagram. If the relationship
  is trivial and a diagram adds no information, explain it compactly instead.
- Exchanges among systems get a sequence or data-flow diagram showing who
  calls whom, where data goes, and which steps are synchronous/asynchronous.
- Significant architecture, model or boundary changes use Before / After
  views to make the difference visible. Mark Existing / NEW / MODIFY / DELETE
  where applicable, using labels rather than relying on color alone.

One diagram answers one question. Split a dense drawing into overall
architecture, changed subsystem and detailed flow. Reuse a diagram that
answers multiple requirements clearly; do not draw the same graph repeatedly.
Keep diagram names and arrows consistent with the schema, tree and prose.

## Database Changes — required for any schema change

Create a standalone **Database Changes** section (translated into the
partner's language). Put the schema diff, ER view and Migration subsection
inside it, so reviewers can find the entire database change in one place.

Show exactly what changes using a schema diff or a table. Cover all applicable
operations: NEW/DROP/ALTER TABLE; NEW/DROP COLUMN; old/new types, NULL/NOT NULL,
DEFAULT, primary and foreign keys (including delete/update behavior), UNIQUE,
INDEX and relationships. State “no default” where applicable; do not silently
invent missing constraints. A reviewer must understand the schema change
from this document alone, without following a link to the Agent Spec.

For example, a new relationship can be summarized without executable SQL:

```text
NEW TABLE user_resource_scopes
+ user_id BIGINT NOT NULL; no default
+ server_id BIGINT NOT NULL; no default
+ PRIMARY KEY (user_id, server_id)
+ FK user_id -> users.id ON DELETE RESTRICT
+ FK server_id -> servers.id ON DELETE RESTRICT
+ INDEX idx_resource_scope_server (server_id)
```

Include an ER diagram for two or more new related tables, changed table
relationships or foreign keys, or a substantial data-model change. Show
cardinality and relevant keys; the schema diff carries full changed column
details so the diagram can remain focused. Mermaid ER is the usual choice;
PlantUML can clarify a more complex model.

Every schema change includes a compact **Migration** subsection covering:

| Review question | Required answer |
|-----------------|-----------------|
| What and in what order? | Expand/alter, code rollout, backfill, validation, switch and any later cleanup |
| Historical data? | Backfill source, handling of missing/invalid records, validation before switching |
| Availability? | Planned downtime or online approach; locking/load assumptions and rehearsal needed |
| Compatibility? | Which old/new code and schema coexist, and for how long or until what condition |
| Data-loss and rollback risk? | What rollback restores, what it cannot restore, and what happens to writes made after rollout |

For a change that needs no backfill, say why in the migration summary.
Distinguish planned online behavior from evidence that it is safe at the
actual scale. Complete SQL and migration code go in the Agent Spec/plan.

## File / Module Structure Changes — required for structural changes

Create a standalone **File / Module Structure Changes** section (translated
into the partner's language), separate from System Architecture. Its contents
are the annotated tree, move list and short responsibility/boundary explanation.

For changed organization, directories, packages, service boundaries or file
responsibilities, show a tree of the affected area. Support NEW, MODIFY,
MOVE and DELETE; state both endpoints of every move.

```text
src/
├── api/
│   └── permissions.ts        # MODIFY: HTTP request/response only
└── services/
    └── permission/           # NEW: reusable permission boundary
        ├── index.ts         # NEW
        └── checker.ts       # NEW
MOVE responsibility: src/api/permissions.ts role checks
                  -> src/services/permission/checker.ts
```

Distinguish moving a responsibility from moving an entire file. After the
tree, briefly explain **Why**, **Responsibility Change** and **Boundary Change**.
Use Before / After for substantial reorganization; Graphviz is useful when
the question is who depends on whom. An ordinary function edit at a line
number is a plan detail, not a structural change.

## API and interaction changes

When APIs change, show NEW / MODIFY / DELETE endpoints, method/path, request
and response changes, authentication, permission and breaking-change impact.
Keep this at contract level; a minimal payload and status codes often suffice.
For example: `PUT /api/users/{id}/resource-scopes`, request
`{"server_ids":[1,2]}`, success `204`, invalid IDs `400`, non-admin `403`,
company login and super-admin authorization required. Explain whether the
operation replaces or appends assignments. Controller, handler and validation
implementations belong in the Agent Spec.

Use sequence/data-flow diagrams for system exchanges; label queues or async
messages and distinguish an accepted job from a completed result. Surface
failure/retry behavior when it affects the user's understanding or approval.

## Decisions, acceptance and review

Record only consequential decisions: e.g. role + resource scope rather than
one role per resource. A small decision table states reason, impact,
alternative and rejection reason. Keep algorithms and incidental library
choices in the Agent Spec unless they change an approval-relevant constraint.

Acceptance is a checklist/table with stable IDs, actions/conditions and
observable results. Cover changed behavior, affected contracts, permissions,
and migration/rollback outcomes when applicable. Test code belongs in the plan.

Before requesting approval, check:

- Can the summary communicate size, impact and breaking behavior at a glance?
- Can the reader trace the principal flow and see current → changed behavior?
- Are applicable schema diffs, migration, ER, module trees and API contracts
  present, with exact values and no hidden decisions deferred to the Agent Spec?
- Do diagrams, trees, diffs and prose agree? Are diagrams focused and readable?
- Are important decisions, risks, assumptions and acceptance visible without
  navigating a long implementation narrative?
- Is this reviewable in roughly 3–5 minutes? Remove repetition or split an
  oversized scope; preserve all information needed to approve the change.
