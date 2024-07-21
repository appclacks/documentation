---
title: Kubernetes operator
weight: 25
---

We provide a Kubernetes operator that allows you to configure health checks on Appclacks using Kubernetes Custom Resources Definitions (CRD).

The operator source code is available [on Github](https://github.com/appclacks/operator). CRD examples are available in the [config/samples](https://github.com/appclacks/operator/tree/main/config/samples) directory.

## Getting started

You can install the operator on your Kubernetes cluster by following this documentation. We plan to provide an Helm chart in a near future to deploy it.

**Creating the required CRDs**

All CRD files present in [this directory on Github](https://github.com/appclacks/operator/tree/main/config/crd/bases) should be applied to your cluster.

**Deploying the operator**

The operator and its resources will be created in a namespace named `appclacks`.

If you need to inject environment variables secrets for authentication in the operator (see the [standard Appclacks environment variables](/getting-started/#environment-variables)), you can add them in the [secret.yaml](https://github.com/appclacks/operator/blob/main/config/deployment/secret.yaml) file and then apply it (using `kubect apply -f secret.yaml`).
You should also configure in this file the `APPCLACKS_API_ENDPOINT` variable.

Once done, you can apply in the same way the other files present in [the deployment directory](https://github.com/appclacks/operator/tree/main/config/deployment).

**Testing the operator**

Create a simple DNS health checks using `kubectl apply -f`:

```yaml
apiVersion: healthchecks.appclacks.com/v1alpha1
kind: DNSHealthcheck
metadata:
  name: dnshealthcheck-sample
  namespace: appclacks
  labels:
    example: label
spec:
  domain: appclacks.com
  interval: 35s
  timeout: 10s
  description: "dns health check example"
  enabled: true
```

Examples for each health check kind (HTTP, TCP, TLS, Command, DNS) are available [on Github](https://github.com/appclacks/operator/tree/main/config/samples).
