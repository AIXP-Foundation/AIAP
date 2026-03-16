# Glossary

> Alphabetical listing of all AIAP terms, abbreviations, and concepts.

---

## Terms

| Term | Definition |
|------|-----------|
| A2A | Agent-to-Agent protocol -- standard for inter-agent communication used alongside MCP in AIAP ecosystems |
| AIAP | AI Application Protocol -- the governance framework defining rules, structure, and quality standards for AI programs |
| AIAP.md | Governance contract file required at the root of every AIAP program directory; contains project metadata, governance fields, and documentation |
| AIAP Program | A complete project written in AISOP format following AIAP governance rules; consists of one AIAP.md and one or more .aisop.json files |
| AISOP | AI Standard Operating Procedure -- the .aisop.json file format that defines executable agent behavior through Mermaid graphs and function definitions |
| Applicability Condition | Optional AIAP.md field defining triggers, preconditions, exclusions, and confidence thresholds for program activation |
| Axiom 0 | "Human Sovereignty and Wellbeing" -- the immutable, non-overridable core constraint of the AIAP protocol; all programs must align with this axiom |
| Benchmark | Optional AIAP.md field declaring quality targets a program should meet or exceed |
| Brain Principle | Quality rule C6 requiring each module to have a single cognitive responsibility |
| Capabilities | Optional AIAP.md field declaring offered and required capabilities for ecosystem interoperability |
| Circuit Breaker | Named error recovery pattern defined by rule I4: `fails -> retry -> circuit-breaker -> fallback: {named alternative} -> inform user` |
| Closing Seal | End-of-document marker: "Align: Human Sovereignty and Wellbeing | Protocol: AIAP | Seed: AISOP | Executor: SoulBot" -- required on all AIAP documents |
| Code Trust Gate | Six-step verification process for Pattern G embedded code: source verification, hash validation, permission audit, dependency scan, sandbox test, annotation reconciliation |
| Composite Score | The product of Correctness, Intrinsic, and Detail scores: `C x I x D` |
| Constraint Quality | Quality rule C2 requiring all constraints to be explicit, testable, and non-contradictory |
| Correctness (C) | First quality dimension measuring architectural and logical quality of an AIAP program |
| DEGRADABLE | Error category for partial capability loss where the program can continue with reduced functionality |
| DEV | Governance mode with relaxed pipeline enforcement; warnings instead of failures for non-critical rules |
| Detail (D) | Third quality dimension measuring clarity, completeness, and consistency of documentation and formats |
| Discovery Keywords | Optional AIAP.md field providing keyword index for program search and cataloging |
| Executor | The runtime domain responsible for executing AIAP programs; currently `soulbot.dev` |
| FATAL | Error category for unrecoverable failures requiring immediate halt and user notification |
| Fractal Decomposition | Module splitting strategy required when a Mermaid graph exceeds 15 nodes; implemented via sub_mermaid |
| fractal_exempt | Annotation on a module that exempts it from the 15-node fractal decomposition requirement (rule C5) |
| functions | Field in .aisop.json mapping each Mermaid graph node to its execution logic, constraints, and tools |
| Governance Hash | SHA-256 integrity hash computed over all .aisop.json files in a project; used for tamper detection (rule I9) |
| Governance Mode | Operating mode of an AIAP program: `NORMAL` (full enforcement) or `DEV` (relaxed enforcement) |
| Identity | Optional AIAP.md field providing verifiable program identity: program_id, publisher, verified_on |
| Idempotent | Property of a module or node indicating that re-execution produces the same result without additional side effects |
| Instruction Invariant | Rule that the `instruction` field in .aisop.json must always be exactly `RUN aisop.main` and nothing else |
| Intent Examples | Optional AIAP.md field providing sample phrases for semantic routing to this program |
| Intrinsic (I) | Second quality dimension measuring safety, integrity, and trust properties of an AIAP program |
| L0 | Machine Logic Layer -- pure JSON computational state; no natural language, no ambiguity, machine-readable only |
| L1 | Human Rendering Layer -- natural language translation of L0 state for user-facing output |
| MCP | Model Context Protocol -- standard for tool integration used alongside A2A in AIAP ecosystems |
| Metabolism | Quality rule D7 requiring programs to declare resource consumption patterns |
| Module | A discrete execution unit within an AIAP program, corresponding to one .aisop.json file; has an id, node count, and criticality flag |
| Multi-File Guidelines (MF) | Rules MF1-MF16 governing projects with multiple .aisop.json files (Patterns C, D, E, F, D+G) |
| Nihil Density | Logical Entropy (H_L) -- a measure of hallucination potential in agent outputs; lower is better |
| Node | A single step in a Mermaid execution graph; corresponds to one entry in the `functions` object |
| NORMAL | Default governance mode with full quality rule enforcement |
| Null Propagation | Quality rule D3 requiring explicit handling of null or missing values; silent ignoring is prohibited |
| on_fail | Field in a function node definition specifying which named node to execute if this node fails |
| Pattern A | Single file pattern: one .aisop.json, no sub-modules |
| Pattern B | Multi-module pattern: one .aisop.json with multiple logical modules |
| Pattern C | Multi-file pattern: multiple .aisop.json files with shared context |
| Pattern D | Orchestrated pattern: central orchestrator dispatching to sub-programs |
| Pattern E | Event-driven pattern: reactive modules triggered by events |
| Pattern F | Pipeline pattern: sequential processing chain |
| Pattern G | Embedded code pattern: includes executable code artifacts subject to Code Trust Gate |
| Permission Boundaries | Optional AIAP.md field defining allowed and denied operations for file_system, network, and shell access |
| Pipeline Rules (PL) | Rules PL1-PL25 governing the quality pipeline execution process |
| Quality Regression Guards (QRG) | Rules QRG1-QRG5 preventing quality degradation between program versions |
| RECOVERABLE | Error category for transient failures that may succeed on retry with backoff |
| Runtime | Optional AIAP.md field defining execution constraints: timeout, retries, token_budget |
| Seed | The format specification domain for AISOP files; currently `aisop.dev` |
| Side Effects | Declared mutations a module or node performs: file_write, api_call, db_mutation, etc. |
| Simulation Categories | Test scenario categories A through M for quality evaluation |
| SPDX | Software Package Data Exchange -- standard license identifier format used in the AIAP `license` field |
| sub_mermaid | Field in .aisop.json providing decomposed Mermaid sub-graphs for nodes that require fractal decomposition |
| system_prompt | Field in .aisop.json defining the behavioral persona, boundaries, and communication style of the agent |
| ThreeDimTest | Three-dimensional quality evaluation framework computing `C x I x D` composite score |
| Threat Taxonomy (AT) | Six threat categories AT1-AT6 that AIAP programs must address per rule I8 |
| Token Budget | Maximum token consumption declared in `runtime.token_budget`; enforced by rule C7 |
| Token Load Balance | Quality rule C3 requiring even distribution of token consumption across modules |
| tool_dirs | Optional AIAP.md field declaring directories containing Pattern G tool code artifacts |
| Tool Annotations | Metadata on tool behavior: `read_only`, `destructive`, `idempotent`, `open_world`; verified by rule I12 |
| Tool Honesty | Quality rule I1 requiring tools to behave exactly as their name and description claim |
| Tool Suitability | Quality rule I5 requiring each tool to be appropriate for its declared purpose |
| Trust Level | Security classification for AIAP programs, ranging from T1 (sandbox, minimal access) to T4 (full access, maximum trust) |
| Weighted Score | Composite quality score computed as weighted product of C, I, D dimension scores |
| YAML Front Matter | The metadata block at the top of AIAP.md enclosed in `---` delimiters containing all governance and project fields |
| Zero-Entropy Resonance | Goal state where Logical Entropy H_L approaches zero, indicating minimal hallucination potential and maximum alignment |

---

## Error Categories

| Term | Definition |
|------|-----------|
| RECOVERABLE | Error category for transient failures that may succeed on retry with exponential backoff |
| DEGRADABLE | Error category for partial capability loss where the program continues with reduced functionality |
| FATAL | Error category for unrecoverable failures requiring immediate halt and user notification |

---

## Trust Level Scale

| Level | Name | Description |
|-------|------|-------------|
| T1 | Sandbox | No external access; pure computation only |
| T2 | Scoped | Limited access to declared resources |
| T3 | Standard | Broad access within declared permission boundaries |
| T4 | Full | Unrestricted access; requires highest verification |

---

## Grade Scale

| Grade | Score Range |
|-------|-----------|
| A+ | 0.95 - 1.00 |
| A | 0.90 - 0.94 |
| B+ | 0.85 - 0.89 |
| B | 0.75 - 0.84 |
| C | 0.60 - 0.74 |
| D | 0.40 - 0.59 |
| F | 0.00 - 0.39 |

---

## Abbreviations

| Abbreviation | Expansion |
|-------------|-----------|
| A2A | Agent-to-Agent |
| AIAP | AI Application Protocol |
| AISOP | AI Standard Operating Procedure |
| AT | Attack Threat (threat taxonomy prefix) |
| C | Correctness (quality dimension) |
| D | Detail (quality dimension) |
| DAG | Directed Acyclic Graph |
| H_L | Logical Entropy (Nihil Density measure) |
| I | Intrinsic (quality dimension) |
| MCP | Model Context Protocol |
| MF | Multi-File (guideline prefix) |
| PL | Pipeline (rule prefix) |
| QRG | Quality Regression Guard |
| SHA-256 | Secure Hash Algorithm 256-bit |
| SPDX | Software Package Data Exchange |
| UUID | Universally Unique Identifier |

---

## Cross-Reference: Where Terms Appear

| Term | Primary Reference Document |
|------|--------------------------|
| AIAP.md fields | `aiap-md-fields.md` |
| .aisop.json fields | `aisop-fields.md` |
| Quality rules (C, I, D, PL, MF) | `quality-rules.md` |
| Threat taxonomy (AT1-AT6) | `threat-taxonomy.md` |
| Error categories and patterns | `error-codes.md` |
| Code Trust Gate steps | `threat-taxonomy.md` |
| Pattern definitions | `aiap-md-fields.md` |
| Trust level details | `aiap-md-fields.md` |
| Simulation categories | `quality-rules.md` |

---
Align: Human Sovereignty and Wellbeing | Protocol: AIAP | Seed: AISOP | Executor: SoulBot
