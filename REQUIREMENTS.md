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
| Grafana 1 | ✅ Done | Grafana Alloy → Grafana Cloud → Elasticsearch | Alloy collector → Grafana Cloud Fleet Management → Elasticsearch | Elasticsearch (ECH or Serverless) + Grafana Cloud Metrics in parallel |
| Grafana 2 | ✅ Done | Prometheus + Grafana (self-managed or Grafana Cloud) → Elasticsearch | Prometheus `remote_write` | Elasticsearch (ECH or Serverless); optional Grafana Cloud Metrics in parallel |

### DataDog

| ID | Status | Use Case | Source | Destination |
|----|--------|----------|--------|-------------|
| DataDog 1 | ✅ Done | DataDog Agent → OTEL Collector → Elasticsearch | DD Agent `additional_endpoints` → OTEL Collector (datadogreceiver) | Elasticsearch (ECH or Serverless) |

### Full Local Test

| ID | Status | Use Case | Source | Destination |
|----|--------|----------|--------|-------------|
| Full Local Test | ✅ Done | Node Exporter + Prometheus + Grafana → Elasticsearch | Prometheus `remote_write` | Elasticsearch (ECH or Serverless) |

## Deliverables per Use Case
- [x] Architecture diagram (Mermaid)
- [x] Working sample configuration files with inline comments
- [x] Root `README.md` covering: What, Why, Use Cases, Common Prerequisites, per-use-case Architecture / How It Works / Prerequisites / Setup / Verification
- [x] Screenshots in `assets/` subdirectory per use case
- [x] Notes on Elastic Cloud Hosted vs. Serverless differences where applicable
- [ ] Grafana data source / dashboard setup steps (TODO in Grafana 2 and Full Local Test)
- [ ] DataDog Fleet Management screenshots for `additional_endpoints` config (TODO in DataDog 1 Step 3)

## Learned Conventions

### Prometheus / Grafana
- Prometheus `remote_write` endpoint for Elasticsearch: `/_prometheus/api/v1/write`
- Auth must use the `authorization` block (`type: ApiKey`) — not raw `headers`
- Grafana Alloy uses `headers` block for the ES endpoint (not `authorization`) — different from standalone Prometheus
- Metrics land in `metrics-generic.prometheus-default` data stream by default
- Verify data in Kibana via ES|QL: `TS metrics-generic.prometheus-default`
- Write API key cannot be used to read/verify data — verification is via Prometheus self-metrics or Kibana

### DataDog
- DataDog Agent ships with a built-in OTEL Collector on default OTLP ports (`:4317`/`:4318`) — use `:8080` for the external Collector Contrib to avoid port conflicts on the same host
- DataDog metrics land in `metrics-datadogreceiver.otel-default` data stream when using `datadogreceiver`
- Verify data in Kibana via ES|QL: `TS metrics-datadogreceiver.otel-default`
- `datadogreceiver` requires the **contrib** OTEL Collector distribution; `otlphttp` exporter is in both core and contrib
- OTEL Collector config is identical for same-host (Option A) and gateway (Option B) patterns — only the `additional_endpoints` address in `datadog.yaml` changes

### General
- Documentation consolidated in a single root `README.md` (not per-use-case READMEs)
- Images go in `assets/` subdirectory per use case (`grafana/alloy-grafana-cloud/assets/`, `datadog/assets/`, etc.)
- All credentials use placeholder strings (`<YOUR_API_KEY>`, `<YOUR_BASE64_API_KEY>`)

## Non-Goals
- Building a production-hardened deployment pipeline
- Covering every possible Grafana/DataDog configuration variant
- Replacing official Elastic or Grafana documentation

## Technical Constraints
- No hardcoded credentials — use placeholder strings throughout
- Configs must work against current stable versions of each tool at time of writing
- Elasticsearch target: 8.x / Serverless (note any version-specific behavior)
