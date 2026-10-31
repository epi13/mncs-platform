# Capability contract design

No capability interface is yet stabilized. This document defines **how to specify and test one**, rather than introducing an API prematurely.

## Contract record

Every proposed capability should describe:

| Field | Required question |
| --- | --- |
| Identity and version | Which capability and contract revision? |
| Consumer need | What real MNCS workload requires it? |
| Semantic operations | What is requested and what observable result must occur? |
| Authority | Who can acquire, delegate, or revoke it, and what does enforcement guarantee? |
| Preconditions | What resources and state must exist first? |
| Safety/correctness | What invariants must never be violated? |
| Failure behavior | What happens on cancellation, crash, partial success, or resource exhaustion? |
| Concurrency and ordering | What ordering, visibility, and atomicity guarantees apply? |
| Resource limits | What are the memory, CPU, storage, and latency bounds? |
| Evidence | What can independently verify claims and detect degradation? |
| Portability | Which requirements are universal and which are explicitly optional? |
| Version evolution | How can providers and consumers negotiate change safely? |

## Candidate capability families

These are a **discovery inventory**, not an approved API surface:

| Family | Example contract concerns |
| --- | --- |
| Execution | Workload lifecycle, cancellation, completion, isolation, output capture |
| Memory | Allocation, protection, sharing, bounds, revocation |
| Time | Monotonic duration, wall clock, uncertainty, timer behavior |
| Persistence | Durable objects/bytes, atomic operations, crash consistency |
| Events / streams | Notifications, backpressure, subscriptions, delivery guarantees |
| Network transport | Addressing, connectivity, stream/datagram semantics |
| Device access | Enumeration, ownership, interrupts, DMA constraints |
| Isolation / virtualization | Resource separation, virtual CPU/memory, guest capabilities |

Domain models, project identity, environmental scheduling *policy*, and presentation/compatibility projections are not automatically Platform contracts.

## Provider declaration

A provider must declare supported contract versions and limits as **facts**, not optimistic promises. Discovery should include unsupported features and explain relevant constraints. A consumer must be able to require a guarantee and receive an explicit refusal if it cannot be met.

Two providers that expose the same method names but differ in cancellation, write durability, clock semantics, security enforcement, or error handling are **not interchangeable** without contract reconciliation.

## Conformance strategy

For each capability:

1. Specify a portable reference behavior and negative/failure cases.
2. Validate a provider against both ordinary and adversarial inputs.
3. Re-run the same tests through a second implementation or constrained/fault-injected provider.
4. Verify that consuming services rely on the contract, not provider-specific side effects.
5. Report behavioral gaps, resource costs, and uncovered requirements as evidence.

A mock provider may test consumer behavior, but it does not establish host portability or satisfy security guarantees. Conformance must include real provider tests and fault-injection where feasible.

## Example: clocks (illustrative only)

A request for elapsed time must specify monotonic behavior and how suspend, rollback, and measurement uncertainty are treated. A wall-clock timestamp is a different capability with different failure modes. The Linux provider may use Linux clocks; the consumer should not need to know the system call used.

This example intentionally avoids defining a final function signature.