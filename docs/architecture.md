# Architecture and integration boundaries

This is a **candidate architecture**, not a statement that APIs or services already exist.

## Layers and flow

```text
Consumer service / native MNCS workload
       | requests a contract-defined capability
       v
Environment: demand, selection policy, orchestration, projections
       | obtains / delegates a suitably authorized capability
       v
mncs-platform: capability contract + provider conformance
       | provider-specific implementation
       v
Linux / Windows / macOS / virtual substrate / future bare metal
```

The diagram describes responsibility, **not a required synchronous call chain**. Consumers may use delegated capabilities directly after provisioning. Providers may consist of multiple small components.

## Distinguish four things

1. **Contract** — observable requirements: inputs, outputs, invariants, limits, errors, and evidence.
2. **Provider** — an implementation on a particular substrate, exposing verified supported capabilities.
3. **Environmental policy** — when a capability is requested, selected, routed, reconciled, or repaired.
4. **Projection** — a consumer-oriented representation or operational interface composed from authoritative services.

A projection can be interactive, not merely a read-only view. Its interface must still be backed by actual mechanisms, with the correct authority boundaries. A terminal is not the executor; a virtual machine view is not itself a hypervisor.

## Platform neutrality

Contracts must not assume POSIX paths, file descriptors, Unix signals, process IDs, shell invocation, or a single filesystem hierarchy unless a specific **compatibility contract** explicitly requires them.

Prefer semantic requirements such as:

- Execute an isolated workload under delegated resource and authority constraints.
- Provide monotonic elapsed-time measurement and explicitly separate wall-clock time.
- Make a durable write with specified ordering and recovery guarantees.
- Expose events and streams with documented delivery, backpressure, and cancellation.
- Describe and enforce access to a resource without presuming a Unix user/group model.

Platform neutrality does not mean all backends are identical. The system must discover restrictions and explicitly refuse unsupported guarantees.

## Bootstrapping and bare metal

In a hosted deployment, Linux or another OS supplies the primitives. In a possible future bare-metal deployment, a native kernel/boot chain and device mechanisms would satisfy the contracts directly.

The native system still needs a real bootstrap path, page tables/memory protection, interrupts, scheduling or execution, required drivers, crash recovery, and appropriate isolation. Contract stability can reduce changes to higher-level consumers; it cannot eliminate physical or architectural boot requirements.

A guest operating system may run as a compatibility projection **only** when a suitable virtualization or emulation implementation actually exists. The present MNCS-VM should not be assumed to be a hardware hypervisor.

## Design hazards

- **POSIX in disguise:** contracts defined as copies of Linux calls prevent meaningful portability.
- **A central mega-service:** all execution and I/O routed through one policy or observation bottleneck.
- **Duplicated authority:** the Platform store or provider accidentally becomes the owner of project/domain objects.
- **Unbounded projections:** background materialization, event replay, or caches overwhelm RAM/CPU.
- **Silent semantic downgrade:** a backend claims isolation or durability it cannot enforce.
- **False portability:** code runs only because a development host has incidental binaries or services.

The intent is independently testable, narrow capabilities with explicit ownership and bounded execution cost.