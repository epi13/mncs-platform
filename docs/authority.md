# Authority and scope

## Purpose

`mncs-platform` is the authoritative home for **foundational machine capability contracts** and their implementation/conformance requirements. Its role is to isolate platform-specific mechanics while preserving the distinct authorities of MNCS Language, Compiler, Environment, and other services.

Owning a contract does **not** imply owning the work performed with the capability, the semantic objects stored through it, or a central coordinator for each operation.

## Owned by Platform

| Concern | Platform responsibility |
| --- | --- |
| Capability definition | Define platform-neutral operation semantics, guarantees, limitations, and failure cases |
| Provider interfaces | Specify what an implementation must offer; keep OS-specific translation within the provider |
| Discovery | Report capabilities, versions, constraints, and unsupported operations truthfully |
| Conformance | Exercise independent providers against the same evidence-bearing contract tests |
| Portability | Identify and eliminate avoidable host assumptions in foundational mechanisms |
| Mechanism diagnostics | Report actionable provider-level evidence without overriding Environment policy |

## Owned elsewhere

| Authority | Retained responsibility |
| --- | --- |
| MNCS Language | Meaning and semantics of MNCS programs |
| MNCS Compiler / VM | Compilation, translation, and language execution behavior within their own contracts |
| MNCS Environment | Self-management, capability selection policy, reconciliation, recovery decisions, context, and projections |
| Domain services | Their authoritative state, identities, contracts, provenance, and business/domain operations |
| Rights & Provenance | Rights policy, provenance semantics, and related governance |

Platform may expose underlying isolation or cryptographic primitives without defining policy about **who should receive** a capability. Environment and other authorities may request, delegate, or revoke access subject to their own policies; the enforcing substrate must still implement the corresponding guarantees.

## Boundary tests

An operation belongs in Platform when it answers **how to obtain or use a machine capability**, and is meaningfully implementable across different substrates.

An operation belongs in Environment when it answers **why, when, for whom, or with what coordination/recovery policy** a capability should be used.

A projection belongs with the consumer-facing authority when it answers **what interface or representation should be shown**; Platform may supply the mechanism necessary to realize it.

Examples:

- Scheduling policy for agent work: Environment. Starting and isolating a workload: Platform mechanism.
- Project object identity and dependency graph: domain/environment authority. Durable write and consistency primitive: Platform.
- Human dashboard or terminal experience: projection owner. Underlying stream and execution primitives: Platform.
- Linux guest-visible virtual hardware: compatibility projection; actual VM-exit handling, memory mapping, and virtualization implementation: underlying provider/VM authority.

## Rules for change

1. New contracts require a documented consumer need and invariants, not convenience wrappers around Linux APIs.
2. Domain ownership must not shift merely because a capability transports or stores that domain's data.
3. Provider limitations must be explicit; security/isolation requirements must never silently degrade.
4. Do not require global mediation for every operation. Separate provisioning, enforcement, and fast-path access.
5. Cross-repository changes require coordination through the existing MNCS authority mechanisms; this repository does not redefine them.

See [ADR 0001](decisions/0001-platform-boundary.md).