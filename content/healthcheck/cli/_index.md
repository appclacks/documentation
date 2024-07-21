---
title: Managing health checks using the Appclacks CLI
weight: 10
---

The [Appclacks CLI](/getting-started/#installing-the-appclacks-cli) can be used to manage health checks.

All commands support an `--output` option to change its output format (default to `table`, `json` is also supported).

## Health check types

Appclacks supports 5 healthchecks types (detailed options for each type are described [at the end of this page](/healthcheck/cli/#health-checks-options-reference)):

- `HTTP(s)`: An HTTP request will be executed on the target. You can configure a lot of options about the health check request (method, path, headers, protocol, TLS configuration, body...) or the expected response (status code, body).
- `TCP`: A TCP connection will be initiated with the target. You can also use this kind of health check to check that a TCP target is *not* responding, to be sure that a component is not exposed on the internet for example.
- `TLS`: A TLS connection will be initiated by the target. If you host the prober yourself you can even configure which certificates to use for the connection.
- `DNS`: A DNS resolution will be executed on a domain. You can use it to see if the resolution is working and to check the IP returned by it.
- `Command`: An arbitrary shell command (with optional arguments) will be executed. The health check is considered failed if the command fails to execute.

## Creating an health check

Use the `appclacks healthcheck <type> create` command to create a health check, where `<type>` is the health check type (`http`, `tcp`, `tls`, `dns`, `command`).

In this example, we create an HTTP health check targeting `appclacks.com`. You can see in the response (in json due to the `--output` option) the default values for HTTP health checks.

```
$ appclacks healthcheck http create --name "hello" --target "appclacks.com" --output json

{"id":"caaa5899-26a7-4a91-a77a-86adc849d23e","name":"hello","type":"http","timeout":"5s","interval":"60s","created-at":"2022-12-04T21:47:03.592347391Z","enabled":true,"Definition":{"valid-status":[200],"target":"appclacks.com","method":"GET","port":443,"redirect":true,"protocol":"https"}}
```
Use the `--help` command to get details about health checks options, for example `appclacks healthcheck http create --help`. The available options are also explained [at the end of this page](/guides/healthcheck/#health-checks-options-reference) for each health check type.

## Managing health checks

The `appclacks healthcheck list` and `appclacks healthcheck get --id <healthcheck_id>` (`get` also supports `--name <healthcheck_name>`) commands can be used to retrieve health checks information.

You can update an existing health check with the `appclacks healthcheck <type> update` commands. All health checks fields which you want to configure should be specified, the update is a replace.

You can delete an health check by ID by running `appclacks healthcheck delete --id <healthcheck_id>`.

## Health checks options reference

### Common fields

These fields are available to all health checks:

- `name`: the health check name. It should be unique.
- `description`: an optional health check description (optional)
- `labels`: key/value pairs of strings, useful to add metadata to health checks (optional, example `environment=production`)
- `interval`: the health check execution interval (example: `30s` for 30 seconds, `1m` for one minute...).
- `timeout`: the health check execution timeout (example: `3s` for 3 seconds)
- `enabled`: Enable or not the health check. The [Cabourotte discovery endpoint](/healthcheck/cabourotte) will only return enabled health checks.

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
- `server-name`: the server name identifier to use (optional).
- `insecure`: enable insecure TLS (default to `false`)

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
