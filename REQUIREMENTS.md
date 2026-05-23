# Requirements

## Goal
Provide working sample code, configuration, and architecture diagrams enabling users of third-party metrics platforms to ship metrics into Elasticsearch (Elastic Cloud Hosted or Elastic Serverless).

## Target Audience
- Platform engineers evaluating Elasticsearch as a metrics backend
- Teams currently running Prometheus/Grafana or DataDog who want unified observability in Elastic
- Elastic field teams demonstrating ingest paths during POCs

## Use Cases

### Grafana

| ID | Status | Use Case | Source | Destination |
|----|--------|----------|--------|-------------|
| G1 | ✅ Done | Prometheus + Grafana (self-managed or Grafana Cloud) | Prometheus `remote_write` | Elasticsearch (ECH or Serverless); optional Grafana Cloud Metrics in parallel |
| G2 | ✅ Done | Grafana Alloy + Grafana Cloud | Alloy collector → Grafana Cloud → Elasticsearch | Elasticsearch |

### DataDog

| ID | Status | Use Case | Source | Destination |
|----|--------|----------|--------|-------------|
| D1 | ✅ Done | DataDog Agent → OTEL Collector → Elasticsearch | DD Agent OTLP export → OTEL Collector | Elasticsearch (ECH or Serverless) |

## Deliverables per Use Case
- [ ] Architecture diagram (Mermaid)
- [ ] Working sample configuration files with inline comments
- [ ] `README.md` covering: Quick Summary, Architecture, Prerequisites, Setup, Verification
- [ ] Screenshots in `assets/` subdirectory
- [ ] Notes on Elastic Cloud Hosted vs. Serverless differences where applicable
- [ ] TODO marker for Grafana data source / dashboard setup (per use case)

## Learned Conventions (from G1)
- Prometheus `remote_write` endpoint for Elasticsearch: `/_prometheus/api/v1/write`
- Auth must use the `authorization` block (`type: ApiKey`) — not raw `headers`
- Metrics land in `metrics-generic.prometheus-default` data stream by default
- Verify data in Kibana via ES|QL: `TS metrics-generic.prometheus-default`
- Write API key cannot be used to read/verify data — verification is via Prometheus self-metrics or Kibana
- Images go in `assets/` subdirectory per use case

## Non-Goals
- Building a production-hardened deployment pipeline
- Covering every possible Grafana/DataDog configuration variant
- Replacing official Elastic or Grafana documentation

## Technical Constraints
- No hardcoded credentials — use placeholder strings throughout
- Configs must work against current stable versions of each tool at time of writing
- Elasticsearch target: 8.x / Serverless (note any version-specific behavior)
