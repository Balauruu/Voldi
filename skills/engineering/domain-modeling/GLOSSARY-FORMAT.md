# GLOSSARY.md Format

The authoritative glossary template, definition rules, confirmation handling, and prompt capture procedure are inline in [SKILL.md — step 4](./SKILL.md#4-capture-each-resolved-term-immediately). Use that template; this supporting file owns only the existing map-format guidance below, not a second glossary specification.

## Single vs multi-context repos

**Single context (most repos):** one `GLOSSARY.md` at the project root.

**Multiple contexts:** an existing root `GLOSSARY-MAP.md` lists the contexts, where their glossaries live, and how they relate. Follow its links; do not replace it with a new root glossary or silently merge context meanings. If the topic's context is unclear, ask before editing.

## Existing glossary map format

This illustrative map preserves the existing interface. It is routing/reference guidance, not a requirement to create a map or to add these example integrations to a real project. Relationships in the map may describe integrations; the glossaries themselves must remain free of implementation detail.

```md
# Glossary Map

## Contexts

- [Ordering](./src/ordering/GLOSSARY.md): receives and tracks customer orders
- [Billing](./src/billing/GLOSSARY.md): generates invoices and processes payments
- [Fulfillment](./src/fulfillment/GLOSSARY.md): manages warehouse picking and shipping

## Relationships

- **Ordering → Fulfillment**: Ordering emits `OrderPlaced` events; Fulfillment consumes them to start picking
- **Fulfillment → Billing**: Fulfillment emits `ShipmentDispatched` events; Billing consumes them to generate invoices
- **Ordering ↔ Billing**: Shared types for `CustomerId` and `Money`
```

The map's paths above are relative to the map at the project root. If a map exists, it governs routing even when a root glossary also exists. If no map exists, use the root glossary. If neither exists, create only the root glossary and only when a first term is resolved and edits are authorized. Do not create glossary/map scaffolding for unresolved proposals. The ordered discovery and conflict handling live in [SKILL.md — step 1](./SKILL.md#1-locate-the-context-and-read-its-language).
