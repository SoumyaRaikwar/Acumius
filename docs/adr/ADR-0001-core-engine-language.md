# ADR-0001: Core Engine Language

**Date:** 2026-04-09  
**Status:** Accepted  
**Deciders:** Core Engine track, Protocol Layer track, project maintainers

## Context

Acumius needs a core memory service that can handle concurrent agent reads/writes, provide predictable low-latency behavior, and remain easy to self-host for local-first deployments.

During Phase 0 alignment, issue [#1](https://github.com/Acumius/Acumius/issues/1) surfaced repeated concerns about scalability and operational simplicity of the core service. Feedback in the issue discussion recommended choosing Go for the memory engine due to its concurrency model and performance profile for infrastructure services.

The project plan also positions Acumius as infrastructure, not only an application API, which favors a compiled, strongly typed, operationally efficient runtime for long-running services.

## Decision

Acumius `core/` will be implemented in **Go** for v0.1 and onward.

## Rationale

- Go's goroutines and channels map well to concurrent memory workloads from many agents.
- Lower runtime overhead and predictable performance suit latency-sensitive store/retrieve paths.
- Static binaries and simple deployment model align with local-first and self-hosted operation.
- Strong standard library support for networking, observability, and service development.
- Team feedback in Phase 0 already converged on Go as the practical default for the core engine.

## Alternatives Considered

- **Python (FastAPI) for core service**
  - Rejected for core runtime because high-concurrency workloads can become harder to scale predictably, and performance tuning tends to arrive earlier.
  - Still useful for adapters, SDKs, tooling, and benchmarks where fast iteration matters more than core service throughput.

- **Hybrid start in Python, rewrite to Go later**
  - Rejected due to migration risk and duplicated engineering effort.
  - Would delay stable contracts and increase rework across protocol and adapter layers.

## Consequences

### Positive

- Better foundation for high-concurrency memory operations.
- Clear runtime baseline for protocol and adapter teams.
- Easier path to production-like load testing and performance tuning in early milestones.

### Tradeoffs

- Team members less familiar with Go face an onboarding curve.
- Some ecosystem tooling may need bridge layers for Python-heavy agent workflows.

## Follow-up

- Create `core/` scaffolding in Go with baseline service structure and health endpoints.
- Define coding standards and lint/test setup for Go in CI.
- Keep Python and TypeScript first-class for adapters/SDKs to preserve developer ergonomics.
