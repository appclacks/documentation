---
title: Terraform integration
weight: 36
---

You can manage your health checks using the [Appclacks Terraform provider](https://registry.terraform.io/providers/appclacks/appclacks/latest/docs). The source code of the provider is [available on Github](https://github.com/appclacks/terraform-provider-appclacks).

Examples are also availabie in [the provider Github repository](https://github.com/appclacks/terraform-provider-appclacks/tree/master/examples).

## Authentication

You need to configure authentication against the Appclacks API to make the provider work.

The Appclacks [authentication documentation](https://www.doc.appclacks.com/getting-started/index.html#authentication) explains how authentication works and how to create an API token. You can either:

- Export the `APPCLACKS_ORGANIZATION_ID` and `APPCLACKS_TOKEN` values before running Terraform.
- Use au thentication file located at `$HOME/.config/appclacks/appaclacks.yaml`
- Pass the credentials directy to the provider block.


## Getting started

More examples are available in the [Terraform provider documentation](https://registry.terraform.io/providers/appclacks/appclacks/latest/docs)

```hcl
terraform {
  required_providers {
    appclacks = {
      source = "appclacks/appclacks"
    }
  }
}

# See the documentation for alternatives authentication methods: https://www.doc.appclacks.com/getting-started/index.html#authentication
provider "appclacks" {
  organization_id = ""
  token = ""
}

resource "appclacks_healthcheck_http" "test_http" {
  name = "test-http"
  interval = "30s"
  timeout = "5s"
  description = "example http health check"
  labels = {
    "env": "prod"
  }
  target = "api.appclacks.com"
  port = 443
  protocol = "https"
  method = "GET"
  path = "/healthz"
  valid_status = [200]
  enabled = true
}
```




