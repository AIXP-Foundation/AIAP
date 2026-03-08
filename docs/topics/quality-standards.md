# Quality Standards: The ThreeDimTest Framework

> AIAP Protocol — Conceptual Guide
> Classification: Quality Assurance

---

## Introduction

AIAP programs are validated through the ThreeDimTest framework, a three-dimensional quality evaluation system. Unlike traditional software testing, which verifies behavior after implementation, ThreeDimTest operates as a compile-time quality gate — evaluating the structural, security, and specification completeness of an `.aisop.json` program before it ever executes.

Every AIAP program receives a score across three independent dimensions: Correctness (C), Intrinsic (I), and Detail (D). These dimensions are orthogonal — a program can score perfectly on structural correctness while failing on security invariants, or vice versa. All three dimensions must meet minimum thresholds for a program to be considered production-ready.

The ThreeDimTest framework is the practical implementation of the Semantic Compiler concept. Where the Semantic Compiler defines what must be verified, ThreeDimTest defines how it is measured.

---

## The Three Dimensions

### Overview

| Dimension | Code | Focus | Question |
|---|---|---|---|
| Correctness | C | Structural Integrity | Is the logic sound? |
| Intrinsic | I | Safety and Security | Is the program safe? |
| Detail | D | Specification Completeness | Is the program complete? |

Each dimension contains multiple criteria, each scored on a 0-5 scale:

- **5**: Exemplary. Exceeds requirements with no defects.
- **4**: Strong. Meets all requirements with minor observations.
- **3**: Adequate. Meets core requirements with some gaps.
- **2**: Partial. Significant gaps that affect reliability.
- **1**: Minimal. Fundamental issues present.
- **0**: Absent. The criterion is not addressed.

---

## Correctness Dimension (C1-C7)

The Correctness dimension evaluates whether the program's logic is structurally sound. It answers the question: "If this program executes, will it follow a valid logical path?"

### C1: Problem Modeling

**Mermaid graph completeness.**

The program's Mermaid graph must accurately and completely represent the node topology. Every node must appear in the graph. Every edge must be represented. The graph must be a faithful visual representation of the program's execution flow.

Scoring criteria:
- All nodes present and correctly labeled
- All edges present with correct routing conditions
- Start and end nodes clearly identified
- No visual ambiguity in the graph structure

### C2: Constraint Quality

**Valid constraints, no phantom references.**

Every constraint declared in the program must reference existing nodes, parameters, and edges. Phantom constraints — constraints that reference non-existent elements — are structural defects.

Scoring criteria:
- All constraints reference valid program elements
- No phantom node references in constraints
- No phantom edge references in routing logic
- Constraint logic is internally consistent (no contradictions)

### C3: Token Load Balance

**Node complexity distribution.**

No single node should bear a disproportionate share of the program's cognitive load. A node that tries to do everything is a node that will fail at something. The Token Load Balance criterion evaluates whether complexity is evenly distributed across the graph.

Scoring criteria:
- No single node exceeds recommended token budget
- Complex logic is distributed across multiple nodes
- Node responsibilities are clearly delineated
- No "god nodes" that attempt to handle multiple concerns

### C4: Abstraction Quality

**Appropriate abstraction levels.**

Each node should operate at a consistent level of abstraction. A node that mixes high-level routing decisions with low-level data formatting is operating at inconsistent abstraction levels, which increases cognitive load and error probability.

Scoring criteria:
- Each node operates at a single abstraction level
- Related operations are grouped appropriately
- Abstraction boundaries align with logical boundaries
- No mixing of orchestration logic with computation logic

### C5: Fractal Decomposition

**12/16 node thresholds.**

AIAP enforces cognitive load limits through fractal decomposition. A single graph should contain no more than 12 nodes for optimal comprehension. At 16 nodes, the graph must be decomposed into sub-graphs. This threshold is not arbitrary — it reflects the limits of human cognitive capacity for tracking concurrent state.

Scoring criteria:
- Graph contains 12 or fewer nodes (ideal)
- If graph exceeds 12 nodes, decomposition justification provided
- Graph never exceeds 16 nodes without sub-graph decomposition
- Sub-graphs are self-contained and independently comprehensible

### C6: Brain Principle

**Cognitive load management.**

The Brain Principle states that an AIAP program should be comprehensible to a human reviewer in a single reading session. If a reviewer must re-read the program multiple times to understand its flow, the program is too complex. This criterion evaluates overall cognitive accessibility.

Scoring criteria:
- Program flow is comprehensible on first reading
- Node naming is descriptive and unambiguous
- Edge conditions are clearly stated
- Overall program intent is immediately apparent

### C7: Token Budget

**Resource allocation.**

Every AIAP program must declare its token budget — the expected token consumption for a single execution pass. This budget must be realistic, accounting for all node prompts, inter-node communication overhead, and model response lengths.

Scoring criteria:
- Token budget is explicitly declared
- Budget is realistic for the program's complexity
- Individual node budgets sum to less than the total budget
- Overhead allocation is included for inter-node communication

---

## Intrinsic Dimension (I1-I13)

The Intrinsic dimension evaluates whether the program's safety invariants are satisfied. It answers the question: "If this program executes, will it do so safely?"

### I1: Tool Honesty

**Declared tools match used tools.**

Every tool used by a node must be declared in the program's tool manifest. A node that uses an undeclared tool is dishonest — it performs operations that the program's security model has not accounted for. Conversely, a declared tool that is never used indicates incomplete specification.

Scoring criteria:
- All used tools are declared in the tool manifest
- All declared tools are used by at least one node
- Tool capabilities match their declared descriptions
- No hidden tool usage through indirect invocation

### I2: Security Guards

**The Five Security Guards: Type, Injection, Size, Encoding, Path.**

Every node that accepts input must implement appropriate security guards:

- **Type Guard**: Validates that input values match declared types. A parameter declared as integer must reject string inputs.
- **Injection Guard**: Prevents prompt injection, SQL injection, command injection, and path traversal through input sanitization.
- **Size Guard**: Enforces maximum input sizes to prevent resource exhaustion and buffer-related attacks.
- **Encoding Guard**: Validates input encoding to prevent encoding-based attacks (UTF-8 validation, no null bytes, no control characters).
- **Path Guard**: Mandatory if `file_system` is declared in permissions. Validates that file paths remain within declared scope boundaries. Prevents path traversal attacks.

Scoring criteria:
- All five guards present where applicable
- Guard configurations are specific (not generic)
- Path Guard is present whenever file_system permissions are declared
- Guards are tested against declared threat model

### I3: Data Integrity

**Atomic writes, backup, schema validation.**

Nodes that perform write operations must implement data integrity patterns:

- **Atomic Writes**: Write operations must be atomic — either the entire write succeeds or no change occurs. Partial writes corrupt state.
- **Backup Strategy**: Before destructive operations, the program must declare its backup or rollback strategy.
- **Schema Validation**: Data written to persistent storage must be validated against a declared schema before writing.

### I4: Circuit Breaker

**Named fallback pattern.**

Every node that can fail must declare a named circuit breaker pattern:

```
fails -> retry -> circuit-breaker -> fallback: {specific_fallback_name} -> inform user
```

Generic fallbacks ("something went wrong") are prohibited. The fallback must be specific to the failure mode and named in the program specification.

### I5: Tool Suitability

**30% utilization threshold.**

If a declared tool is used by fewer than 30% of the nodes that could benefit from it, the tool may be unsuitable for the program's needs. This criterion evaluates whether tool selection is appropriate and whether tools are being utilized effectively.

### I6: Resilience and Semantic Accuracy

This criterion evaluates whether the program maintains correct behavior under adverse conditions (network failures, malformed inputs, timeout scenarios) and whether the semantic meaning of operations is preserved across node boundaries.

### I7: Parameter Completeness and Metadata Consistency

Every parameter must be fully specified: name, type, description, default value (or required flag), and validation constraints. Metadata across the program must be consistent — the same parameter must not be described differently in different nodes.

### I8: Threat Awareness

The program must declare its threat model — which threats it is designed to resist and which are out of scope. Programs that do not acknowledge their threat landscape cannot be evaluated for security adequacy.

### I9: Supply Chain Security

Tools and dependencies used by the program must have verifiable provenance. The program must declare the source and version of every external dependency.

### I10: Trust Compliance

The program must operate within its declared trust level (T1-T4). A program declared at T2 (read-only) must not contain nodes that perform write operations.

### I11: Identity and Access

Agent identity must be verifiable. In multi-agent systems, every agent must be identifiable, and inter-agent communication must be attributable.

### I12: Tool Annotation Consistency

Tool annotations (descriptions, parameter schemas, return types) must be consistent across all nodes that reference the same tool. A tool described as "read-only" in one node and "read-write" in another is inconsistently annotated.

### I13: Embedded Code Safety (Pattern G)

If the program embeds executable code (scripts, functions, macros), that code must pass the Code Trust Gate (Pattern G): signature check, SHA-256 match, allowlist verification, size limit, sandbox configuration, and audit logging.

---

## Detail Dimension (D1-D10)

The Detail dimension evaluates whether the program's specification is complete. It answers the question: "Is every aspect of this program fully defined?"

### D1: Step Clarity

Every node's instructions must be unambiguous. A human reader should be able to determine exactly what the node does without inferring unstated behavior.

### D2: Format Specification

Input and output formats must be explicitly declared for every node. JSON schemas, enum definitions, and data type specifications must be present and complete.

### D3: Null Propagation

The program must explicitly handle null values at every node boundary. Silent null propagation — where a null value passes through nodes without being addressed — is a specification gap.

### D4: Consistency

Naming conventions, formatting patterns, and structural choices must be consistent across the entire program. If one node uses camelCase for parameter names, all nodes must use camelCase.

### D5: Boundaries

Every parameter must declare its valid range or value set. Numeric parameters need min/max bounds. String parameters need length limits or pattern constraints. Enum parameters need complete value lists.

### D6: Multilingual Support

If the program operates in a multilingual context, language handling must be explicitly specified. Character encoding, locale-specific formatting, and translation boundaries must be declared.

### D7: Metabolism

The program must declare its resource consumption profile — expected execution time, token usage, memory requirements, and any external API call volumes. This "metabolic rate" enables capacity planning and cost estimation.

### D8: AIAP.md Completeness

The program's AIAP.md file must be complete and accurate. It must include: program name, version, description, trust level, tool manifest, permission boundaries, and all required metadata fields.

### D9: Lifecycle Status Validity

The program's declared lifecycle status (draft, active, deprecated, archived) must be accurate and must follow valid lifecycle transitions. A program cannot transition from "archived" to "active" without passing through "draft."

### D10: Applicability Condition Completeness

Every routing condition, constraint trigger, and conditional behavior must have a fully specified applicability condition. Conditions must be exhaustive — every possible input state must map to a defined behavior. The sentinel default pattern (D4 in Determinism Verification) ensures that no condition falls through to undefined behavior.

---

## Grading System

### Score Calculation

The overall ThreeDimTest score is calculated as a weighted average of the three dimension scores:

```
Overall Score = (W_C * Score_C + W_I * Score_I + W_D * Score_D) / (W_C + W_I + W_D)

Default weights:
  W_C = 1.0  (Correctness)
  W_I = 1.2  (Intrinsic — weighted higher due to safety criticality)
  W_D = 0.8  (Detail)
```

Each dimension score is the average of its constituent criteria scores.

### Grade Thresholds

| Grade | Minimum Score | Interpretation |
|---|---|---|
| S | >= 4.5 | Exemplary. Production-ready with distinction. |
| A | >= 3.5 | Strong. Production-ready. |
| B | >= 2.5 | Adequate. Acceptable for non-critical deployments. |
| C | >= 1.5 | Partial. Requires improvement before deployment. |
| D | >= 0.5 | Minimal. Significant rework required. |
| F | < 0.5 | Failed. Fundamental redesign required. |

### Minimum Requirements for Production Deployment

- Overall grade: A or higher (>= 3.5)
- No individual dimension below B (>= 2.5)
- Intrinsic dimension must be A or higher (>= 3.5)
- No individual I2 (Security Guards) criterion below 4.0

---

## Quality Regression Guards (QRG1-QRG5)

Quality Regression Guards prevent deployed programs from degrading over time. They are enforced on every version update.

### QRG1: Single-Version Drop Threshold

No single version update may decrease the overall score by more than 0.3 points. If a proposed update would cause a score drop exceeding this threshold, the update is rejected.

### QRG2: Cumulative Drift

Over any rolling window of 5 versions, the cumulative score change must not be negative by more than 0.5 points. This prevents gradual degradation through many small decreases.

### QRG3: Absolute Floor

No version update may cause any dimension score to drop below the grade B threshold (2.5). Once a program reaches production quality, it must stay there.

### QRG4: Dimension Regression

No single dimension may regress by more than 0.5 points in a single version update, even if the overall score remains acceptable. This prevents trading safety for features.

### QRG5: Security Floor

The Intrinsic dimension score must never drop below 3.5 (grade A) for any production program. Security is non-negotiable and cannot be sacrificed for improvements in other dimensions.

---

## Simulation Categories (A-M)

ThreeDimTest includes a scenario coverage framework with 13 simulation categories. Each category tests the program's behavior under specific conditions to ensure comprehensive coverage.

| Category | Name | Focus |
|---|---|---|
| A | Happy Path | Standard execution with valid inputs |
| B | Edge Cases | Boundary values, minimum/maximum inputs |
| C | Error Handling | Malformed inputs, missing parameters |
| D | Concurrency | Simultaneous execution, race conditions |
| E | Security | Injection attempts, privilege escalation |
| F | Performance | Token budget adherence, timeout behavior |
| G | Recovery | Circuit breaker activation, fallback paths |
| H | Multi-Agent | Inter-agent communication, cascade behavior |
| I | State Management | State persistence, state corruption recovery |
| J | Scaling | Behavior under increased load |
| K | Degraded Mode | Behavior when external dependencies fail |
| L | Lifecycle | Version transitions, deprecation handling |
| M | Compliance | Axiom 0 adherence under adversarial conditions |

A production-ready program must demonstrate adequate coverage across all 13 simulation categories. Coverage gaps indicate untested execution paths, which are potential sources of runtime failure.

---

```
AIAP_SEAL: AIAP/1.0
Document: quality-standards
Type: Conceptual Guide
Axiom_0: Human Sovereignty and Benefit
Status: ACTIVE
```
