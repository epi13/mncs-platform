# Development direction

This is a **dependency-ordered exploration**, not a dated implementation commitment. The current focus remains maturation of MNCS Language, Compiler, Environment, and native tooling.

## Phase 0 — Charter and authority (this repository)

- Publish ownership boundaries, design constraints, and a lightweight decision record.
- Preserve the existing project license.
- Avoid claiming an implementation, API stability, or OS portability before evidence exists.

**Exit condition:** contributors can distinguish Platform contracts from Environment policy, domain authority, and compatibility projections.

## Phase 1 — Dependency evidence

- Inventory actual host coupling across existing tools and Environment.
- Identify high-value capability seams based on observed duplication, agent burden, correctness, and resource cost.
- Prioritize a narrow demonstrable contract.

**Exit condition:** at least one contract candidate has a concrete consumer, a behavioral specification, and realistic negative tests.

## Phase 2 — First provider and conformance

- Introduce an explicitly versioned contract and a Linux provider only when needed.
- Exercise a second constrained or independent implementation to expose accidental Linux assumptions.
- Record performance, reliability, capability absence, and safety evidence.

**Exit condition:** a consumer can change provider without semantic regressions or code changes to its domain logic.

## Phase 3 — Environment integration

- Let Environment acquire and coordinate Platform capabilities under existing authority.
- Ensure projections, ambient repair, and diagnostics remain Environment responsibilities.
- Verify reduced host-specific knowledge and coordination work for agents.

**Exit condition:** a working consumer demonstrates measurable operational burden reduction rather than merely additional tool activity.

## Later possibilities, not current deliverables

- Dedicated minimal LFS deployment on worker-03 after self-hosting maturity.
- Windows/macOS providers based on real user demand and conformance evidence.
- Compatibility projections such as terminals, filesystem views, or hosted guests.
- A bare-metal boot substrate, native provider, or hypervisor if MNCS reaches the necessary compiler/runtime and hardware maturity.

Avoid prematurely introducing a kernel, a universal ABI, a mandatory filesystem, or speculative language/runtime requirements merely to populate this repository.