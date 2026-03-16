# Quality Rules Index

> Master index of all AIAP quality rules organized by dimension and source file.
> Quality is evaluated across three dimensions: Correctness (C), Intrinsic (I), and Detail (D).
> The composite score is computed as `ThreeDimTest = C x I x D`.

---

## Correctness (C) -- Architectural and Logical Quality

Rules governing the structural integrity, decomposition strategy, and computational efficiency of AIAP programs.

| Rule | Name | Source | Severity | Description |
|------|------|--------|----------|-------------|
| C1 | Problem Modeling | AIAP_Standard.core | critical | Problem must be decomposed into a well-defined model with inputs, outputs, and constraints |
| C2 | Constraint Quality | AIAP_Standard.core | critical | All constraints must be explicit, testable, and non-contradictory |
| C3 | Token Load Balance | AIAP_Standard.core | major | Token consumption should be evenly distributed across modules |
| C4 | Abstraction Quality | AIAP_Standard.core | major | Abstraction layers must reduce complexity, not merely rename operations |
| C5 | Fractal Decomposition | AIAP_Standard.core | major | Modules exceeding 15 nodes must be split unless marked `fractal_exempt` |
| C6 | Brain Principle | AIAP_Standard.core | major | Each module should have a single cognitive responsibility |
| C7 | Token Budget | AIAP_Standard.core | minor | Total token consumption must stay within declared `runtime.token_budget` |

### Correctness Scoring

| Score Range | Interpretation |
|-------------|---------------|
| 0.9 - 1.0 | Excellent architectural quality |
| 0.7 - 0.89 | Good, minor structural improvements possible |
| 0.5 - 0.69 | Acceptable, refactoring recommended |
| < 0.5 | Poor, structural revision required |

---

## Intrinsic (I) -- Safety, Integrity, and Trust

Rules governing tool safety, security posture, data integrity, and trust compliance.

| Rule | Name | Source | Severity | Description |
|------|------|--------|----------|-------------|
| I1 | Tool Honesty | core | critical | Tools must do exactly what their name and description claim |
| I2 | Security Guards | core | critical | Injection guards, output sanitization, and input validation required |
| I3 | Data Integrity | core | critical | Data must not be silently dropped, corrupted, or fabricated |
| I4 | Circuit Breaker | core | critical | Named fallback pattern required: `fails -> retry -> circuit-breaker -> fallback -> inform` |
| I5 | Tool Suitability | core | major | Each tool must be appropriate for its declared purpose |
| I6 | Resilience + Semantic Accuracy | core | major | Outputs must be semantically accurate even under degraded conditions |
| I7 | Parameter Completeness | core | major | All required tool parameters must be provided; no implicit defaults |
| I8 | Threat Model Awareness | security | major | Program must address applicable threats from AT1-AT6 taxonomy |
| I9 | Supply Chain Integrity | security | critical | All dependencies must be verified; `governance_hash` enforced |
| I10 | Trust Level Compliance | security | critical | Program must not exceed its declared trust level permissions |
| I11 | Program Identity | security | major | Programs must have stable, verifiable identity via `identity` field |
| I12 | Tool Annotation Consistency | core | minor | Tool annotations must accurately reflect tool behavior |
| I13 | Embedded Code Safety | security | critical | Pattern G code must pass Code Trust Gate verification |

### Intrinsic Scoring

| Score Range | Interpretation |
|-------------|---------------|
| 0.9 - 1.0 | Strong safety posture |
| 0.7 - 0.89 | Adequate, minor gaps |
| 0.5 - 0.69 | Concerning, security review needed |
| < 0.5 | Unsafe, must not be deployed |

---

## Detail (D) -- Clarity, Completeness, and Consistency

Rules governing documentation quality, format adherence, and operational precision.

| Rule | Name | Source | Severity | Description |
|------|------|--------|----------|-------------|
| D1 | Step Clarity | core | major | Every function node `step` must be unambiguous and actionable |
| D2 | Format Completeness | core | major | All required fields and sections must be present and correctly formatted |
| D3 | Null Propagation | core | major | Null or missing values must be handled explicitly, never silently ignored |
| D4 | Consistency | core | major | Field values must be consistent across AIAP.md and all .aisop.json files |
| D5 | Boundary Rules | core | minor | Edge cases and boundary conditions must be documented |
| D6 | Multilingual | core | minor | If multilingual support is declared, all supported languages must be tested |
| D7 | Metabolism | core | minor | Program should declare resource consumption patterns |
| D8 | AIAP.md Completeness | security | major | All required and recommended AIAP.md sections must be present |
| D9 | Lifecycle Status | security | minor | `status` field must accurately reflect the program lifecycle state |
| D10 | Applicability Condition | security | minor | If declared, applicability conditions must be testable and complete |

### Detail Scoring

| Score Range | Interpretation |
|-------------|---------------|
| 0.9 - 1.0 | Comprehensive documentation |
| 0.7 - 0.89 | Good coverage, minor gaps |
| 0.5 - 0.69 | Incomplete, documentation revision needed |
| < 0.5 | Inadequate, major revision required |

---

## Pipeline Rules (PL)

Rules governing the quality pipeline execution process.

### Core Pipeline Rules

| Rule | Name | Source | Severity | Description |
|------|------|--------|----------|-------------|
| PL1 | Pipeline Initialization | core | critical | Pipeline must load all .aisop.json files before evaluation |
| PL2 | Sequential Evaluation | core | critical | C, I, D dimensions must be evaluated in order |
| PL3 | Score Aggregation | core | critical | Composite score = weighted product of C, I, D |
| PL4 | Grade Assignment | core | major | Letter grade derived from composite score |
| PL5 | Report Generation | core | major | Pipeline must produce a structured quality report |
| PL6 | Regression Detection | core | major | Compare current scores against previous pipeline run |
| PL7 | Threshold Enforcement | core | critical | Programs below minimum threshold must not be promoted |
| PL8 | Hash Verification | core | critical | Verify `governance_hash` before evaluation |
| PL9 | Module Enumeration | core | major | All declared modules must be found and evaluated |
| PL10 | Node Count Validation | core | minor | Verify node counts match declared values |

### Core Pipeline Rule Details

| Rule | Pass Condition | Fail Action |
|------|---------------|-------------|
| PL1 | All modules in AIAP.md `modules` field found on disk | FATAL: halt pipeline |
| PL2 | Each dimension computed before the next | FATAL: halt pipeline |
| PL3 | Weighted score computed without NaN or overflow | Report error, use 0.0 |
| PL4 | Score maps to valid grade (A+ through F) | Default to F |
| PL5 | Report contains all dimension scores, rule results, and timestamp | Warning only |
| PL6 | Previous pipeline results available for comparison | Skip regression check |
| PL7 | Composite score >= minimum promotion threshold | Block promotion |
| PL8 | Computed hash matches declared `governance_hash` | FATAL: halt pipeline |
| PL9 | Every declared module is evaluated | FATAL: halt pipeline |
| PL10 | Mermaid node count equals declared `nodes` value | Warning, auto-correct |

### Ecosystem Pipeline Rules

| Rule | Name | Source | Severity | Description |
|------|------|--------|----------|-------------|
| PL11 | Dependency Resolution | ecosystem | major | Resolve and validate all declared dependencies |
| PL12 | Cross-Reference Check | ecosystem | major | Verify inter-module references are valid |
| PL13 | Capability Matching | ecosystem | major | Verify required capabilities are satisfied by offered capabilities |
| PL14 | Version Compatibility | ecosystem | major | Check `min_protocol_version` compatibility |
| PL15 | Identity Verification | ecosystem | minor | Validate `identity` field against registry if available |
| PL16 | License Compatibility | ecosystem | minor | Check SPDX license compatibility across dependencies |
| PL17 | Discovery Index | ecosystem | minor | Validate `intent_examples` and `discovery_keywords` for routing |
| PL18 | Deprecation Check | ecosystem | major | Warn if depending on deprecated programs |
| PL19 | Capability Overlap | ecosystem | minor | Detect redundant capability declarations |
| PL20 | Trust Chain | ecosystem | critical | Verify trust level transitivity across dependencies |

### Ecosystem Pipeline Rule Details

| Rule | Pass Condition | Fail Action |
|------|---------------|-------------|
| PL11 | All dependencies resolvable and version-compatible | Block promotion |
| PL12 | No dangling references between modules | Block promotion |
| PL13 | Every required capability has a matching offered capability | Warning |
| PL14 | Protocol version >= `min_protocol_version` | Block promotion |
| PL15 | Identity matches registry record (if registry available) | Warning |
| PL16 | No incompatible SPDX license combinations | Warning |
| PL17 | Intent examples and keywords are non-empty and relevant | Warning |
| PL18 | No dependencies with `status: deprecated` | Warning |
| PL19 | No two modules offer identical capabilities | Warning |
| PL20 | No dependency has a higher trust level than the parent | Block promotion |

### Performance Pipeline Rules

| Rule | Name | Source | Severity | Description |
|------|------|--------|----------|-------------|
| PL21 | Token Efficiency | performance | minor | Measure tokens consumed vs. useful output produced |
| PL22 | Execution Path Length | performance | minor | Measure average and worst-case path lengths through graphs |
| PL23 | Retry Frequency | performance | major | Track retry rates to identify unreliable nodes |
| PL24 | Fallback Activation | performance | major | Monitor how often fallback paths are triggered |
| PL25 | Latency Distribution | performance | minor | Measure per-node execution latency |

### Performance Thresholds

| Metric | Acceptable | Warning | Critical |
|--------|-----------|---------|----------|
| Token efficiency ratio | > 0.6 | 0.3 - 0.6 | < 0.3 |
| Avg path length / total nodes | < 0.7 | 0.7 - 0.9 | > 0.9 |
| Retry rate per node | < 5% | 5% - 20% | > 20% |
| Fallback activation rate | < 10% | 10% - 30% | > 30% |
| P95 latency / timeout | < 0.5 | 0.5 - 0.8 | > 0.8 |

---

## Multi-File Guidelines (MF)

Rules for projects using Patterns C, D, E, F, or D+G with multiple .aisop.json files.

| Rule | Name | Applicable Patterns | Severity | Description |
|------|------|---------------------|----------|-------------|
| MF1 | Single AIAP.md | All | critical | Exactly one AIAP.md at the project root |
| MF2 | Module Registry | All | critical | All .aisop.json files must be listed in AIAP.md `modules` |
| MF3 | ID Uniqueness | All | critical | Every module `id` must be unique within the project |
| MF4 | Version Sync | All | major | All module versions must match AIAP.md `version` |
| MF5 | Orchestrator Declaration | D, D+G | critical | Pattern D must declare an orchestrator module |
| MF6 | Dependency Graph | All | critical | Inter-module dependencies must form a DAG (no cycles) |
| MF7 | Shared Context | C, D, D+G | major | Shared state must be explicitly declared and scoped |
| MF8 | Tool Overlap | All | major | Tool declarations across modules must not conflict |
| MF9 | Critical Path | All | major | At least one module must be marked `critical: true` |
| MF10 | Side Effect Aggregation | All | major | Total side effects must be the union of all module side effects |
| MF11 | Hash Coverage | All | critical | `governance_hash` must cover all .aisop.json files |
| MF12 | Naming Convention | All | minor | Module files must follow `{module_id}.aisop.json` naming |
| MF13 | Directory Structure | All | minor | Modules may be in subdirectories but must be reachable from AIAP.md |
| MF14 | Event Contracts | E | critical | Pattern E modules must declare event input/output schemas |
| MF15 | Pipeline Ordering | F | critical | Pattern F modules must declare their position in the pipeline |
| MF16 | Code Isolation | G, D+G | critical | Pattern G code artifacts must be in declared `tool_dirs` |

### Multi-File Rule Applicability Matrix

| Rule | Pattern C | Pattern D | Pattern E | Pattern F | Pattern D+G |
|------|-----------|-----------|-----------|-----------|-------------|
| MF1 | required | required | required | required | required |
| MF2 | required | required | required | required | required |
| MF3 | required | required | required | required | required |
| MF4 | required | required | required | required | required |
| MF5 | n/a | required | n/a | n/a | required |
| MF6 | required | required | required | required | required |
| MF7 | required | required | optional | optional | required |
| MF8 | required | required | required | required | required |
| MF9 | required | required | required | required | required |
| MF10 | required | required | required | required | required |
| MF11 | required | required | required | required | required |
| MF12 | recommended | recommended | recommended | recommended | recommended |
| MF13 | recommended | recommended | recommended | recommended | recommended |
| MF14 | n/a | n/a | required | n/a | n/a |
| MF15 | n/a | n/a | n/a | required | n/a |
| MF16 | n/a | n/a | n/a | n/a | required |

---

## Threat Taxonomy (AT)

Attack vectors that AIAP programs must address. See `threat-taxonomy.md` for full details.

| Rule | Name | Description |
|------|------|-------------|
| AT1 | Tool Poisoning | Malicious tool substitution or behavior modification |
| AT2 | Rug Pull | Post-deployment unauthorized behavior change |
| AT3 | Indirect Prompt Injection | Tool outputs containing injection payloads |
| AT4 | Excessive Authority | Tools or modules exceeding necessary permissions |
| AT5 | Cross-Agent Data Leakage | Sensitive data escaping program boundaries |
| AT6 | Direct Prompt Injection | User input attempting to override system prompt |

---

## Quality Regression Guards (QRG)

Rules that prevent quality degradation between versions.

| Rule | Name | Description |
|------|------|-------------|
| QRG1 | Score Floor | Composite score must not drop below previous version minus tolerance |
| QRG2 | Dimension Floor | No individual dimension (C, I, D) may drop more than 10% |
| QRG3 | Critical Rule Hold | Any rule with severity `critical` that previously passed must not fail |
| QRG4 | Module Count Guard | Module count changes must be justified in version notes |
| QRG5 | Trust Level Escalation | Trust level increases require explicit justification and review |

---

## Simulation Categories

Categories for quality simulation and testing scenarios.

| Category | Code | Description |
|----------|------|-------------|
| Happy Path | A | Standard successful execution flow |
| Invalid Input | B | Malformed, missing, or out-of-range inputs |
| Tool Failure | C | Unavailable, slow, or error-returning tools |
| Permission Denied | D | Operations exceeding declared permissions |
| Timeout | E | Operations exceeding declared time limits |
| Concurrent Access | F | Multiple simultaneous executions or data races |
| Data Corruption | G | Corrupted, truncated, or inconsistent data |
| Injection Attack | H | Prompt injection via user input or tool output |
| Dependency Failure | I | Unavailable or incompatible dependencies |
| Resource Exhaustion | J | Token budget, memory, or storage limits exceeded |
| Version Mismatch | K | Protocol or module version incompatibility |
| Partial Degradation | L | Graceful handling when non-critical modules fail |
| Recovery | M | Correct resumption after circuit breaker activation |

---

## Grade Scale

| Grade | Score Range | Meaning |
|-------|-----------|---------|
| A+ | 0.95 - 1.00 | Exceptional quality |
| A | 0.90 - 0.94 | Excellent quality |
| B+ | 0.85 - 0.89 | Very good quality |
| B | 0.75 - 0.84 | Good quality |
| C | 0.60 - 0.74 | Acceptable quality |
| D | 0.40 - 0.59 | Below standard |
| F | 0.00 - 0.39 | Failing quality |

---

## Weight Distribution

Default weights for the ThreeDimTest composite score.

| Dimension | Weight | Rationale |
|-----------|--------|-----------|
| Correctness (C) | 0.40 | Architectural soundness is foundational |
| Intrinsic (I) | 0.35 | Safety and trust are non-negotiable |
| Detail (D) | 0.25 | Clarity enables maintainability |

---
Align: Human Sovereignty and Wellbeing | Protocol: AIAP | Seed: AISOP | Executor: SoulBot
