# ADR-002: Use .aisop.json as the Program Format

## Status

Accepted

## Context

AI programs need a machine-readable format that defines their behavior. Several options were considered:

- **YAML**: Human-friendly, widely used for configuration. However, YAML has well-known parsing ambiguities (the Norway problem, implicit type coercion) and multiple valid representations for the same data.
- **TOML**: Simpler than YAML, but limited support for deeply nested structures. Not widely supported by LLMs for generation.
- **Custom DSL**: Maximum expressiveness, but requires a custom parser, has no existing tooling, and creates a learning barrier.
- **JSON**: Universally supported, unambiguous parsing, excellent LLM compatibility (models can generate and validate JSON natively), and extensive tooling in every programming language.

The format also needed to support:
- Execution graphs (how the program flows from step to step).
- Function definitions (what each step does).
- Constraints and guards (security and validation rules).
- Metadata (protocol version, program identity, alignment).

## Decision

AIAP uses **`.aisop.json`** as its program format. The format follows this structure:

1. **Top-level `role`/`content` wrapper**: Compatible with the system message format used by LLMs, making AIAP programs directly usable as structured system prompts.
2. **Mermaid execution graphs**: Stored as strings in the `aisop` object. Mermaid syntax is widely known, human-readable, and renderable by common tools (GitHub, VS Code, documentation platforms).
3. **Step-based functions**: Each graph node maps to a function in the `functions` object. Functions contain numbered steps (`step1`, `step2`, ...) that describe actions in natural language.
4. **Inline constraints**: Security guards, edge conditions, and fallbacks are declared directly within function definitions.

The `.aisop.json` extension signals that the file is an AI Standard Operating Procedure formatted as JSON, distinguishing it from generic JSON configuration files.

## Consequences

### What becomes easier

- **Machine parsing**: JSON parsers exist in every language. No custom parser needed.
- **LLM compatibility**: Large language models can generate, validate, and execute `.aisop.json` files natively. The `role`/`content` structure aligns with how LLMs process system messages.
- **Version control**: JSON files produce clean diffs in Git, making program changes trackable and reviewable.
- **Tooling**: JSON Schema can validate structure. Existing JSON editors, linters, and formatters work out of the box.
- **Determinism**: JSON has exactly one parsing interpretation. No ambiguity.
- **Graph rendering**: Mermaid graphs can be rendered in GitHub Markdown, MkDocs, VS Code, and many other tools without any AIAP-specific tooling.

### What becomes harder

- **Human editing**: JSON is more verbose than YAML. Writing and reading large `.aisop.json` files requires more scrolling. No comments are allowed in standard JSON.
- **Mermaid limitations**: Complex execution graphs with many branches can produce dense Mermaid strings. Very large graphs may be hard to read as inline strings.
- **Natural language steps**: Because function steps are natural language strings, there is an inherent ambiguity in what "step1: Parse input" means at the implementation level. AIAP mitigates this through constraints and guards, but the steps themselves remain semantic rather than executable.
