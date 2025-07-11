# Phase 5: Monitoring Tools (Prometheus, Loki, Grafana)

## Overview

This phase focuses on setting up a comprehensive monitoring solution for observing the health and performance of the applications and infrastructure deployed on Kubernetes. The monitoring stack includes:

- **Prometheus** for metrics collection and monitoring.
- **Loki** for centralized log aggregation and visualization.
- **Grafana** for dashboard visualization and alerting.

---

## Deliverables

### 1. Prometheus Configuration

- Kubernetes manifests are provided for deploying Prometheus using either the Prometheus Operator or Helm charts.
- Prometheus is configured to scrape metrics from:
  - Frontend application pods
  - Backend application pods
  - Redis
  - PostgreSQL

### 2. Loki Configuration

- Kubernetes manifests or Helm charts are used to deploy Loki.
- Loki collects logs from all application pods through Promtail or an equivalent log collector.

### 3. Grafana Dashboards

- JSON files for dashboards are available under `./grafana/dashboards/`.
- Dashboards cover:
  - **Application-specific metrics**: frontend/backend performance and error rates.
  - **Kubernetes cluster metrics**: node health, pod resource utilization.
  - **Redis and PostgreSQL metrics**.
  - **Log visualization** powered by Loki.

### 4. Alerting Rules (Optional but Recommended)

- Prometheus Alertmanager is configured with basic alerting rules.
- Alerts include critical events such as:
  - High error rates
  - Pod restarts
  - Resource exhaustion (CPU, Memory)

---

## Deployment Steps

### Prometheus

1. Deploy Prometheus using the provided Kubernetes manifests or Helm charts:
   ```bash
   kubectl apply -f monitoring/prometheus/
   # OR
   helm install prometheus prometheus-community/prometheus -f monitoring/prometheus/values.yaml
