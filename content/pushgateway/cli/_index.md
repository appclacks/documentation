---
title: Managing metrics using the Appclacks CLI
weight: 10
---

The [Appclacks CLI](/getting-started/#installing-the-appclacks-cli) can be used to manage metrics.

All commands support an `--output` option to change its output format (default to `table`, `json` is also supported).

## Creating your first metrics

Let's create several metrics using the Appclacks CLI:

```
appclacks pushgateway create --name example_1 --value 10
appclacks pushgateway create --name example_2 --value 20 --labels environment=production --labels application=foo --description "my metric description" --type gauge
```

Appclacks will expose these metrics on the `/pushgateway/metrics` endpoint (see the [Observability section](/observability/):

```
example_1{} 10
# HELP example_2 my metric description
# TYPE example_2 gauge
example_2{application="foo", environment="production"} 20
```

## Metrics TTL

By default, metrics in the push gateway will never expire.

You can configure a TTL on the metrics by specifying the `ttl` to the command, for example for a 2 minutes TTL:

```
appclacks pushgateway create --name example_3 --value 20 --labels environment=production --labels application=foo --description "my metric description" --type gauge --ttl 2m
```

## Managing metrics

- You can list the metrics with `appclacks pushgateway list`
- You can delete a metric by ID by using `appclacks pushgateway delete`
- You can delete **all** metrics using `appclacks pushgateway delete-all`
