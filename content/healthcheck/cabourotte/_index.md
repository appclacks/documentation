---
title: Running Cabourotte to execute the health checks
weight: 15
---

Once health checks are configured in the Appclacks server, you need to run Cabourotte to execute them.

It can be retrieved from its [Github repository](https://github.com/mcorbin/cabourotte/releases) and its documentation is available on [https://www.cabourotte.appclacks.com/](on a dedicated website).

You should configure the `discovery` section of the Cabourotte [configuration file](https://www.cabourotte.appclacks.com/installation/configuration/) to integrate it with your Appclacks server.
Cabourotte will retrieve the health check configurations from Appclacks and run the health checks.

This is a Cabourotte configuration example:

```yaml
http:
  host: "0.0.0.0"
  port: 9013
discovery:
  http:
    - name: "appclacks"
      interval: 60s
      host: "api.appclacks.com"
      port: 443
      path: "/api/v1/cabourotte/discovery"
      protocol: "https"
      query:
        labels: "env=prod,project=foo"
      headers:
        Authorization: "Basic aGVsbG86d29ybGQ="
```

The optional `Authorization` header contains the `Basic` string followed the (optional) username and password that you configured on the server, in base64

The `query` parameter is optional and can be used to retrieve only health checks having these labels. You can for example use this feature to dynamically configure several Cabourotte instances dynamically.

{{% notice style="blue" %}}
Only health checks with `enabled` set to true will be executed by Cabourotte.
{{% /notice %}}
