# Reviewable Software Design

Use this guide for the single written software design at
`docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`. The same document
must support your human partner's approval and give a builder enough
detail to plan. Put exact implementation steps, test code, and commands
in the later implementation plan.

## Start with the story and the decision

Open with a short account of what happens today, who is affected, what
problem that causes, what will happen instead, and why. A reader should
understand the change before seeing table names, endpoints, or component
names. State which choices the human partner already made, which are your
proposals, and which still need an answer. Do not turn an unstated choice
into an agreed requirement.

Then give a compact, roughly one-screen impact overview. Cover database,
architecture and data movement, file or module organization, user or
business workflow, interfaces and permissions, deployment, compatibility,
and rollback. Say what changes, what stays the same when that matters,
and what is still unknown. Distinguish an unchanged interface shape from
changed behavior. "No impact" requires investigation; an unknown is a
question to resolve before approval when its answer changes the design.

Use your human partner's language. Expand each acronym and explain each
specialized term at first use, including terms in headings, diagrams, and
tables. Do this even for terms common to engineers, such as a worker,
queue, database index, nullable field, or transaction. For example,
describe "backfill" as copying existing records into the new structure,
and "dual-write" as writing both old and new structures during a
transition. Before naming a mechanism, say in ordinary words what it
does and why this change needs it. Explain what a technical choice means
for users, data, or operations. Give cause and effect, not a list of
unexplained shorthand. Keep the opening free of code and dense schema
notation; put precise technical detail in the relevant section below.

## Show applicable changes in the same document

Choose sections by the decisions this project actually needs. A small
change need not acquire empty database or architecture sections. A
substantial change should let the reader compare the current and proposed
state without reconstructing it from implementation steps.

### Database and stored data

If tables, fields, relationships, or stored meaning change, use a
findable **Database Changes** section. Explain in plain language what
each changed structure represents and why it is needed. Then show the
complete affected before/after schema or a clear diff: created, changed,
and removed tables and columns; old and new types; nullability; defaults;
primary and foreign keys (including delete/update behavior); uniqueness,
indexes, and relationships. State exact values when decided, and mark
undecided values as open questions rather than inventing them. When two
or more related tables, a foreign key, or a substantial data model changes,
include an entity relationship diagram with cardinality and relevant keys.
The schema diff still carries the exact constraints.

Explain each new field's purpose in ordinary words before or beside its
exact type and constraint. Explain how existing records reach the new
structure and in what order schema, code, backfill, validation, and cleanup
will roll out. State the expected availability or lock/load effect, which
old and new versions must coexist, and when switching is safe. Describe
what rollback restores, what it cannot restore, and what happens to data
written after rollout.
If no existing data moves, say why. Distinguish an intended online
migration from evidence that it is safe at the actual scale.

### Architecture, modules, and workflow

Show the relevant current components and responsibilities, then the
proposed ones. Explain who calls whom, where data moves, what is
synchronous or asynchronous, and which boundaries or dependencies change.
For a meaningful user or business flow, walk through the actor's action,
the system's response, the visible outcome, and important failure or
recovery paths. A before/after diagram, flow, or short annotated module
tree helps when it makes these relationships easier to see; label new,
changed, moved, and removed parts without relying on color alone. Explain
why responsibilities move. Do not make a reader infer the flow from a
list of files or functions.

For changed file, module, service, or package boundaries, add a findable
**File / Module Structure Changes** section with an annotated tree or
before/after list. Mark new, changed, moved, and removed parts. For a
move, state both the old and new home of the responsibility, why it moves,
and how the boundary with callers changes. Ordinary line edits belong in
the plan; the design owns structural decisions.

Use a simple diagram when a meaningful user journey, new worker or
service, changed relationship, or three or more important components
would be harder to follow in prose. A before/after view should show
substantial boundary changes, and a sequence or data-flow view should
distinguish an accepted request from completed asynchronous work. Prefer
a self-contained, renderable Mermaid diagram for a simple flow or entity
relationship; use PlantUML or Graphviz only when they express a more
complex relationship more clearly. Label the parts and arrows in plain
language, mark what changed without relying on color, and check that the
diagram agrees with the schema and prose. Split an unreadably dense
diagram instead of making the reader decode it.

### Interfaces, permissions, and compatibility

For changed interfaces, show the method or event, request and response
shape, validation and error behavior, authentication, and who may perform
each action. State important state transitions, ordering, concurrency,
and failure or retry behavior when they affect the approved outcome. Say
whether behavior changes even if the external shape is unchanged.
Describe compatibility for existing callers and users, and the transition
or rollback behavior that affects them.

### Decisions, risks, and acceptance

Record consequential choices with their reason, impact, and any serious
alternative considered. Surface assumptions and risks that affect
approval. Give each acceptance criterion a stable identifier and an
observable condition and outcome. Cover changed behavior and, where
applicable, database migration, compatibility, rollback, interfaces,
and permissions. Include enough precise rules and failure handling for
a builder to plan from this document alone; do not defer them to another
spec. Leave exact file edits, test bodies, and commands to the plan.

## Before handing it over

Read the opening as a reviewer who has only a few minutes. Can they say
why this change is needed, what will be different, which database and
architecture decisions they are approving, how the main flow works,
and what could go wrong during rollout? Follow every acronym, schema
diff, and diagram label back to a plain explanation. Check that the
sections agree, that no guessed requirement is presented as approved,
and that no approval-relevant decision is hidden in the later plan.
Revise the same document until it passes this check, then use the
existing builder check and human approval gate in `SKILL.md`.
