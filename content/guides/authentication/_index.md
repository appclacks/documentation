---
title: Authentication and API tokens
weight: 35
---

This page explains how to create Appclacks API token and the supported methods to authenticate against the API.

We recommend you to follow the [getting started](/getting-started/) section that explains how to install and configure the Appclacks CLI before reading this documentation.

## Authentication on Appclacks

An API token is needed to authenticate against Appclacks. The [getting started](/getting-started/) section explains how to use the `appclacks login` CLI command to create your first token.

## Creating additional tokens

Tokens can be created using the CLI. To do so, you should:

- Define two environment variables containing your Appclacks account email and password: `export APPCLACKS_ACCOUNT_EMAIL='<your_account_email'` and `export APPCLACKS_ACCOUNT_PASSWORD='<your_account_password'`.
- You can run the `appclacks account organization` command to validate that authentication is working.
- Create a new API token with the command `appclacks token create`, for example `appclacks token create --name "admin"`.

By default, tokens have a time to live (TTL) of 72 hours. After this period, the token will be expired and not work anymore. You can set a custom TTL for your token by setting the `--ttl` flag, where the value is a number of seconds, minutes of hours, for example:

- `--ttl 10000h`: the token expires after 10 000 hours.
- `--ttl 60m`: the token expires in 60 minutes.
- `--ttl 10s`: the token expires in 10 seconds.

** Tokens permissions**

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
- `GetHealthchecksMetrics`: Get health checks metrics. See the [Metrics and Prometheus integration guide](/guides/metrics/).
- `CabourotteDiscovery`: Cabourotte discovery endpoint. See the [Appclacks on your private infrastructure guide](/guides/private-infrastructure/).

## Authenticating on the API

Appclacks CLI and tooling supports several authentication methods once you have a token

**Appclacks configuration file**

This file is automatically created by the `appclacks login` command but can also be edited manually. The file is available at the path `$HOME/.config/appclacks/appaclacks.yaml` on Linux, (see [this Golang function](https://pkg.go.dev/os#UserConfigDir) for other platforms).

This is a commented example of the configuration file format:

```yaml
# default profile used by the CLI
default-profile: my-profile
profiles:
    # profile name
    my-profile:
        # Your Appclacks organization ID
        organization-id: 5a062154-dc9f-11ed-9cd1-673503b45134
        # Your API token
        api-token: <api_token>
```

Profiles allows you to configure multiple configuration methods. The Appclacks CLI supports a `--profile` parameter to select the profile to use. The `APPCLACKS_PROFILE` environment variable can also be used to switch between profiles.

**Appclacks environment variables**

Set the `APPCLACKS_ORGANIZATION_ID` environment variable to your organization ID, and the `APPCLACKS_TOKEN` environment variable to your token value. These variables will override the configuration file if they exist.

## Managing tokens

The `appclacks token list` and `appclacks token get --id <token_id>` commmands can be used to list tokens.

You can delete tokens by ID by using `appclacks token delete --id <token_id>` command.
