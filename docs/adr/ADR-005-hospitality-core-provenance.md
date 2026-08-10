# ADR-005: hospitality_core Provenance

- Status: Accepted
- Date: 2026-08-10

## Context

`hospitality_core` is a separate codebase and licensing/provenance concern
from the HUF decision.

It has been modified and therefore its origin, license, and modification
history must remain independently documented.

## Decision

`hospitality_core` provenance must be reviewed and documented separately from
HUF.

The following information must be established:

- Original source
- Original license
- Current license
- Modification history
- Git history
- Commercial-use rights
- Third-party dependencies
- Relationship to other High Kode codebases

## Repository Separation

Code provenance must remain traceable.

Where necessary, source history and ownership must be preserved rather than
silently mixing unrelated histories.

## Legal Review

Before commercial distribution of functionality derived from
`hospitality_core`, High Kode must verify:

- License compatibility
- Copyright ownership
- Modification rights
- Commercial-use rights
- Distribution obligations

Legal review should be obtained where uncertainty remains.

## Consequences

### Positive

- Clear provenance
- Reduced licensing ambiguity
- Easier future audit
- Safer commercial productization

### Negative

- Additional documentation work
- Possible need to rewrite or separate code
- Commercialization may be delayed until provenance is verified

## Final Decision

`hospitality_core` is treated as a separate provenance and licensing matter
and must not be assumed to be covered by the HUF decision.