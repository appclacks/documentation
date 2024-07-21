---
title: Terraform integration
weight: 20
---

You can manage your health checks using the [Appclacks Terraform provider](https://registry.terraform.io/providers/appclacks/appclacks/latest/docs). The source code of the provider is [available on Github](https://github.com/appclacks/terraform-provider-appclacks).

Examples are also available in [the provider Github repository](https://github.com/appclacks/terraform-provider-appclacks/tree/master/examples).

## Configuration

The [standard Appclacks environment variables](/getting-started/#environment-variables) are understood by Terraform.

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

# You can also use environment variables to configure the provider
provider "appclacks" {
  api_endpoint = ""
  username = ""
  password = ""
  tls_key = ""
  tls_cert = ""
  tls_cacert = ""
  insecure = false
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




