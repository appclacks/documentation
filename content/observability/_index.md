---
title: Observability
weight: 20
---

The Appclacks API exposes health checks metrics in the [Prometheus](https://prometheus.io/) format. +
It also contains Prometheus queries examples (for alerting for example) as well as the JSON definition of a Grafana dashboard ready to be imported into your Grafana instance.

Appclacks exposes two metrics endpoints:

- `/metrics`: metrics about the Appclacks server itself (Go routine, HTTP-related metrics...)
- `/pushgateway/metrics`: metrics created using the Appclacks push gateway.*

Basic authentication can be configured for these metrics endpoints using the [configuration file](/getting-started/#configuration-file), in tge `http.metrics` yaml map.


