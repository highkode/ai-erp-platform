# High Kode AI Action & Autonomy Registry

- Status: Accepted
- Date: 2026-08-10
- Scope: High Kode AI ERP Platform

## 1. Purpose

This registry defines which actions an AI system may perform, under what
conditions, and what level of human control is required.

The fundamental rule is:

> Autonomy belongs to the Action, not to the Agent.

An Agent does not receive unrestricted authority simply because it is trusted.

Each individual action must have its own authorization and autonomy policy.

## 2. Autonomy Levels

### L0 — Observe

AI may:

- Read authorized data
- Analyze information
- Produce summaries
- Produce recommendations

AI must not modify business state.

### L1 — Propose

AI may:

- Prepare an action
- Generate a draft
- Recommend a decision

Human approval is required before execution.

### L1.5 — Execute After Explicit Approval

AI prepares the action and presents it for approval.

A human explicitly approves the specific action.

The system then executes the approved action.

### L2 — Controlled Autonomous Execution

AI may execute the action automatically when:

- The action is explicitly registered
- Input data is within policy
- Preconditions are satisfied
- No escalation condition is triggered
- The action is auditable
- Idempotency is enforced where applicable

### L3 — High-Impact Autonomous Execution

Not enabled by default.

Any L3 action requires a separate architecture, security, legal, and
business approval.

## 3. Action Registry

| Action | Default Level | Human Approval | Notes |
|---|---|---:|---|
| Read customer data | L0 | No | Authorized data only |
| Summarize customer/account data | L0 | No | No state change |
| Classify support ticket | L2 | No | Deterministic output required |
| Draft customer reply | L1 | Yes | AI prepares response |
| Send customer reply | L1.5 | Yes | Explicit approval |
| Create sales lead | L2 | No | Within defined rules |
| Update lead status | L2 | No | Within defined workflow |
| Create quotation draft | L1 | Yes | Human reviews commercial terms |
| Send quotation | L1.5 | Yes | Explicit approval |
| Create invoice | L1.5 | Yes | Financial impact |
| Record payment | L1.5 | Yes | Financial record |
| Refund customer | L1.5 | Yes | Financial impact |
| Cancel subscription | L1.5 | Yes | Customer-impacting action |
| Change subscription price | L1.5 | Yes | Commercial impact |
| Delete business record | L1.5 | Yes | Destructive action |
| Change accounting configuration | L1.5 | Yes | High-risk action |
| Send mass campaign | L1.5 | Yes | External communication |
| Change AI policy | L1.5 | Yes | Governance-sensitive |
| Grant AI permissions | L1.5 | Yes | Security-sensitive |

This table is the initial registry and must evolve as real workflows are
validated.

## 4. Action Authorization

Every executable action must define:

1. Action identifier
2. Description
3. Required input
4. Allowed actor
5. Required data scope
6. Autonomy level
7. Approval requirement
8. Preconditions
9. Validation rules
10. Failure behavior
11. Audit requirements
12. Idempotency requirements

## 5. Agent vs Action

An Agent is not assigned a blanket autonomy level.

For example:

```text
Sales Agent
    |
    ├── Read Lead                 → L0
    ├── Update Lead Status        → L2
    ├── Draft Quotation           → L1
    └── Send Quotation            → L1.5
## 6. Human Approval Gate

When an action requires approval:

```text
AI Agent
   ↓
Action Request
   ↓
Policy Check
   ↓
Human Approval
   ↓
Action Execution
   ↓
Audit Log