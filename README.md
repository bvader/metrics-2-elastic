# metrics-2-elastic

Sample code, configurations, and architecture diagrams for shipping metrics from third-party observability platforms into **Elasticsearch** (Elastic Cloud Hosted or Elastic Serverless).

## Why?

Teams running Prometheus/Grafana or DataDog often want a single observability backend — or are evaluating Elasticsearch as their metrics store. This repo provides ready-to-use patterns for each common ingest path.

## Use Cases

### Grafana

| Directory | Use Case |
|-----------|----------|
| [`grafana/prometheus-grafana`](grafana/prometheus-grafana/) | Prometheus → Elasticsearch (self-managed or via Grafana Cloud) via `remote_write` |
| [`grafana/alloy-grafana-cloud`](grafana/alloy-grafana-cloud/) | Grafana Alloy → Grafana Cloud → Elasticsearch |

### DataDog

| Directory | Use Case |
|-----------|----------|
| [`datadog/agent-otel-elasticsearch`](datadog/agent-otel-elasticsearch/) | DataDog Agent → OTEL Collector → Elasticsearch |

## Elasticsearch Targets

All use cases target both:
- **Elastic Cloud Hosted (ECH)** — traditional Elasticsearch deployment on Elastic Cloud
- **Elastic Serverless** — fully managed, consumption-based Elasticsearch

Differences between the two targets are called out in each use case README.

## Prerequisites (common)

- Access to an Elasticsearch deployment (ECH or Serverless)
- An Elasticsearch API key with `write` access to the target data stream or index
- Familiarity with the source metrics platform (Prometheus, Grafana, or DataDog)

## Contributing

Each use case lives in its own directory. To add a new pattern:
1. Create a new subdirectory under the appropriate platform folder.
2. Add a `README.md` following the structure in existing use cases.
3. Include a Mermaid architecture diagram and working sample configs.

## License

Apache 2.0
