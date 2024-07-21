---
title: Push gateway
weight: 12
---

Appclacks server provides a [Prometheus push gateway](https://github.com/prometheus/pushgateway) with additional features:

- Full featured CRUD API
- Highly available setup: your push gateway is not a Single Point of Failure anymore
- TTL support on the metrics being pushed: you can for example configure a metric to expire after one minute, or one hour.
- Integrated into the Appclacks CLI
