---
title: Key Concepts
linkTitle: Key Concepts
description: Explains key concepts used in Fluent Operator.
keywords: Kubernetes, Fluent Operator, CRD

logo: 
weight: 2200



---

The following explains key concepts used in Fluent Operator.

## Concepts for Fluentd

- **`Fluentd`**: Defines the Fluentd Statefulset and its configs. A custom Fluentd image `kubesphere/fluentd` is required to work with Fluentd Operator for dynamic configuration reloading.
- **`FluentdConfig`**: Selects cluster-level or namespace-level input/filter/output plugins and generates the final config into a Secret.
- **`ClusterFluentdConfig`**: Selects cluster-level input/filter/output plugins and generates the final config into a Secret.
- **`Filter`**: Defines namespace-level filter config sections.
- **`ClusterFilter`**: Defines cluster-level filter config sections.
- **`Output`**: Defines namespace-level output config sections.
- **`ClusterOutput`**: Defines cluster-level output config sections.

## Concepts for Fluent Bit

- **`FluentBit`**: Defines the Fluent Bit DaemonSet and its configs. A custom Fluent Bit image `kubesphere/fluent-bit` is required to work with FluentBit Operator for dynamic configuration reloading.
- **`ClusterFluentBitConfig`**: Selects cluster-level input/filter/output plugins and generates the final config into a Secret.
- **`ClusterInput`**: Defines cluster-level input config sections.
- **`clusterParser`**: Defines cluster-level parser config sections.
- **`ClusterFilter`**: Defines cluster-level filter config sections.
- **`ClusterOutput`**: Defines cluster-level output config sections.

Each **`ClusterInput`**, **`ClusterParser`**, **`ClusterFilter`**, **`ClusterOutput`** represents a Fluent Bit config section, which is selected by **`ClusterFluentBitConfig`** via label selectors. Fluent Operator watches those objects, constructs the final config, and finally creates a Secret to store the config which will be mounted to the Fluent Bit DaemonSet. The following figure shows the workflow.

![fluent-bit-operator-workflow](/Users/bettygogo/Documents/GitHub/fluent-operator/docs/images/fluent-bit-operator-workflow.svg)

To ensure that Fluent Bit picks up and uses the latest config whenever the Fluent Bit config changes, Fluent Bit watcher is added to restart the Fluent Bit process as soon as Fluent Bit config changes are detected. In this case, the Fluent Bit pod does not need to be restarted to reload the new config. The Fluent Bit config is reloaded in this way because there is no reloading interface in Fluent Bit itself. For more information, please refer to this [known issue](https://github.com/fluent/fluent-bit/issues/365) for more details.

![fluentbit-operator](/Users/bettygogo/Documents/GitHub/fluent-operator/docs/images/fluentbit-operator.svg)

