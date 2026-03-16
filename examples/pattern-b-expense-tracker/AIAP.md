---
protocol: "AIAP V1.0.0"
authority: aiap.dev
seed: aisop.dev
executor: soulbot.dev
axiom_0: Human_Sovereignty_and_Wellbeing
governance_mode: NORMAL
name: expense_tracker
version: "1.0.0"
pattern: B
summary: "Personal expense tracker — records expenses and generates spending reports."
tools:
  - name: file_system
    required: true
    annotations:
      read_only: false
      destructive: false
      idempotent: false
      open_world: false
modules:
  - id: expense_tracker.main
    file: main.aisop.json
    nodes: 5
    critical: true
    idempotent: true
    side_effects: []
  - id: expense_tracker.record
    file: record.aisop.json
    nodes: 7
    critical: true
    idempotent: false
    side_effects: [file_write]
  - id: expense_tracker.query
    file: query.aisop.json
    nodes: 6
    critical: false
    idempotent: true
    side_effects: []
trust_level:
  level: 3
  justification: "Requires file_system read/write for expense data persistence."
  constraints:
    - "file_system write scope limited to ./data/"
permissions:
  file_system:
    scope: "./data/"
    operations: ["read", "write"]
status: active
license: Apache-2.0
copyright: "Copyright 2026 AISOP.dev | AIAP.dev | SoulBot.dev"
---

## Governance Declaration

Expense Tracker follows AIAP V1.0.0 with Axiom 0 alignment. Financial data privacy is a core design principle.

## Feature Overview

| Module | Responsibility | Tools |
|--------|---------------|-------|
| main.aisop.json | Intent classification router | None |
| record.aisop.json | Record new expenses | file_system |
| query.aisop.json | Query and report on expenses | file_system |

## Usage

Entry file: `main.aisop.json` --- stateless NLU router.
Tools: `file_system` (required) for data persistence.

Align: Human Sovereignty and Wellbeing. Version: AIAP V1.0.0. www.aiap.dev
