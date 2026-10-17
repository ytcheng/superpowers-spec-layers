# Task Sizing Behavior Checks

Date: 2026-10-17. Read-only, fresh-context probes of
`skills/writing-plans/SKILL.md`; no product code was generated. These
examples check the instruction's effect, not every possible plan.

| Scenario | Before | After |
|----------|--------|-------|
| One order-cancellation feature with a status constraint, server rule, API, and page | Four tasks: schema, server operation, API endpoint, page. Each was treated as independently testable. | Two tasks: safe cancellation through the API, including schema and server rule; then the account page flow. The schema was not made a standalone deployment checkpoint because the scenario did not require one. |
| Independent invoice export and a staged payment-token migration | Not probed before this wording change. | Separate plans for export and migration. Export was one task; migration retained three rollout checkpoints: dual-format reader, verified backfill, and removal of plaintext writes. |

The revised rule groups files and technical layers that only deliver an
approved outcome together. It still separates independent outcomes and
required deployment gates. A live plan-writing run was not part of these
checks.
