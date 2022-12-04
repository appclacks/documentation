---
title: Getting Started
weight: 10
archetype: default
---

## Creating your organization

Appclacks organizations are entities that can hold several accounts.

Appclacks is currently in Alpha. To use the platform, you should subscribe to it on the Appclacks [main website](https://appclacks.com/#register). You will then receive an email announcing that your organization is enabled. This step can take up to a day.

The email will also provide you the ID of your organization.

For now, an organization can only contain one account that is automatically generated when the organization is created. Supporting multiple accounts within the same organization is in the roadmap.

## Installing the Appclacks CLI

We don't provide a user interface, for now, to work with the Appclacks cloud platform. We provide a full-featured [command line interface](https://github.com/appclacks/cli) to interact with the platform.

To install the CLI, you should:

- Get the latest release from the [releases page](https://github.com/appclacks/cli/releases). Be sure to download the archive for your platform and operating system.
- Unarchive it and add the `appclacks` binary into your path.
- `appclacks help` should work and give you information about the appclacks CLI

## Authentication

**Creating a token**

You need an API token in order to interact with the Appclacks cloud platform:

- Define two environment variables containing your Appclacks account email and password: `export APPCLACKS_ACCOUNT_EMAIL='<your_account_email'` and `export APPCLACKS_ACCOUNT_PASSWORD='<your_account_password'`.
- Create a new API token with the command `appclacks token create`, for example `appclacks token create --name "admin"`.
You can run `appclacks token create --help` to list all options for this command. You can for example configure the allowed API calls for the token and configure a TTL on it.

See the [Authentication](/guides/authentication/) documentation for more information about permissions and tokens.

**Using a token**

Once you have a token, set the `APPCLACKS_ORGANIZATION_ID` environment variable to your organization ID, and the `APPCLACKS_TOKEN` environment variable to your token value.

You should now be able to execute commands if your token allows them. `appclacks healthcheck list` should work for example.

All commands accept an `--output` flag. By default, the `table` output is used but you can use the `json` output to get the JSON payload returned by the API.

You can now follow the [guides](/guides) to learn about how to use the Appclacks cloud platform.
