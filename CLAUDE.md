# metrics-2-elastic — Claude Code Instructions

## Project Purpose
Sample code and diagrams showing how to ship metrics from third-party solutions (Prometheus/Grafana, DataDog) into Elasticsearch (Elastic Cloud Hosted or Serverless).

## Structure
```
metrics-2-elastic/
├── grafana/
│   ├── prometheus-grafana-self-managed/   # Prometheus + Grafana OSS → ES
│   ├── prometheus-grafana-cloud/          # Prometheus → Grafana Cloud → ES
│   └── alloy-grafana-cloud/               # Grafana Alloy → Grafana Cloud → ES
└── datadog/
    └── agent-otel-elasticsearch/          # DD Agent → OTEL Collector → ES
```

## Conventions
- Each use case lives in its own subdirectory with: `README.md`, `diagram.png` or `diagram.mermaid`, and working sample config/code.
- Diagrams: prefer Mermaid (`.mermaid`) so they render in GitHub; export PNG only when Mermaid is insufficient.
- Config files: annotate with inline comments explaining every non-obvious field.
- README per use case: must include Prerequisites, Architecture Diagram, Setup Steps, and Verification.
- Credentials/API keys: always use placeholder strings (`<YOUR_API_KEY>`) — never hardcode real values.
- Target both Elastic Cloud Hosted (ECH) and Elastic Serverless unless a constraint prevents it; call out differences explicitly.

## Autonomy Rules
- Act, don't ask. Make judgment calls and log why.
- Only stop for irreversible actions or missing customer-specific values.
