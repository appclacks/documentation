---
title: Appclacks
weight: 30
chapter: false
---

# Welcome to the Appclacks server documentation

Appclacks server provides several observability-related features.

Appclacks server is part of the Appclacks ecosystem. This website will focus on the documentation of the Appclacks server, other tools maintained by Appclacks having dedicated documentation websites that you can find in the [Appclacks main page](appclacks.com)

**Blackbox monitoring**

Appclacks server can be used to monitor public or private endpoints by executing configured health checks on them from multiple point of presences.
The prober used by Appclacks named Cabourotte [is a free software](https://github.com/appclacks/cabourotte) that you can host on your private infrastructure and plug on the Appclacks API server to autoconfigure it.

This flexibility allows you to manage your health checks globally without having to use a different tool for public and for private infrastructures.

**Prometheus Push gateway alternative**

Appclacks server provides an alternative to Prometheus push gateway, with additional feature:

- Full featured CRUD API
- Highly available setup: your push gateway is not a Single Point of Failure anymore
- TTL support on the metrics being pushed: you can for example configure a metric to expire after one minute, or one hour.
- Integrated into the Appclacks CLI

## Open ecosystem

Appclacks server is (like all Appclacks projects) fully open source and integrates well with your existing observability stack, like Prometheus for example.

## Good tooling

We know at Appclacks the importance of good tooling for users, and that's why it's one of the primary focuses of the Appclacks team.

We provide a full-featured [CLI](https://github.com/appclacks/cli), a [Terraform provider](https://github.com/appclacks/terraform-provider-appclacks/) and a [Kubernetes operator](/guides/kubernetes/) to let you efficiently configure resources.

## What's next

Follow the [getting started](/getting-started/) section to learn how to start using Appclacks.
