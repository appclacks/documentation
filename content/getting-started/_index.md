---
title: Getting Started
weight: 10
archetype: default
---

This guide explains how to use the Appclacks CLI to create your first health check on Appclacks.

## Architecture

## Run the server

In order to run Appclacks server, you need to deploy its [server](https://github.com/appclacks/server).

The Appclacks server requires a PostgreSQL database to run.

### Configuration file

The Appclacks server requires a configuration file to run:

```yaml
---
# Configuration of the HTTP server
http:
  # IP to listen to, mandatory
  host: 127.0.0.1
  # Port to listen to, mandatory
  port: 9000
  # Basic auth credentials for the server, optional
  basic-auth:
    username: "foo"
    password: "bar"
  # TLS key, optional
  key: "/path/to/key-file.key"
  # TLS cert, optional
  cert: "/path/to/cert-file.pem"
  # TLS cacert, optional
  cacert: "/path/to/cacert-file.pem"
  # Enable TLS insecure, optional
  insecure: false
  # TLS server name, optional
  server-name: "my.host"
  # Prometheus metrics configuration, optional
  metrics:
    # Enable basic auth on Prometheus metrics endpoints, optional
    basic-auth:
      username: metric-user
      password: metric-password
# Database configuration
database:
  # database username, mandatory
  username: "appclacks"
  # database password, mandatory
  password: "appclacks"
  # database name, mandatory
  database: "appclacks"
  # database host, mandatory
  host: "127.0.0.1"
  # database port, mandatory
  port: 5432
  # SSL mode (optional): see the PostgreSQL documentation for possible values: https://www.postgresql.org/docs/current/libpq-ssl.html
  ssl-mode: disable
# Healthcheck configuration section
```

### Run the server using Docker

Appclacks server images are [available on the docker hub](https://hub.docker.com/r/appclacks/server/tags).

A docker-compose file running the Appclacks server, PostgreSQL, and Cabourotte is available [on github](https://github.com/appclacks/server/blob/main/docker-compose.yaml).

### Run the server using the prebuilt binaries

Appclacks server binaries can be found on [https://github.com/appclacks/server/releases](Github).

You can you the binary with the command `appclacks-server server --config <path-to-config-file>`.

Log level can be configured with the `--log-level` flag (debug, info, warn, error) and log formats by the `--log-format` one (text, json).

## Installing the Appclacks CLI

We provide a full-featured [command line interface](https://github.com/appclacks/cli) to interact with the Appclacks server.

**MacOS**

You can use brew to install the latest release of the Appclacks LI: `brew install appclacks/tap/appclacks`.

You can also install the prebuilt MacOS binaries as described below.

**Other platforms and prebuilt binaries**

To install the CLI from the prebuilt binaries, you should:

- Get the latest release from the [releases page](https://github.com/appclacks/cli/releases). Be sure to download the archive for your platform and operating system.
- Unarchive it and add the `appclacks` binary into your path.
- `appclacks help` should work and give you information about the appclacks CLI. Each subcommands described in this page have a lot of options availables and all of them supports an `--help` flag (for example: `appclacks healthcheck http create --help`).

### Environment variables

The Appclacks CLI (and actually all Appclacks tooling and SDK) supports several environment variables to configure it:

- `APPCLACKS_API_ENDPOINT`: the Appclacks API endpoint
- `APPCLACKS_USERNAME`: optional basic auth username
- `APPCLACKS_PASSWORD`: optional basic auth password
- `APPCLACKS_TLS_KEY`: optional path to a TLS key file for mTLS
- `APPCLACKS_TLS_CERT`: optional path to a TLS cert file for mTLS
- `APPCLACKS_TLS_CACERT`: optional path to a TLS cacert file for mTLS
- `APPCLACKS_TLS_INSECURE`: optional insecure configuration for TLS

## Testing everything using docker compose

- Clone the [Appclacks server repository](https://github.com/appclacks/server).
- Run `docker compose up -d` to start all components
- Run `export APPCLACKS_API_ENDPOINT='http://localhost:9000'`
- Once the CLI is [installed](/getting-started/#installing-the-appclacks-cli), run `appclacks healthcheck http create --name "test" --target "localhost" --protocol http --port 9000 --path /healthz` to create your first health check
- Run `appclacks healthcheck list` to list health checks
- Run `curl localhost:9013/metrics` to get metrics about your health checks in Prometheus format (see [Cabourotte documentation](https://cabourotte.appclacks.com/installation/monitoring/) for more details about metrics)

You can now explore the various health check types and options and discover the Appclacks tooling (CLI commands, Terraform provider, Kubernetes operator...) on the [health check section](/healthcheck/index.html) of the documentation.

