# Discovery and Registry

> AIAP Protocol — Conceptual Guide
> Classification: Infrastructure

---

## Introduction

AIAP programs do not exist in isolation. They are discovered, resolved, composed, and distributed across environments. The AIAP discovery and registry system defines how programs are found — whether they reside on a local file system, are indexed through semantic search, or are published to a federated registry.

Discovery is the mechanism by which an agent (or a human operator) locates an AIAP program that satisfies a specific need. Registry is the mechanism by which AIAP programs are published, versioned, and made available for discovery. Together, they form the distribution layer of the AIAP ecosystem.

---

## Discovery Layers

AIAP defines three discovery layers, ordered by increasing scope and complexity. Each layer builds on the previous one.

### L1: Passive Discovery

**File system scan for `*_aiap/` directories and `AIAP.md` files.**

The simplest form of discovery. An agent or tool scans the local file system for directories matching the `*_aiap/` naming convention and for `AIAP.md` files within those directories. No network access is required. No index is consulted.

Mechanism:
- Scan the current workspace or a declared root directory.
- Identify directories whose names end with `_aiap/`.
- Within each matching directory, locate the `AIAP.md` file.
- Parse the `AIAP.md` to extract program metadata (name, version, description, trust level).

Use cases:
- Local development environments where programs are co-located.
- Monorepo structures where multiple AIAP programs reside in a single repository.
- Offline environments without network access.

Limitations:
- Only discovers programs present on the local file system.
- No semantic matching — discovery is purely structural (naming convention).
- Scales linearly with file system size.

### L2: Semantic Discovery

**Intent matching, keyword indexing, tag-based search.**

Semantic discovery goes beyond file system structure to match programs based on their declared capabilities and intent.

Mechanism:
- **intent_examples**: Each AIAP program declares a set of example intents — natural language descriptions of what the program does. During discovery, a query intent is compared against these examples using semantic similarity.
- **discovery_keywords**: Each program declares a set of keywords that describe its domain, capabilities, and purpose. These keywords are indexed and searchable.
- **Tag-based search**: Programs are tagged with standardized category tags (e.g., `code-review`, `documentation`, `deployment`). Tags enable categorical filtering before semantic matching.

Example metadata in AIAP.md:
```
intent_examples:
  - "Review Python code for security vulnerabilities"
  - "Analyze source files for injection risks"
  - "Audit codebase security posture"

discovery_keywords:
  - security
  - code-review
  - vulnerability
  - python

tags:
  - code-review
  - security-analysis
```

Use cases:
- Finding the right program when you know what you need but not which program provides it.
- Agent-to-agent discovery where one agent needs to locate a capability provided by another program.
- Workspace-level program recommendation.

Limitations:
- Requires a local index to be built and maintained.
- Semantic similarity depends on the quality of intent_examples declarations.
- Does not discover programs outside the indexed scope.

### L3: Registry Discovery

**Federated registry query, timeout policies, capability negotiation.**

Registry discovery enables programs to be found across organizational and network boundaries through a federated registry system.

Mechanism:
- **Registry query**: An agent sends a discovery query to one or more registry endpoints. The query may include intent descriptions, keyword filters, tag filters, version constraints, and trust level requirements.
- **Federated resolution**: If the primary registry does not contain a matching program, the query may be forwarded to federated registries (other organizations, public registries) according to federation policies.
- **Timeout policies**: Every registry query must have a declared timeout. If the registry does not respond within the timeout, discovery falls back to L2 or L1.
- **Capability negotiation**: When multiple programs match a query, the registry returns a ranked list based on capability match, trust level, version recency, and quality score (ThreeDimTest grade).

Use cases:
- Discovering programs published by other teams or organizations.
- Locating specialized capabilities not available locally.
- Automated dependency resolution for programs with declared dependencies.

Limitations:
- Requires network access.
- Depends on registry availability and responsiveness.
- Federated queries introduce latency and trust considerations.

---

## The AIAP.md as Discovery Entry Point

The `AIAP.md` file serves as the program's "Agent Card" equivalent — a standardized, human-readable and machine-parseable document that describes the program's identity, capabilities, and requirements.

### Required Fields

| Field | Description |
|---|---|
| `name` | The program's unique identifier |
| `version` | Semantic version (major.minor.patch) |
| `description` | Brief description of what the program does |
| `trust_level` | Declared trust level (T1-T4) |
| `intent_examples` | Natural language descriptions of the program's purpose |
| `discovery_keywords` | Searchable keywords |
| `tags` | Standardized category tags |
| `author` | Program author or organization |
| `license` | License under which the program is distributed |
| `tools` | List of tools the program requires |
| `permissions` | Permission boundaries (file_system, shell, network) |
| `quality_grade` | Most recent ThreeDimTest grade |
| `governance_hash` | SHA-256 integrity hash |

### Role in Discovery

At each discovery layer, the AIAP.md serves a different purpose:
- **L1 (Passive)**: The AIAP.md is the file that L1 discovery scans for. Its presence in a `*_aiap/` directory marks that directory as an AIAP program.
- **L2 (Semantic)**: The AIAP.md's `intent_examples`, `discovery_keywords`, and `tags` fields are indexed for semantic matching.
- **L3 (Registry)**: The AIAP.md's metadata is published to the registry and used for query matching and capability negotiation.

---

## Dependency Resolution

AIAP programs may declare dependencies on other AIAP programs. When a program references another program, the dependency must be resolved before execution.

### Version Constraints

Dependencies are declared with version constraints using semantic versioning:

```json
{
  "dependencies": [
    {
      "name": "code-security-scanner",
      "version": ">=2.0.0 <3.0.0"
    },
    {
      "name": "json-validator",
      "version": "^1.5.0"
    }
  ]
}
```

- **Exact**: `"1.2.3"` — only this version.
- **Range**: `">=2.0.0 <3.0.0"` — any version in the range.
- **Compatible**: `"^1.5.0"` — any version compatible with 1.5.0 (same major version).

### Conflict Detection

When two dependencies require incompatible versions of the same program, a conflict exists. The resolver detects conflicts before execution and reports them to the developer.

Example conflict:
```
Program A requires json-validator >=2.0.0
Program B requires json-validator <2.0.0
```

This conflict is irreconcilable — no single version satisfies both constraints.

### Resolution Strategies

1. **Newest compatible**: Select the newest version that satisfies all constraints. This is the default strategy.
2. **Locked**: Use the exact versions recorded in a lock file. This ensures reproducible builds.
3. **Manual**: Present the conflict to the developer and require explicit resolution.

Unresolvable conflicts halt the resolution process. A program with unresolvable dependency conflicts cannot be compiled or executed.

---

## AIAP Packaging (.aiap)

AIAP programs are distributed as `.aiap` archive files — self-contained packages that include everything needed to compile and execute the program.

### Archive Structure

```
program_name.aiap
  manifest.json          # Package metadata and file listing
  AIAP.md                # Discovery entry point
  program.aisop.json     # Primary program definition
  sub_programs/          # Sub-graph definitions (if any)
    sub_a.aisop.json
    sub_b.aisop.json
  tools/                 # Embedded tools (if any)
    tool_a.py
    tool_a.py.sig        # Cryptographic signature
  schemas/               # JSON schemas for validation
    input_schema.json
    output_schema.json
```

### manifest.json

The manifest declares the package contents and their integrity hashes:

```json
{
  "name": "program_name",
  "version": "1.0.0",
  "files": [
    {
      "path": "AIAP.md",
      "sha256": "a3f2b8c9..."
    },
    {
      "path": "program.aisop.json",
      "sha256": "7d1e4f6a..."
    },
    {
      "path": "tools/tool_a.py",
      "sha256": "b2c4d6e8..."
    }
  ],
  "governance_hash": "f1a2b3c4...",
  "trust_level": "T3",
  "quality_grade": "A"
}
```

### Code Trust Gate for Embedded Tools

Any executable code included in the `.aiap` archive must pass the Code Trust Gate (Pattern G) before it can be executed:

1. **Signature check**: The tool's `.sig` file must contain a valid signature from a trusted signer.
2. **SHA-256 match**: The tool's hash must match the hash declared in `manifest.json`.
3. **Allowlist**: The tool must appear on the program's declared tool allowlist.
4. **Size limit**: The tool must not exceed the declared maximum tool size.
5. **Sandbox config**: The tool's sandbox requirements must be declared and enforced.
6. **Audit log**: Every execution of the tool must be logged.

Packages containing tools that fail any Code Trust Gate step are rejected during installation.

---

## Registry Protocol

The AIAP registry protocol defines how programs are published, updated, and discovered through remote registries.

### Publishing

To publish a program to a registry:

1. **Package**: Create the `.aiap` archive with all required files and a valid manifest.
2. **Compile**: Run the Semantic Compiler and ensure the program passes all checks.
3. **Sign**: Sign the package with the publisher's cryptographic key.
4. **Submit**: Upload the signed package to the registry endpoint.
5. **Verify**: The registry independently verifies the package signature, governance hash, and manifest integrity.
6. **Index**: Upon successful verification, the registry indexes the program's metadata for discovery.

### Updating

Version updates follow the same publishing flow with additional checks:

- The new version must have a higher semantic version number than the current version.
- Quality Regression Guards (QRG1-QRG5) are evaluated against the previous version.
- If any QRG check fails, the update is rejected.

### Discovery Queries

Registries accept discovery queries with the following parameters:

| Parameter | Description |
|---|---|
| `intent` | Natural language description of desired capability |
| `keywords` | Keyword filter list |
| `tags` | Tag filter list |
| `version` | Version constraint expression |
| `trust_level` | Minimum trust level requirement |
| `quality_grade` | Minimum quality grade requirement |
| `author` | Author or organization filter |

The registry returns a ranked list of matching programs, ordered by relevance, quality grade, and version recency.

### Federation

Registries may federate with other registries to expand discovery scope. Federation is governed by bilateral agreements that specify:

- Which program categories are shared
- What trust level requirements apply to federated programs
- What quality grade minimums are enforced
- How queries are routed and how results are merged

Federated queries carry a provenance chain indicating which registries contributed to the results, enabling trust decisions based on program origin.

---

```
AIAP_SEAL: AIAP/1.0
Document: discovery-and-registry
Type: Conceptual Guide
Axiom_0: Human Sovereignty and Wellbeing
Status: ACTIVE
```
