<!-- markdownlint-disable MD041 -->
<div align="center">
  <h1>AI Application Protocol (AIAP)</h1>
  <p><b>An open protocol for structuring, validating, and governing AI programs.</b></p>
  <p>
    <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg" alt="Apache 2.0" /></a>
    <img src="https://img.shields.io/badge/Protocol-AIAP_V1.0.0-brightgreen.svg" alt="AIAP V1.0.0" />
    <img src="https://img.shields.io/badge/Axiom_0-Human_Sovereignty_and_Wellbeing-orange.svg" alt="Axiom 0" />
    <a href="https://aiap.dev"><img src="https://img.shields.io/badge/docs-aiap.dev-blue.svg" alt="Docs" /></a>
  </p>
  <p>
    <a href="README_CN.md">中文文档</a> | English
  </p>
</div>

## What is AIAP?

The AI Application Protocol (AIAP) is an open standard that defines how AI programs are **structured**, **validated**, and **secured**. While protocols like [A2A](https://github.com/a2aproject/A2A) address how agents communicate and [MCP](https://modelcontextprotocol.io/) defines how models access tools, AIAP addresses a different challenge: **how AI programs themselves should be governed**.

AIAP ensures that autonomous AI systems operate with:

- **Deterministic structure** through dual blueprint formats: AISOP (`.aisop.json`) and AISIP (`.aisip.json`)
- **Verifiable quality** through the ThreeDimTest quality framework (Correctness, Intrinsic, Detail)
- **Graduated security** through trust levels (T1-T4) and threat-aware validation (AT1-AT6)
- **Immutable alignment** through Axiom 0: Human Sovereignty and Wellbeing

### Why AIAP?

As AI agents become more capable and autonomous, the risk of uncontrolled hallucination, alignment drift, and security violations grows. Traditional "prompt engineering" attempts to solve this by adding more instructions — which paradoxically increases entropy and ambiguity. AIAP takes the opposite approach:

1. **Semantic Compiling** — AIAP validators act as compilers, rejecting ambiguous instructions, unhandled error routes, and semantic contradictions *before* runtime.
2. **Zero-Entropy Resonance** — By collapsing the possibility space through rigid structural patterns, AIAP minimizes the tokens available for free-form interpretation.
3. **L0/L1 Isolation** — Strict separation of machine-readable state (L0) from human-readable narrative (L1) prevents cross-contamination in multi-agent cascades.
4. **Axiom 0 Enforcement** — Hard alignment to "Human Sovereignty and Wellbeing" at the protocol level, not as a suggestion but as an immutable constraint.

## Key Features

| Feature | Description |
|---------|-------------|
| **Dual Blueprints** | `.aisop.json` (Mermaid flow) and `.aisip.json` (JSON flow) — two equivalent formats for defining deterministic agent behavior |
| **Structural Patterns** | Seven patterns (A-G) from single scripts to embedded runtimes with tool directories |
| **Quality Standards** | 50+ validation rules across Correctness (C1-C7), Intrinsic (I1-I13), and Detail (D1-D10) dimensions |
| **Trust Levels** | Four graduated levels: T1 (metadata-only) to T4 (autonomous with network access) |
| **Threat Taxonomy** | Six attack categories (AT1-AT6) with Code Trust Gate verification for embedded code |
| **Program Lifecycle** | Draft → Active → Deprecated → Archived with migration guides |
| **Interoperability** | Bidirectional MCP tool mapping, A2A agent card integration, Pattern G embedded runtimes |
| **Governance Hash** | SHA-256 integrity verification of all `.aisop.json` / `.aisip.json` files in a program |
| **AIAP Packaging** | `.aiap` archive format with manifest, file hashes, and Code Trust Gate |

## Architecture

AIAP operates over dual blueprint formats — **AISOP** (Mermaid flow) and **AISIP** (JSON flow) — through a tripartite federated trust model:

```
┌─────────────────────────────────────────────────────┐
│                  AIAP Governance Stack               │
├─────────────────────────────────────────────────────┤
│                                                     │
│  aisop.dev (Seed Layer)                             │
│    Defines .aisop.json and .aisip.json formats      │
│    Neutral, static, foundational                    │
│                         ↓                           │
│  aiap.dev (Authority Layer)                         │
│    Defines governance rules, quality standards,     │
│    security constraints, and Axiom 0 enforcement    │
│                         ↓                           │
│  soulbot.dev (Executor Layer)                       │
│    Reference runtime that executes AISOP code,      │
│    resolves tools, manages memory and permissions   │
│                                                     │
├─────────────────────────────────────────────────────┤
│  Axiom 0: Human Sovereignty and Wellbeing (Immutable) │
└─────────────────────────────────────────────────────┘
```

### AIAP in the Agent Stack

```
┌──────────────────────────────────────┐
│  A2A Protocol  — Agent Communication │  How agents talk to each other
├──────────────────────────────────────┤
│  MCP Protocol  — Tool Access         │  How agents access tools and data
├──────────────────────────────────────┤
│  AIAP Protocol — Program Governance  │  How agent programs are structured,
│                                      │  validated, and secured
├──────────────────────────────────────┤
│  AISOP / AISIP — Blueprint Languages │  .aisop.json + .aisip.json formats
├──────────────────────────────────────┤
│  LLM Runtime   — Model Execution    │  Claude, Gemini, GPT, etc.
└──────────────────────────────────────┘
```

## Getting Started

- **Protocol Specification**: Read the complete structural rules in [`specification/AIAP_Protocol.md`](specification/AIAP_Protocol.md)
- **Quality Standards**: Review the validation rules in [`specification/standards/`](specification/standards/)
- **Conceptual Guides**: Explore topics in [`docs/topics/`](docs/topics/) for accessible explanations
- **Practical Guides**: Follow step-by-step tutorials in [`docs/guides/`](docs/guides/)
- **Examples**: See working AIAP programs in [`examples/`](examples/)
- **Reference**: Look up fields, rules, and terms in [`docs/reference/`](docs/reference/)

## AIAP Program Structure

Every AIAP program lives in an `*_aiap/` directory with this minimum structure:

```
my_program_aiap/
├── AIAP.md              # Governance contract (identity, tools, modules, trust)
├── main.aisop.json      # Entry point — AISOP format (Mermaid flow)
│   or main.aisip.json   # Entry point — AISIP format (JSON flow)
├── module1.aisop.json   # Functional module
├── module2.aisop.json   # Functional module
└── memory/              # Optional: state persistence (Pattern E)
```

The `AIAP.md` file serves as the program's governance contract, declaring its protocol compliance, tools, modules, trust level, and capabilities. It is the entry point for both human review and automated discovery.

> **AISOP vs AISIP**: AISOP uses Mermaid flowcharts for control flow; AISIP uses JSON structures. Both share the same AIAP governance layer (§0/§2/§5/§6) and differ only in flow definition (§4). Programs may use either format.

## Contributing

We welcome contributions to enhance and evolve the AIAP protocol!

- **Questions & Discussions**: Use [GitHub Discussions](https://github.com/AISOP-AIAP-SoulBot/AIAP/discussions)
- **Issues & Feedback**: Report issues via [GitHub Issues](https://github.com/AISOP-AIAP-SoulBot/AIAP/issues)
- **Contribution Guide**: See [CONTRIBUTING.md](CONTRIBUTING.md) for details

## License

This project is licensed under the Apache License 2.0 — see the [LICENSE](LICENSE) file for details.

```
Copyright 2026 AIXP Foundation AIXP.dev | AIAP.dev
```

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIAP V1.0.0. www.aiap.dev
