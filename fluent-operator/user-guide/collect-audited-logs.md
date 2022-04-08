---
title: Collect Audited Logs
linkTitle: Collect Audited Logs
description: Describes how to collect audited logs.
keywords: Kubernetes, Fluent Bit, Fluentd, Fluent Operator

logo: 
weight: 5300

---

This section describes how to collect audited logs.

The Linux audit framework provides a CAPP-compliant auditing system that reliably collects information about events on a system. Refer to `manifests/logging-stack/auditd`, it supports a method for collecting audit logs from the Linux audit framework.

Run the following command to deploy the logging stack.

```bash
kubectl apply -f manifests/logging-stack/auditd

# You can change the namespace in manifests/logging-stack/auditd/kustomization.yaml 
# and then use command below to install to another namespace
# kubectl kustomize manifests/logging-stack/auditd/ | kubectl apply -f -
```

Within a couple of minutes, you can view an index available:

```bash
$ curl localhost:9200/_cat/indices
green open ks-logstash-log-2021.04.06 QeI-k_LoQZ2h1z23F3XiHg  5 1 404879 0 298.4mb 149.2mb
```
