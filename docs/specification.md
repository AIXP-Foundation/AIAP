# AIAP Protocol Specification

The complete AIAP V1.0.0 protocol specification is maintained in the [`specification/`](../specification/) directory.

---

## Normative Specification

- **[AIAP Protocol Specification (English)](../specification/AIAP_Protocol.md)** --- The authoritative normative specification
- **[AIAP Protocol Specification (Chinese)](../specification/AIAP_Protocol_cn.md)** --- Chinese translation

---

## Quality Standards

The quality validation rules are defined in four AIAP_Standard files:

| Standard File | Scope |
|---------------|-------|
| [AIAP_Standard.core](../specification/standards/AIAP_Standard.core.aisop.json) | Core quality rules (C1-C7, I1-I7, D1-D7) |
| [AIAP_Standard.security](../specification/standards/AIAP_Standard.security.aisop.json) | Security extension (I8-I11, D8-D10, AT1-AT6) |
| [AIAP_Standard.ecosystem](../specification/standards/AIAP_Standard.ecosystem.aisop.json) | Ecosystem extension (MF10-MF14, K1-K5, Categories F-M) |
| [AIAP_Standard.performance](../specification/standards/AIAP_Standard.performance.aisop.json) | Performance extension (PL13-PL15, QRG1-QRG5) |

These four files together define the complete ThreeDimTest quality framework. Programs are evaluated against these rules to produce Correctness, Intrinsic, and Detail dimension scores.

---

## Proto Definition

The [aiap.proto](../specification/aiap.proto) file provides the proto3 definition of AIAP data structures and service operations. It can be used to generate typed client and server code in any language supported by Protocol Buffers.

---

> Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIAP V1.0.0. www.aiap.dev
