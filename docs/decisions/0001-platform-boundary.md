# ADR 0001 — Platform capability authority

- **Status:** Initial architectural direction
- **Date:** 2026-10-30
- **Scope:** mncs-platform repository authority and first design constraints

## Context

Host-specific dependencies are currently distributed across MNCS Environment and its tooling. The project is converging on an environment that maintains authoritative state, serves agents, and exposes projections to human and machine consumers. Over time this environment could operate on minimal hosted systems or even a native bare-metal substrate.

A Linux-only compatibility wrapper would harden host assumptions into the MNCS model. Putting every provider mechanism inside Environment would blur authority and expand orchestration scope.

## Decision

Create an independent `mncs-platform` repository with authority over machine capability contracts, platform-provider translation, capability discovery, and provider conformance.

MNCS Environment retains authority over environmental coordination, self-management, consumer projections, and recovery policy. Language, Compiler/VM, Rights & Provenance, and other MNCS services retain their own distinct domains.

Design contracts in MNCS terms; do not use Linux or POSIX mechanics as universal assumptions. Providers may implement one or more contracts directly. Capability use must not require a central Environment hot-path broker.

## Consequences

- A narrow implementation can be independently reviewed and tested.
- Portability is testable at the provider/consumer boundary.
- Linux may be the first provider without becoming an architectural dependency.
- A future LFS host or bare-metal implementation can be explored without redefining consumer semantics.
- Native storage semantics, terminal views, and VM compatibility projections remain separable.
- Contract versioning, failure semantics, security enforcement, and bootstrapping must be addressed explicitly as the project matures.

## Deferred

No API signatures, initial provider, package structure, self-hosting timeline, bare-metal kernel, bootloader, or virtualization mechanism is selected by this ADR.

## Revisit when

Evidence from real consumers shows the authority boundary is misplaced, a proposed capability cannot be expressed without leaking platform assumptions, or a second provider reveals material semantic differences.