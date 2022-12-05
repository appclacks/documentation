---
title: Metrics and Prometheus integration
weight: 40
---

The Appclacks API exposes health checks metrics in the [Prometheus](https://prometheus.io/) format.

## Exposed metrics

Two metrics about health checks are exposed by the Appclacks API:

- `healthcheck_duration_seconds_bucket`: A [Prometheus histogram](https://prometheus.io/docs/concepts/metric_types/#histogram) showing the duration of health checks executions.
The labels for this metric are `le` (the bucket), `name` (the health check name), and `zone` (the zone where the health check is executed).
- `healthcheck_total`: A [Prometheus counter](https://prometheus.io/docs/concepts/metric_types/#counter), counting the health checks executions.
The labels for this metric are `name` (the health check name), `status` (`success` or `failure`), and `zone` (the zone where the health check is executed).

This for example would return the API for a health check executed in the `fr-par1` zone:

```
healthcheck_duration_seconds_bucket{le="+Inf", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 1
healthcheck_duration_seconds_bucket{le="0.005", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 0
healthcheck_duration_seconds_bucket{le="0.01", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 0
healthcheck_duration_seconds_bucket{le="0.025", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 0
healthcheck_duration_seconds_bucket{le="0.05", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 0
healthcheck_duration_seconds_bucket{le="0.1", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 0
healthcheck_duration_seconds_bucket{le="0.25", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 0
healthcheck_duration_seconds_bucket{le="0.5", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 0
healthcheck_duration_seconds_bucket{le="0.75", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 1
healthcheck_duration_seconds_bucket{le="1", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 1
healthcheck_duration_seconds_bucket{le="10", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 1
healthcheck_duration_seconds_bucket{le="2.5", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 1
healthcheck_duration_seconds_bucket{le="5", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 1
healthcheck_duration_seconds_bucket{le="7.5", name="21223a4c-256c-48f2-9495-9836264191b8", zone="fr-par1"} 1
healthcheck_total{name="21223a4c-256c-48f2-9495-9836264191b8", status="success", zone="fr-par1"} 1
```

## Retrieving the metrics

**Using the Appclacks CLI**

The `appclacks healthcheck metrics get` command returns all metrics for your health checks in Prometheus format.

**Using Prometheus**

You can configure Prometheus to scrape the Appclacks API by adding a new job on the [scrape_configs](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config) section:

```yaml
scrape_configs:
  - job_name: "appclacks"
    metrics_path: "/api/v1/metrics/healthchecks"
    scheme: https
    basic_auth:
      username: '<YOUR ORGANIZATION ID>'
      password: '<YOUR API TOKEN'
    static_configs:
      - targets:
          - "api.appclacks.com"
```

Be sure to use a token allowing you to get metrics, as explained in the [Authentication guide](/guides/authentication/).
