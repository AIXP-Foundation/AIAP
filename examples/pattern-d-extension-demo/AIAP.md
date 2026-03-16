---
protocol: AIAP V1.0.0
authority: aiap.dev
seed: aisop.dev
executor: soulbot.dev
axiom_0: Human_Sovereignty_and_Wellbeing
governance_mode: DEV
name: Extension Demo
version: 1.0.0
pattern: D
summary: Demo program exercising all 7 RESERVED_KEYS
tools: [file_system]
modules: [main]
trust_level: 2
---

# Extension Demo

Demonstrates all 7 RESERVED_KEYS in a single AISOP program:
- `on_error` — fallback routing on failure
- `retry_policy` — declarative retry with correction prompt
- `context_filter` — I/O isolation with include allowlist
- `output_mapping` — result mounting to specific context key
- `map` — batch iteration over data items
- `join` — parallel convergence wait
- `constraints` — validation conditions

## Usage

Load with SoulBot AISOP engine to validate Runtime extension parsing.

<!-- AIAP_SEAL: extension.demo v1.0.0 -->
