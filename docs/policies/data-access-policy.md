# High Kode Data Access Policy

- Status: Accepted
- Date: 2026-08-10
- Scope: High Kode AI ERP Platform

## 1. Purpose

This policy defines what business data may move between High Kode systems and
what data must remain outside the default AI operating boundary.

The purpose is to keep data access:

- Explicit
- Minimal
- Auditable
- Tenant-safe
- Deterministic
- Compatible with contractual and privacy requirements

## 2. System Boundaries

### ERPNext

ERPNext is the authoritative internal System of Record for High Kode
business operations.

### high_kode_integrations

`high_kode_integrations` is responsible for deterministic data movement
between ERPNext and external systems.

The integration layer must not use an LLM to decide how database records are
synchronized.

### high_kode_ai

`high_kode_ai` consumes authorized business data and requests authorized
actions.

AI access does not imply unrestricted database access.

## 3. Default Data Classification

### Class A — Business Operations Data

Allowed for controlled synchronization.

Examples:

- Customer account
- Company
- Contact metadata
- Subscription
- Product
- Plan
- Revenue
- Invoice status
- Payment status
- Usage metrics
- Support ticket metadata
- Sales pipeline status
- Product status
- Operational KPIs

These fields must still be explicitly mapped by the integration.

### Class B — Internal Operational Data

Allowed when required for a defined business workflow.

Examples:

- Employee operational records
- Department
- Project
- Task
- Workflow state
- Internal KPI
- Internal activity metadata

Access must follow the minimum-required-data principle.

### Class C — End-Customer Content

Not included in the default P0 synchronization boundary.

Examples:

- Private chat messages
- Conversation transcripts
- Customer-generated content
- Social-media message contents
- Uploaded customer documents
- Personal messages
- Sensitive customer payloads

Access to this class requires a separate technical, contractual, privacy,
and security review.

## 4. DingDongBot Boundary

DingDongBot is an independently architected product.

High Kode may synchronize DingDongBot business-operations data into ERPNext
through `high_kode_integrations`.

The default synchronization boundary includes:

- Customer account
- Subscription
- Plan
- Revenue
- Product status
- Usage metrics
- Support metadata

The default boundary does not include end-customer conversation content.

An AI agent operating High Kode business operations must not automatically
gain access to DingDongBot end-customer conversations merely because it has
access to DingDongBot operational data.

## 5. AIWeb Boundary

AIWeb is also an independently architected product.

Business-operational data may be synchronized where required.

Examples:

- Customer
- Project
- Subscription
- Revenue
- Product status
- Usage
- Operational metrics

End-customer content remains outside the default P0 boundary.

## 6. Integration Rules

All external synchronization must be:

- Deterministic
- Explicitly mapped
- Logged
- Retry-safe
- Idempotent where applicable
- Auditable

LLMs must not directly modify synchronization mappings.

## 7. Minimum Required Data

Each integration must define:

1. Source system
2. Source object
3. Source fields
4. Destination object
5. Destination fields
6. Purpose
7. Data classification
8. Retention requirement
9. Access permissions

Only fields required for the declared purpose should be synchronized.

## 8. Tenant Isolation

Although multi-tenant SaaS is not a P0 feature, integrations must never
accidentally mix records belonging to different businesses or customers.

Every integration must preserve the source system's ownership and business
boundaries.

## 9. Access Control

AI agents receive access through explicit tools and policies.

Database credentials must not be exposed directly to an AI agent.

An agent should request an operation through an authorized tool rather than
constructing arbitrary database queries.

## 10. Action Data vs Read Data

Permission to read data does not imply permission to perform an action.

For example:

- An agent may read a customer's subscription.
- That does not automatically authorize cancellation.
- An agent may read an invoice.
- That does not automatically authorize a refund.

Action authorization is governed separately by the AI Action & Autonomy
Registry.

## 11. Privacy and Contractual Review

Before enabling a new external synchronization:

1. Identify the data being transferred.
2. Classify the data.
3. Review the applicable customer agreement and privacy terms.
4. Confirm that the intended processing is permitted.
5. Document the decision.
6. Implement only the approved fields.

No assumption should be made that because data is operational data it is
automatically permitted to be transferred.

## 12. P0 Restrictions

The following are outside the default P0 scope:

- End-customer chat content
- Private social-media conversations
- Sensitive personal information
- Unnecessary customer documents
- Unbounded database access
- LLM-controlled database synchronization

## 13. Audit Requirements

Every integration should be able to answer:

- What data was transferred?
- From where?
- To where?
- When?
- Why?
- Which integration version performed it?
- Which business/customer boundary did it belong to?

## 14. Change Control

Changes to an integration's data scope require review before deployment.

Adding a new field is a policy change if the field:

- Changes data classification
- Introduces customer content
- Introduces personal or sensitive information
- Changes contractual/privacy exposure
- Expands AI access

## 15. Core Principle

> Business Operations Data ≠ End-Customer Content

The AI operating system should manage the business without unnecessarily
ingesting the private content of the business's customers.