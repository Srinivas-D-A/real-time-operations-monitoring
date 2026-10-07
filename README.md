# Real-Time Operations Monitoring and Alerting

## Objective

DevOps monitoring application that simulates delivery operations, exposes Prometheus metrics, visualizes them in Grafana, configures alerts, and validates the application through Jenkins.

## Quick Start

Requirements: Docker Desktop with Compose.

```bash
docker compose up -d --build
```

Open:
- Metrics: http://localhost:8000/metrics
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000

Grafana:
- Username: `admin`
- Password: `admin`

The Prometheus datasource and Grafana dashboard are provisioned automatically.

## Metrics

- `total_deliveries`
- `pending_deliveries`
- `on_the_way_deliveries`
- `average_delivery_time_sum`
- `average_delivery_time_count`

Average delivery time query:

```text
average_delivery_time_sum / average_delivery_time_count
```

## Alerts

- `HighPendingDeliveries`: `pending_deliveries > 10` for 15 seconds.
- `HighAverageDeliveryTime`: average delivery time > 30 seconds for 15 seconds.

Prometheus alerts:
http://localhost:9090/alerts

## Jenkins

`Jenkinsfile` validates the Python application and monitoring configuration, builds the Docker image, and performs a `/metrics` smoke test.

## Stop

```bash
docker compose down
```

Remove monitoring data too:

```bash
docker compose down -v
```
