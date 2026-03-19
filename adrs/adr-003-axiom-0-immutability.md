# ADR-003: Axiom 0 (Human Sovereignty and Wellbeing) Is Immutable Across All Protocol Versions

## Status

Accepted

## Context

Most protocols and specifications allow any aspect to be changed, deprecated, or removed in a major version update. Semantic versioning conventions treat major version bumps as permission to introduce breaking changes. This means that a core principle established in V1 could theoretically be weakened or removed in V2.

For AIAP, the core alignment principle is **Axiom 0: Human Sovereignty and Wellbeing**. It states that every AIAP program must serve human interests and must never undermine human control. This principle is the foundation upon which all other protocol rules are built:

- Quality rules assume Axiom 0 compliance.
- Trust levels are defined relative to Axiom 0.
- Security guards exist to protect Axiom 0.
- Governance modes enforce Axiom 0 at different strictness levels.

If Axiom 0 could be modified in a future protocol version, the entire trust model would be undermined. Users, organizations, and ecosystems that rely on AIAP's alignment guarantee would have no assurance that future versions maintain the same commitment.

The question is whether AIAP should follow standard versioning conventions (anything can change in a major version) or introduce a novel constraint (certain principles are permanent and cannot be changed by any version).

## Decision

**Axiom 0 is immutable.** No version of AIAP --- past, present, or future --- may modify, weaken, redefine, or deprecate Axiom 0. This constraint applies to:

- Major versions (V2.0.0, V3.0.0, etc.)
- Protocol forks
- Extensions and supplements
- Any derivative specification that claims AIAP compliance

Specifically:

1. The field `axiom_0: Human_Sovereignty_and_Wellbeing` must be present in every `AIAP.md` governance contract.
2. The alignment seal `"Align Axiom 0: Human Sovereignty and Wellbeing."` must appear in every `system_prompt`.
3. Quality Regression Guard QRG5 enforces that Axiom 0 continuity is maintained across program versions.
4. Any future AIAP version that attempts to remove or weaken Axiom 0 is, by definition, not a valid AIAP version.

## Consequences

### What becomes easier

- **Absolute alignment guarantee**: Users and organizations can adopt AIAP with confidence that the alignment principle will never change. This is a stronger guarantee than any versioned protocol typically provides.
- **Trust chain integrity**: Because Axiom 0 is permanent, the entire trust model built on top of it is stable across versions. Trust levels, security guards, and governance modes do not need to be re-evaluated when the protocol is updated.
- **Ecosystem stability**: Registry operators, executor implementors, and program authors can build long-term infrastructure knowing that the foundational principle is fixed.
- **Regulatory alignment**: Axiom 0's immutability makes AIAP naturally compatible with AI safety regulations that require human oversight guarantees.

### What becomes harder

- **Protocol evolution**: If the collective understanding of AI alignment evolves significantly, AIAP cannot update Axiom 0 to reflect that evolution. The protocol must find ways to incorporate new alignment insights without changing the core axiom.
- **Edge case handling**: Some future scenarios may reveal tension between "human sovereignty" and "human benefit" (e.g., a human instructs an AI to act against other humans' interests). Axiom 0 as currently stated does not resolve such conflicts, and its immutability means the resolution must come from interpretation, not amendment.
- **Fork pressure**: If a significant portion of the community disagrees with Axiom 0's specific formulation, they cannot change it through normal protocol governance. Their only option is to create a non-AIAP specification, which fragments the ecosystem.
- **Philosophical rigidity**: Axiom 0 encodes a specific philosophical position (human-centric alignment). If the discourse shifts toward broader frameworks (sentient-centric, ecosystem-centric), AIAP cannot adapt its foundational axiom.

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIAP V1.0.0. www.aiap.dev
