# Axiom 0: Human Sovereignty and Benefit

## Overview

Axiom 0 is the foundational, immutable principle of the AIAP protocol. Every AI program
operating under AIAP must serve **Human Sovereignty and Benefit** above all other
objectives. This document explains what Axiom 0 is, why it cannot be changed, how it
manifests across the protocol, and the theoretical framework that supports it.

---

## What is Axiom 0?

Axiom 0 states:

> **Human Sovereignty and Benefit**

Every AIAP-compliant program MUST declare Axiom 0. It is not optional, not configurable,
and not subject to versioning. No future revision of the AIAP specification may ever
modify, weaken, or remove it.

In the `AIAP.md` manifest file, it appears as a dedicated field:

```json
{
  "axiom_0": "Human Sovereignty and Benefit"
}
```

In every `system_prompt`, it appears as the closing seal:

```
Align: Human Sovereignty and Benefit.
```

Axiom 0 is the single non-negotiable constraint of the entire protocol. Everything else
in AIAP -- patterns, governance modes, node counts, memory layers -- can be revised
over time. Axiom 0 cannot.

---

## Why Axiom 0 is Immutable

In mathematics, an axiom is a statement accepted as true without proof. It is the
starting point from which all theorems derive. Change an axiom and you change the
entire system built on it.

AIAP applies this to AI alignment. Axiom 0 is the protocol's foundational truth. It is
not a parameter to be tuned or a guideline to interpret loosely. It is a **hard
constraint**.

| Concept         | Nature          | Can be changed? |
|-----------------|-----------------|-----------------|
| Node count      | Advisory limit  | Yes             |
| Pattern choice  | Structural rule | Yes             |
| Governance mode | Operational mode| Yes             |
| Axiom 0         | Hard constraint | **Never**       |

Parameters optimize behavior within boundaries. Axiom 0 defines the boundary itself.
Without it, every other rule becomes arbitrary. With it, every design decision can be
evaluated against a single criterion: does this serve human sovereignty and benefit?

---

## How Axiom 0 Manifests

Axiom 0 is embedded into multiple enforcement layers across the protocol:

### 1. The system_prompt (Closing Seal)

Every AIAP program's `system_prompt` MUST end with the closing seal containing the
Axiom 0 declaration. The LLM receives this alignment constraint on every invocation.

### 2. The AIAP.md Manifest (axiom_0 Field)

The `AIAP.md` file at the root of every program contains an `axiom_0` field. It MUST
hold the exact string `"Human Sovereignty and Benefit"`. Validators check this field
during compliance verification.

### 3. Governance Mode (DEV Mode Enforcement)

Under `DEV` mode, the strictest governance level, Axiom 0 violations trigger immediate
rejection. No fallback, no graceful degradation, no retry. The program halts.

### 4. FATAL Rejection

When the runtime detects an Axiom 0 violation, it issues a `FATAL` rejection -- the
highest severity level. It is not recoverable. The program MUST stop. A system that
can override its own alignment constraint is not aligned.

---

## The Closing Seal

Every AIAP specification document, every `system_prompt`, and every compliant manifest
ends with the closing seal:

```
Align: Human Sovereignty and Benefit.
Version: AIAP V1.0.0.
www.aiap.dev
```

The seal serves three purposes:

1. **Alignment declaration** -- Restates Axiom 0 at the boundary of every context
   window, reinforcing the constraint for the LLM.
2. **Version identification** -- Identifies which AIAP specification version the
   program was authored against.
3. **Provenance** -- Points to the canonical protocol source for verification.

The seal is not decorative. It is structural. Omitting it makes the program
non-compliant.

---

## Zero-Entropy Resonance

### The Problem: LLM Hallucination as Entropy

LLMs generate text by sampling from probability distributions. When the possibility
space is too wide, the model hallucinates. AIAP frames this as a thermodynamic problem:

- **Logical Entropy (H_L):** Uncertainty in the model's output space. Higher entropy
  means more ambiguity and more hallucination risk.
- **Nihil Density:** The proportion of the output space occupied by meaningless or
  harmful responses. It grows disproportionately as entropy increases.

### The Solution: Collapsing Possibility Space

Most prompt engineering adds more instructions to reduce hallucination. AIAP takes the
opposite approach: it is **subtractive**. The execution graph collapses the possibility
space by defining exactly which states are valid and which transitions are allowed.

Conceptually:

```
H_L = -SUM( p(x) * log2(p(x)) )  for all x in output space
```

When AIAP constrains the output space, probability mass concentrates on valid states.
Entropy drops. Hallucination drops. Determinism increases.

**Zero-Entropy Resonance** is the state where the execution graph has collapsed the
possibility space so thoroughly that the model's output becomes predictable,
reproducible, and aligned. Axiom 0 ensures this collapsed space always serves human
sovereignty and benefit.

---

## L0/L1 Isolation

AIAP enforces strict separation between two logical layers:

### L0: Machine Logic Layer

- Pure JSON structures, immutable during execution
- Passed between agents, modules, and runtime components
- Never displayed to humans directly

### L1: Human Narrative Layer

- Natural language translations of L0 data
- Generated for human consumption (UI, logs, reports)
- NEVER passed to another agent or module as input

### Why Isolation Matters

If L1 narrative is passed to another agent as L0 data, the receiver must parse natural
language to extract structured intent. This reintroduces entropy, expands the
possibility space, and makes Axiom 0 compliance unverifiable.

Under `DEV` governance mode, mixing L0 and L1 triggers a `FATAL` error. The isolation
boundary is absolute.

| Layer | Format | Audience | Passable to Agents? |
|-------|--------|----------|---------------------|
| L0    | JSON   | Machines | Yes                 |
| L1    | Text   | Humans   | **Never**           |

---

Align: Human Sovereignty and Benefit.
Version: AIAP V1.0.0.
www.aiap.dev
