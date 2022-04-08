---
title: Fluent Bit + Fluentd Mode
linkTitle: Fluent Bit + Fluentd Mode
description: Describes how to use Fluent Operator in the Fluent Bit + Fluentd mode.
keywords: Kubernetes, Fluent Bit, Fluentd, Fluent Operator

logo: 
weight: 5130

---

With its rich plugins, Fluentd acts as a log aggregation layer and is able to perform more advanced log processing. You can forward logs from Fluent Bit to Fluentd with ease using Fluent Operator.

## Forward Logs from Fluent Bit to Fluentd

To forward logs from Fluent Bit to Fluentd, you need to enable the Fluent Bit forward plugin.

```bash
cat <<EOF | kubectl apply -f -
apiVersion: fluentbit.fluent.io/v1alpha2
kind: ClusterOutput
metadata:
  name: fluentd
  labels:
    fluentbit.fluent.io/enabled: "true"
    fluentbit.fluent.io/component: logging
spec:
  matchRegex: (?:kube|service)\.(.*)
  forward:
    host: fluentd.fluent.svc
    port: 24224
EOF
```

## Enable Fluentd Forward Input plugin to receive logs from Fluent Bit

The Fluentd forward Input plugin has been enabled by default when you deploy Fluentd, so you do not need to manually enable it.

```yaml
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
  replicas: 1
  image: kubesphere/fluentd:v1.14.4
  fluentdCfgSelector: 
    matchLabels:
      config.fluentd.fluent.io/enabled: "true"
```

## ClusterFluentdConfig: Fluentd Cluster-wide Configuration

You can send logs Fluentd received from Fluent Bit to a cluster-wide sink defined by a `ClusterOutput`.

```bash
cat <<EOF | kubectl apply -f -
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterFluentdConfig
metadata:
  name: cluster-fluentd-config
  labels:
    config.fluentd.fluent.io/enabled: "true"
spec:
  watchedNamespaces: 
  - kube-system
  - default
  clusterOutputSelector:
    matchLabels:
      output.fluentd.fluent.io/scope: "cluster"
      output.fluentd.fluent.io/enabled: "true"
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterOutput
metadata:
  name: cluster-fluentd-output-es
  labels:
    output.fluentd.fluent.io/scope: "cluster"
    output.fluentd.fluent.io/enabled: "true"
spec: 
  outputs: 
  - elasticsearch:
      host: elasticsearch-master.elastic.svc
      port: 9200
      logstashFormat: true
      logstashPrefix: fluent-log-cluster-fd
EOF
```

## FluentdConfig: Fluentd Namespace-wide Configuration

You can send logs Fluentd received from Fluent Bit to a namespace-wide sink defined by an `Output`. Only logs in the same namespace as `FluentdConfig` will be sent to the `Output`.

```bash
cat <<EOF | kubectl apply -f -
apiVersion: fluentd.fluent.io/v1alpha1
kind: FluentdConfig
metadata:
  name: namespace-fluentd-config
  namespace: fluent
  labels:
    config.fluentd.fluent.io/enabled: "true"
spec:
  outputSelector:
    matchLabels:
      output.fluentd.fluent.io/scope: "namespace"
      output.fluentd.fluent.io/enabled: "true"
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: Output
metadata:
  name: namespace-fluentd-output-es
  namespace: fluent
  labels:
    output.fluentd.fluent.io/scope: "namespace"
    output.fluentd.fluent.io/enabled: "true"
spec: 
  outputs: 
  - elasticsearch:
      host: elasticsearch-master.elastic.svc
      port: 9200
      logstashFormat: true
      logstashPrefix: fluent-log-namespace-fd
EOF
```

## Use Cluster-wide and Namespace-wide FluentdConfig Together

You can use `FluentdConfig` and `ClusterFluentdConfig` together. `FluentdConfig` will send log of the `Fluent` namespace to the `ClusterOutput`, and `ClusterFluentdConfig` will send log of the global `watchedNamespaces` to the `ClusterOutput`.

```bash
cat <<EOF | kubectl apply -f -
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterFluentdConfig
metadata:
  name: cluster-fluentd-config-hybrid
  labels:
    config.fluentd.fluent.io/enabled: "true"
spec:
  watchedNamespaces: 
  - kube-system
  - default
  clusterOutputSelector:
    matchLabels:
      output.fluentd.fluent.io/scope: "hybrid"
      output.fluentd.fluent.io/enabled: "true"
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: FluentdConfig
metadata:
  name: namespace-fluentd-config-hybrid
  namespace: fluent
  labels:
    config.fluentd.fluent.io/enabled: "true"
spec:
  clusterOutputSelector:
    matchLabels:
      output.fluentd.fluent.io/scope: "hybrid"
      output.fluentd.fluent.io/enabled: "true"
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterOutput
metadata:
  name: cluster-fluentd-output-es-hybrid
  labels:
    output.fluentd.fluent.io/scope: "hybrid"
    output.fluentd.fluent.io/enabled: "true"
spec: 
  outputs: 
  - elasticsearch:
      host: elasticsearch-master.elastic.svc
      port: 9200
      logstashFormat: true
      logstashPrefix: fluent-log-hybrid-fd
EOF
```

## Use Cluster-wide and Namespace-wide FluentdConfig Together in Multi-Tenant Scenarios

```bash
cat <<EOF | kubectl apply -f -
apiVersion: fluentd.fluent.io/v1alpha1
kind: FluentdConfig
metadata:
  name: namespace-fluentd-config-user1
  namespace: fluent
  labels:
    config.fluentd.fluent.io/enabled: "true"
spec:
  outputSelector:
    matchLabels:
      output.fluentd.fluent.io/enabled: "true"
      output.fluentd.fluent.io/user: "user1"
  clusterOutputSelector:
    matchLabels:
      output.fluentd.fluent.io/enabled: "true"
      output.fluentd.fluent.io/user: "user1"
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterFluentdConfig
metadata:
  name: cluster-fluentd-config-cluster-only
  labels:
    config.fluentd.fluent.io/enabled: "true"
spec:
  watchedNamespaces: 
  - kube-system
  - kubesphere-system
  clusterOutputSelector:
    matchLabels:
      output.fluentd.fluent.io/enabled: "true"
      output.fluentd.fluent.io/scope: "cluster-only"
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: Output
metadata:
  name: namespace-fluentd-output-user1
  namespace: fluent
  labels:
    output.fluentd.fluent.io/enabled: "true"
    output.fluentd.fluent.io/user: "user1"
spec: 
  outputs: 
  - elasticsearch:
      host: elasticsearch-master.elastic.svc
      port: 9200
      logstashFormat: true
      logstashPrefix: fluent-log-user1-fd
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterOutput
metadata:
  name: cluster-fluentd-output-user1
  labels:
    output.fluentd.fluent.io/enabled: "true"
    output.fluentd.fluent.io/user: "user1"
spec: 
  outputs: 
  - elasticsearch:
      host: elasticsearch-master.elastic.svc
      port: 9200
      logstashFormat: true
      logstashPrefix: fluent-log-cluster-user1-fd
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterOutput
metadata:
  name: cluster-fluentd-output-cluster-only
  labels:
    output.fluentd.fluent.io/enabled: "true"
    output.fluentd.fluent.io/scope: "cluster-only"
spec: 
  outputs: 
  - elasticsearch:
      host: elasticsearch-master.elastic.svc
      port: 9200
      logstashFormat: true
      logstashPrefix: fluent-log-cluster-only-fd
EOF
```

## Route Logs to Different Kafka Topics Based on Namespace

You can use the Fluentd filter to route logs to different topics based on namespace.

```bash
cat <<EOF | kubectl apply -f -
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterFluentdConfig
metadata:
  name: cluster-fluentd-config-kafka
  labels:
    config.fluentd.fluent.io/enabled: "true"
spec:
  watchedNamespaces: 
  - kube-system
  - default
  clusterFilterSelector:
    matchLabels:
      filter.fluentd.fluent.io/type: "k8s"
      filter.fluentd.fluent.io/enabled: "true"
  clusterOutputSelector:
    matchLabels:
      output.fluentd.fluent.io/type: "kafka"
      output.fluentd.fluent.io/enabled: "true"
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterFilter
metadata:
  name: cluster-fluentd-filter-k8s
  labels:
    filter.fluentd.fluent.io/type: "k8s"
    filter.fluentd.fluent.io/enabled: "true"
spec: 
  filters: 
  - recordTransformer:
      enableRuby: true
      records:
      - key: kubernetes_ns
        value: ${record["kubernetes"]["namespace_name"]}
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterOutput
metadata:
  name: cluster-fluentd-output-kafka
  labels:
    output.fluentd.fluent.io/type: "kafka"
    output.fluentd.fluent.io/enabled: "true"
spec: 
  outputs: 
  - kafka:
      brokers: my-cluster-kafka-bootstrap.default.svc:9091,my-cluster-kafka-bootstrap.default.svc:9092,my-cluster-kafka-bootstrap.default.svc:9093
      useEventTime: true
      topicKey: kubernetes_ns
EOF
```

## Use Buffer for Fluentd Output

You can add the `buffer` section to cache logs for the output plugins.

```bash
cat <<EOF | kubectl apply -f -
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterFluentdConfig
metadata:
  name: cluster-fluentd-config-buffer
  labels:
    config.fluentd.fluent.io/enabled: "true"
spec:
  watchedNamespaces: 
  - kube-system
  - default
  clusterFilterSelector:
    matchLabels:
      filter.fluentd.fluent.io/type: "buffer"
      filter.fluentd.fluent.io/enabled: "true"
  clusterOutputSelector:
    matchLabels:
      output.fluentd.fluent.io/type: "buffer"      
      output.fluentd.fluent.io/enabled: "true"
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterFilter
metadata:
  name: cluster-fluentd-filter-buffer
  labels:
    filter.fluentd.fluent.io/type: "buffer"
    filter.fluentd.fluent.io/enabled: "true"
spec: 
  filters: 
  - recordTransformer:
      enableRuby: true
      records:
      - key: kubernetes_ns
        value: ${record["kubernetes"]["namespace_name"]}
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: ClusterOutput
metadata:
  name: cluster-fluentd-output-buffer
  labels:
    output.fluentd.fluent.io/type: "buffer"      
    output.fluentd.fluent.io/enabled: "true"
spec: 
  outputs: 
  - stdout: {}
    buffer:
      type: file
      path: /buffers/stdout.log
  - elasticsearch:
      host: elasticsearch-master.elastic.svc
      port: 9200
      logstashFormat: true
      logstashPrefix: fluent-log-buffer-fd
    buffer:
      type: file
      path: /buffers/es.log
EOF
```

## 
