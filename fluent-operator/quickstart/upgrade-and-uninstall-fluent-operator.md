---
title: Upgrade and Uninstall Fluent Operator
linkTitle: Upgrade and Uninstall Fluent Operator
description: Instructs you to upgrade and uninstall Fluent Operator.
keywords: Kubernetes, Fluent Bit, Fluentd, Fluent Operator

logo: 
weight: 4600

---

This section instructs you to upgrade and uninstall Fluent Operator.

## Upgrade Fluent Operator

Now, Fluent Operator 1.0.0 has been released. If you are still using an earlier version, run the following command to upgrade to the latest version:

If your container runtime is `docker`, run the follwing command:

```bash
helm upgrade fluent-operator --create-namespace -n kubesphere-logging-system charts/fluent-operator/  --set Kubernetes=true,containerRuntime=docker
```

If your container runtime is `containerd`, run the follwing command:

```bash
helm upgrade fluent-operator --create-namespace -n kubesphere-logging-system charts/fluent-operator/  --set Kubernetes=true,containerRuntime=containerd
```

If your container runtime is `cri-o`, run the follwing command:

```bash
helm upgrade fluent-operator --create-namespace -n kubesphere-logging-system charts/fluent-operator/  --set Kubernetes=true,conta
```





