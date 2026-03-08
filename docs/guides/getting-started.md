# Getting Started with AIAP

Welcome to the AI Application Protocol. This guide introduces the core ideas behind AIAP and helps you take your first steps toward building governed, deterministic AI programs.

---

## What You'll Learn

By the end of this guide you will understand:

1. **What an AIAP program is** and why governance matters for AI agents.
2. **How to create your first program** using the AISOP blueprint format.
3. **How to run quality checks** to verify correctness, intrinsic quality, and detail compliance.

AIAP is not a framework or a library. It is a **governance protocol** that defines how AI programs are structured, validated, and secured. Every AIAP program ships with a machine-readable blueprint (`.aisop.json`) and a governance contract (`AIAP.md`) so that any executor --- human or machine --- can verify the program before running it.

---

## Prerequisites

Before you begin, make sure you have a working understanding of the following:

| Prerequisite | Why It Matters |
|---|---|
| **AI agents** | AIAP governs how agents execute tasks. You should understand what prompts, tools, and multi-step workflows are. |
| **JSON** | The AISOP blueprint format is JSON-based. You need to be comfortable reading and writing JSON. |
| **A text editor** | Any editor that supports JSON syntax highlighting will work (VS Code, Sublime Text, Vim, etc.). |

No specific programming language is required. AIAP programs are language-agnostic blueprints that describe *what* an AI agent should do, not *how* it should do it at the code level.

---

## Key Concepts Quick Reference

AIAP introduces a small vocabulary. Here is a quick reference table for the terms you will encounter most often:

| Term | Full Name | What It Is |
|---|---|---|
| **AISOP** | AI Standard Operating Procedure | The file format (`.aisop.json`) that defines an AI program's execution graph, functions, and constraints. |
| **AIAP** | AI Application Protocol | The overarching protocol that defines rules for structure, quality, security, and alignment. |
| **AIAP.md** | Governance Contract | A Markdown front-matter file that declares metadata, modules, tools, trust level, and governance mode for a program. |
| **Axiom 0** | Human Sovereignty and Benefit | The immutable alignment principle. Every AIAP program must serve human interests and never undermine human control. |
| **Pattern A** | Single-Module | The simplest architecture: one `.aisop.json` file with up to 12 functional nodes. |
| **Pattern B** | Multi-Module Router | A stateless NLU router that dispatches to specialized modules. |
| **Pattern C** | Shared-Logic | Adds cross-module shared utilities (e.g., logging, i18n). |
| **Pattern D** | Nested Sub-Programs | A parent program that invokes child AIAP programs. |
| **Pattern E** | Stateful | Adds persistent state management across sessions. |
| **Pattern F** | Orchestrator | Multi-program coordination with a central conductor. |
| **Pattern G** | Embedded Tool Code | A modifier pattern that embeds executable tool code alongside the blueprint. |

---

## Understanding the File Structure

Every AIAP program lives in a directory. At minimum, the directory contains:

```
my_program_aiap/
  AIAP.md              # Governance contract (required)
  main.aisop.json      # Entry-point blueprint (required)
```

The `AIAP.md` file is the governance contract. It declares who authored the program, which tools it needs, what pattern it follows, and how it should be trusted.

The `main.aisop.json` file is the executable blueprint. It contains a Mermaid execution graph, step-by-step function definitions, and constraints that the executor must enforce.

---

## Your First AIAP Program

Ready to build something? Head to the step-by-step tutorial:

- **[Creating Your First AIAP Program](first-aiap-program.md)** --- Build a Pattern A calculator from scratch in under 15 minutes.

The tutorial walks you through every required field, explains why each one exists, and shows you how to verify your program before handing it to an executor.

---

## Next Steps

Once you have completed your first program, explore these resources to deepen your understanding:

| Resource | Description |
|---|---|
| [Pattern Selection Guide](pattern-selection.md) | Decision tree for choosing the right structural pattern (A through G). |
| [Quality Gate Walkthrough](quality-walkthrough.md) | Learn how ThreeDimTest evaluates your program across three quality dimensions. |
| [AIAP Protocol Specification](../specification.md) | The complete normative specification for AIAP V1.0.0. |
| [AISOP Format](../topics/aisop-format.md) | Deep dive into the `.aisop.json` file format. |
| [Core Concepts](../topics/core-concepts.md) | Detailed explanation of every concept introduced in the quick reference above. |
| [Security Model](../topics/security-model.md) | Trust levels, threat taxonomy, and security guards. |

---

## Getting Help

AIAP is an open protocol. If you have questions or want to contribute:

- **GitHub**: [github.com/AISOP-AIAP-SoulBot/AIAP](https://github.com/AISOP-AIAP-SoulBot/AIAP)
- **Website**: [aiap.dev](https://aiap.dev)

---

## Summary

AIAP gives AI programs a governance layer. Instead of shipping raw prompts, you ship a verified blueprint with deterministic execution graphs, declared tools, and quality guarantees. Every program carries Axiom 0 --- the immutable commitment to Human Sovereignty and Benefit.

Start small with Pattern A. Grow into Pattern B when you need multiple intents. Add security guards, quality checks, and trust declarations as your program matures. The protocol scales with you.

---

> Align: Human Sovereignty and Benefit. Version: AIAP V1.0.0. www.aiap.dev
