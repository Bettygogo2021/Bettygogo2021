---
title: Fluentd Only Mode
linkTitle: Fluentd Only Mode
description: Describes how to use Fluent Operator in the Fluentd Only mode.
keywords: Kubernetes, Fluent Bit, Fluentd, Fluent Operator

logo: 
weight: 5110

---

## Use Fluentd to Receive Logs from HTTP and Send the Output to Stdout

You can use Fluentd only to receive logs via HTTP.

```bash
cat <<EOF | kubectl apply -f -
apiVersion: fluentd.fluent.io/v1alpha1
kind: Fluentd
metadata:
  name: fluentd-http
  namespace: fluent
  labels:
    app.kubernetes.io/name: fluentd
spec:
  globalInputs:
    - http: 
        bind: 0.0.0.0
        port: 9880
  replicas: 1
  image: kubesphere/fluentd:v1.14.4
  fluentdCfgSelector: 
    matchLabels:
      config.fluentd.fluent.io/enabled: "true"
   
---
apiVersion: fluentd.fluent.io/v1alpha1
kind: FluentdConfig
metadata:
  name: fluentd-only-config
  namespace: fluent
  labels:
    config.fluentd.fluent.io/enabled: "true"
spec:
  filterSelector:
    matchLabels:
      filter.fluentd.fluent.io/mode: "fluentd-only"
      filter.fluentd.fluent.io/enabled: "true"
  outputSelector:
    matchLabels:
      output.fluentd.fluent.io/mode: "true"
      output.fluentd.fluent.io/enabled: "true"

---
apiVersion: fluentd.fluent.io/v1alpha1
kind: Filter
metadata:
  name: fluentd-only-filter
  namespace: fluent
  labels:
    filter.fluentd.fluent.io/mode: "fluentd-only"
    filter.fluentd.fluent.io/enabled: "true"
spec: 
  filters: 
    - stdout: {}

---
apiVersion: fluentd.fluent.io/v1alpha1
kind: Output
metadata:
  name: fluentd-only-stdout
  namespace: fluent
  labels:
    output.fluentd.fluent.io/enabled: "true"
    output.fluentd.fluent.io/enabled: "true"
spec: 
  outputs: 
    - stdout: {}
EOF
```

Then you can send logs to the endpoint by using curl:

```bash
curl -X POST -d 'json={"foo":"bar"}' http://<localhost:9880>/app.log
```

<Notice type='note'>

Replace `localhost:9880` with the actual service or endpoint of Fluentd, for example, `fluentd-http.fluent.svc:9880`.

</Notice>



