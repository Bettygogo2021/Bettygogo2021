---
title: Install Fluent Operator
linkTitle: Install Fluent Operator
description: Instructs you to install Fluent Operator.
keywords: Kubernetes, Fluent Operator

logo: 
weight: 4300


---

You can install Fluent Operator either using YAML or Helm. 

## Install Fluent Operator with Helm Chart (Recommended)

<Notice type='note'>

- The Helm version must be Helm v3.2.1 or higher.

- This installation method is recommended because it obviates the need to configure Fluent Operator.

</Notice>

Fluent Operator supports different CRI `docker`, `containerd`, and `CRI-O`. `containerd` and `CRI-O` use the `CRI Log` format which is different from `docker`, and they require additional parser to parse application logs in JSON format. Therefore, you should set different `containerRuntime` depending on your actual container runtime.

The default runtime is docker, but you can also choose other runtimes.

- If your container runtime is `containerd`:

```bash
helm install fluent-operator --create-namespace -n kubesphere-logging-system charts/fluent-operator/  --set containerRuntime=containerd
```

- If your container runtime is `cri-o`:

```bash
helm install fluent-operator --create-namespace -n kubesphere-logging-system charts/fluent-operator/  --set containerRuntime=crio
```

Run the following command to install Fluent Operator:

```bash
helm install fluent-operator --create-namespace -n kubesphere-logging-system https://github.com/fluent/fluent-operator/releases/download/< version >/fluent-operator.tgz
```

<Notice type='note'>

Please replace < version > with an actual version like v1.0.0.

</Notice>

## Install Fluent Operator with YAML

- Run the following command to install the latest stable version:

```bash
kubectl apply -f https://raw.githubusercontent.com/fluent/fluent-operator/release-1.0/manifests/setup/setup.yaml

# You can change the namespace in manifests/setup/kustomization.yaml in corresponding release branch 
# and then use command below to install to another namespace
# kubectl kustomize manifests/setup/ | kubectl apply -f -
```

- Run the following command to install the development version:

```bash
kubectl apply -f https://raw.githubusercontent.com/fluent/fluentbit-operator/master/manifests/setup/setup.yaml

# You can change the namespace in manifests/setup/kustomization.yaml 
# and then use command below to install to another namespace
# kubectl kustomize manifests/setup/ | kubectl apply -f -
```

- To configure Fluent Operator using YAML, please refer to [Setup](https://github.com/Bettygogo2021/fluent-operator/blob/master/manifests/setup/setup.yaml).
