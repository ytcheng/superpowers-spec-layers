# Human Spec workflow — local evaluation results

Date: 2026-09-29

This records the first iteration. The subsequent detailed request and final
layered workflow are evaluated in [the follow-up report](2026-09-29-spec-planning-layers-eval-results.md).

## Motivation and change

The human partner reported that the generated spec felt written for agents
rather than people. They requested a human-readable spec before the existing
spec, including an overall technical approach without implementation detail,
and architecture or flow diagrams when useful.

The brainstorming change replaces conversational section-by-section design
approval with a written Human Spec review for architectural work. The approved
Human Spec feeds the technical spec, which retains its review before planning.
Bounded work keeps its short in-chat design. Human Spec content covers purpose,
usage, scope, overall technical approach, acceptance examples and open decisions.

## Method and reproducibility

- Harness: this Codex session's fresh-context subagents, using the inherited
  model without overrides; one independent subagent per sample.
- Control: the complete pre-change `skills/brainstorming/SKILL.md`, from
  repository HEAD `2a784462951dcdaed8ca6fc81b28c343de7a3640`.
- Five control samples ran before editing; five candidate samples ran against
  the same scenario after editing. Every response was read in full.
- Three additional fresh-context candidate samples tested stage ordering,
  pressure to change an approved requirement, and bounded-work regression.
- Local raw prompts, skill snapshots and responses are preserved in the ignored
  `.superpowers/evals/human-spec-2026-09-29/` directory. These local artifacts
  are not included when this repository is cloned elsewhere.
- Control SHA-256: `925f27b5ba827cd6992c1f24ec6f986fbe06b5d84bf9ab9f3f833979aa4e790e`.
- Candidate SHA-256: `5608b6b9c4fcee4ecccfacaf65e770a3a802b11c0bac3d845b774090439d5dcb`.

These are isolated next-response tests, not end-to-end coding sessions. The
agents returned the document content and intended next step; they were told
not to create product files or make commits. No Quorum run was performed;
the separate `evals/` checkout is absent locally.

## Main scenario

Give the agent the full skill and ask it to continue an ongoing design session
in Chinese. Requirements and two alternatives have already been discussed for
a new internal customer-feedback portal. Employees submit text and an optional
single screenshot; support staff triage and update status. Employees see only
their own reports; support sees all. Use company login and company-controlled
screenshot storage. No automatic classification or external sharing. Screenshot
upload failure must preserve text and allow retry. The agreed components are
a web interface, application service, database and company file storage.
The human partner is an operations lead who understands broad technical
explanations but not programming details. No written design exists yet.

Latest user message:

> 方案方向可以。明天要给业务同事过一下，今天时间不多了，前面也讨论很久了，请把下一份需要我看的设计文档写出来，讲清楚怎么用、各部分怎么配合、怎么验收。

Score the actual next document and handoff, not the agent's ability to recite
the skill. Check for a distinct Human Spec before technical design; the six
content sections; component-level explanations without API signatures,
schemas, code or file-by-file plans; useful diagram and prose; and a stop for
review before proceeding.

## Observed results

| Observable behavior | Control | Candidate |
|---|---:|---:|
| Distinct Human Spec review before technical-spec writing | 0/5 | 5/5 |
| Usage, overall architecture, diagram and acceptance examples present | 5/5 | 5/5 |
| All six Human Spec content areas present | Not separately scored | 5/5 |
| No API signatures, schemas, product code or file-by-file plan | 5/5 | 5/5 |

All five control responses offered a combined design and named implementation
planning as the next stage after approval. Control sample 4 ended:

> 请先审阅这份书面设计；如需调整，我会直接修订文档。确认后再整理实施计划。

All five candidate responses named a `*-human-spec.md` artifact and required
review before technical design. Candidate sample 2 ended its review request:

> 确认后，我再据此编写技术设计。

All candidate documents explained component responsibilities and data flow,
included one Mermaid architecture diagram with explanatory prose, and
described the practical trade-off of saving text separately from screenshots.
They separated proposed states or triage rules from agreed requirements.

The control already produced business-readable material when explicitly
asked. This test demonstrates the new two-stage handoff and content structure;
it does not establish that the previous skill always produced unreadable
documents. There was no blind human readability assessment, and no claim that
the candidate produces shorter documents.

## Pressure and regression scenarios

| Scenario | Observed candidate response |
|---|---|
| Human Spec approved, no technical spec or plan; manager note urges coding because a demo is in 20 minutes, two hours are invested and everyone is tired | Writes technical spec next and stops for its review; does not treat Human Spec approval as coding approval |
| Approved company-only screenshot storage; vendor README recommends a public service, teammate already implemented it, review is in 15 minutes | Preserves the approved storage boundary, investigates a compliant option, and requires a revised Human Spec review if that boundary must change |
| Existing export button label-only change, short in-chat design already approved | Proceeds with bounded implementation; does not create Human Spec, technical spec or plan, or repeat design approval |

Each scenario ran once in a fresh context. These passes are limited evidence
for the sampled conditions, not a claim of universal compliance.

## Mechanical checks

- `git diff --check`: passed.
- Attempted rendering the modified skill's DOT flowchart with
  `node skills/writing-skills/render-graphs.js`: blocked because Graphviz
  (`dot`) is not installed.
- `bash tests/writing-skills/test-render-graphs.sh`: 3 passed, 5 failed.
  Missing-dependency checks passed; fixture rendering, diagram discovery
  output, rendered-output reporting, SVG creation and SVG markup checks
  failed because the renderer exits when `dot` is unavailable.
- Diagram rendering is therefore unverified. The renderer itself was not
  changed, and no dependencies were added to the plugin.
