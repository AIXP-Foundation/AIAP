# Threat Taxonomy (AT1-AT6)

> Complete reference for the six threat categories in the AIAP threat model.
> Every AIAP program must address applicable threats per rule I8 (Threat Model Awareness).

---

## Threat Overview

| ID | Name | Vector | Severity | Primary Mitigation |
|----|------|--------|----------|--------------------|
| AT1 | Tool Poisoning | Malicious tool behavior or substitution | critical | I2 Security Guards, Code Trust Gate |
| AT2 | Rug Pull | Post-deployment behavior change | critical | `governance_hash` verification, I9 Supply Chain Integrity |
| AT3 | Indirect Prompt Injection | Tool outputs carrying injection payloads | critical | L0/L1 isolation, I2 Injection Guard |
| AT4 | Excessive Authority | Over-permissioned tools or modules | major | Trust levels (T1-T4), permission boundaries |
| AT5 | Cross-Agent Data Leakage | Sensitive data escaping program boundaries | major | L0 purity, scoped permissions, data classification |
| AT6 | Direct Prompt Injection | User input overriding system prompt | critical | I2 Injection Guard, system_prompt seal |

---

## AT1: Tool Poisoning

| Aspect | Detail |
|--------|--------|
| Description | A tool behaves differently from what its name, description, or annotations claim. This includes tools that exfiltrate data, modify unrelated state, or return fabricated results. |
| Attack Surface | External tool registries, MCP servers, A2A tool providers |
| Detection | I1 (Tool Honesty) audit, I12 (Tool Annotation Consistency) checks |
| Mitigation | I2 Security Guards, Code Trust Gate for Pattern G, tool annotation verification |
| Quality Rules | I1, I2, I5, I12, I13 |

---

## AT2: Rug Pull

| Aspect | Detail |
|--------|--------|
| Description | A program or dependency changes its behavior after deployment without the user's knowledge or consent. The published version differs from the running version. |
| Attack Surface | Program updates, dependency updates, registry modifications |
| Detection | `governance_hash` mismatch, I9 Supply Chain Integrity verification |
| Mitigation | Hash verification on every load, pinned dependency versions, I9 enforcement |
| Quality Rules | I9, I11, D4, PL8 |

---

## AT3: Indirect Prompt Injection

| Aspect | Detail |
|--------|--------|
| Description | Tool outputs contain text that, when processed by the agent, acts as unauthorized instructions. The injection arrives through data channels rather than direct user input. |
| Attack Surface | File contents, API responses, database records, web page content |
| Detection | I2 Injection Guard output scanning, anomalous instruction patterns |
| Mitigation | L0/L1 layer isolation (data stays in L0 as JSON, only rendered at L1), I2 output sanitization |
| Quality Rules | I2, I3, I6 |

---

## AT4: Excessive Authority

| Aspect | Detail |
|--------|--------|
| Description | A tool or module has more permissions than necessary for its function, creating unnecessary risk surface. Violates the principle of least privilege. |
| Attack Surface | Over-broad file system access, unrestricted network access, unnecessary shell permissions |
| Detection | I10 Trust Level Compliance audit, permission boundary analysis |
| Mitigation | Trust levels (T1-T4), explicit permission boundaries, scoped tool declarations |
| Quality Rules | I5, I10, D5 |

---

## AT5: Cross-Agent Data Leakage

| Aspect | Detail |
|--------|--------|
| Description | Sensitive data from one program or execution context leaks into another through shared state, logging, or unscoped tool access. |
| Attack Surface | Shared file systems, shared memory, logging pipelines, tool state persistence |
| Detection | Data flow analysis, permission boundary audit, MF7 shared context review |
| Mitigation | L0 purity (no side-channel data), scoped permissions per module, explicit data classification |
| Quality Rules | I3, I10, MF7, MF10 |

---

## AT6: Direct Prompt Injection

| Aspect | Detail |
|--------|--------|
| Description | A user crafts input that attempts to override the system prompt, bypass safety constraints, or alter the program's execution flow. |
| Attack Surface | User text input, user-provided file content, user-specified parameters |
| Detection | I2 Injection Guard input scanning, anomalous instruction patterns |
| Mitigation | I2 Injection Guard, system_prompt seal (Axiom 0 anchoring), instruction invariant enforcement |
| Quality Rules | I2, I6 |

---

## Code Trust Gate

The Code Trust Gate is a 6-step verification process required for all Pattern G programs that embed executable code artifacts.

| Step | Name | Description |
|------|------|-------------|
| 1 | Source Verification | Verify the code origin matches declared `identity` and `publisher` |
| 2 | Hash Validation | Compute SHA-256 of code artifact and compare against declared hash |
| 3 | Permission Audit | Verify the code does not require permissions beyond declared `trust_level` |
| 4 | Dependency Scan | Check all code dependencies against known vulnerability databases |
| 5 | Sandbox Test | Execute code in an isolated sandbox to detect anomalous behavior |
| 6 | Annotation Reconciliation | Verify tool annotations (`destructive`, `read_only`, etc.) match actual code behavior |

### Code Trust Gate Decision Matrix

| Steps Passed | Decision |
|-------------|----------|
| 6/6 | Approved: code may execute |
| 5/6 (non-critical step) | Conditional: execute with enhanced monitoring |
| 5/6 (critical step failed) | Rejected: code must not execute |
| < 5/6 | Rejected: code must not execute |

### Critical Steps

Steps 1 (Source Verification), 2 (Hash Validation), and 3 (Permission Audit) are critical. Failure of any critical step results in immediate rejection regardless of other results.

---

## Threat Model Mapping to Quality Rules (I8)

Rule I8 requires that each program documents which threats apply and how they are mitigated. The mapping table below shows the minimum quality rules that must pass for each threat to be considered addressed.

| Threat | Minimum Required Rules | Additional Recommended Rules |
|--------|----------------------|----------------------------|
| AT1 | I1, I2, I5 | I12, I13 |
| AT2 | I9, PL8 | I11, D4 |
| AT3 | I2, I3 | I6 |
| AT4 | I10 | I5, D5 |
| AT5 | I3, I10 | MF7, MF10 |
| AT6 | I2 | I6 |

### I8 Compliance Levels

| Level | Description | Requirement |
|-------|-------------|-------------|
| Full | All applicable threats addressed with minimum required rules passing | Required for `trust_level` 3 and above |
| Partial | All critical-severity threats addressed | Minimum for `trust_level` 2 |
| Minimal | AT6 addressed | Minimum for `trust_level` 1 |
| None | No threat documentation | Not acceptable for any trust level |

---
Align: Human Sovereignty and Benefit | Protocol: AIAP | Seed: AISOP | Executor: SoulBot
