# ADR-001: AIAP Is a Governance Protocol, Not a Transport Protocol

## Status

Accepted

## Context

When designing AIAP, a fundamental architectural decision needed to be made about the protocol's scope. The AI ecosystem already includes protocols that address specific layers of the stack:

- **A2A (Agent-to-Agent)** handles agent communication --- how agents discover, authenticate, and exchange messages with each other.
- **MCP (Model Context Protocol)** handles tool access --- how agents discover and invoke external tools and data sources.

AIAP could have attempted to define its own agent communication layer or its own tool access protocol. Doing so would have created overlap and fragmentation in the ecosystem. Alternatively, AIAP could focus exclusively on the governance layer --- how AI programs are structured, validated, and secured --- and complement the existing protocols rather than compete with them.

## Decision

AIAP is a **governance protocol**. It does not define agent communication (that is A2A's responsibility) and it does not define tool access (that is MCP's responsibility). Instead, AIAP governs:

1. **Program structure** --- How AI programs are organized into modules, execution graphs, and functions (AISOP format, Patterns A-G).
2. **Quality validation** --- How programs are evaluated for correctness, intrinsic quality, and detail compliance (ThreeDimTest, AIAP_Standard rules).
3. **Security** --- How programs declare trust levels, enforce guards, and mitigate threats (Trust Levels 0-5, Threat Taxonomy).
4. **Alignment** --- How programs maintain immutable commitment to human sovereignty (Axiom 0).

AIAP complements A2A and MCP. An AIAP program can participate in the A2A ecosystem (as an agent that communicates with other agents) and the MCP ecosystem (as a consumer of tools) through Pattern G interoperability.

## Consequences

### What becomes easier

- **Ecosystem integration**: AIAP programs can adopt A2A for communication and MCP for tools without conflict. There is no protocol collision.
- **Separation of concerns**: Teams can focus on governance (AIAP), communication (A2A), and tooling (MCP) independently.
- **Adoption**: Organizations already using A2A or MCP can adopt AIAP without replacing their existing infrastructure.
- **Pattern G interop**: The Pattern G modifier provides a clean integration point for embedding A2A and MCP adapters within AIAP programs.

### What becomes harder

- **Standalone operation**: An AIAP program that needs communication or tool access must rely on external protocols. AIAP alone does not provide these capabilities.
- **Consistency enforcement**: AIAP can govern program structure but cannot enforce runtime communication patterns. An AIAP program might be perfectly structured but use A2A incorrectly --- AIAP has no authority over the transport layer.
- **Dependency on external standards**: AIAP's interoperability story depends on A2A and MCP continuing to evolve in compatible directions.
