---
title: Appclacks on your private infrastructure
weight: 50
---

Cabourotte, the Appclacks prober, is a free software you can host on your private infrastructure.

It can be retrieved from its [Github repository](https://github.com/mcorbin/cabourotte/releases) and its documentation is available on [https://www.cabourotte.mcorbin.fr/](https://www.cabourotte.mcorbin.fr/).

You should configure the `discovery` section of the Cabourotte [configuration file](https://www.cabourotte.appclacks.com/installation/configuration/) to integrate it with Appclacks.
Cabourotte will retrieve the health check configurations from Appclacks and run the health checks.

This is a Cabourotte configuration example:

```yaml
discovery:
  http:
    interval: 60s
    host: "api.appclacks.com"
    port: 443
    path: "/api/v1/cabourotte/discovery"
    protocol: "https"
    query:
      labels: "env=prod,project=foo"
    headers:
      Authorization: "Basic WU9VUl9PUkdBTklaQVRJT05fSUQ6WU9VUl9UT0tFTg=="
```

The `Authorization` header contains the `Basic` string followed by the string `YOUR_ORGANIZATION_ID:YOUR_API_TOKEN` encoded in base64.

The `query` parameter is optional and can be used to retrieve only health checks having these labels.
