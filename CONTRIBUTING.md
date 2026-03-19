# Contributing to AIAP

Thank you for your interest in contributing to the AI Application Protocol! This document provides guidelines for contributing to the project.

## How to Contribute

### Reporting Issues

- Use [GitHub Issues](https://github.com/AISOP-AIAP-SoulBot/AIAP/issues) to report bugs, suggest features, or propose specification changes
- For specification changes, use the `spec-change` issue template
- Provide clear descriptions with examples where possible

### Submitting Changes

1. **Fork** the repository
2. **Create a branch** from `main` for your changes
3. **Make your changes** following the guidelines below
4. **Commit** using [Conventional Commits](https://www.conventionalcommits.org/) format
5. **Submit a Pull Request** to `main`
6. **Wait for review** from maintainers

### Specification Changes

Changes to normative content (anything in `specification/`) require:

1. An issue with the `spec-change` label describing the proposed change
2. A minimum 14-day discussion period for non-trivial changes
3. An Architecture Decision Record (ADR) in `adrs/` for significant decisions
4. Axiom 0 compliance review by maintainers
5. Updated documentation reflecting the change

### Documentation Changes

Changes to non-normative content (topics, guides, reference) are welcome and follow the standard PR process without the extended discussion period.

## Writing Guidelines

### RFC 2119 Keywords

When writing normative specification text, use the keywords defined in [RFC 2119](https://tools.ietf.org/html/rfc2119):

- **MUST** / **MUST NOT** — Absolute requirements
- **SHOULD** / **SHOULD NOT** — Strong recommendations with documented exceptions
- **MAY** — Truly optional behavior

These keywords MUST be capitalized when used in their normative sense.

### Terminology

- Use "AIAP program" (not "AIAP agent" or "AIAP application") for governed software
- Use "AISOP file" for individual `.aisop.json` files
- Capitalize "Axiom 0" (it is a proper noun)
- Capitalize Pattern names: "Pattern A", "Pattern G"
- Write trust levels as "T1", "T2", "T3", "T4"

### Document Structure

- Begin each topic document with a one-paragraph introduction
- Use tables for field specifications
- Use code blocks with language annotations for examples
- Use Mermaid diagrams for flow and architecture illustrations
- Cross-reference between documents using relative links

### Closing Seal

All specification documents MUST end with:

```
Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIAP V1.0.0. www.aiap.dev
```

## Code of Conduct

All contributors must follow our [Code of Conduct](CODE_OF_CONDUCT.md). The Axiom 0 pledge applies to all contributions.

## License

By contributing, you agree that your contributions will be licensed under the Apache License 2.0.

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIAP V1.0.0. www.aiap.dev
