# Single Design Document: Behavior Checks

Date: 2026-10-17. Upstream baseline: `obra/superpowers` main at
`bb92a77741419a4ab5f06e711a283343f1ada0c3` (v7.0.0).

These were fresh-context, read-only prompt probes, not automated proof that
every future design will meet the guide. No product code was generated.

| Probe | Baseline observation | Candidate observation |
|-------|----------------------|-----------------------|
| Design artifact count | The prior fork probe produced a Human Spec and then proposed an Agent Spec. | Both candidate project probes produced one design document for approval. |
| Database review | An upstream single-document probe omitted detailed schema and migration decisions. | The order-cancellation probe showed the affected before/after schema, status constraint uncertainty, rollout, and rollback including already-cancelled orders. |
| Plain-language review | An upstream probe used unexplained technical shorthand and introduced unstated constraints. | The background-thumbnail probe began with the present problem and user-visible change, separated proposals from unknowns, and showed architecture, flow, data fields, client compatibility, and recovery. It still used a few unexplained mechanism names, so the guide and builder check were tightened. |
| Builder check after refinement | Earlier guidance asked primarily for missing implementation decisions. | Given a deliberately jargon-heavy outbox/CDC/worker excerpt, the revised builder check raised six substantive questions, including the meaning and consequences of those terms and the migration sequence. |

The candidate probes also exposed approval-blocking unknowns rather than
inventing answers: payment or fulfillment effects for cancellation, and
older client compatibility and worker deployment for thumbnails. The
reviewer-model rule was checked in the execution skill and prompt templates.
A fresh-context read-only probe selected the controller model for task,
scoped re-review, and final review, and the executing session's model for
the native final review. This was not a live plan-execution session.
