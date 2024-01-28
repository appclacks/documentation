---
title: Getting Started
weight: 10
archetype: default
---

This guide explains how to use the Appclacks CLI to create your first health check on Appclacks.

## Creating your organization

To use the platform, you should subscribe to it on the Appclacks [main website](https://appclacks.com/#register). You will then receive an email with an activation link.

The email will also provide you the ID of your organization.

For now, an organization can only contain one account that is automatically generated when the organization is created. Supporting multiple accounts within the same organization is in the roadmap.

You should now be able to log in in the [Appclacks user interface](https://api.appclacks.com). This interface is today a read-only one, using the CLI or other integrations are required to create resources.

## Installing the Appclacks CLI

We provide a full-featured [command line interface](https://github.com/appclacks/cli) to interact with the platform.

**MacOS**

You can use brew to install the latest release of the Appclacks LI: `brew install appclacks/tap/appclacks`.

You can also install the prebuilt MacOS binaries as described below.

**Other platforms and prebuilt binaries**

To install the CLI from the prebuilt binaries, you should:

- Get the latest release from the [releases page](https://github.com/appclacks/cli/releases). Be sure to download the archive for your platform and operating system.
- Unarchive it and add the `appclacks` binary into your path.
- `appclacks help` should work and give you information about the appclacks CLI. Each subcommands described in this page have a lot of options availables and all of them supports an `--help` flag (for example: `appclacks healthcheck http create --help`).

## Authentication

You need an API token in order to interact with the Appclacks cloud platform.

Run `appclacks login` to configure authentication. The command will ask you several informations:
- A profile name: i'll allow you to manage multiple appclacks accounts by selecting the profile to use in commands. If you don't have any profile configured, the first created profile will be the default one
- Your Appclacks account email
- Your Appclacks account password

An authentication token will be automatically created for your account. A configuration file will be created in your OS configuration directory (`$HOME/.config/appclacks/appaclacks.yaml` on Linux, see [this Golang function](https://pkg.go.dev/os#UserConfigDir) for other platforms) and automatically picked by the CLI.

You should now be able to successfully run commands, for example `appclacks healthcheck list`.

## Create your first health checks

Let's create an HTTP health check targeting `GET https://api.appclacks.com/healthz` every 30 seconds:

```shell
appclacks healthcheck http create api.appclacks.com --target api.appclacks.com --path "/healthz" --name "http-check-example"

ID                                    Name                Description  Interval  Timeout  Labels  Enabled  Definition
--                                    ----                -----------  --------  -------  ------  -------  ----------
d9b17d26-a4cd-47f0-86c0-6cf02069a9dc  http-check-example               60s       5s       null    true     {"valid-status":[200],"target":"api.appclacks.com","method":"GET","port":443,"redirect":true,"protocol":"https","path":"/healthz"}
```

Appclacks will start executing the health check from multiple countries.

You can then list your health checks with `appclacks healthcheck list`, or retrieve its configuration with `appclacks healthcheck get --name http-check-example` (or `--id d9b17d26-a4cd-47f0-86c0-6cf02069a9dc`).

## Retrieving health check results

Retrieve health checks results executed by Appclacks by launching `appclacks healthcheck result list --healthcheck-id d9b17d26-a4cd-47f0-86c0-6cf02069a9dc`:

```
appclacks healthcheck result list --healthcheck-id d9b17d26-a4cd-47f0-86c0-6cf02069a9dc
ID                                    Created At                     Success  Duration (ms)  Summary                                    Message  Healthcheck ID                        Labels
--                                    ----------                     -------  -------------  -------                                    -------  --------------                        ------
f4a3f05f-4d89-415b-b189-c42e467d9f06  2024-01-10 21:39:56 +0000 UTC  true     5              HTTP healthcheck on api.appclacks.com:443  success  d9b17d26-a4cd-47f0-86c0-6cf02069a9dc  {"healthcheck_name":"http-check-example","zone":"fr-par-1"}
d32c246c-5ac8-473e-ac9a-1b100e4a6457  2024-01-10 21:39:56 +0000 UTC  true     37             HTTP healthcheck on api.appclacks.com:443  success  d9b17d26-a4cd-47f0-86c0-6cf02069a9dc  {"healthcheck_name":"http-check-example","zone":"pl-waw-1"}
```

Appclacks commands also support JSON outputs by passing the `-o json` flag.

## Retrieving health check metrics

You can retrieve health checks metrics in Prometheus format by running `appclacks healthcheck metrics get`. The [Prometheus integration documentation](https://www.doc.appclacks.com/guides/metrics/) explains how to configure Prometheus (or any Prometheus-compatible tool) to scrape Appclacks metrics.

## Going further

The dedicated [health check documentation page](/guides/healthcheck/) explains all health checks kinds and options available on Appclacks.

The [guides](/guides/) will deep dive on topics like Terraform, Kubernetes, Prometheus integrations, or how to run Appclacks health checks on your private infrastructure.

Happy monitoring !
