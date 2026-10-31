# Contributing to mncs-platform

This repository is at the architecture/documentation foundation stage. Contributions should strengthen evidence, boundaries, and portability, rather than prematurely committing the project to a particular OS or kernel design.

## Before proposing a change

Read [README.md](README.md), [authority](docs/authority.md), [architecture](docs/architecture.md), and the [capability contract design](docs/capability-contracts.md).

Describe:

1. The real MNCS consumer and dependency being addressed.
2. Why this belongs in Platform rather than Environment, Language, Compiler, or a domain service.
3. The required observable semantics, limitations, authority, and failure behavior.
4. How at least one provider could be tested, including negative cases and resource impact.
5. Which current assumptions may prevent another host or bare-metal implementation.

Prefer narrow, reviewable changes. Do not treat POSIX as the universal baseline, invent fully supported backends, or move policy into providers.

## Documentation changes

Keep cross-links accurate and distinguish **implemented**, **proposed**, and **future** capabilities. Changes to authority or contract semantics should include an architecture decision record in `docs/decisions/`.

## Code changes (when development begins)

Include conformance tests and evidence for error handling, cancellation, resource bounds, and relevant security properties. A passing mock alone is not sufficient to claim portability. Do not add heavyweight dependencies or central request brokers without measured justification.

## License

Contributions follow the repository's existing [Apache License 2.0](LICENSE).