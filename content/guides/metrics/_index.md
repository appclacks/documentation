---
title: Metrics and Prometheus integration
weight: 40
---

The Appclacks API exposes health checks metrics in the [Prometheus](https://prometheus.io/) format.

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
      password: '<YOUR API TOKEN>'
    static_configs:
      - targets:
          - "api.appclacks.com"
```

Be sure to use a token allowing you to get metrics, as explained in the [Authentication guide](/guides/authentication/).

## Exposed metrics

Two metrics about health checks are exposed by the Appclacks API:

- `healthcheck_duration_seconds_bucket`: A [Prometheus histogram](https://prometheus.io/docs/concepts/metric_types/#histogram) showing the duration of health checks executions.
The labels for this metric are `le` (the bucket), `id` (the health check id), `name` (the health check name), and `zone` (the zone where the health check is executed).
- `healthcheck_total`: A [Prometheus counter](https://prometheus.io/docs/concepts/metric_types/#counter), counting the health checks executions.
The labels for this metric are `id` (the health check id), `name` (the health check name), `status` (`success` or `failure`), and `zone` (the zone where the health check is executed).

This for example would return the API for a health check executed in the `fr-par1` zone:

```
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="+Inf", name="mcorbin-blog", zone="fr-par-1"} 3
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="0.005", name="mcorbin-blog", zone="fr-par-1"} 0
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="0.01", name="mcorbin-blog", zone="fr-par-1"} 0
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="0.025", name="mcorbin-blog", zone="fr-par-1"} 0
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="0.05", name="mcorbin-blog", zone="fr-par-1"} 0
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="0.1", name="mcorbin-blog", zone="fr-par-1"} 3
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="0.25", name="mcorbin-blog", zone="fr-par-1"} 3
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="0.5", name="mcorbin-blog", zone="fr-par-1"} 3
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="0.75", name="mcorbin-blog", zone="fr-par-1"} 3
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="1", name="mcorbin-blog", zone="fr-par-1"} 3
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="10", name="mcorbin-blog", zone="fr-par-1"} 3
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="2.5", name="mcorbin-blog", zone="fr-par-1"} 3
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="5", name="mcorbin-blog", zone="fr-par-1"} 3
healthcheck_duration_seconds_bucket{id="cc145703-af98-4c0b-96da-8e06dc934fc7", le="7.5", name="mcorbin-blog", zone="fr-par-1"} 3
healthcheck_total{id="cc145703-af98-4c0b-96da-8e06dc934fc7", name="mcorbin-blog", status="success", zone="fr-par-1"} 3
```

## Useful Prometheus queries

**Compute Health checks latency**

This Prometheus query computes the p99 execution time for each health check on 5 minutes ranges. You can compute other quantiles by changing the `0.99` value to something else (`0.5`, `0.75`...).

```
histogram_quantile(0.99, sum(rate(healthcheck_duration_seconds_bucket{}[5m])) by (le, id, name))
```

You can easily alert on it by adding a condition at the end (for example `> 3`). You can also add the `zone` label in `by` to have per-zone latency.

**Detect failed health checks**

Appclacks executes health checks from 2 zones.
We recommend you to alert if health checks fail from several zones during the same timeframe to avoid false positive.

This Prometheus query will for example be triggered if the 2 zones are reporting errors on a 5 minutes timeframe (don't hesitate to adjust the timeframe based on your needs and health checks interval):

```
count(sum by (id, name, zone) (increase(healthcheck_total{status="failure", name="appclacks-api-failure"}[5m]))) > 1
```

**Execution rate rates**

Use this query to compute the rate of health check executions for each health check, per zone and status:

```
sum by (name, id, zone, status) (rate(healthcheck_total{}[5m]))
```

You can also set labels in `healthcheck_total{}` to filter for example only failed health check (by adding `status="failure"` in this case).
