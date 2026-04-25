# vibeproxy-monitoring-unified

**Unified Monitoring & Health Check System for Phenotype Services**

vibeproxy-monitoring-unified provides a standardized health check and monitoring interface for all Phenotype services. It enables consistent service health reporting, dependency monitoring, and graceful degradation across distributed systems.

## Overview

Every service has a different notion of "healthy." vibeproxy-monitoring-unified defines a standard health check contract—liveness, readiness, and startup probes—with consistent HTTP endpoints and response formats. Services register health checks at startup; the system periodically evaluates them and exposes results for orchestrators (Kubernetes, Docker Compose) and monitoring tools.

## Technology Stack

- **Languages**: Rust (core), Go (adapters), Python (SDK)
- **Protocols**: HTTP/JSON (health checks), gRPC (health v1 service)
- **Integrations**:
  - **Kubernetes**: Standard `/healthz`, `/readyz`, `/startup` endpoints
  - **Docker**: Health check scripts with configurable intervals
  - **Prometheus**: Metrics on probe results (latency, success rate)
  - **Grafana**: Pre-built health status dashboards
- **Features**: Liveness, readiness, startup probes; dependency checking; cascading failures

## Key Features

- **Standard Endpoints**: Kubernetes-compatible health check endpoints
  - `/healthz` — liveness probe (is the service running?)
  - `/readyz` — readiness probe (can the service handle requests?)
  - `/startup` — startup probe (has the service finished initialization?)
- **Dependency Tracking**: Register checks for databases, queues, external APIs
- **Cascading Failures**: Mark service unhealthy if critical dependencies fail
- **Graceful Shutdown**: Health probes return 503 during shutdown
- **Detailed Status**: Return structured info (component status, version, uptime)
- **Metrics**: Prometheus metrics for health check latency and success rates
- **Multi-Language**: Consistent checks across Rust, Go, Python services
- **Configuration**: Declarative threshold configuration (timeouts, retry counts)

## Quick Start

```bash
# Clone the repository
git clone https://github.com/KooshaPari/vibeproxy-monitoring-unified.git
cd vibeproxy-monitoring-unified

# Review the health check specification
cat docs/HEALTH_CHECK_SPEC.md

# See a simple example service
cat examples/simple-service/src/main.rs

# Run example service
cd examples/simple-service && cargo run

# Test health endpoints
curl http://localhost:8080/healthz         # liveness
curl http://localhost:8080/readyz          # readiness
curl http://localhost:8080/startup         # startup

# View Prometheus metrics
curl http://localhost:8080/metrics | grep health_

# Run tests
cargo test --workspace
```

## Project Structure

```
vibeproxy-monitoring-unified/
├── src/
│   ├── health/                    # Core health check abstractions
│   │   ├── check.rs               # Health check trait
│   │   ├── probe.rs               # Liveness/readiness/startup probes
│   │   ├── status.rs              # Status enum and reporting
│   │   └── registry.rs            # Check registration
│   ├── http/                      # HTTP endpoints
│   │   ├── handler.rs             # Request handlers
│   │   ├── response.rs            # JSON response formatting
│   │   └── middleware.rs          # Metrics/logging
│   ├── checks/                    # Built-in check implementations
│   │   ├── database.rs            # Database connectivity
│   │   ├── http_dependency.rs     # HTTP service dependency
│   │   ├── cache.rs               # Cache backend (Redis, Memcached)
│   │   └── file_system.rs         # File system health
│   ├── metrics/                   # Prometheus integration
│   └── config/                    # Configuration and thresholds
├── examples/
│   ├── simple-service/            # Basic health checks
│   ├── with-dependencies/         # Multi-component health
│   ├── kubernetes/                # K8s probe configuration
│   └── docker/                    # Docker HEALTHCHECK script
├── tests/
│   ├── integration/               # End-to-end health scenarios
│   ├── failure_modes/             # Degradation and recovery
│   └── fixtures/                  # Test services
├── python/
│   ├── vibeproxy_monitoring/      # Python SDK
│   └── tests/
├── go/
│   ├── vibeproxy/                 # Go SDK
│   └── tests/
└── docs/
    ├── HEALTH_CHECK_SPEC.md       # Probe specification
    ├── INTEGRATION_GUIDE.md       # How to add health checks
    ├── KUBERNETES.md              # K8s probe configuration
    ├── DOCKER.md                  # Docker HEALTHCHECK setup
    └── TROUBLESHOOTING.md         # Common issues
```

## Related Phenotype Projects

- **PhenoDevOps** — Uses health checks for service orchestration and deployment
- **Tracera** — Exports health check results as observability metrics
- **AgilePlus** — Instrumented with unified health checks
- **pheno-cli** — CLI health probe subcommand

## Quality & Testing

Comprehensive test coverage for health check scenarios:
- Unit tests for check implementations
- Integration tests with real dependencies (testcontainers)
- Failure mode tests (cascade, timeout, recovery)
- Kubernetes probe simulation tests

```bash
cargo test --workspace --all-features
pytest tests/ -v
go test ./...
```

## Probe Configuration Examples

### Kubernetes (liveness + readiness)
```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 10
readinessProbe:
  httpGet:
    path: /readyz
    port: 8080
  initialDelaySeconds: 5
```

### Docker Compose
```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:8080/healthz"]
  interval: 10s
  timeout: 3s
  retries: 3
```

## Governance

All work tracked in AgilePlus. All new check types must:
- Implement the Health trait
- Include unit and integration tests
- Document timeout and retry behavior
- Support graceful shutdown signaling

---

**Version**: v0.1.0  
**Last Updated**: 2026-04-25
