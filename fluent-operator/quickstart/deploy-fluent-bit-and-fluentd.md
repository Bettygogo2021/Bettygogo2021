---
title: Deploy Fluent Bit and Fluentd
linkTitle: Deploy Fluent Bit and Fluentd
description: Instructs you to deploy Fluent Bit and Fluentd.
keywords: Kubernetes, Fluent Bit, Fluentd, Fluent Operator

logo: 
weight: 4400

---

This section instructs you to deploy Fluent Bit and Fluentd.

Both Fluent Bit and Fluentd are defined as CRDs in Fluent Operator, and you can create the Fluent Bit DaemonSet or the Fluentd StatefulSet by declaring FluentBit or Fluentd CR.

## Deploy Fluent Bit

```yaml
cat <<EOF | kubectl apply -f -
apiVersion: fluentbit.fluent.io/v1alpha2
kind: FluentBit
metadata:
  name: fluent-bit
  namespace: fluent
  labels:
    app.kubernetes.io/name: fluent-bit
spec:
  image: kubesphere/fluent-bit:v1.8.11
  positionDB:
    hostPath:
      path: /var/lib/fluent-bit/
  resources:
    requests:
      cpu: 10m
      memory: 25Mi
    limits:
      cpu: 500m
      memory: 200Mi
  fluentBitConfigName: fluent-bit-config
  tolerations:
    - operator: Exists
EOF
```

## Deploy Fluentd

```yaml
cat <<EOF | kubectl apply -f -
apiVersion: fluentd.fluent.io/v1alpha1
kind: Fluentd
metadata:
  name: fluentd
  namespace: fluent
  labels:
    app.kubernetes.io/name: fluentd
spec:
  globalInputs:
  - forward: 
      bind: 0.0.0.0
      port: 24224
  replicas: 3
  image: kubesphere/fluentd:v1.14.4
  fluentdCfgSelector: 
    matchLabels:
      config.fluentd.fluent.io/enabled: "true"
EOF
```
