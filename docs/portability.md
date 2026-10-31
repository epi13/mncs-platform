# Portability and verification

## Goal

Move incidental host dependencies behind real platform capability contracts **without** converting the MNCS computing model into POSIX, and without making agents responsible for routine environment coordination.

## Initial inventory

Audit current MNCS Environment and tools for actual usage of:

- Processes, shells, external commands, signals, exit statuses.
- Paths, file permissions, filesystem watchers, locks, temp storage.
- Clocks, timers, random sources, sleep/suspension assumptions.
- Network sockets, DNS, loopback expectations, remote execution.
- Memory mapping, isolation, CPU affinity, hardware enumeration.
- Host-specific tools, system services, init systems, containers, and assumptions in tests.

For each dependency, record its consumer, current owner, essential semantics, failure modes, portability blocker, and whether it belongs in Platform, Environment, a domain service, or a compatibility projection. Do not migrate just because a Linux API exists.

## Minimal migration method

1. Pick one observed dependency with a clear consumer and limited blast radius.
2. Document the actual required semantics and contract tests.
3. Put host-specific translation inside a provider, preserving the owner of higher-level policy/state.
4. Test the consumer against a normal provider **and** a constrained or fault-injected implementation.
5. Confirm identical intended behavior and explicit differences before replacing call sites.
6. Measure agent intervention, redundant discovery, failure handling, and resource overhead.

The environment should discover, report, reconcile, or route provider constraints whenever those responsibilities are deterministic. Platform implements mechanisms and supplies evidence; it does not absorb Environment's policies.

## Evidence gates

| Gate | Evidence required |
| --- | --- |
| Host independence | The environment runs without incidental development-machine tools or configuration |
| Provider independence | A consumer can switch between conformant providers without source changes |
| Semantic conformance | Success, failure, authority, ordering, and security behavior match required contracts |
| Operational efficiency | Translation and projection costs stay bounded; agent burden measurably decreases |
| Bare-metal readiness | Native bootstrap and essential hardware capabilities work independently of a hosted OS |

A minimal Linux From Scratch installation on worker-03 would be an eventual **host independence/deployment experiment** after MNCS language maturation, native tooling conversion, self-compilation, and self-hosting. Since LFS still uses Linux, it is **not evidence by itself** of cross-OS or bare-metal portability.

## Future backends

Windows and macOS providers should be built around contracts that already survive Linux tests and intentionally exercise differing OS semantics. A bare-metal provider is a separate long-term effort with hard bootstrap and hardware requirements. No operating-system port or kernel work is currently promised.