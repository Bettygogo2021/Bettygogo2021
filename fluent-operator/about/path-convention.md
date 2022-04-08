---
title: Path Convention
linkTitle: Path Convention
description: Introduce the path convention of Fluent Operator.
keywords: Kubernetes, path convention, Fluent Operator

logo: 
weight: 2400


---

## Path Convention

Fluent Operator adopts the following convention internally.

| Directory Path                    | Description                                                  |
| --------------------------------- | ------------------------------------------------------------ |
| /fluent-bit/tail                  | Stores `tail` related files, for example, file tracking db. Using [fluentbit.spec.positionDB](https://github.com/fluent/fluent-operator/blob/master/docs/fluentbit.md#fluentbitspec) will mount a file `pos.db` under this directory by default. |
| /fluent-bit/secrets/{secret_name} | Stores Secrets , for example,  TLS files. Specify Secrets to mount in [fluentbit.spec.secrets](https://github.com/fluent/fluent-operator/blob/master/docs/fluentbit.md#fluentbitspec), then you have access. |
| /fluent-bit/config                | Stores the main config file and user-defined parser config file. |


<Notice type='note'>

Note that service account files are mounted to `/var/run/secrets/kubernetes.io/serviceaccount`.

</Notice>
