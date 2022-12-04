---
title: Authentication
weight: 35
---

The [Getting Started](/getting-started/) section explains how to install the Appclacks CLI, create an organization and a new token. This guide will go into detail about API token management.

## API Tokens permissions

You can attach permissions to API tokens to only allow some actions when used. The `--permission` flag can be used (and repeated) on the `appclacks token create` command to configure permissions.

The support permissions are:

- `*`: Enable all API calls
- `GetOrganization`: Get information about your organization
- `ListAPITokens`: List API tokens
- `GetAPIToken`: Get information about a specific API token
- `DeleteAPIToken`: Delete API tokens
- `GetHealthcheck`: Get information about a specific API token
- `CreateHealthcheck`: Create health checks
- `DeleteHealthcheck`: Delete health checks
- `UpdateHealthcheck`: Update health checks
- `ListHealthchecks`: List health checks
- `ListHealthchecksResults`: List health checks results
- `GetHealthchecksMetrics`: Get health checks metrics. See the [Metrics and Prometheus integration](/guides/metrics/) guide.
- `CabourotteDiscovery`: Cabourotte discovery endpoint. See the [Appclacks on your private infrastructure guide](/guides/private-infrastructure/) guide.

## API tokens TTL

You can configure a TTL on API tokens using the `--ttl` flag. Once the token is expired, it cannot be used anymore.

The `--ttl` format accepts a string like for example `10s` for 10 seconds, `1m` for one minute, or `400h` for 400 hours.

## Managing tokens

The `appclacks token list` and `appclacks token get --id <token_id>` commmands can be used to list tokens.

You can delete tokens by ID by using `appclacks token delete --id <token_id>` command.
