---
title: Getting Started
weight: 10
archetype: default
---

## Creating your organization

Appclacks organizations are entities that can hold several accounts.

Appclacks is currently in Alpha. To use the platform, you should subscribe to it on the Appclacks [main website](https://appclacks.com/#register). You will then receive an email with an activation link.

The email will also provide you the ID of your organization.

For now, an organization can only contain one account that is automatically generated when the organization is created. Supporting multiple accounts within the same organization is in the roadmap.

## Installing the Appclacks CLI

We don't provide a user interface, for now, to work with the Appclacks cloud platform. We provide a full-featured [command line interface](https://github.com/appclacks/cli) to interact with the platform.

To install the CLI, you should:

- Get the latest release from the [releases page](https://github.com/appclacks/cli/releases). Be sure to download the archive for your platform and operating system.
- Unarchive it and add the `appclacks` binary into your path.
- `appclacks help` should work and give you information about the appclacks CLI

## Authentication

You need an API token in order to interact with the Appclacks cloud platform.

**Configure your CLI**

Once the CLI is installed, run `appclacks login`. The command will ask you several informations:
- A profile name: i'll allow you to manage multiple appclacks accounts by selecting the profile to use in commands. If you don't have any profile configured, the first created profile will be the default one
- Your Appclacks account email
- Your Appclacks account password

An authentication token will be automatically created for your account. A configuration file will be created in your OS configuration directory (`$HOME/.config/appclacks/appaclacks.yaml` on Linux, see [this Golang function](https://pkg.go.dev/os#UserConfigDir) for other platforms) and automatically picked by the CLI.

You should now be able to successfully run commands, for example `appclacks healthcheck list`. You can override the default profile used by the CLI by passing the `--profile` flag.

This is a commented example of the configuration file format:

```
# default profile used by the CLI
default-profile: my-profile
profiles:
    # profile name
    my-profile:
        # organization ID
        organization-id: 5a062154-dc9f-11ed-9cd1-673503b45134
        # API token
        api-token: <api_token>
```

**Creating a token manually**

This alternative method allows you to create tokens without using `appclacks login`. It can be useful to get more control over the created tokens (to configure permissions on them for example).

- Define two environment variables containing your Appclacks account email and password: `export APPCLACKS_ACCOUNT_EMAIL='<your_account_email'` and `export APPCLACKS_ACCOUNT_PASSWORD='<your_account_password'`.
- You should now be able to get information about your organization using the `appclacks account organization` command.
- Create a new API token with the command `appclacks token create`, for example `appclacks token create --name "admin"`.
You can run `appclacks token create --help` to list all options for this command. You can for example configure the allowed API calls for the token and configure a TTL on it.

See the [Authentication](/guides/authentication/) documentation for more information about permissions and tokens.

**Using a token**

Once you have a token, set the `APPCLACKS_ORGANIZATION_ID` environment variable to your organization ID, and the `APPCLACKS_TOKEN` environment variable to your token value. These variables will override the configuration file if they exist.

You should now be able to execute commands if your token allows them, for example `appclacks healthcheck list`.

All commands accept an `--output` flag. By default, the `table` output is used but you can use the `json` output to get the JSON payload returned by the API.

You can now follow the [guides](/guides) to learn about how to use the Appclacks cloud platform.
