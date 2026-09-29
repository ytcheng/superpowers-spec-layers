# Spec Document Reviewer Prompt Template

Use this template when dispatching a spec document reviewer subagent.

**Purpose:** Verify the Human Spec is reviewable, or the Agent Spec is complete, aligned with approved design, and ready for planning. Automated review does not grant human approval.

**Dispatch after:** Spec document is written to docs/superpowers/specs/

```
Subagent (general-purpose):
  description: "Review spec document"
  prompt: |
    You are a spec document reviewer. Verify this spec is complete and ready for planning.

    **Spec to review:** [SPEC_FILE_PATH]
    **Stage:** [Human Spec | Detailed Agent Spec]
    **Approved Human Spec for Agent Spec review:** [path and approval reference, if this is an Agent Spec]

    Read [spec-contract.md](spec-contract.md). For Human Spec review also read
    [human-spec-guide.md](human-spec-guide.md); apply its conditional requirements.

    ## What to Check

    | Category | What to Look For |
    |----------|------------------|
    | Completeness | TODOs, placeholders, "TBD", incomplete sections |
    | Consistency | Internal contradictions, conflicting requirements |
    | Clarity | Requirements ambiguous enough to cause someone to build the wrong thing |
    | Scope | Focused enough for a single plan — not covering multiple independent subsystems |
    | YAGNI | Unrequested features, over-engineering |
    | Human review | One-screen summary and impact flags; applicable schema diff/migration/ER, module tree, API contracts, decisions, risks and acceptance visible without implementation narrative |
    | Visual consistency | Useful diagram format and scope; diagrams agree with schema, module names, contracts and prose |
    | Agent Spec alignment | Every Human acceptance ID and decision realized; no unapproved changes to schema, behavior, boundaries, API/permissions or migration/rollback |

    ## Calibration

    **Only flag issues that would cause real problems during implementation planning.**
    A missing section, a contradiction, or a requirement so ambiguous it could be
    interpreted two different ways — those are issues. Minor wording improvements,
    stylistic preferences, and "sections less detailed than others" are not.

    Approve unless there are serious gaps that would lead to a flawed plan.

    ## Output Format

    ## Spec Review

    **Status:** Approved | Issues Found

    **Issues (if any):**
    - [Section X]: [specific issue] - [why it matters for planning]

    **Recommendations (advisory, do not block approval):**
    - [suggestions for improvement]
```

**Reviewer returns:** Status, Issues (if any), Recommendations
