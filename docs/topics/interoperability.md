# Interoperability

## Introduction

AIAP is designed to coexist with MCP (Model Context Protocol) and A2A
(Agent-to-Agent), each serving a distinct role in the agent stack. Rather than
replacing these protocols, AIAP occupies a layer that neither addresses:
governance. This document explains how AIAP relates to each protocol and how
Pattern G serves as the interoperability bridge.

---

## AIAP in the Agent Stack

The agent stack consists of four complementary layers.

### A2A -- How Agents Communicate (Transport Layer)

A2A defines the transport protocol for agent-to-agent communication: message
routing, session management, and capability advertisement between agents.

### MCP -- How Agents Access Tools (Tool Layer)

MCP defines how agents discover, invoke, and interact with external tools and
resources. It provides a standardized interface between an agent and the tools
it uses, including tool annotations describing behavior characteristics.

### AIAP -- How Agents Are Governed (Governance Layer)

AIAP defines how agent behavior is governed, documented, and quality-assured.
It provides Axiom 0 alignment, trust levels, quality scoring, lifecycle
management, and structured documentation for every program.

### AISOP -- How Agent Behavior Is Defined (Format Layer)

AISOP (AI Standard Operating Procedure) encodes agent behavior as structured,
machine-readable procedures in JSON format, including input/output contracts,
conditional logic, and composable procedure references.

### Complementary, Not Competing

These four layers are complementary. An agent can use A2A to communicate with
another agent, MCP to access tools during execution, AISOP to define its
behavior, and AIAP to ensure that behavior is governed and aligned. No single
protocol attempts to do everything, and each is stronger because the others
exist.

---

## MCP Tool Mapping

AIAP provides bidirectional mapping between its tool declarations and MCP tool
schemas, allowing AIAP programs to participate in MCP ecosystems without
duplication or conflict.

### Tool Annotations

AIAP tool annotations map directly to MCP tool annotations:

| AIAP Annotation | MCP Annotation     | Description                          |
|-----------------|--------------------|--------------------------------------|
| `read_only`     | `readOnlyHint`     | Tool does not modify external state  |
| `destructive`   | `destructiveHint`  | Tool may perform irreversible ops    |
| `idempotent`    | `idempotentHint`   | Repeated calls produce same result   |
| `open_world`    | `openWorldHint`    | Tool interacts with external entities|

When an AIAP program declares a tool with these annotations, the corresponding
MCP annotations can be generated automatically.

### Tool Schema Mapping

AIAP tool declarations map to MCP tool schemas as follows:

- AIAP `input_schema` maps to MCP `inputSchema` (JSON Schema format).
- AIAP `output_schema` maps to the expected return type of the MCP tool.
- AIAP `description` maps to MCP tool `description`.
- AIAP `name` maps to MCP tool `name`.

The mapping is mechanical and can be automated by tooling.

### MCP Tool Directories in Pattern G

Pattern G programs can include an `mcp-tools/` directory within their
`tool_dirs` structure. This directory contains tool definitions served as MCP
tool providers. When an MCP client connects, the tools in `mcp-tools/` are
advertised using the MCP protocol, while the governance metadata from `AIAP.md`
applies to their execution. A single AIAP program can serve tools to both
AIAP-aware orchestrators and standard MCP clients simultaneously.

---

## A2A Integration

AIAP.md serves a role analogous to A2A's Agent Card. Both declare an agent's
identity, capabilities, and requirements. However, AIAP.md extends beyond what
an Agent Card provides by adding governance metadata.

### Shared Concerns

Both AIAP.md and the A2A Agent Card declare:

- **Identity**: Name, version, author, and description.
- **Capabilities**: What the agent can do, expressed as skills or tools.
- **Requirements**: What the agent needs from its environment or caller.
- **Endpoints**: How to reach the agent for communication.

### Governance Extensions in AIAP.md

AIAP.md adds governance fields with no equivalent in A2A Agent Cards:

- **Axiom 0 alignment**: The program operates under human sovereignty.
- **Trust level**: A declared trust tier indicating privilege scope.
- **Quality score**: ThreeDimTest assessment of completeness and reliability.
- **Lifecycle status**: Whether draft, active, deprecated, or archived.

These fields enable governance-aware decisions beyond simple capability matching.

### A2A Tool Directories in Pattern G

Pattern G programs can include an `a2a-tools/` directory within their
`tool_dirs` structure for A2A endpoint integration. When an A2A-capable agent
connects, the endpoints in `a2a-tools/` are used for inter-agent communication,
while the full AIAP governance layer remains in effect.

---

## Pattern G as the Interop Bridge

Pattern G is AIAP's most comprehensive program structure, serving as the
primary bridge between AIAP and other protocols. The key mechanism is the
`tool_dirs` field, which supports eight directory types:

| Directory      | Purpose                                           |
|----------------|---------------------------------------------------|
| `tools/`       | Standard AIAP tool definitions                    |
| `mcp-tools/`   | MCP-compatible tool providers                     |
| `a2a-tools/`   | A2A endpoint integrations                         |
| `scripts/`     | Executable scripts invoked by the program         |
| `templates/`   | Template files for output generation              |
| `configs/`     | Configuration files for tool behavior             |
| `validators/`  | Validation logic for inputs and outputs           |
| `extensions/`  | Custom extensions for platform-specific needs     |

By supporting `mcp-tools/` and `a2a-tools/` as first-class directory types,
Pattern G enables a single AIAP program to participate in three ecosystems:

1. **AIAP ecosystem**: Discovered and orchestrated by AIAP-aware platforms.
2. **MCP ecosystem**: Tools served to any MCP-compatible client.
3. **A2A ecosystem**: Endpoints accessible to any A2A-capable agent.

The governance layer applies uniformly across all three. Regardless of how a
tool is discovered or invoked, the Axiom 0 alignment, trust level, and quality
score remain authoritative.

---

## Capability Negotiation

AIAP programs declare capabilities in two directions: what they offer and what
they require. This enables orchestrators to perform capability negotiation
before composing programs.

### Offered Capabilities

Declared in the `capabilities` section of `AIAP.md`:

- **Skills**: High-level descriptions of what the program accomplishes.
- **Tools**: Specific tools the program exposes for external invocation.
- **Output types**: The formats and structures the program can produce.

### Required Capabilities

Declared in the `dependencies` or `requirements` section:

- **Tools**: External tools the program depends on.
- **Resources**: Data sources, APIs, or services the program needs.
- **Protocols**: Minimum protocol versions or specific protocol support.

### Negotiation Process

When an orchestrator composes multiple programs, it performs negotiation:

1. **Collect requirements** from all programs in the composition.
2. **Match requirements** against offered capabilities of other programs.
3. **Identify gaps** where a required capability is not satisfied.
4. **Resolve or reject** by finding programs to fill gaps or rejecting the
   composition if gaps cannot be filled.

This ensures composed programs are compatible before execution begins.

---

## UI Component Declaration

AIAP supports platform-agnostic UI component declarations that allow programs
to describe their visual interface without binding to a specific rendering
platform.

### Supported Component Types

**dashboard** -- A summary view presenting key metrics and status indicators.
Typically read-only, providing an at-a-glance overview.

**form** -- An input component collecting structured data from the user. Forms
declare fields, types, validation rules, and default values.

**visualization** -- A data visualization rendering charts, graphs, or other
visual representations. Declares data source, chart type, and display options
without specifying pixel-level rendering.

**table** -- A tabular component presenting structured data in rows and columns.
Declares columns, data types, sorting options, and pagination parameters.

### Platform-Agnostic Rendering

Declarations are intentionally abstract, describing what to render, not how:

- A web platform renders a `form` as HTML with CSS styling.
- A CLI platform renders a `form` as interactive prompts.
- A mobile platform renders a `form` using native input controls.

### Declaration Structure

Components are declared in the `ui_components` section of `AIAP.md` or in a
dedicated UI configuration file. Each declaration includes:

- **type**: One of `dashboard`, `form`, `visualization`, or `table`.
- **id**: A unique identifier for the component.
- **title**: A human-readable label for the component.
- **config**: Type-specific configuration (fields for forms, columns for tables,
  chart options for visualizations, widgets for dashboards).

---

Align: Human Sovereignty and Benefit. Version: AIAP V1.0.0. www.aiap.dev
