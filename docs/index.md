# AIAP Protocol Documentation

Welcome to the **AI Application Protocol (AIAP)** documentation.

AIAP is an open standard for structuring, validating, and governing AI programs. It ensures **deterministic behavior** through AISOP blueprints, **verifiable quality** through ThreeDimTest, and **immutable alignment** through Axiom 0.

---

## What is AIAP?

AIAP defines how AI programs should be built, tested, and trusted. Every AIAP program consists of:

- **AIAP.md** --- A governance contract declaring metadata, tools, modules, and alignment.
- **.aisop.json files** --- Machine-readable blueprints with execution graphs, step-based functions, and security constraints.
- **ThreeDimTest validation** --- Quality checks across three dimensions: Correctness, Intrinsic quality, and Detail compliance.

The protocol is anchored by **Axiom 0: Human Sovereignty and Benefit** --- an immutable principle that no version of AIAP may weaken or remove.

---

## Quick Navigation

### For Newcomers

| Resource | Description |
|----------|-------------|
| [What is AIAP?](topics/what-is-aiap.md) | Understanding the protocol and why it exists |
| [Getting Started](guides/getting-started.md) | Quick introduction for first-time users |
| [Core Concepts](topics/core-concepts.md) | Key ideas explained: AISOP, patterns, quality, security |

### For Developers

| Resource | Description |
|----------|-------------|
| [First AIAP Program](guides/first-aiap-program.md) | Step-by-step tutorial to build a Pattern A calculator |
| [Pattern Selection](guides/pattern-selection.md) | Decision tree for choosing the right architecture (A-G) |
| [AISOP Format](topics/aisop-format.md) | Deep dive into the `.aisop.json` file format |
| [Structural Patterns](topics/structural-patterns.md) | Detailed reference for all seven structural patterns |

### For Reviewers

| Resource | Description |
|----------|-------------|
| [Specification](specification.md) | Complete AIAP V1.0.0 protocol specification |
| [Quality Standards](topics/quality-standards.md) | ThreeDimTest framework and scoring |
| [Security Model](topics/security-model.md) | Trust levels, threat taxonomy, and security guards |
| [Axiom 0](topics/axiom-0.md) | The immutable alignment principle |

### Reference

| Resource | Description |
|----------|-------------|
| [AIAP.md Fields](reference/aiap-md-fields.md) | Complete reference for governance contract fields |
| [AISOP Fields](reference/aisop-fields.md) | Complete reference for blueprint fields |
| [Quality Rules](reference/quality-rules.md) | All validation rules (C1-C7, I1-I11, D1-D10) |
| [Threat Taxonomy](reference/threat-taxonomy.md) | Security threat categories and mitigations |
| [Error Codes](reference/error-codes.md) | Error code reference |
| [Glossary](reference/glossary.md) | Term definitions for the AIAP vocabulary |

---

## Ecosystem

AIAP is a **governance protocol** that complements existing AI infrastructure:

- **A2A (Agent-to-Agent)** handles agent communication.
- **MCP (Model Context Protocol)** handles tool access.
- **AIAP** governs program structure, quality, security, and alignment.

AIAP programs can participate in both the A2A and MCP ecosystems through Pattern G interoperability.

---

## Contributing

AIAP is an open protocol. Contributions are welcome:

- **Repository**: [github.com/AISOP-AIAP-SoulBot/AIAP](https://github.com/AISOP-AIAP-SoulBot/AIAP)
- **Website**: [aiap.dev](https://aiap.dev)

---

> Align: Human Sovereignty and Benefit. Version: AIAP V1.0.0. www.aiap.dev
