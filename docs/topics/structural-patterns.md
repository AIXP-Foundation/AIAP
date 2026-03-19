# Structural Patterns A through G

## Introduction

AIAP defines seven structural patterns (A-G) that govern how AI programs are organized
based on functional complexity. Each pattern prescribes a directory structure, file
layout, and architectural constraints. The patterns form a progression: as requirements
grow, the pattern advances accordingly.

---

## Pattern Selection

| Condition                                  | Pattern   |
|--------------------------------------------|-----------|
| Single task, 12 or fewer nodes             | Pattern A |
| Multiple user intents requiring routing    | Pattern B |
| Shared cross-module logic (OAuth, logging) | Pattern C |
| Nested sub-programs as self-contained units| Pattern D |
| State persistence across sessions          | Pattern E |
| Three or more AIAP components coordinated  | Pattern F |
| Embedded executable tool code              | Pattern G |

### Decision Flowchart

```
Single task, <= 12 nodes?
  YES --> Pattern A
  NO  --> Multiple intents?
            YES --> Shared cross-module concerns?
                      YES --> Pattern C
                      NO  --> Pattern B
            NO  --> Nested sub-programs needed?   YES --> Pattern D
                    Persistent state needed?      YES --> Pattern E
                    Coordinating 3+ components?   YES --> Pattern F
                    Embedding executable code?    YES --> Pattern G
```

Patterns are not mutually exclusive. Valid combinations include D+G, E+G, B+E, and
D+E+G. When combined, each pattern's constraints apply independently.

---

## Pattern A: Script

Single-purpose program with one execution graph, no more than twelve nodes.

```
my-program/
  AIAP.md
  main.aisop.json
```

Two files. `AIAP.md` is the manifest. `main.aisop.json` holds the execution graph and
all function definitions. No sub-directories, shared modules, or memory layers.

**Constraints**: Max 12 nodes. Exactly one AISOP file. Self-contained in two files.

**Use cases**: Unit converters, text summarizers, form validators, simple Q&A bots.

---

## Pattern B: Package

Introduces routing. A program handling multiple user intents uses a router that
classifies intent and dispatches to functional modules.

```
my-program/
  AIAP.md
  main.aisop.json
  faq.aisop.json
  returns.aisop.json
  order-status.aisop.json
```

`main.aisop.json` is the **NLU Router**. It classifies the user's intent and routes to
the appropriate module.

```
graph TD
  Start[Receive Input] --> Classify[Classify Intent]
  Classify -->|faq| FAQ[Route to FAQ]
  Classify -->|return| Returns[Route to Returns]
  Classify -->|order| Order[Route to Order Status]
  Classify -->|unknown| Fallback[Handle Unknown Intent]
```

**Critical Rule**: `main.aisop.json` in Pattern B and above MUST be a stateless intent
classifier. It MUST NOT contain business logic. Each intent has its own `.aisop.json`.
Individual modules follow Pattern A rules (up to 12 nodes).

---

## Pattern C: Package + Shared

Extends Pattern B with `shared.aisop.json` for cross-module logic. When multiple modules
duplicate the same concerns -- OAuth, logging, error formatting -- that logic is
extracted into a shared file.

```
my-program/
  AIAP.md
  main.aisop.json
  shared.aisop.json
  module-a.aisop.json
  module-b.aisop.json
```

Modules invoke shared logic via step references:

```json
{
  "Authenticate": {
    "step1": "RUN shared.oauth_refresh",
    "step2": "If token valid, proceed. If expired, follow 'error' edge."
  }
}
```

**Constraints**: All Pattern B rules apply. Shared logic MUST NOT be duplicated across
modules. `shared.aisop.json` is never an entry point. If no cross-module logic exists,
use Pattern B.

---

## Pattern D: Nested Package

Sub-components complex enough to be standalone programs are organized as nested packages
in sub-directories, each with its own `AIAP.md`.

```
aiap-creator/
  AIAP.md
  main.aisop.json
  research/
    AIAP.md
    main.aisop.json
    web-search.aisop.json
  generate/
    AIAP.md
    main.aisop.json
    template.aisop.json
  validate/
    AIAP.md
    main.aisop.json
    compliance-check.aisop.json
```

Each sub-directory is a complete AIAP program that can be developed, tested, and reused
independently. The parent orchestrates without knowing internal structure.

**Nesting limit**: Maximum **2 levels**. Programs needing deeper nesting should
restructure as Pattern F.

**Constraints**: All Pattern B rules apply per nested package. Each sub-directory needs
its own `AIAP.md`. Root router does not access sub-program internals.

---

## Pattern E: Package + Memory

Adds state persistence via a `memory/` directory with three layers.

```
my-program/
  AIAP.md
  main.aisop.json
  module-a.aisop.json
  memory/
    working/
    episodic/
    semantic/
```

**Working** (`memory/working/`) -- Session state: current input, intermediate results,
temporary flags. Cleared at session end.

**Episodic** (`memory/episodic/`) -- Conversation logs: timestamped interaction records.
Provides cross-session continuity.

**Semantic** (`memory/semantic/`) -- Extracted knowledge: facts, preferences, patterns
derived from episodic memory. Persists indefinitely subject to decay.

| Layer    | Persistence  | Decay Policy                          |
|----------|--------------|---------------------------------------|
| Working  | Session only | Cleared at session end                |
| Episodic | Time-bounded | Oldest entries pruned after threshold |
| Semantic | Indefinite   | Relevance-scored; low-relevance decays|

**Constraints**: All Pattern B rules apply. `memory/` needs at least one sub-directory.
Memory access goes through `aisop.memory`. Working memory does not persist unless
promoted.

---

## Pattern F: Ecosystem

Coordinates three or more independent AIAP components via `blueprint.json`. Analogous
to microservices architecture.

```
my-ecosystem/
  blueprint.json
  component-a/
    AIAP.md
    main.aisop.json
  component-b/
    AIAP.md
    main.aisop.json
  component-c/
    AIAP.md
    main.aisop.json
```

`blueprint.json` defines: component registry, inter-agent interfaces, data contracts
(input/output schemas), and orchestration rules. Components never call each other
directly -- all communication flows through defined interfaces, maintaining L0/L1
isolation across boundaries.

| Microservices Concept | Pattern F Equivalent     |
|-----------------------|--------------------------|
| Service               | AIAP component           |
| API contract          | Data contract in blueprint|
| Service registry      | Component registry        |
| API gateway           | Orchestration rules       |

**Constraints**: 3+ components required. `blueprint.json` at root. Each component is a
valid AIAP program. All communication via blueprint interfaces. No direct cross-component
file access.

---

## Pattern G: Embedded Runtime

Embeds executable code in `tool_dirs` directories for tasks beyond LLM capabilities:
API calls, computation, database access, external system interaction.

```
my-program/
  AIAP.md
  main.aisop.json
  python-tools/
    calculator.py
  ts-tools/
    formatter.ts
  mcp-tools/
    server.json
```

### Supported Tool Types

| Directory       | Runtime                | Use Cases                     |
|-----------------|------------------------|-------------------------------|
| `python-tools/` | Python                | Data processing, ML, APIs     |
| `ts-tools/`     | TypeScript            | Web APIs, transformation      |
| `mcp-tools/`    | MCP servers           | External service integration  |
| `a2a-tools/`    | A2A tools             | Inter-agent communication     |
| `go-tools/`     | Go                    | High-performance utilities    |
| `rust-tools/`   | Rust                  | Systems-level operations      |
| `shell-tools/`  | Shell scripts         | System automation             |
| `n8n-tools/`    | n8n workflows         | Visual workflow automation    |

### Code Trust Gate

Six-step verification every tool must pass before execution:

1. **Signature Check** -- Cryptographic signature verified against known public key.
2. **SHA-256 Hash Match** -- File hash must match the manifest declaration.
3. **Allowlist Verification** -- Tool must appear on the program's allowlist.
4. **Size Limit** -- File must not exceed configured maximum size.
5. **Sandbox Configuration** -- Tool declares required filesystem, network, and syscall
   permissions. Execution is isolated.
6. **Audit Log** -- Execution recorded: tool ID, hash, timestamp, inputs, outputs,
   sandbox config, duration, exit code.

**Constraints**: All tools pass the Code Trust Gate. Standard directory naming required.
Tool outputs returned as L0 JSON. Tools must not modify AISOP files at runtime.

---

## Pattern Combinations

| Notation | Meaning                                              |
|----------|------------------------------------------------------|
| D+G      | Nested sub-programs + embedded tools                  |
| E+G      | State persistence + embedded tools                    |
| B+E      | Multiple intents + session memory                     |
| D+E+G    | Nested sub-programs + memory + embedded tools         |
| F+G      | Ecosystem + embedded tools in components              |

**Rules**: All constituent constraints apply independently. Pattern A does not combine.
Pattern F absorbs lower patterns (components use any pattern internally). Modifier
notation is descriptive, not prescriptive.

---

## Node Counting Rules

| Count | Action                                                  |
|-------|---------------------------------------------------------|
| 1-12  | Within limits.                                           |
| 13-16 | **Advisory split.** Consider splitting.                  |
| 17+   | **Recommended split.** Too complex for a single graph.   |

Every named node counts. Start/end nodes count. Decision nodes count as one. Sub-graph
invocations do NOT count toward the calling graph. In Pattern B+, `main.aisop.json`
MUST NEVER perform business logic regardless of node count.

---

## Pattern Upgrade Path

**A to B** -- Rename `main.aisop.json`, create new router, add per-intent files, update
manifest.

**B to C** -- Identify duplicated logic, extract to `shared.aisop.json`, replace with
`RUN shared.<graph>` calls.

**B to D** -- Move module into sub-directory with own `AIAP.md`, update root router.

**B to E** -- Create `memory/` with sub-directories, add `aisop.memory` graph, define
decay policies.

**Any to F** -- Separate components into own directories, create `blueprint.json`,
define interfaces and data contracts.

**Any to G** -- Create `tool_dirs`, add signed tool files, configure allowlist and
sandbox, implement Code Trust Gate.

**Convergence**: When multiple upgrades apply, apply each independently. Result is the
combination (e.g., E+G). If constraints conflict, the stricter one wins.

---

## Summary

| Pattern | Name              | Key Feature                              |
|---------|-------------------|------------------------------------------|
| A       | Script            | Single file, single task, up to 12 nodes |
| B       | Package           | NLU router with functional modules       |
| C       | Package + Shared  | Cross-module shared logic                |
| D       | Nested Package    | Self-contained sub-programs              |
| E       | Package + Memory  | Three-layer persistent state             |
| F       | Ecosystem         | Multi-component coordination             |
| G       | Embedded Runtime  | Executable tool code with trust gate     |

Patterns combine to address compound requirements. Node counting guides complexity.
Upgrade paths provide clear migration steps. Through all patterns, Axiom 0 -- Human
Sovereignty and Wellbeing -- remains the immutable constraint.

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIAP V1.0.0. www.aiap.dev
