---
title: Overview
linkTitle: Overview
description: Briefly introduces Fluent Operator.
keywords: Kubernetes, Fluent Bit, Fluentd, Fluent Operator

logo: 
weight: 2110


---

Fluent Operator, formerly known as Fluentbit-operator, is donated by the KubeSphere community and accepted as a sub-project of the Fluent community. It collect logs from each node via the Fluent Bit DaemonSet, which then forwards the logs to Fluentd for aggregation and forwarding to its sinks such as ElasticSearch, Kafka, Loki, S3, Splunk, or other log analysis tools.

Fluent Bit is a good choice as a logging agent because it is lightweight and efficient, while Fluentd is more powerful to perform advanced processing of logs because of its rich plugins. Although both Fluent Bit and Fluentd are able to collect, parse, filter and then forward logs to the final sinks, Fluent Operator provides feature-rich plugins for both Fluent Bit and Fluentd, which automates the setup and allows you to use  Fluent Bit and Fluentd as you like. The following three modes are supported:

- Fluent Bit only mode: If you just need to collect logs and send them to the final sinks, all you need is Fluent Bit.

- Fluent Bit + Fluentd mode: If you need to perform some advanced processing of the collected logs or send the logs to more sinks, this mode is recommended.
- Fluentd only mode: If you need to receive logs through network like HTTP or Syslog and then process and send the logs to the final sinks, you only need Fluentd.

Fluent Bit will be deployed as a DaemonSet while Fluentd will be deployed as a StatefulSet. The following figure shows the workflow, which involves input and forward, filter, and output.

![fluent-operator](/Users/bettygogo/Documents/GitHub/fluent-operator/docs/images/fluent-operator.svg)

- Input and forward

  When Fluent Bit and Fluentd are separately deployed, you can collect logs by using input plugins of Fluent Bit and forward and http plugins. When both Fluent Bit and Fluentd are deployed, Fluent Bit collects and forwards logs on each node.

- Filter

  As collected logs usually contain redundant information, it is necessary that the log processing tools are capable of filtering redundant information. Both Fluent Bit and Fluentd support filter plugins, so that you can view only the information that interests you.

- Output

  Output plugins of Fluent Bit and Fluentd forward logs to other sinks such as ElasticSearch, Kafka, Loki, S3, Splunk.

