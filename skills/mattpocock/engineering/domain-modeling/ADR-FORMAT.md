# ADR Format

The authoritative minimal template, optional sections/status values, three-part eligibility gate, creation permission, lazy creation, and sequential numbering procedure are inline in [SKILL.md — step 5](./SKILL.md#5-offer-and-produce-adrs-only-conditionally). This file supplies illustrations, not an independent ADR specification. A single paragraph may be sufficient; do not add mandatory sections or records merely because this file is present.

## Potentially qualifying decisions

These are prompts for applying all three criteria, not automatic approvals or proof that every instance qualifies:

- **Architectural shape.** “We're using a monorepo.” “The write model is event-sourced, the read model is projected into Postgres.”
- **Integration patterns between contexts.** “Ordering and Billing communicate via domain events, not synchronous HTTP.”
- **Technology choices that carry lock-in.** Database, message bus, auth provider, deployment target. Not every library: only choices with meaningful replacement cost.
- **Boundary and scope decisions.** “Customer data is owned by the Customer context; other contexts reference it by ID only.” Explicit exclusions may be as valuable as inclusions.
- **Deliberate deviations from the obvious path.** “We're using manual SQL instead of an ORM because X.” A reasonable reader would assume the opposite; preserve the actual reason so it is not later mistaken for an accident.
- **Constraints not visible in the code.** “We can't use AWS because of compliance requirements.” “Response times must be under 200ms because of the partner API contract.”
- **Rejected alternatives when rejection is non-obvious.** A REST-over-GraphQL choice may merit a record when the supplied trade-off is subtle, so the same rejected alternative is not repeatedly proposed.

The existing output interface remains `docs/adr/NNNN-slug.md`: root for system-wide decisions, context-local for context-specific decisions. Apply SKILL.md's new-record gate to new substantive choices; requested maintenance edits preserve existing rationale without retrospective requalification. Do not infer an accepted decision, genuine trade-off, or write authorization from these examples; an unaccepted requested draft stays explicitly proposed.
