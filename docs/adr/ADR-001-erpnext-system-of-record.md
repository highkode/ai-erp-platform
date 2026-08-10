# ADR-001: ERPNext as System of Record

- Status: Accepted
- Date: 2026-08-10

## Context

High Kode is building an AI-first business operating system.

The system needs one authoritative source for business state. AI must not
create a second ERP or maintain competing business records.

## Decision

ERPNext is the authoritative System of Record for High Kode business
operations.

ERPNext owns persistent business state including:

- Customers
- Contacts
- Leads
- Opportunities
- Quotations
- Sales Orders
- Invoices
- Payments
- Projects
- Tasks
- Employees
- Departments
- Accounting
- Operational business records

## Responsibilities

### ERPNext

ERPNext is responsible for:

- Storing authoritative business records
- Maintaining business state
- Business workflows
- Accounting
- Operational records

### high_kode_ai

`high_kode_ai` is responsible for:

- AI reasoning
- Planning
- Agent orchestration
- Tool selection
- Action requests
- Controlled execution

It must not become a second ERP.

### high_kode_integrations

`high_kode_integrations` is responsible for:

- External API communication
- Webhooks
- Data synchronization
- Deterministic field mapping
- Retry handling
- Idempotency

## Consequences

This separation keeps the architecture clear:

ERPNext = source of truth

high_kode_integrations = deterministic data movement

high_kode_ai = AI execution

Business state remains auditable and does not become dependent on an LLM.