# Program Lifecycle Management

## Introduction

AIAP programs follow a defined lifecycle from creation to archival. Every program
exists in one of four discrete states, and transitions between those states are
governed by explicit rules. This ensures programs are created deliberately,
validated before production use, retired gracefully, and preserved for reference.

---

## Lifecycle States

Every AIAP program declares its current state in the `status` field of both
`AIAP.md` and `main.aisop.json`. The four states are:

### draft

Initial creation, under development, not yet validated.

- The program structure may be incomplete.
- Fields may contain placeholder values.
- Quality scores have not been established.
- The program MUST NOT be referenced by production orchestrators.

### active

Validated, quality-gated, ready for production use.

- All required fields are populated with final values.
- The ThreeDimTest quality assessment has been completed.
- The program can be discovered, composed, and executed by orchestrators.
- Governance metadata (Axiom 0 alignment, trust level) is authoritative.

### deprecated

Marked for retirement, successor identified, migration period active.

- The program remains functional during the migration period.
- The `deprecated_date` field indicates when deprecation began.
- The `successor` field points to the replacement program.
- Orchestrators SHOULD begin migrating to the successor.
- New integrations MUST NOT target a deprecated program.

### archived

Fully retired, read-only, preserved for reference.

- The program MUST NOT be executed in production.
- All code and documentation are preserved as-is.
- The program serves as a reference for its successor and for auditing.

---

## State Transitions

Each transition has preconditions that must be satisfied before the program can
move to the next state.

**draft to active** -- Requires a passing ThreeDimTest assessment across three
dimensions:

- **Completeness**: All required fields, documentation, and examples are present.
- **Reliability**: The program produces consistent, correct outputs.
- **Governance**: Axiom 0 alignment is verified and trust metadata is sound.

**active to deprecated** -- Requires two preconditions:

1. A **successor** program must be identified and referenced.
2. A **deprecated_date** must be set, marking the start of the migration period.

The successor SHOULD already be in the `active` state before deprecation.

**deprecated to archived** -- Occurs after the migration period has elapsed.
Consumers have had time to transition to the successor. Once the period ends,
the program moves to `archived` and becomes read-only.

---

## Deprecation Protocol

When retiring a program, authors MUST follow this protocol:

1. **Set the status** to `deprecated` in both `AIAP.md` and `main.aisop.json`.
2. **Identify the successor** by populating the `successor` field with the
   identifier of the replacement program.
3. **Generate a migration guide** documenting differences between the current
   program and its successor, including input/output changes, behavioral
   differences, and any breaking changes.
4. **Set the deprecated_date** to the current date, marking the start of the
   grace period.
5. **Notify consumers** through available channels that the program is entering
   its retirement phase.

The grace period provides a window during which both the deprecated program and
its successor are operational. The recommended minimum grace period is 90 days,
though this may vary depending on the program's usage scope.

---

## Versioning

AIAP uses semantic versioning following the **Major.Minor.Patch** convention.

### Program Version

Each AIAP program carries its own version number (e.g., `1.2.3`):

- **Major**: Incremented for breaking changes to inputs, outputs, or behavior.
- **Minor**: Incremented for backward-compatible feature additions.
- **Patch**: Incremented for backward-compatible fixes and corrections.

The program version MUST be synchronized between `AIAP.md` and `main.aisop.json`.
When one is updated, the other MUST be updated to match.

### Protocol Version

The protocol version (e.g., `AIAP V1.0.0`) is separate from the program version.
It describes which version of the AIAP specification the program conforms to.
A single protocol version can govern programs at many different program versions.

---

## Orchestration Patterns

AIAP programs are designed to compose. Orchestrators combine programs using four
primary patterns:

**Sequential** -- Programs execute in a defined order. The output of one program
feeds into the input of the next.

**Parallel** -- Programs execute concurrently and their results are aggregated.
Appropriate when programs are independent and require no coordination.

**Conditional** -- Programs are selected based on intent analysis. The
orchestrator examines the request and routes to the most appropriate program.

**Handoff** -- A program delegates execution to another program mid-task,
identifying that another program is better suited for a portion of the work.

---

## Version Compatibility

AIAP programs declare compatibility requirements using the
`min_protocol_version` field, specifying the minimum AIAP protocol version
the program requires. Guarantees follow semantic versioning:

- **Patch-level** updates are always backward-compatible.
- **Minor-level** updates add features but do not break existing programs.
- **Major-level** updates may introduce breaking changes.

Orchestrators MUST verify that `min_protocol_version` is satisfied before
executing a program. If the orchestrator's protocol version is lower than the
requirement, execution MUST be rejected.

---

## Documentation Completeness Levels

AIAP defines three levels of documentation completeness.

### Level 1 -- Minimum

An `AIAP.md` file with all required fields: program name, version, status,
description, author, input/output specifications, and Axiom 0 alignment.

### Level 2 -- Recommended

Level 1 plus usage examples, an applicability section, and ThreeDimTest quality
scores. Level 2 programs are significantly easier for orchestrators and human
operators to evaluate and trust.

### Level 3 -- Complete

Level 2 plus a full reference section, benchmark data, and migration guides.
Level 3 represents the gold standard for AIAP program documentation.

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIAP V1.0.0. www.aiap.dev
