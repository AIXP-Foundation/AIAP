# AIAP.md Field Reference

> Complete field reference for the `AIAP.md` governance contract file.
> Every AIAP program directory must contain exactly one `AIAP.md` at its root.

---

## Required Governance Fields (6)

These fields establish protocol identity and runtime authority. All six are mandatory.

| Field | Type | Description | Example | Validation |
|-------|------|-------------|---------|------------|
| `protocol` | string | AIAP version identifier | `"AIAP V1.0.0"` | Must match `AIAP V\d+\.\d+\.\d+` |
| `authority` | string | Governance domain | `aiap.dev` | Must be `aiap.dev` |
| `seed` | string | Format specification domain | `aisop.dev` | Must be `aisop.dev` |
| `executor` | string | Runtime execution domain | `soulbot.dev` | Must be `soulbot.dev` |
| `axiom_0` | string | Core immutable axiom | `Human_Sovereignty_and_Benefit` | Exact match required |
| `governance_mode` | string | Operating mode | `NORMAL` | `NORMAL` or `DEV` |

### Governance Mode Values

| Mode | Description | Constraints |
|------|-------------|-------------|
| `NORMAL` | Production governance | All quality rules enforced |
| `DEV` | Development governance | Relaxed pipeline rules, warnings instead of failures |

---

## Required Project Fields (6)

These fields describe the project itself. All six are mandatory.

| Field | Type | Description | Example | Validation |
|-------|------|-------------|---------|------------|
| `name` | string | Project name in snake_case | `health_tracker` | `[a-z][a-z0-9_]*` |
| `version` | string | Semantic version | `"1.0.0"` | Must match `\d+\.\d+\.\d+` |
| `pattern` | string | Structural pattern identifier | `A` | One of `A`, `B`, `C`, `D`, `E`, `F`, `G`, `D+G` |
| `summary` | string | 1-2 sentence project overview | `"Personal health tracking application"` | Max 300 characters |
| `tools` | list or object | Tool declarations | See Tools section below | At least one tool required |
| `modules` | list | Module inventory | See Modules section below | At least one module required |

### Pattern Reference

| Pattern | Name | Description |
|---------|------|-------------|
| `A` | Single file | One `.aisop.json`, no sub-modules |
| `B` | Multi-module | One `.aisop.json` with multiple logical modules |
| `C` | Multi-file | Multiple `.aisop.json` files, shared context |
| `D` | Orchestrated | Central orchestrator dispatching to sub-programs |
| `E` | Event-driven | Reactive modules triggered by events |
| `F` | Pipeline | Sequential processing chain |
| `G` | Embedded code | Includes executable code artifacts |
| `D+G` | Orchestrated + Code | Combines orchestration with embedded code |

---

## Optional Fields

### Basic Optional

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `governance_hash` | string | null | SHA-256 hash of all `.aisop.json` files for integrity verification |
| `quality` | object | null | Quality assessment: `{weighted_score, grade, last_pipeline}` |
| `tags` | list | [] | Classification tags for discovery |
| `author` | string | null | Author name or organization |
| `license` | string | null | SPDX license identifier (e.g., `MIT`, `Apache-2.0`) |
| `copyright` | string | null | Copyright notice |
| `identity` | object | null | Program identity: `{program_id, publisher, verified_on}` |
| `capabilities` | object | null | Capability declarations: `{offered: [], required: []}` |
| `tool_dirs` | list | [] | Pattern G tool directory declarations |
| `ui` | object | null | UI component declarations |

### Quality Object Structure

| Sub-field | Type | Description |
|-----------|------|-------------|
| `weighted_score` | number | 0.0-1.0 composite quality score |
| `grade` | string | Letter grade: `A+`, `A`, `B+`, `B`, `C`, `D`, `F` |
| `last_pipeline` | string | ISO 8601 timestamp of last quality pipeline run |

### Identity Object Structure

| Sub-field | Type | Description |
|-----------|------|-------------|
| `program_id` | string | Unique program identifier (UUID or namespaced) |
| `publisher` | string | Publisher name or domain |
| `verified_on` | string | ISO 8601 verification date |

### Security and Runtime Optional

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `trust_level` | object | `{level: 3}` | Trust classification with justification |
| `permissions` | object | null | Permission boundaries |
| `runtime` | object | null | Runtime constraints |

#### Trust Level Object

| Sub-field | Type | Description |
|-----------|------|-------------|
| `level` | integer | 1 (sandbox) to 4 (full access) |
| `justification` | string | Why this trust level is needed |
| `constraints` | list | Specific restrictions applied |

#### Permissions Object

| Sub-field | Type | Description |
|-----------|------|-------------|
| `file_system` | object | `{read: [], write: [], deny: []}` path patterns |
| `network` | object | `{allow: [], deny: []}` domain patterns |
| `shell` | object | `{allow: [], deny: []}` command patterns |

#### Runtime Object

| Sub-field | Type | Description |
|-----------|------|-------------|
| `timeout` | integer | Maximum execution time in seconds |
| `retries` | integer | Maximum retry attempts per module |
| `token_budget` | integer | Maximum token consumption per execution |

### Engineering Optional

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `status` | string | `"draft"` | Lifecycle state: `draft`, `active`, `deprecated`, `archived` |
| `deprecated_date` | string | null | ISO 8601 deprecation date |
| `successor` | string | null | Replacement program name or ID |
| `applicability_condition` | object | null | Conditional activation rules |
| `intent_examples` | list | [] | Semantic routing anchor phrases |
| `discovery_keywords` | list | [] | Keyword index for search |
| `dependencies` | list | [] | Cross-project dependency declarations |
| `min_protocol_version` | string | null | Minimum required AIAP version |
| `benchmark` | object | null | Quality benchmark targets |

#### Applicability Condition Object

| Sub-field | Type | Description |
|-----------|------|-------------|
| `triggers` | list | Conditions that activate this program |
| `preconditions` | list | Requirements that must be met before execution |
| `exclusions` | list | Conditions that prevent activation |
| `confidence_threshold` | number | 0.0-1.0 minimum confidence for activation |

---

## Tools Field Formats

### Compact Format (List)

```yaml
tools: [file_system, shell, browser]
```

Use when tools require no configuration and defaults are acceptable.

### Structured Format (Object)

```yaml
tools:
  - name: file_system
    required: true
    min_version: "1.0"
    fallback: null
    annotations:
      read_only: false
      destructive: false
      idempotent: true
```

#### Structured Tool Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | yes | Tool identifier |
| `required` | boolean | no | Whether the tool is mandatory (default: `true`) |
| `min_version` | string | no | Minimum tool version |
| `fallback` | string | no | Named fallback tool if unavailable |
| `annotations` | object | no | Tool behavior annotations |

#### Tool Annotation Fields

| Field | Type | Description |
|-------|------|-------------|
| `read_only` | boolean | Tool performs no mutations |
| `destructive` | boolean | Tool may cause irreversible changes |
| `idempotent` | boolean | Repeated calls produce same result |
| `open_world` | boolean | Tool accesses external systems |

---

## Modules Field Format

```yaml
modules:
  - id: core_logic
    file: core_logic.aisop.json
    nodes: 8
    critical: true
    idempotent: false
    side_effects: [file_write, api_call]
```

### Module Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | yes | Unique module identifier (snake_case) |
| `file` | string | yes | Path to `.aisop.json` file |
| `nodes` | integer | yes | Number of execution nodes in the module |
| `critical` | boolean | yes | If `true`, failure is FATAL; if `false`, DEGRADABLE |
| `idempotent` | boolean | yes | Whether re-execution is safe |
| `side_effects` | list | no | Declared side effects (e.g., `file_write`, `api_call`, `db_mutation`) |

---

## Markdown Body Sections

The AIAP.md file body uses standard Markdown with specific required and optional sections.

### Required Sections

| Section | Purpose |
|---------|---------|
| Governance Declaration | YAML front matter with all required fields |
| Feature Overview | Bullet list of what the program does |
| Usage | How to invoke and interact with the program |

### Recommended Sections

| Section | Purpose |
|---------|---------|
| Example Interactions | Sample user/agent exchanges |
| Applicable Conditions | When this program should be activated |

### Optional Sections

| Section | Purpose |
|---------|---------|
| Data Storage | Where and how data is persisted |
| Configuration | Configurable parameters and defaults |
| Quality Status | Current quality score and grade |

---

## Closing Seal

Every `AIAP.md` must end with the closing seal:

```
---
Align: Human Sovereignty and Benefit | Protocol: AIAP | Seed: AISOP | Executor: SoulBot
```

The seal is a machine-readable and human-readable marker that confirms the document adheres to the AIAP governance framework. Omitting the seal is a D2 (Format Completeness) violation.

---
Align: Human Sovereignty and Benefit | Protocol: AIAP | Seed: AISOP | Executor: SoulBot
