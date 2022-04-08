---
title: Prerequisites
linkTitle: Prerequisites
description: Describes prerequisites for using Fluent Operator.
keywords: Kubernetes, Fluent Bit, Fluentd, Fluent Operator

logo: 
weight: 4100


---

The following describes prerequisites for using Fluent Operator.

To get hands-on experience of Fluent Operator, you need a Kind cluster. You also need to set up a Kafka cluster and an ElasticSearch cluster in this Kind cluster.

1. Create a Kind cluster named **fluent**:

   ```bash
   ./create-kind-cluster.sh
   ```

2. Create a Kafka cluster in the Kafka namespace:

   ```bash
   ./deploy-kafka.sh
   ```

3. Create an ElasticSearch cluster in the elastic namespace:

   ```bash
   ./deploy-es.sh
   ```

   

