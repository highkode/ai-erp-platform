# High Kode — AI-First Business
## Technical Architecture & Repository Structure

**Version:** 1.1  
**Date:** 2026-08-10  
**Status:** Architecture baseline for implementation

---

# 1. North Star

High Kode is being built as an **AI-First Business**.

The objective is not to build an AI chatbot around an ERP. The objective is to make High Kode itself increasingly operable by AI.

The core model is:

```text
ERPNext
  = System of Record

high_kode_ai
  = AI Execution Layer

high_kode_integrations
  = Integration Layer

Human Governance
  = Risk / Approval Layer

Flutter
  = AI Interface Layer (P0.5 / P1)

SaaS
  = Future Productization Layer
```

The order matters:

```text
Build
  ↓
Use internally
  ↓
Measure
  ↓
Prove
  ↓
Harden
  ↓
Productize
```

High Kode is **Customer Zero**.

---

# 2. Core Architecture

```text
                         HIGH KODE
                             │
              ┌──────────────┴──────────────┐
              │                             │
        Business Core                 AI Interface
              │                       P0.5 / P1
          ERPNext                    Web / Flutter
              │                             │
              └──────────────┬──────────────┘
                             │
                      high_kode_ai
                             │
                    AI Execution Layer
                             │
              ┌──────────────┼──────────────┐
              │              │              │
           Agents         Actions          Runs
              │              │              │
              └──────────────┼──────────────┘
                             │
                  high_kode_integrations
                             │
              ┌──────────────┼──────────────┐
              │              │              │
         DingDongBot       AIWeb       External APIs
```

The architecture intentionally keeps responsibilities separate.

---

# 3. ERPNext — System of Record

ERPNext remains the authoritative source of business state and transactions.

Examples:

- CRM
- Leads
- Opportunities
- Customers
- Quotations
- Sales Orders
- Projects
- Tasks
- Accounting
- HR
- Support
- KPIs
- Workflows
- Permissions
- Reports

## Principle

> Do not rebuild ERPNext inside the AI layer.

AI operates the business through ERPNext.

ERPNext remains the business record.

---

# 4. `high_kode_ai` — AI Execution Layer

`high_kode_ai` is a Frappe custom app and the primary AI engineering project.

Its responsibility is AI execution, not ERP replacement.

## Initial responsibilities

- Agent Registry
- Agent Execution
- Run Management
- Context Management
- Planning
- Tool Calling
- Action Execution
- Action Policies
- Approval Requirements
- AI Provider abstraction
- Token Budgets
- Step / Execution Limits
- Retry / Failure Handling
- Audit Hooks

Memory and advanced orchestration should only be added when real workflows prove they are necessary.

## P0 principle

Keep v0.1 deliberately small.

The minimum conceptual model is:

```text
Agent
  ↓
Action
  ↓
Policy
  ↓
Run
```

Do **not** introduce unnecessary business hierarchy or multi-tenancy into the P0 schema.

---

# 5. Action-Level Autonomy

Autonomy belongs to an **Action**, not to an Agent.

Example:

```text
Sales Agent
│
├── Read Lead              → Allowed
├── Qualify Lead           → Allowed
├── Draft Reply            → Allowed
├── Send Quotation         → Human Approval
└── Refund Customer        → Human Approval
```

An Agent can therefore perform different actions with different autonomy levels.

Every consequential action should have:

- Defined policy
- Permission
- Autonomy level
- Approval requirement where applicable
- Audit trail
- Failure handling

This registry must exist before production execution.

---

# 6. `high_kode_integrations` — Integration Layer

`high_kode_integrations` is responsible for deterministic communication between High Kode and external systems.

Examples:

- DingDongBot
- AIWeb
- Google services
- Meta services
- Email systems
- External SaaS
- APIs
- Webhooks

```text
External Product
      │
      ▼
API / Webhook
      │
      ▼
high_kode_integrations
      │
      ▼
ERPNext
      │
      ▼
high_kode_ai
```

## Principle

> AI does not perform database synchronization.

Integration code handles:

- API communication
- Webhooks
- Data mapping
- Synchronization
- Retry
- Idempotency
- Authentication
- Integration logs

AI handles reasoning and permitted business actions.

---

# 7. `high_kode_business`

**Status:** Later / only when justified.

This app contains High Kode-specific business logic that does not belong in standard ERPNext.

Examples:

- High Kode-specific DocTypes
- Business rules
- Internal processes
- KPI extensions
- Special workflows

Boundary:

```text
Stable business rule
        ↓
high_kode_business

AI reasoning / execution
        ↓
high_kode_ai
```

Do not create this app merely because a concept can be given a custom app.

Use standard ERPNext first.

---

# 8. Flutter AI Interface

Flutter is part of the long-term architecture but is **not a P0 requirement**.

The first AI workflows should be proven through the simplest available interface:

- ERPNext Desk
- ERPNext Workflow
- Email
- Existing internal communication channels
- Other minimal interfaces where appropriate

## P0.5 / P1

After the first workflows are proven, build the Flutter AI interface.

Its purpose is not to reproduce ERPNext.

It is:

> **AI Interface for the Business**

Example:

```text
High Kode AI

Good morning.

7 things need attention

2 approvals
3 sales opportunities
2 project risks

[ Ask AI ]

"Doanh thu tháng này?"
```

Possible interactions:

- What needs my attention today?
- Which sales opportunities are at risk?
- Show me overdue projects.
- Approve these quotations.
- Why did revenue fall this week?
- What should Sales do next?
- Which support tickets require escalation?

## Principle

Flutter is an interface, not the business logic layer.

Critical business rules remain in ERPNext and backend custom apps.

---

# 9. Git Repository Structure

Recommended Git structure:

```text
High Kode GitHub
│
├── high-kode-docs
│
├── ai-erp-platform/apps/high_kode_ai/
│
├── ai-erp-platform/apps/high_kode_integrations/
│
├── high-kode-mobile       ← P0.5 / P1
│
└── high-kode-business     ← later
```

ERPNext itself is not a High Kode repository.

---

# 10. `high-kode-docs`

This repository is the architectural and governance source of truth.

```text
high-kode-docs/
│
├── README.md
├── master-plan.md
│
├── architecture/
│   ├── system-architecture.md
│   ├── ai-architecture.md
│   ├── integration-architecture.md
│   └── mobile-ai-interface.md
│
├── policies/
│   ├── data-access-policy.md
│   ├── ai-action-autonomy-registry.md
│   └── security-policy.md
│
└── adr/
    ├── ADR-001-erpnext-system-of-record.md
    ├── ADR-002-high-kode-ai-independent-from-huf.md
    ├── ADR-003-customer-zero.md
    ├── ADR-004-product-vs-operating-architecture.md
    └── ADR-005-hospitality-core-provenance.md
```

---

# 11. `high-kode-ai`

```text
high-kode-ai/
│
├── README.md
├── docs/
├── high_kode_ai/
├── tests/
├── pyproject.toml
└── ...
```

This repository contains implementation code for the AI execution layer.

It does not contain ERPNext itself.

---

# 12. `high-kode-integrations`

```text
high-kode-integrations/
│
├── README.md
├── docs/
├── high_kode_integrations/
├── tests/
└── ...
```

Only implement integrations that have an actual business requirement.

Do not build a generic integration platform before the first real integration proves the need.

---

# 13. `high-kode-mobile`

This repository is for Flutter.

It should be created when the first proven AI workflow requires a dedicated AI interface.

Recommended initial structure:

```text
high-kode-mobile/
│
├── apps/
│   └── high_kode_ai/
│
├── packages/
│   ├── api_client/
│   ├── auth/
│   ├── design_system/
│   └── ai_interface/
│
├── test/
└── README.md
```

A Flutter monorepo is preferred if multiple High Kode AI interfaces are eventually required.

Do not build a large design system before actual product requirements emerge.

---

# 14. Server Architecture

Start with a clean environment.

```text
SERVER
│
└── frappe-bench
    │
    ├── frappe
    ├── erpnext
    ├── high_kode_ai
    └── high_kode_integrations
```

Later, when justified:

```text
    └── high_kode_business
```

ERPNext should be installed from upstream.

High Kode manages only its own custom applications through Git.

---

# 15. SaaS Strategy

SaaS should be **considered in architecture but not implemented as a P0 feature**.

The sequence is:

```text
Phase 1
Clean ERPNext
      +
high_kode_ai
      +
high_kode_integrations
      │
      ▼
High Kode operates itself
      │
      ▼
Customer Zero
      │
      ▼
Proven workflows
      │
      ▼
Phase 2
AI Business Platform
      │
      ▼
Multi-tenant SaaS
      │
      ▼
External customers
```

The guiding rule:

> Do not build a SaaS product for an operating model that High Kode has not yet proven internally.

---

# 16. SaaS-Ready Without Premature Multi-Tenancy

Do not add `Tenant` and `Business` as mandatory P0 entities merely to be "SaaS-ready".

P0 should use the concepts required by the actual internal system:

```text
Agent
  ↓
Action
  ↓
Policy
  ↓
Run
```

Future SaaS concepts are recorded as an architectural consideration:

```text
Future
Tenant
  ↓
Business
  ↓
Department
  ↓
Agent
  ↓
Action
  ↓
Policy
  ↓
Run
```

This should be introduced only when multi-business or multi-tenant requirements become real.

---

# 17. Product Architecture vs Operating Architecture

These are separate concerns.

## Product Architecture

Existing products:

```text
DingDongBot
AIWeb
```

Future:

```text
AI Business Platform
```

## Business Operating Architecture

```text
High Kode
    │
    ▼
ERPNext
    │
    ├── high_kode_ai
    ├── high_kode_integrations
    ├── high_kode_business
    └── Flutter AI Interface
```

DingDongBot and AIWeb remain independent products.

Their business-operation data can be integrated into ERPNext where appropriate.

---

# 18. Data Boundary

The most important boundary is:

> **Business Operations Data ≠ End-Customer Content**

Business operations data may include:

- Customer account
- Subscription
- Plan
- Revenue
- Usage
- Ticket metadata
- KPI
- Sales status

End-customer content may include:

- Chat conversations
- Private messages
- Personal information
- Customer-specific content
- Sensitive tenant information

Do not automatically copy end-customer content into the internal AI system.

Any direct AI processing of end-customer content requires separate:

- Data Access Policy
- Tenant isolation where applicable
- Privacy review
- Contractual review
- Retention policy
- Audit controls

Before opening synchronization, verify the current ToS/privacy terms of the relevant product.

---

# 19. HUF / Licensing Decision

High Kode will not make `high_kode_ai` dependent on HUF.

The decision must be preserved as an ADR because it is an architectural and legal-risk decision, not merely an implementation preference.

## Decision

`high_kode_ai` will be developed independently.

The design may learn from general concepts and publicly observable architecture, but implementation must not copy HUF code or reproduce its implementation by direct reference.

The clean-room principle is:

```text
HUF
  │
  ▼
Conceptual understanding / functional requirements
  │
  ▼
Independent specification
  │
  ▼
Independent implementation
  │
  X
No copied implementation
```

During implementation, developers should work from the High Kode specification rather than keeping HUF source open as an implementation reference.

## Important legal note

Clean-room development is a risk-control method, not an automatic legal guarantee.

Before commercializing a materially similar system, obtain appropriate legal review.

The exact HUF licensing situation and any commercial licensing option should be documented separately rather than assumed.

---

# 20. `hospitality_core` Provenance

`hospitality_core` is a separate provenance/licensing matter and must not be conflated with the HUF decision.

Create and maintain:

```text
ADR-005-hospitality-core-provenance.md
```

The ADR should document:

- Original source
- Original license
- High Kode modifications
- Git history/provenance
- Current distribution status
- Compatibility considerations
- Required legal review

The purpose is to make the provenance auditable for future engineers, investors and legal review.

---

# 21. P0 Scope

P0 is intentionally small.

## P0 deliverables

1. Data Access Policy
2. AI Action & Autonomy Registry
3. Clean Frappe + ERPNext environment
4. `high_kode_ai` v0.1
5. `high_kode_integrations`
6. One or two real Customer Zero workflows
7. Shadow Mode
8. Human Approval
9. Audit
10. KPI measurement

## Explicitly not P0

- Full Flutter application
- Full SaaS
- Multi-tenancy
- Large visual workflow builder
- Dozens of Agents
- Complex memory platform
- Full ERP replacement
- Mobile replication of ERPNext
- Generic integration platform
- Customer-facing AI platform

---

# 22. P0 Workflow

The first workflow should demonstrate a complete loop.

```text
Business Event
      │
      ▼
ERPNext
      │
      ▼
high_kode_ai
      │
      ├── Context
      ├── Planning
      ├── Action Selection
      ├── Policy Check
      └── Approval Check
      │
      ▼
Action Execution
      │
      ▼
ERPNext Update
      │
      ▼
Audit / KPI
      │
      ▼
Human Review where required
```

The first workflow should be small enough to measure accurately.

---

# 23. Development Order

## Step 1 — Documentation first

Lock:

- System Architecture
- Data Access Policy
- AI Action & Autonomy Registry
- Security Policy
- HUF ADR
- `hospitality_core` provenance ADR

## Step 2 — Clean server

Install:

- Frappe
- ERPNext

Do not bring legacy custom apps into the environment yet unless specifically required.

## Step 3 — `high_kode_ai`

Implement only the smallest execution loop.

## Step 4 — `high_kode_integrations`

Implement only the first required external integration.

## Step 5 — Customer Zero

Run one or two real High Kode workflows.

## Step 6 — Shadow Mode

Let AI operate alongside humans without taking irreversible actions.

## Step 7 — Human Approval

Gradually allow approved actions.

## Step 8 — Measure

Track:

- Task Success Rate
- Human Hours Saved
- Error Rate
- Cost per Task
- Approval Rate
- Autonomous Task Ratio
- Business / Revenue impact

## Step 9 — Flutter P0.5 / P1

Build the AI Command Center only after the first workflows prove the need.

## Step 10 — SaaS P1+

Only after internal proof:

```text
Internal AI Operating System
          ↓
Standardize
          ↓
Harden
          ↓
Generalize
          ↓
Multi-tenant
          ↓
AI Business Platform
```

---

# 24. Definition of Done for P0

P0 is not complete because the app installs.

P0 is complete when:

- ERPNext remains the authoritative business record.
- `high_kode_ai` can execute a real business workflow.
- Actions are individually governed.
- High-risk actions require human approval.
- Runs are auditable.
- Token/step limits prevent runaway execution.
- External data synchronization is isolated in `high_kode_integrations`.
- Sensitive end-customer content is not unintentionally synchronized.
- At least one real High Kode workflow runs in Shadow Mode.
- The workflow produces measurable results.
- The team can explain exactly what the AI is allowed and not allowed to do.

---

# 25. Final Repository Structure

```text
High Kode GitHub
│
├── high-kode-docs
│   ├── master-plan.md
│   ├── architecture/
│   ├── policies/
│   └── adr/
│
├── ai-erp-platform/apps/high_kode_ai/
│   ├── high_kode_ai/
│   ├── docs/
│   └── tests/
│
├── ai-erp-platform/apps/high_kode_integrations/
│   ├── high_kode_integrations/
│   ├── docs/
│   └── tests/
│
├── high-kode-mobile       ← P0.5 / P1
│   ├── apps/
│   ├── packages/
│   └── test/
│
└── high-kode-business     ← later
```

Server:

```text
frappe-bench
│
├── frappe
├── erpnext
├── high_kode_ai
└── high_kode_integrations
```

---

# 26. Immutable Principles

These principles should not change casually as implementation progresses.

1. **ERPNext is the System of Record.**
2. **`high_kode_ai` is the AI Execution Layer.**
3. **`high_kode_integrations` owns deterministic integrations and synchronization.**
4. **Flutter is an AI Interface, not a business-logic layer.**
5. **Autonomy belongs to Actions, not Agents.**
6. **High-risk actions require explicit governance.**
7. **Business Operations Data is separated from End-Customer Content.**
8. **High Kode is Customer Zero.**
9. **Prove workflows internally before productizing them.**
10. **SaaS is a future productization stage, not a P0 distraction.**
11. **Do not introduce multi-tenancy before a real requirement exists.**
12. **Do not build generic infrastructure before a real workflow proves the need.**
13. **HUF is not a dependency of `high_kode_ai`.**
14. **HUF concepts may inform functional understanding, but implementation must be independently developed.**
15. **`hospitality_core` provenance must be separately documented.**
16. **Keep P0 small enough that it can reach production-like evidence quickly.**

---

# 27. North Star

> **Build High Kode as an AI-First Business.**
>
> Let AI increasingly operate the business.
>
> Keep ERPNext as the system of record.
>
> Govern every consequential action.
>
> Prove the system on High Kode first.
>
> Then turn the proven operating system into the product.


---

# 24. P0 Execution Addendum

This section supplements the architecture above. It does not replace or narrow the previously approved architecture.

The purpose is to convert the approved architecture into an implementation sequence while preserving the existing boundaries and principles.

## 24.1 Mandatory order before implementation

The following sequence is mandatory:

```text
Data Access Policy
        ↓
AI Action & Autonomy Registry
        ↓
Clean ERPNext Server
        ↓
high_kode_ai v0.1
        ↓
high_kode_integrations
        ↓
First Customer Zero Workflow
        ↓
Shadow Mode
        ↓
Human Approval
        ↓
Audit + Evaluation
        ↓
Measure Results
        ↓
Expand Autonomy Carefully
```

Policy must be written and reviewed before integration code is implemented.

The purpose is to prevent the integration layer from accidentally synchronizing data that the approved policy later prohibits.

---

# 25. Shadow Mode & Evaluation

The first production-like workflows must begin in Shadow Mode.

```text
Business Event
      ↓
AI observes
      ↓
AI produces recommendation
      ↓
Human performs / reviews actual action
      ↓
Compare AI vs Human
      ↓
Store evaluation
```

Shadow Mode is not merely a logging mode. It is the evaluation stage used to determine whether the AI is actually capable of performing useful business work.

## 25.1 Evaluation record

For each meaningful AI recommendation, capture where practical:

- Run ID
- Agent
- Action
- Input/context reference
- AI recommendation
- Human decision
- Human correction
- Final result
- Rejection/correction reason
- Execution time
- Estimated human effort
- Outcome

A minimal human feedback mechanism may include:

```text
Good
Bad
Needs Correction
```

A more structured evaluation can be added when the workflow requires it.

## 25.2 Metrics

At minimum, measure:

- Task Success Rate
- Human Hours Saved
- Error Rate
- Correction Rate
- Approval Rate
- Autonomous Task Ratio
- Cost per Task
- Latency
- Business / Revenue Impact

The system should not be considered successful merely because an Agent produces plausible text or completes a technical function.

Success means measurable improvement to a real business workflow.

---

# 26. Human Approval Gate

P0 does not require a dedicated mobile application.

Use the existing Frappe/ERPNext interface first:

- ERPNext Desk
- Frappe Workflow
- Comments
- Timeline
- Notifications
- Minimal custom Frappe Page when necessary

The approval gate must be enforced by the backend policy/action layer, not merely by hiding or showing a UI button.

Conceptually:

```text
AI selects Action
      ↓
Action Registry
      ↓
Policy Check
      ↓
Autonomy Check
      ↓
┌──────────────────────┐
│ Human approval needed│
└──────────────────────┘
      ↓
Approval
      ↓
Execution
      ↓
Audit
```

If an Action requires approval, an Agent must not be able to bypass the gate by calling an underlying tool directly.

---

# 27. Idempotency for Side-Effect Actions

Every Action that can create an external or irreversible side effect must support idempotency.

Examples include:

- Create Invoice
- Send Email
- Send Quotation
- Create Subscription
- Refund
- Payment-related operation
- External system update
- Customer notification

The integration layer should use an idempotency key derived from the logical execution identity, for example:

```text
idempotency_key
=
Run ID + Action ID + execution identifier
```

The exact implementation may vary by integration.

The invariant is:

> A retry caused by timeout, network failure, worker restart, or duplicate delivery must not accidentally perform the same side effect twice.

Idempotency belongs in the deterministic execution/integration layer rather than relying on the LLM or Agent to remember whether an operation already happened.

---

# 28. P0 Scope Guard

The architecture intentionally keeps P0 small.

The following are not required for the first proof cycle unless a real workflow demonstrates a concrete need:

- Dedicated Flutter application
- Full SaaS platform
- Multi-tenancy
- Tenant hierarchy
- Large visual workflow builder
- Advanced memory platform
- Large Agent catalog
- Generic integration marketplace
- Full autonomous enterprise
- ERP replacement
- Mobile ERP clone
- Customer-facing AI platform

These remain roadmap items rather than P0 requirements.

---

# 29. Flutter Roadmap

Flutter is part of the longer-term AI interface architecture, but it is not required to prove the first workflow.

## P0

Use ERPNext/Frappe interfaces for approval and operations.

## P0.5

Introduce a minimal Flutter interface if the proven workflow demonstrates that a dedicated interface materially improves operations.

## P1

Develop the High Kode AI Command Center.

Potential capabilities:

- AI conversation
- Approval queue
- Business alerts
- KPI summaries
- Agent activity
- Action history
- Human review
- Mobile notifications

Flutter remains an interface layer.

Business rules, policies, authorization and execution remain server-side.

---

# 30. SaaS Productization Sequence

SaaS is a productization stage, not a P0 implementation requirement.

The intended sequence remains:

```text
Build
  ↓
Internal Use
  ↓
Customer Zero
  ↓
Measure
  ↓
Prove
  ↓
Harden
  ↓
Generalize
  ↓
Multi-Tenant
  ↓
SaaS
```

Do not introduce a full `Tenant` / `Business` hierarchy merely to claim SaaS readiness.

The P0 domain model should remain focused on the concepts required to run the real workflow:

```text
Agent
  ↓
Action
  ↓
Policy
  ↓
Run
```

Multi-tenancy becomes an explicit architecture decision when external customers actually require it.

---

# 31. Customer Zero Operating Model

High Kode is the first customer of its own AI operating system.

The first workflows should improve the operation of High Kode itself, including where appropriate the business operations of existing products such as:

- DingDongBot
- AIWeb

These products remain separate product architectures.

The operating relationship is:

```text
                    High Kode AI Operating System
                              │
             ┌────────────────┼────────────────┐
             ↓                ↓                ↓
       High Kode          DingDongBot        AIWeb
       operations         operations        operations
             │                │                │
             └────── business-operation data ─┘
                              ↓
                    ERPNext / SoR
```

`high_kode_integrations` is responsible for deterministic synchronization.

This does not mean the AI layer automatically receives all end-customer content from those products.

The previously defined boundary remains:

> **Business Operations Data ≠ End-Customer Content**

---

# 32. P0 Definition of Done

P0 is complete only when all applicable conditions below are satisfied:

- ERPNext remains the authoritative System of Record.
- `high_kode_ai` executes at least one real business workflow.
- `high_kode_integrations` handles external synchronization deterministically.
- Every consequential Action is registered.
- Every consequential Action has an explicit autonomy policy.
- High-risk Actions require human approval.
- Approval cannot be bypassed by direct tool invocation.
- AI Runs are auditable.
- Side-effect Actions support idempotency.
- Sensitive end-customer content is not unintentionally synchronized.
- At least one workflow has completed Shadow Mode.
- Human evaluation is captured.
- Task Success Rate can be measured.
- Human effort saved can be measured.
- Error/correction rate can be measured.
- The team can state exactly what the AI may and may not do.
- The workflow has evidence of real operational value.

P0 is not complete merely because the software technically runs.

---

# 33. Implementation Baseline

The architecture is now sufficiently mature to move from design into implementation.

The next work is execution, not another expansion of the architecture:

```text
LOCK POLICY
     ↓
BUILD CLEAN SERVER
     ↓
BUILD P0
     ↓
RUN CUSTOMER ZERO
     ↓
MEASURE
     ↓
IMPROVE
```

The governing rule remains:

> **Do not build the platform we imagine. Build the smallest system that can operate High Kode better, measure the result, and expand only from evidence.**

---

# 34. Immutable Principles — Updated

The following principles are considered architectural constraints:

1. ERPNext is the System of Record.
2. `high_kode_ai` is the AI Execution Layer.
3. `high_kode_integrations` owns deterministic integrations.
4. Autonomy belongs to Actions, not Agents.
5. High-risk Actions require explicit governance.
6. Business Operations Data is separated from End-Customer Content.
7. High Kode is Customer Zero.
8. Prove internal workflows before productization.
9. Flutter is an interface layer, not a business-logic layer.
10. SaaS is a future productization stage, not a P0 feature.
11. Do not introduce multi-tenancy before a real requirement exists.
12. Do not build generic infrastructure before a real workflow proves the need.
13. HUF is not a dependency of `high_kode_ai`.
14. `hospitality_core` provenance remains separately documented.
15. P0 must remain small enough to reach measurable real-world evidence quickly.
16. Idempotency is mandatory for side-effect Actions.
17. Shadow Mode must produce measurable evaluation data.
18. Human approval must be enforced by backend policy, not only by UI.
19. Integration synchronization must be deterministic and must not be delegated to the LLM.
20. Policy and Action Registry must be established before integration implementation.
21. Architecture should expand from proven operational requirements, not speculative platform requirements.

---

# 35. Version Decision

**Version 1.3 supersedes Version 1.2 as the technical architecture baseline.**

The changes in this version are additive. Existing architectural decisions remain valid unless explicitly updated by a later ADR.

The implementation team should treat this document as the current technical reference and record future material architectural changes as ADRs rather than silently changing the baseline.

---

# 36. Git Repository & Monorepo Architecture

## 36.1 Decision

High Kode will use a **monorepo** for its internal ERP/AI platform source code.

The repository is:

```text
ai-erp-platform/
```

The repository contains multiple Frappe custom apps, while each app retains a separate responsibility and internal boundary.

This is a source-control decision. It does **not** merge the responsibilities of the applications.

## 36.2 Repository structure

```text
ai-erp-platform/
│
├── apps/
│   ├── high_kode_ai/
│   │   └── AI Execution Layer
│   │
│   └── high_kode_integrations/
│       └── Deterministic Integration Layer
│
├── docs/
│   ├── architecture/
│   │   └── system-architecture.md
│   │
│   ├── policies/
│   │   ├── data-access-policy.md
│   │   └── ai-action-autonomy-registry.md
│   │
│   └── adr/
│       ├── ADR-001-*.md
│       ├── ADR-002-high-kode-ai-independent-from-huf.md
│       ├── ADR-003-*.md
│       ├── ADR-004-*.md
│       └── ADR-005-hospitality-core-provenance.md
│
├── scripts/
├── tests/
├── README.md
└── pyproject.toml
```

## 36.3 Application boundaries

A monorepo does not mean that applications may freely depend on one another's internal implementation.

### `high_kode_ai`

Owns:

- Agent
- Action
- Policy
- Run
- Planning
- AI execution
- AI evaluation
- autonomy enforcement

### `high_kode_integrations`

Owns:

- external APIs
- webhooks
- synchronization
- deterministic data mapping
- idempotency at integration boundaries
- DingDongBot integration
- AIWeb integration
- other external-system connectors

The integration layer must not delegate deterministic synchronization decisions to an LLM.

## 36.4 ERPNext is not copied into the repository

The repository contains High Kode's custom applications and documentation.

ERPNext and Frappe remain installed dependencies in the deployment environment.

```text
GitHub
  │
  └── ai-erp-platform
       ├── high_kode_ai
       └── high_kode_integrations

Server
  │
  └── Frappe Bench
       ├── Frappe
       ├── ERPNext
       ├── high_kode_ai
       └── high_kode_integrations
```

The repository must not become a fork or copy of the complete ERPNext source tree unless a future, explicitly documented engineering requirement makes that necessary.

## 36.5 Why monorepo

The monorepo is appropriate for the current Customer Zero phase because the applications:

- are developed by the same team
- are released as part of one internal platform
- share architecture and policies
- require cross-application testing
- frequently change together
- benefit from one versioning and review workflow

If a future product boundary requires independent release cycles, independent teams, or independent distribution, an application may be extracted into its own repository later.

That extraction is not required for P0.

## 36.6 Repository versus Frappe app

The following distinction is mandatory:

```text
Repository ≠ Frappe App
```

One repository may contain multiple Frappe apps.

Each Frappe app remains independently structured, testable, and responsibility-bounded.

Therefore:

```text
ai-erp-platform
        │
        ├── high_kode_ai
        │
        └── high_kode_integrations
```

is intentional and does not violate the layered architecture.

---

# 37. ADR-006 — AI ERP Platform Monorepo

**Status:** Accepted  
**Date:** 2026-08-10

## Context

High Kode needs multiple custom Frappe applications for the AI operating platform.

Creating a separate repository for every application at the P0 stage would add repository and release-management overhead without providing a meaningful architectural benefit.

## Decision

Use `ai-erp-platform` as a monorepo containing the initial custom Frappe applications.

The initial applications are:

- `high_kode_ai`
- `high_kode_integrations`

Application responsibilities remain separated even though source code is stored in one repository.

## Consequences

### Positive

- simpler development workflow
- unified architecture documentation
- atomic changes across related apps
- simpler CI/testing
- simpler deployment coordination
- easier Customer Zero iteration

### Negative

- repository grows over time
- CI may require selective testing as the platform expands
- extraction may be required if a future product becomes independently distributed

The negative consequences are acceptable at the current stage.

---

# 38. Version Decision

**Version 1.3 supersedes Version 1.2 as the technical architecture baseline.**

The principal change from v1.2 is the formalization of the AI ERP Platform monorepo.

This does not change:

- system boundaries
- application responsibilities
- ERPNext's System of Record role
- AI execution architecture
- integration architecture
- autonomy policy
- data boundaries
- Customer Zero strategy
- HUF independence decision
- P0 scope

It only establishes the official source-control and repository structure for implementation.
