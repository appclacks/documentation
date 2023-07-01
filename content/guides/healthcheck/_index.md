---
title: Managing health checks
weight: 10
---

The [Appclacks CLI](/getting-started/#installing-the-appclacks-cli) can be used to manage health checks

## How health checks are executed

You can choose when you create an health check if it should be executed or not by the Appclacks cloud platform.

It's possible to create health checks on Appclacks but to host yourself the prober executing the health check, or to execute it by both Appclacks and on your infrastructure. See the [Appclacks on your private infrastructure](/guides/private-infrastructure/) guide for more information about self-hosting.

## Health check types

Appclacks supports 5 healthchecks types (detailed options for each type are described [at the end of this page](/guides/healthcheck/#health-checks-options-reference)):

- `HTTP(s)`: An HTTP request will be executed on the target. You can configure a lot of options about the health check request (method, path, headers, protocol, TLS configuration, body...) or the expected response (status code, body).
- `TCP`: A TCP connection will be initiated with the target. You can also use this kind of health check to check that a TCP target is *not* responding, to be sure that a component is not exposed on the internet for example.
- `TLS`: A TLS connection will be initiated by the target. If you host the prober yourself you can even configure which certificates to use for the connection.
- `DNS`: A DNS resolution will be executed on a domain. You can use it to see if the resolution is working and to check the IP returned by it.
- `Command`: An arbitrary shell command (with optional arguments) will be executed. The health check is considered failed if the command fails to execute. This health check type cannot be executed by the Appclacks cloud platform but can run in your private infrastructure.

## Creating an health check

Use the `appclacks healthcheck <type> create` command to create a health check, where `<type>` is the health check type (`http`, `tcp`, `tls`, `dns`, `command`).

In this example, we create an HTTP health check targeting `appclacks.com`. You can see in the response (in json due to the `--output` option) the default values for HTTP health checks.

```
$ appclacks healthcheck http create --name "hello" --target "appclacks.com" --output json

{"id":"caaa5899-26a7-4a91-a77a-86adc849d23e","name":"hello","type":"http","timeout":"5s","interval":"60s","created-at":"2022-12-04T21:47:03.592347391Z","enabled":true,"Definition":{"valid-status":[200],"target":"appclacks.com","method":"GET","port":443,"redirect":true,"protocol":"https"}}
```

The `--disabled` flag controls whether or not the health check should be executed by the Appclacks cloud platform. If set, the health check `enabled` field will be set to false and the health will not be executed by Appclacks. Disabling health checks can be useful if you [self-host](http://localhost:1313/guides/private-infrastructure/) the Appclacks prober and want to execute these checks on your private infrastructure.

Use the `--help` command to get details about health checks options, for example `appclacks healthcheck http create --help`. The available options are also explained [at the end of this page](/guides/healthcheck/#health-checks-options-reference) for each health check type.


{{% notice style="blue" %}}
Some health check types and parameters cannot be used for health checks running one Appclacks cloud platform:

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

## Health checks options reference

### Common fields

These fields are available to all health checks:

- `name`: the health check name. It should be unique.
- `description`: an optional health check description (optional)
- `labels`: key/value pairs of strings, useful to add metadata to health checks (optional, example `environment=production`)
- `interval`: the health check execution interval (example: `30s` for 30 seconds, `1m` for one minute...).
- `timeout`: the health check execution timeout (example: `3s` for 3 seconds)
- `enabled`: Enable or not the health check on the Appclacks cloud platform. Only enabled health checks are executed by Appclacks. Setting health checks can be useful if you only want to [self-host](http://localhost:1313/guides/private-infrastructure/) the Appclacks prober.

### HTTP

- `valid-status`: a list of HTTP status code to consider the health check successful (example: `200`, `201`).
- `target`: the URL or IP address to target (example: `api.appclacks.com`).
- `method`: the HTTP method to use (example: `GET`).
- `port`: the port to target (example: `443`).
- `protocol`: the protocol to use (`http` or `https`)
- `host`: override the HTTP Host header (optional, example: `api.appclacks.com`).
- `redirect`: follow redirect (optional, default `false`).
- `query`: HTTP query parameters to pass to the request (optional).
- `body`: HTTP query to pass to the request (optional).
- `body-regexp`: A list of regular expressions on which the HTTP response body will be matched. The health check is considered failed if the response body does not have the regular expressions (optional).
- `headers`: extra HTTP headers to pass to the request (optional).
- `key`: A path to a file containing a certificate private key for mTLS (optional).
- `cert`: A path to a file containing a certificate public key for mTLS (optional).
- `cacert`: A path to a file containing certificates authorities to trust (optional).

### TCP

- `target`: the URL or IP address to target (example: `api.appclacks.com`).
- `port`: the port to target (example: `443`).
- `should-fail`: if set to true, the health check will be considered successful if the TCP connection fails (default to `false`).

### TLS

- `target`: the URL or IP address to target (example: `api.appclacks.com`).
- `port`: the port to target (example: `443`).
- `key`: A path to a file containing a certificate private key for mTLS (optional).
- `cert`: A path to a file containing a certificate public key for mTLS (optional).
- `cacert`: A path to a file containing certificates authorities to trust (optional).
- `server-name`: the server name identifier to use (optional).
- `insecure`: enable insecure TLS (default to `false`)
- `expiration-delay`: duration before alerting for the target certificat expiration date (optional, for example an expiration delay of `48h` would make the health check fails if the certificate expires in less than 48 hours).

### DNS

- `domain`: the domain to target (example: `google.com`)
- `expected-ips`: a list of IP ddresses (v4 or v6) that should be present in the DNS answer to consider the health checks successful (optional)

### Command

- `command`: the command to execute (example: `ls`)
- `arguments`: the list of arguments to pass th the command (example: `-l`).
