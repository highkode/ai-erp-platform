# ADR-003: High Kode as Customer Zero

- Status: Accepted
- Date: 2026-08-10

## Context

High Kode is building an AI-first business operating platform.

The platform should not be commercialized before it has demonstrated that it
can operate a real business effectively.

## Decision

High Kode will be the first customer of its own AI operating system.

The system will first be used internally to operate High Kode.

This includes business operations associated with existing products such as:

- DingDongBot
- AIWeb

These products remain independently architected, but their internal business
operations may be managed through High Kode's operating system.

## Productization Principle

The progression is:

Build
→ Internal Use
→ Measure
→ Prove
→ Standardize
→ Productize

A capability should only become a commercial platform feature after it has
demonstrated value internally.

## Consequences

### Positive

- Real-world validation
- Faster discovery of operational problems
- Measurable AI performance
- Lower risk of building unused SaaS features
- Stronger foundation for future commercialization

### Negative

- Internal workflows may require iteration
- Productization is intentionally delayed
- Initial architecture is optimized for High Kode's real operations

## Final Decision

High Kode is Customer Zero for the AI-first business operating platform.