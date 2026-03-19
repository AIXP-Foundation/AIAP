# The AISOP File Format

## Introduction

AISOP (AI Standard Operating Procedure) is the underlying language format of the AIAP
protocol. Every AIAP program is composed of one or more AISOP files. These files use
the `.aisop.json` extension and define the complete behavior of an AI program: its
identity, its execution graph, its step-by-step logic, and its alignment constraints.

An AISOP file is not a prompt. It is a **program specification** written in JSON that
an LLM executes deterministically by following a Mermaid flowchart and its associated
function definitions.

---

## File Structure

An AISOP file is a JSON object with two top-level fields: `role` and `content`. The
`role` field is always `"system"`. The `content` object contains the full program
specification.

### Minimal Example

```json
{
  "role": "system",
  "content": {
    "id": "my_program.main",
    "name": "My Program v1.0.0",
    "version": "1.0.0",
    "summary": "Brief capability description",
    "description": "Detailed architecture description",
    "system_prompt": "Role description. Domain guidelines. Mirror User's exact language and script variant. Align Axiom 0: Human Sovereignty and Wellbeing.",
    "instruction": "RUN aisop.main",
    "aisop": {
      "main": "graph TD\n  Start[Input] --> Process[Process] --> End[Output]"
    },
    "functions": {
      "Start": { "step1": "Validate input" },
      "Process": { "step1": "Execute logic" },
      "End": { "step1": "Return result" }
    }
  }
}
```

This defines a three-node program with a linear execution path: receive input, process
it, return a result. Every node in the `aisop.main` graph has a corresponding entry in
the `functions` object.

### The role Field

The `role` field MUST be `"system"`. This tells the LLM runtime that the content is a
system-level instruction set. AISOP files are always injected at the system level.

### The content Object

The `content` object contains all program logic and metadata. Every field within it has
a specific responsibility, documented in the field reference below.

---

## Field Reference

| Field           | Type   | Required | Responsibility                                      |
|-----------------|--------|----------|-----------------------------------------------------|
| `id`            | string | Yes      | Unique identifier. Format: `program.module` (e.g., `my_app.main`). Used for cross-module references and logging. |
| `name`          | string | Yes      | Human-readable name, typically including the version (e.g., `"My Program v1.0.0"`). |
| `version`       | string | Yes      | Semantic version (`MAJOR.MINOR.PATCH`). Must follow SemVer. |
| `summary`       | string | Yes      | One-line description of the program's capability. |
| `description`   | string | Yes      | Detailed description of architecture, design decisions, and operational context. |
| `system_prompt` | string | Yes      | Core behavioral instruction. Defines role, domain guidelines, language mirroring. MUST end with `"Align Axiom 0: Human Sovereignty and Wellbeing."` |
| `instruction`   | string | Yes      | Entry point command. Typically `"RUN aisop.main"`. |
| `aisop`         | object | Yes      | Contains one or more named Mermaid flowchart graphs. `main` is always required. |
| `aisop.main`    | string | Yes      | Primary execution graph in Mermaid `graph TD` syntax. |
| `functions`     | object | Yes      | Maps each node name to step-by-step logic, constraints, and edge conditions. |

All fields are required. Omitting any makes the program non-compliant. The
`system_prompt` MUST contain the Axiom 0 closing seal; without it, the file fails
compliance validation.

---

## The Execution Graph

The `aisop.main` field contains a Mermaid flowchart defining the program's deterministic
execution path. This is the most important structural element.

### Syntax

```
graph TD
  Start[Receive Input] --> Validate[Validate Input]
  Validate -->|valid| Process[Process Data]
  Validate -->|invalid| Error[Return Error]
  Process --> Format[Format Output]
  Format --> End[Return Result]
```

### Nodes

Each node has a name and a label: `Start[Receive Input]`. The **name** (`Start`) maps
to the `functions` object. The **label** (`Receive Input`) is a human-readable
description.

### Edges

- `Validate --> Process` -- Unconditional transition.
- `Validate -->|valid| Process` -- Conditional: taken when "valid" is met.
- `Validate -->|invalid| Error` -- Alternative path for "invalid" condition.

### Determinism

The graph is deterministic. Given the same input and state, the program MUST follow the
same path every time. The graph does not merely guide the LLM -- it **constrains** it.
At each node, the model executes only the logic in the corresponding `functions` entry.
At each edge, it evaluates only the defined conditions. There is no room for
improvisation.

---

## The Functions Object

The `functions` object maps each node name to an object containing step-by-step logic.

### Structure

```json
{
  "Validate": {
    "step1": "Check that input is a non-empty string",
    "step2": "Verify input length does not exceed 1000 characters",
    "step3": "Confirm input contains no prohibited patterns",
    "constraints": "Input must be UTF-8 encoded text only",
    "edge_condition": "If all checks pass, follow 'valid' edge. Otherwise, follow 'invalid' edge."
  }
}
```

- **Steps**: Key-value pairs executed in order. Keys are step identifiers (`step1`,
  `step2`), values are natural language instructions.
- **Constraints**: Invariants the node must never violate, regardless of input.
- **Edge conditions**: Logic for selecting the outgoing edge. Required when a node has
  multiple outgoing edges. Must cover all possible cases.

### Completeness Requirement

Every node in `aisop.main` MUST have a corresponding `functions` entry. A node without
a function definition is a structural error. Orphaned function entries (no matching
node) indicate dead code and should be removed.

---

## sub_mermaid: Multiple Execution Graphs

An AISOP file can contain multiple named graphs within the `aisop` object.

| Graph Key          | Purpose                                              |
|--------------------|------------------------------------------------------|
| `aisop.main`       | Primary execution flow. Always required.              |
| `aisop.orchestrate`| Coordination logic for multiple modules.              |
| `aisop.memory`     | Memory management: read, write, and decay state.      |

### Example

```json
{
  "aisop": {
    "main": "graph TD\n  A[Start] --> B[Process] --> C[End]",
    "orchestrate": "graph TD\n  D[Receive Task] --> E[Route] --> F[Dispatch]",
    "memory": "graph TD\n  G[Load State] --> H[Merge] --> I[Persist]"
  }
}
```

The entry point is always `aisop.main` (via `"instruction": "RUN aisop.main"`). Other
graphs execute only when explicitly invoked by a node's step logic (e.g.,
`"step3": "RUN aisop.memory"`) or by the orchestration flow. Sub-graphs follow the
same rules: every node needs a `functions` entry, edges must be deterministic.

---

## Mermaid as Circuit Diagram

This is the key design insight behind the AISOP format.

### The Analogy

A Mermaid flowchart functions like a **circuit diagram** in electrical engineering:

- **Nodes** are components (resistors, logic gates).
- **Edges** are wires connecting components.
- **The graph** is the circuit layout.
- **The functions object** provides component parameters.

In a circuit diagram, layout (topology) is separate from component values (parameters).
You can change a resistor's value without rewiring the circuit. AISOP applies the same
principle:

| Concern              | AISOP Element     | Circuit Equivalent    |
|----------------------|-------------------|-----------------------|
| Execution topology   | `aisop.main`      | Circuit layout        |
| Node behavior        | `functions`       | Component parameters  |
| Behavioral context   | `system_prompt`   | Operating environment |

### Reproducibility

This separation makes AISOP programs reproducible. Two different LLMs given the same
file follow the same execution path because the path is defined by the graph, not by
the model's interpretation of prose. This is fundamentally different from prompt
engineering, where control flow, business logic, and context are mixed into a single
natural language blob.

### Deterministic Paths, Contextual Execution

The graph defines **what happens**. The functions define **how it happens**. The
system_prompt defines **in what context** it happens. A program can be ported to a
different domain by changing `system_prompt` and `functions` while keeping the same
graph. The execution flow can be restructured by modifying the graph without touching
node logic. This modularity is impossible when everything is a prompt.

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIAP V1.0.0. www.aiap.dev
