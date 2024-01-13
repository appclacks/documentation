---
title: Appclacks
weight: 30
chapter: false
---

# Welcome to the Appclacks documentation

Appclacks is a cloud monitoring platform allowing users to create, manage and run various health checks to monitor the health of websites and infrastructures.

Appclacks can monitor public endpoints by executing configured health checks on them from multiple point of presences. But unlike alternatives, Appclacks can also be used to monitor private infrastructures.
The prober used by Appclacks named Cabourotte [is a free software](https://github.com/appclacks/cabourotte) that you can host on your private infrastructure and plug on the Appclacks API to autoconfigure it.

This flexibility allows you to manage your health checks globally without having to use a different tool for public and for private infrastructures.

![Appclacks architecture](/img/architecture.png?height=600px)

## Open ecosystem

As previously stated, Appclacks prober is a free software. +
Appclacks also exposes the metrics of the health checks executed on the cloud platform as [Prometheus](https://prometheus.io/) metrics (same format) on its API. This means that you can integrate the Appclacks cloud platform with your existing monitoring system.

## Good tooling

All actions on the Appclacks cloud platform are performed through its API. We know at Appclacks the importance of good tooling for users, and that's why it's one of the primary focuses of the Appclacks team.

We provide a full-featured [CLI](https://github.com/appclacks/cli), a [Terraform provider](https://github.com/appclacks/terraform-provider-appclacks/) and a [Kubernetes operator](/guides/kubernetes/) to let you efficiently configure the platform.

## What's next

Follow the [getting started](/getting-started/) section to learn how to start using Appclacks.

If you need support or want to provide 
