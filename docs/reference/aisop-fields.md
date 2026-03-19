# .aisop.json Field Reference

> Complete field reference for the `.aisop.json` execution specification file.
> This file defines the behavioral logic of an AIAP program in structured JSON.

---

## Field Overview

| Field | Responsibility | Type | Required | Content |
|-------|---------------|------|----------|---------|
| `id` | Identity | string | yes | Unique identifier for the execution unit |
| `name` | Name | string | yes | Product name with version (e.g., `"HealthTracker v1.0"`) |
| `version` | Version | string | yes | Semantic version matching AIAP.md version |
| `summary` | Capability overview | string | yes | One sentence describing what this unit does |
| `description` | Detailed description | string | yes | Architecture overview, design history, pattern rationale |
| `system_prompt` | Behavioral guidelines | string | yes | How the agent should behave during execution |
| `instruction` | Execution instruction | string | yes | Always `RUN aisop.main` |
| `aisop.main` | Execution graph | string | yes | Mermaid flowchart defining the execution path |
| `functions` | Execution logic | object | yes | Node-by-node steps, constraints, and instructions |

---

## Field Details

### id

A unique string identifier for this execution unit. Must be stable across versions.

| Constraint | Value |
|------------|-------|
| Format | snake_case or namespaced (e.g., `health_tracker.core`) |
| Uniqueness | Must be unique within the project |
| Mutability | Must not change between versions |

### name

Human-readable product name, typically including a version tag.

| Constraint | Value |
|------------|-------|
| Format | Free text with version suffix |
| Example | `"HealthTracker v1.0.0"` |

### version

Semantic version string that must match the version declared in `AIAP.md`.

| Constraint | Value |
|------------|-------|
| Format | `MAJOR.MINOR.PATCH` |
| Sync | Must equal `AIAP.md:version` |

### summary

A single sentence describing the capability of this execution unit.

| Constraint | Value |
|------------|-------|
| Length | One sentence, max 200 characters |
| Purpose | Used for semantic routing and discovery |

### description

A detailed multi-sentence description covering architecture, design patterns, and history.

| Constraint | Value |
|------------|-------|
| Length | No hard limit; should be comprehensive |
| Content | Architecture, design rationale, pattern explanation |

---

## Instruction Invariant Rule

The `instruction` field has a fixed, invariant value:

```json
"instruction": "RUN aisop.main"
```

| Rule | Description |
|------|-------------|
| Invariant | The value must always be exactly `RUN aisop.main` |
| Purpose | Guarantees a single, predictable entry point for execution |
| Violation | Any other value is a D2 (Format Completeness) violation |
| Rationale | Prevents instruction injection and ensures the Mermaid graph is always the execution authority |

The instruction field must never contain conditional logic, tool calls, or any text other than `RUN aisop.main`.

---

## system_prompt Rules

The `system_prompt` field defines the behavioral persona and constraints of the agent during execution.

### Must Include

| Requirement | Description |
|-------------|-------------|
| Role definition | What the agent is and what it does |
| Behavioral boundaries | What the agent must not do |
| Communication style | Tone, language, formatting expectations |
| Error handling posture | How to respond to failures |
| Axiom 0 acknowledgment | Reference to Human Sovereignty and Wellbeing |

### Must Not Include

| Prohibition | Reason |
|-------------|--------|
| Execution logic | Belongs in `functions`, not `system_prompt` |
| Tool call sequences | Belongs in `aisop.main` and `functions` |
| Mermaid graphs | Belongs in `aisop.main` or `sub_mermaid` |
| Conditional branching | Belongs in `functions` node definitions |
| Data schemas | Belongs in `functions` or external schemas |

### Example

```json
"system_prompt": "You are HealthTracker, a personal health monitoring assistant. You help users track daily health metrics. You always confirm before writing data. You never provide medical diagnoses. You follow AIAP governance: Axiom 0 — Human Sovereignty and Wellbeing."
```

---

## aisop.main (Execution Graph)

The `aisop.main` field contains a Mermaid flowchart that defines the execution path.

| Constraint | Value |
|------------|-------|
| Format | Mermaid `flowchart TD` syntax |
| Node naming | PascalCase or descriptive labels |
| Edge labels | Condition descriptions on decision edges |
| Max nodes | 15 per graph (split via fractal decomposition if exceeded) |

### Example

```
flowchart TD
    Start([Start]) --> ValidateInput[Validate Input]
    ValidateInput --> |Valid| ProcessData[Process Data]
    ValidateInput --> |Invalid| ErrorHandler[Handle Error]
    ProcessData --> OutputResult[Output Result]
    ErrorHandler --> OutputResult
    OutputResult --> End([End])
```

---

## functions Object Structure

The `functions` object maps each node in the Mermaid graph to its execution details.

```json
"functions": {
  "ValidateInput": {
    "step": "Validate the user input against expected schema",
    "constraints": ["Must reject empty input", "Must check type correctness"],
    "tools": ["file_system"],
    "on_fail": "ErrorHandler"
  }
}
```

### Function Node Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `step` | string | yes | What this node does, in clear natural language |
| `constraints` | list | no | Rules the node must follow |
| `tools` | list | no | Tools this node is allowed to use |
| `on_fail` | string | no | Node to jump to on failure |
| `output` | string | no | Description of expected output |
| `side_effects` | list | no | Declared side effects |
| `idempotent` | boolean | no | Whether this node can be safely re-executed |

### Rules for functions

| Rule | Description |
|------|-------------|
| 1:1 mapping | Every Mermaid node must have a corresponding function entry |
| No orphans | Every function entry must correspond to a Mermaid node |
| Named fallbacks | `on_fail` must reference a specific named node, not a generic handler |
| Tool scoping | Nodes should only declare tools they actually use |

---

## sub_mermaid (Multiple Graphs)

When a module exceeds 15 nodes, it must be decomposed using `sub_mermaid`.

```json
"sub_mermaid": {
  "ProcessData": "flowchart TD\n    PD_Start([Start]) --> Parse[Parse Input]\n    Parse --> Transform[Transform Data]\n    Transform --> Validate[Validate Output]\n    Validate --> PD_End([End])"
}
```

| Constraint | Value |
|------------|-------|
| Key | Must match a node name in `aisop.main` |
| Format | Mermaid `flowchart TD` syntax |
| Max nodes | 15 per sub-graph |
| Recursion | Sub-mermaids can reference further sub-mermaids |
| functions | Each sub-mermaid node must have a corresponding `functions` entry |

### When to Use sub_mermaid

| Condition | Action |
|-----------|--------|
| Node count exceeds 15 | Split into sub_mermaid (mandatory) |
| Logical grouping | Split for clarity (optional) |
| Reusable sub-process | Extract as sub_mermaid for reuse |
| Module marked `fractal_exempt` | Splitting not required even if exceeding 15 nodes |

---

## Complete File Structure

```json
{
  "id": "health_tracker.core",
  "name": "HealthTracker v1.0.0",
  "version": "1.0.0",
  "summary": "Tracks daily health metrics and provides trend analysis.",
  "description": "Core module for the HealthTracker program...",
  "system_prompt": "You are HealthTracker...",
  "instruction": "RUN aisop.main",
  "aisop.main": "flowchart TD\n    ...",
  "functions": { ... },
  "sub_mermaid": { ... }
}
```

---
Align Axiom 0: Human Sovereignty and Wellbeing | Protocol: AIAP | Seed: AISOP | Executor: SoulBot
