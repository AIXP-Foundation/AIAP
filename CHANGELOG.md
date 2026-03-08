# Changelog

All notable changes to the AIAP Protocol will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [1.0.0] - 2026-03-03

### Added

- Complete AIAP V1.0.0 protocol specification (26 sections + 3 appendices)
- AISOP V1.0.0 file format specification (`.aisop.json`)
- Seven structural patterns (A-G): Script, Package, Package+Shared, Nested Package, Package+Memory, Ecosystem, Embedded Runtime
- Quality standards framework: AIAP_Standard.core, security, ecosystem, performance
- ThreeDimTest quality evaluation (Correctness C1-C7, Intrinsic I1-I13, Detail D1-D10)
- Trust levels T1-T4 with graduated permission boundaries
- Threat taxonomy AT1-AT6 (Tool Poisoning, Rug Pull, Indirect Injection, Excessive Authority, Cross-Agent Leakage, Direct Injection)
- Code Trust Gate for Pattern G embedded runtime verification
- Governance hash (SHA-256) integrity verification
- Program lifecycle management (draft/active/deprecated/archived)
- Discovery protocol (L1 passive, L2 semantic, L3 registry)
- MCP bidirectional tool mapping and A2A interoperability
- AIAP packaging specification (`.aiap` archive format)
- UI component declaration (dashboard, form, visualization, table)
- Pipeline rules PL1-PL25 and multi-file guidelines MF1-MF16
- Quality Regression Guards QRG1-QRG5
- Auto-Fix Protocol (PL24) and License Declaration (PL25)
- Simulation categories A-M with comprehensive scenario coverage
- Proto3 service definition (`aiap.proto`)
- Conceptual documentation (12 topic guides)
- Practical guides (4 tutorials)
- Reference documentation (6 reference pages)
- Example AIAP programs (Pattern A, Pattern B)
- Architecture Decision Records (3 initial ADRs)

### Governance

- Axiom 0 (Human Sovereignty and Benefit) established as immutable protocol constraint
- Tripartite governance chain: aisop.dev (Seed) → aiap.dev (Authority) → soulbot.dev (Executor)

---

Align: Human Sovereignty and Benefit. Version: AIAP V1.0.0. www.aiap.dev
