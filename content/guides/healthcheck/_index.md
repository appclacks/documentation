---
title: Managing health checks
weight: 10
---

The [Appclacks CLI](/getting-started/#installing-the-appclacks-cli) can be used to manage health checks

## How health checks are executed

You can choose when you create an health check if it should be executed or not by the Appclacks cloud platform.

It's possible to create health checks on Appclacks but to host yourself the prober executing the health check, or to execute it by both Appclacks and on your infrastructure. See the [Appclacks on your private infrastructure](guides/private-infrastructure/) guide for more information about self-hosting.

## Health check types

Appclacks supports 4 healthchecks types today:

- `HTTP(s)`: An HTTP request will be executed on the target. You can configure a lot of options about the health check request (method, path, headers, protocol, TLS configuration, body...) or the expected response (status code, body).
- `TCP`: A TCP connection will be initiated with the target. You can also use this kind of health check to check that a TCP target is *not* responding, to be sure that a component is not exposed on the internet for example.
- `TLS`: A TLS connection will be initiated by the target. If you host the prober yourself you can even configure which certificates to use for the connection.
- `DNS`: A DNS resolution will be executed on a domain. You can use it to see if the resolution is working and to check the IP returned by it.
- `Command`: An arbitrary command (with optional arguments) will be executed. The health check is considered failed if the command fails to execute.

## Creating an health check

Use the `appclacks healthcheck <type> create` command to create a health check, where `<type>` is the health check type (`http`, `tcp`, `tls`, `dns`, `command`).

In this example, we create an HTTP health check targeting `appclacks.com`. You can see in the response (in json due to the `--output` option) the default values for HTTP health checks.

```
$ appclacks healthcheck http create --name "hello" --target "appclacks.com" --output json

{"id":"caaa5899-26a7-4a91-a77a-86adc849d23e","name":"hello","type":"http","timeout":"5s","interval":"60s","created-at":"2022-12-04T21:47:03.592347391Z","enabled":true,"Definition":{"valid-status":[200],"target":"appclacks.com","method":"GET","port":443,"redirect":true,"protocol":"https"}}
```

The `--disabled` flag controls whether or not the health check should be executed by the Appclacks cloud platform. If set, the health check `enabled` field will be set to false and the health will not be executed by Appclacks.

Use the `--help` command to get details about health checks, for example `appclacks healthcheck http create --help`.


{{% notice style="blue" %}}
Some health check parameters cannot be used for health checks running one Appclacks cloud platform:

- `command` health checks cannot be enabled on the Cloud platform.
- TLS and HTTP health checks: the TLS configurations parameters (`key`, `cert`, `cacert`) cannot be used.
- The local interface (`localhost`, `127.0.0.1`...) cannot be used as a health check target.

{{% /notice %}}



## Managing health checks

The `appclacks healthcheck list` and `appclacks healthcheck get --id <healthcheck_id>` (`get` also supports `--name <healthcheck_name>`) commands can be used to retrieve health checks information.

You can update an existing health check with the `appclacks healthcheck <type> update` commands. All health checks fields which you want to configure should be specified, the update is a replace.

## Health checks results

Each time a health check is executed by the Appclacks cloud platform, its result is stored and available through the Appclacks API.

The `appclacks healthcheck result list` command retrieves health checks results. By default, all health check results from the last 5 minutes are retrieved. +
Health checks results are paginated and only 100 results are returned. The `--page` option can be used to retrieve the next results from the list.

You can also configure which results you want to retrieve with the flags:

- `--healthcheck-id`: Get the results for a specific health check only.
- `--start-date`: The start date (UTC) for retrieving results, in format `2006-01-02T15:04:05`.
- `--end-date`: The end date (UTC) for retrieving results, in the format `2006-01-02T15:04:05`.
- `--end-date`: The end date (UTC) for retrieving results, in the format `2006-01-02T15:04:05`.
- `--error`: Retrieve only failed health checks.
- `--success`: Retrieve only successful health checks.
