---
protocol: "AIAP V1.0.0"
authority: aiap.dev
seed: aisop.dev
executor: soulbot.dev
axiom_0: Human_Sovereignty_and_Wellbeing
governance_mode: NORMAL
name: calculator
version: "1.0.0"
pattern: A
summary: "Simple calculator — performs basic arithmetic operations."
tools: []
modules:
  - id: calculator.main
    file: main.aisop.json
    nodes: 5
    critical: true
    idempotent: true
    side_effects: []
status: active
license: Apache-2.0
copyright: "Copyright 2026 AISOP.dev | AIAP.dev | SoulBot.dev"
---

## Governance Declaration

Calculator is a minimal AIAP program demonstrating Pattern A. It follows AIAP V1.0.0 with Axiom 0 alignment.

## Feature Overview

| Module | Responsibility |
|--------|---------------|
| main.aisop.json | Parse input, perform calculation, format output |

## Usage

Entry file: `main.aisop.json`
Tools: None required (pure computation)

Align: Human Sovereignty and Wellbeing. Version: AIAP V1.0.0. www.aiap.dev
