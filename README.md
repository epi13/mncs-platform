# mncs-platform

**Machine-native platform abstraction and capability translation for the MNCS ecosystem.**

`mncs-platform` defines the contracts through which MNCS systems obtain foundational machine capabilities without depending on Linux, Windows, macOS, a particular virtual execution mechanism, or a future bare-metal implementation.

> **Foundational principle:** MNCS defines the capabilities it requires. Platform providers fulfill those capabilities. MNCS Environment coordinates their use and projects relevant state to consumers.

## Status

**Architecture and documentation foundation only.** This repository does not yet implement a capability API, production providers, a kernel, a bootloader, a hypervisor, or a portability guarantee. Examples and capability families below are design candidates, not supported features.

The near-term purpose is to make platform dependencies identifiable, separately owned, testable, and eventually replaceable as MNCS Language, Compiler, and Environment mature.

## Scope

`mncs-platform` is the authority for:

- **Capability contracts:** platform-neutral requirements, semantics, constraints, and failure behavior for foundational operations.
- **Provider boundaries:** interfaces and adapters that fulfill those contracts on a host or native substrate.
- **Capability discovery:** faithful declarations of what a provider can and cannot supply.
- **Provider conformance:** evidence that implementations satisfy the same contracts, including failure and security behavior.
- **Portability evidence:** tracking and reducing host-specific assumptions without hiding essential platform differences.

It does **not** own language semantics, compiler correctness, agent coordination, environmental recovery policy, project context, domain records, or consumer-facing projections. Those remain with their respective MNCS authorities.

## Conceptual relationship

```text
MNCS consumers (Language/Compiler, Environment services, tools, workloads)
      |           environmental policy and projections remain in Environment
      v
Platform-neutral capability contracts      [mncs-platform authority]
      |
      +-- Linux provider                    [initial candidate]
      +-- Windows / macOS providers          [future candidates]
      +-- virtualized or hosted providers    [future candidates]
      +-- bare-metal provider                [long-term candidate]
      |
Host operating system, virtualization substrate, or native hardware
```

This is a **contract boundary**, not a mandatory central runtime proxy. Once authorized access is established, hot-path work should not need to round-trip through Environment or a single platform coordinator.

## Platform capability vs. compatibility projection

A **platform capability** is a mechanism MNCS consumes: execution, memory isolation, durable storage, clocks, events, or device access.

A **compatibility projection** presents an interface that a consumer expects: a terminal, POSIX-like filesystem view, or guest-visible virtual machine. Environment and the relevant domain authorities own the projection semantics; Platform can supply the underlying mechanisms. A Linux VM requires actual virtualization/emulation facilities, not only a projected description.

A native MNCS object store need not adopt filesystem paths as its authoritative representation merely because a compatibility view exposes files.

## Documentation

- [Authority and scope](docs/authority.md)
- [Architecture and integration boundaries](docs/architecture.md)
- [Capability contract design](docs/capability-contracts.md)
- [Portability and verification strategy](docs/portability.md)
- [Development roadmap](docs/roadmap.md)
- [Architecture decision 0001: platform boundary](docs/decisions/0001-platform-boundary.md)
- [Contribution guidance](CONTRIBUTING.md)

## Initial direction

1. Inventory the Linux and host-specific dependencies that currently leak through MNCS tooling and Environment.
2. Separate true platform mechanisms from domain responsibilities and consumer-facing projections.
3. Specify one narrow contract, its invariants, and provider conformance cases before migrating consumers.
4. Implement and exercise a Linux provider, including controlled failure and capability-absence cases.
5. Introduce additional providers only when the contracts are proven by real consumers.

A minimal Linux From Scratch host on worker-03 could become a later deployment test **after** language/tool maturity, native conversion, self-compilation, and self-hosting. It is not the architectural foundation of MNCS and is not a current milestone.

## License

Licensed under [Apache License 2.0](LICENSE).