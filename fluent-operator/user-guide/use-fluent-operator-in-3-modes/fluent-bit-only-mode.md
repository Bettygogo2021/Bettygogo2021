---
title: Fluent Bit Only mode
linkTitle: Fluent Bit Only mode
description: Describes how to use Fluent Operator in Fluent Bit Only mode.
keywords: Kubernetes, Fluent Bit, Fluentd, Fluent Operator

logo: 
weight: 5120

---

Fluent Bit is lightweight, and you can only use Fluent Bit to process logs.

##  Collect Kubernetes Application Logs and Send the Output to Kafka

```bash
cat <<EOF | kubectl apply -f -
apiVersion: fluentbit.fluent.io/v1alpha2
kind: ClusterFluentBitConfig
metadata:
  name: fluent-bit-config
  labels:
    app.kubernetes.io/name: fluent-bit
spec:
  service:
    parsersFile: parsers.conf
  inputSelector:
    matchLabels:
      fluentbit.fluent.io/enabled: "true"
      fluentbit.fluent.io/mode: "k8s"
  filterSelector:
    matchLabels:
      fluentbit.fluent.io/enabled: "true"
      fluentbit.fluent.io/mode: "k8s"
  outputSelector:
    matchLabels:
      fluentbit.fluent.io/enabled: "true"
      fluentbit.fluent.io/mode: "k8s"
---
apiVersion: fluentbit.fluent.io/v1alpha2
kind: ClusterInput
metadata:
  name: tail
  labels:
    fluentbit.fluent.io/enabled: "true"
    fluentbit.fluent.io/mode: "k8s"
spec:
  tail:
    tag: kube.*
    path: /var/log/containers/*.log
    parser: docker
    refreshIntervalSeconds: 10
    memBufLimit: 5MB
    skipLongLines: true
    db: /fluent-bit/tail/pos.db
    dbSync: Normal
---
apiVersion: fluentbit.fluent.io/v1alpha2
kind: ClusterFilter
metadata:
  name: kubernetes
  labels:
    fluentbit.fluent.io/enabled: "true"
    fluentbit.fluent.io/mode: "k8s"
spec:
  match: kube.*
  filters:
  - kubernetes:
      kubeURL: https://kubernetes.default.svc:443
      kubeCAFile: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
      kubeTokenFile: /var/run/secrets/kubernetes.io/serviceaccount/token
      labels: false
      annotations: false
  - nest:
      operation: lift
      nestedUnder: kubernetes
      addPrefix: kubernetes_
  - modify:
      rules:
      - remove: stream
      - remove: kubernetes_pod_id
      - remove: kubernetes_host
      - remove: kubernetes_container_hash
  - nest:
      operation: nest
      wildcard:
      - kubernetes_*
      nestUnder: kubernetes
      removePrefix: kubernetes_
---
apiVersion: fluentbit.fluent.io/v1alpha2
kind: ClusterOutput
metadata:
  name: kafka
  labels:
    fluentbit.fluent.io/enabled: "false"
    fluentbit.fluent.io/mode: "k8s"
spec:
  matchRegex: (?:kube|service)\.(.*)
  kafka:
    brokers: my-cluster-kafka-bootstrap.kafka.svc:9091,my-cluster-kafka-bootstrap.kafka.svc:9092,my-cluster-kafka-bootstrap.kafka.svc:9093
    topics: fluent-log
EOF
```

## Check the Configuration and Data

To check the output, refer to the following.

1. View the state of the Fluent Operator:

   ```bash
   kubectl get po -n fluent
   ```

2. View the generated Fluent Bit configurations:

   ```bash
   kubectl -n fluent get secrets fluent-bit-config -ojson | jq '.data."fluent-bit.conf"' | awk -F '"' '{printf $2}' | base64 --decode
   ```

   Information similar to the following is displayed:

   ```bash
   [Service]
       Parsers_File    parsers.conf
   [Input]
       Name    systemd
       Path    /var/log/journal
       DB    /fluent-bit/tail/docker.db
       DB.Sync    Normal
       Tag    service.docker
       Systemd_Filter    _SYSTEMD_UNIT=docker.service
   [Input]
       Name    systemd
       Path    /var/log/journal
       DB    /fluent-bit/tail/kubelet.db
       DB.Sync    Normal
       Tag    service.kubelet
       Systemd_Filter    _SYSTEMD_UNIT=kubelet.service
   [Input]
       Name    tail
       Path    /var/log/containers/*.log
       Refresh_Interval    10
       Skip_Long_Lines    true
       DB    /fluent-bit/tail/pos.db
       DB.Sync    Normal
       Mem_Buf_Limit    5MB
       Parser    docker
       Tag    kube.*
   [Filter]
       Name    lua
       Match    kube.*
       script    /fluent-bit/config/containerd.lua
       call    containerd
       time_as_table    true
   [Filter]
       Name    kubernetes
       Match    kube.*
       Kube_URL    https://kubernetes.default.svc:443
       Kube_CA_File    /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
       Kube_Token_File    /var/run/secrets/kubernetes.io/serviceaccount/token
       Labels    false
       Annotations    false
   [Filter]
       Name    nest
       Match    kube.*
       Operation    lift
       Nested_under    kubernetes
       Add_prefix    kubernetes_
   [Filter]
       Name    modify
       Match    kube.*
       Remove    stream
       Remove    kubernetes_pod_id
       Remove    kubernetes_host
       Remove    kubernetes_container_hash
   [Filter]
       Name    nest
       Match    kube.*
       Operation    nest
       Wildcard    kubernetes_*
       Nest_under    kubernetes
       Remove_prefix    kubernetes_
   [Filter]
       Name    lua
       Match    service.*
       script    /fluent-bit/config/systemd.lua
       call    add_time
       time_as_table    true
   [Output]
       Name    forward
       Match_Regex    (?:kube|service)\.(.*)
       Host    fluentd.fluent.svc
       Port    24224
   ```

3. View the generated Fluentd configurations:

   ```bash
   kubectl -n fluent get secrets fluentd-config -ojson | jq '.data."app.conf"' | awk -F '"' '{printf $2}' | base64 --decode 
   ```

   Information similar to the following is displayed:

   ```bash
   <source>
     @type  forward
     bind  0.0.0.0
     port  24224
   </source>
   <match **>
     @id  main
     @type  label_router
     <route>
       @label  @48b7cb809bc2361ba336802a95eca0d4
       <match>
         namespaces  kube-system,default
       </match>
     </route>
   </match>
   <label @48b7cb809bc2361ba336802a95eca0d4>
     <filter **>
       @id  ClusterFluentdConfig-cluster-fluentd-config::cluster::clusterfilter::fluentd-filter-0
       @type  record_transformer
       enable_ruby  true
       <record>
         kubernetes_ns  ${record["kubernetes"]["namespace_name"]}
       </record>
     </filter>
     <match **>
       @id  ClusterFluentdConfig-cluster-fluentd-config::cluster::clusteroutput::fluentd-output-es-0
       @type  elasticsearch
       host  elasticsearch-master.elastic.svc
       logstash_format  true
       logstash_prefix  fluent-log
       port  9200
     </match>
     <match **>
       @id  ClusterFluentdConfig-cluster-fluentd-config::cluster::clusteroutput::fluentd-output-kafka-0
       @type  kafka2
       brokers  my-cluster-kafka-bootstrap.kafka.svc:9091,my-cluster-kafka-bootstrap.kafka.svc:9092,my-cluster-kafka-bootstrap.kafka.svc:9093
       topic_key  kubernetes_ns
       use_event_time  true
       <format>
         @type  json
       </format>
     </match>
   </label>
   ```

4. Query the `kubernetes_ns buckets` of the ElasticSearch cluster:

```bash
kubectl -n elastic exec -it elasticsearch-master-0 -c elasticsearch --  curl -X GET "localhost:9200/fluent-log*/_search?pretty" -H 'Content-Type: application/json' -d '{                                                           
   "size" : 0,
   "aggs" : {
      "kubernetes_ns": {
         "terms" : {
           "field": "kubernetes.namespace_name.keyword"
         }
      }
   }
}'
```

5. Check the Kafka cluster:

```bash
# kubectl -n kafka exec -it my-cluster-kafka-0 -- bin/kafka-console-consumer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic <namespace or fluent-log>
```

<Notice type='note'>

Replace <namespace or fluent-log> with the actual topic name.

</Notice>

