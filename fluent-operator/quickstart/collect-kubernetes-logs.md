---
title: Collect Kubernetes Logs
linkTitle: Collect Kubernetes Logs
description: Describes how to collect Kubernetes logs.
keywords: Kubernetes, Fluent Bit, Fluentd, Fluent Operator

logo: 
weight: 4500


---

This section provisions a logging pipeline including the Fluent Bit DaemonSet and its log input/filter/output configurations to collect Kubernetes logs, involving container logs and kubelet logs.

https://github.com/fluent/fluent-operator/blob/master/docs/images/logging-stack.svg

## Prerequisites

- You need to install an ElasticSearch v5+ cluster to receive the log data.

- You need to replace [output-elasticsearch.yaml](https://github.com/fluent/fluent-operator/blob/master/manifests/logging-stack/output-elasticsearch.yaml) with your own ElasticSearch setup.

## Procedures

1. Run the following commands to deploy the logging stack with YAML:

```shell
kubectl apply -f manifests/setup/
kubectl apply -f manifests/logging-stack/fluentbit-fluentBit.yaml
kubectl apply -f manifests/logging-stack/fluentbitconfig-fluentBitConfig.yaml
```

2. Change the service log directory. Create directory `/var/log/journal `if it does not exist, and then restart the `systemd-journald` service.

```shell
mkdir /var/log/journal/
systemctl restart systemd-journald
```

3. Set up the Fluent Bit pipeline, and then the logs will be collected to the ElasticSearch.

```shell
kubectl create cm fluent-bit-lua -n kubesphere-logging-system --from-file=config/scripts/systemd.lua
kubectl apply -f manifests/logging-stack/input-systemd-kubelet.yaml
kubectl apply -f manifests/logging-stack/filter-systemd.yaml
kubectl apply -f manifests/logging-stack/output-elasticsearch.yaml
```

- If you want to collect the docker log, you can add the docker input.

  ```bash
  kubectl apply -f manifests/logging-stack/input-systemd-docker.yaml
  ```

- If you want to collect other service logs, such as containerd, you can add a input like the docker input, 
  and modify the systemdFilter.
  
  ```bash
   systemdFilter:
        - _SYSTEMD_UNIT=containerd.service
  ```
  
  
