# ADR-002: high_kode_ai Independent from HUF

- Status: Accepted
- Date: 2026-08-10

## Context

High Kode evaluated HUF as a possible foundation for the AI execution layer.

The available licensing information contains unresolved ambiguity, including
conflicting references between MIT and AGPLv3.

Because `high_kode_ai` is intended to become part of a commercial AI business
platform, an unresolved third-party licensing dependency is not acceptable.

## Decision

`high_kode_ai` will be developed independently from HUF.

HUF is not a runtime dependency.

High Kode may use HUF for conceptual and architectural research, but the
implementation of `high_kode_ai` must be independently developed.

## Clean-Room Principle

Allowed:

- Study general concepts
- Study architectural patterns
- Understand functional requirements
- Write independent specifications
- Implement independently

Not allowed:

- Copy HUF source code
- Copy implementation details
- Port HUF code into `high_kode_ai`
- Create a runtime dependency on HUF

## License Follow-up

The following must still be documented:

1. Confirm the actual HUF repository license.
2. Resolve conflicting README/LICENSE references.
3. Request written clarification from Tridz.
4. Preserve the written response.
5. Obtain legal review before relying on HUF-derived material commercially.

Clean-room development does not by itself eliminate legal risk.

## Consequences

### Positive

- No unresolved HUF runtime dependency
- Clear ownership of `high_kode_ai`
- Lower licensing risk for future SaaS commercialization
- Independent architecture and implementation

### Negative

- High Kode must implement capabilities independently
- Some development effort may be duplicated
- HUF cannot be treated as a drop-in dependency

## Final Decision

`high_kode_ai` is independent from HUF.