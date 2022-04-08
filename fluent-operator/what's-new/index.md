---
title: What's New
linkTitle: What's New
description: Describes new features of Fluent Operator.
keywords: Kubernetes, Fluent Bit, Fluentd, Fluent Operator

logo: 
weight: 1000

---

## Release 1.0.0 (2022-03-25)

### Breaking Changes

- Change all Fluent Bit CRDs from namespace-scope to cluster-scope.
- Add CRDs and controllers for Fluentd.

### Features

- Add priority class to Fluent Bit type. ([#146](https://github.com/fluent/fluent-operator/pull/146))
- Add support for Fluent Bit RetryLimit in outputs. ([#148](https://github.com/fluent/fluent-operator/pull/148))
- Add Fluent Bit Datadog output. ([#149](https://github.com/fluent/fluent-operator/pull/149))
- Add support for Fluent Bit rewrite tag. ([#155](https://github.com/fluent/fluent-operator/pull/155))
- Add Fluent Bit multiline logs support. ([#172](https://github.com/fluent/fluent-operator/pull/172))
- Add the Fluent Bit AWS Filter  plugin. ([#173](https://github.com/fluent/fluent-operator/pull/173))
- Add the Fluent Bit multiline filter plugin. ([#176](https://github.com/fluent/fluent-operator/pull/176))
- Add Fluent Bit Firehose plugin support. ([#178](https://github.com/fluent/fluent-operator/pull/178))
- Add Fluent Bit volume CRD. ([#186](https://github.com/fluent/fluent-operator/pull/186))
- Rename Fluentbit Operator to Fluent Operator. ([#189](https://github.com/fluent/fluent-operator/pull/189) [#190](https://github.com/fluent/fluent-operator/issues/190))
- Add more Fluentd examples. ([#194](https://github.com/fluent/fluent-operator/pull/194))
- Add Fluentd to helm charts. ([#204](https://github.com/fluent/fluent-operator/pull/204) [#208](https://github.com/fluent/fluent-operator/pull/208) )
- Encrypt sensitive information for the Fluentd output plugin. ([#219](https://github.com/fluent/fluent-operator/pull/219))
- Enable multi-workers in one Fluentd pod. ([#194](https://github.com/fluent/fluent-operator/pull/194))
- Integrate e2e/function tests for generating Fluentd configuration. ([#203](https://github.com/fluent/fluent-operator/pull/203) [#206](https://github.com/fluent/fluent-operator/pull/206) )
- Refine documents. ([#199](https://github.com/fluent/fluent-operator/pull/199) [#228](https://github.com/fluent/fluent-operator/pull/228))
- Refactor multi images/binaries build and add github CI. ([#152](https://github.com/fluent/fluent-operator/pull/152) [#154](https://github.com/fluent/fluent-operator/pull/154) [#213](https://github.com/fluent/fluent-operator/pull/213) [#214](https://github.com/fluent/fluent-operator/pull/214))
- Add CI templates. ([#248](https://github.com/fluent/fluent-operator/pull/248))
- Add the Time_Key_Nanos field. ([#250](https://github.com/fluent/fluent-operator/pull/250))

### Enhancements

- Set the crictl path to a variable. ([#181](https://github.com/fluent/fluent-operator/pull/181))
- Improve the Fluent Bit Kafka plugin. ([#182](https://github.com/fluent/fluent-operator/pull/182))

### Bug Fixes

- Fix the incorrect key of the Fluent Bit es parser plugin. ([#164](https://github.com/fluent/fluent-operator/pull/164))
- Fix the incorrect keys of the Fluent Bit es output plugin. ([#160](https://github.com/fluent/fluent-operator/pull/160))
- Fix the initcontainer script. ([#202](https://github.com/fluent/fluent-operator/pull/202))
- Refine Fluentd CRs status. ([#225](https://github.com/fluent/fluent-operator/pull/225))
- Fix ci and make the repository importable and downloadable. ([#229](https://github.com/fluent/fluent-operator/pull/229))
- Fix codegen and add support for verifying codegen. ([#234](https://github.com/fluent/fluent-operator/pull/234) [#238](https://github.com/fluent/fluent-operator/pull/238))
- Fix and optimize Helm. ([#236](https://github.com/fluent/fluent-operator/pull/236) [#245](https://github.com/fluent/fluent-operator/pull/245) [#246](https://github.com/fluent/fluent-operator/pull/246))

## Release 0.13.0  (2022-3-14)

### Features

- Add priority class to Fluent Bit type. [#146](https://github.com/fluent/fluent-operator/pull/146)
- Add support for Fluent Bit RetryLimit in outputs. [#148](https://github.com/fluent/fluent-operator/pull/148)
- Add Fluent Bit Datadog output. [#149](https://github.com/fluent/fluent-operator/pull/149)
- Add main workflow actions. [#152](https://github.com/fluent/fluent-operator/pull/152)
- Add support for the rewrite tag. [#155](https://github.com/fluent/fluent-operator/pull/155)
- Add the AWS Filter plugin. [#173](https://github.com/fluent/fluent-operator/pull/173)
- Add the multiline filter plugin. [#176](https://github.com/fluent/fluent-operator/pull/176)
- Add Firehose plugin support. [#178](https://github.com/fluent/fluent-operator/pull/178)
- Add the volume CRD. [#186](https://github.com/fluent/fluent-operator/pull/186)

### Enhancements

- Upgrade layout from Kubebuilder v2 to Kubebuilder v3.1. [#147](https://github.com/fluent/fluent-operator/pull/147)
- Set the crictl path to a variable. [#181](https://github.com/fluent/fluent-operator/pull/181)
- Improve the Fluent Bit kafka plugin. [#182](https://github.com/fluent/fluent-operator/pull/182)

### Bug Fixes

- Fix the incorrect keys of the Fluent Bit es output plugin. [#160](https://github.com/fluent/fluent-operator/pull/160)
- Fix the incorrect key of the Fluent Bit es parser plugin. [#164](https://github.com/fluent/fluent-operator/pull/164)

## Release 0.12.0 (2021-09-07)

### Features

- Add support for collecting contained and cri-o service logs. [#142](https://github.com/fluent/fluent-operator/pull/142)

### Enhancements

- Optionally enable namespace-scope RBAC. [#137](https://github.com/fluent/fluent-operator/pull/137)

### Bug Fixes

- Update CRD YAML. [#136](https://github.com/fluent/fluent-operator/pull/136)

## Release 0.11.0 (2021-09-01)

### Features

- Add support for Read_From_Head to the tail input plugin. [#129](https://github.com/fluent/fluent-operator/pull/129)

### Enhancements

- Improve the Canonical Config. [#131](https://github.com/fluent/fluent-operator/pull/131)

### Bug Fixes

- Adjust FluentBit permissions. [#133](https://github.com/fluent/fluent-operator/pull/133)

## Release 0.10.0 (2021-08-20)

### Features

- Add support for polling in fluent-bit-watcher. [#126](https://github.com/fluent/fluent-operator/pull/126)
- Add support for Amazon ElasticSearch Service and Elastic's Elasticsearch Service. [#125](https://github.com/fluent/fluent-operator/pull/125)

## Release 0.9.0 (2021-08-13)

### Features

- Add support for `Containerd` and `CRI-O`. [#112](https://github.com/fluent/fluent-operator/pull/112)
- Add support for `inotify_watcher` configuration of tail input plugin. [#114](https://github.com/fluent/fluent-operator/pull/114)
- Add support for throttle filter plugin. [#115](https://github.com/fluent/fluent-operator/pull/115)
- Add `runtimeClassName` support to Fluent Bit CRD. [#116](https://github.com/fluent/fluent-operator/pull/116)
- Provide the optional `watch-namespaces` for controller manager. [#117](https://github.com/fluent/fluent-operator/pull/117)

### Bug Fixes

- Fix some bugs.

## Release 0.8.0 (2021-07-23)

### Features

- Support the ability to set imagePullSecrets for both operator and Fluent Bit. [#93](https://github.com/fluent/fluent-operator/pull/93) [#94](https://github.com/fluent/fluent-operator/pull/94)
- Add switch to input.tail.memBufLimit in helm chart. [#87](https://github.com/fluent/fluent-operator/pull/87)

### Enhancements

- Use hostpath instead of emptydir to store position db. [#72](https://github.com/fluent/fluent-operator/pull/72)
- Improve the fluent-bit-watcher synchronization mechanism. [#74](https://github.com/fluent/fluent-operator/pull/74)
- Terminate the fluent-bit process in a more elegant way in fluent-bit-watcher. [#90](https://github.com/fluent/fluent-operator/pull/90)
- Update README and the roadmap. [#97](https://github.com/fluent/fluent-operator/pull/97) [#100](https://github.com/fluent/fluent-operator/pull/100)
- Add the kustomize file to manifests. [#99](https://github.com/fluent/fluent-operator/pull/99)

### Bug Fixes

- Fix the bug that the forward output can only use the default port. [#89](https://github.com/fluent/fluent-operator/pull/89)
- Fix the bug that logs are lost when the DamemonSet restarts. [#90](https://github.com/fluent/fluent-operator/pull/90)
- Update groupname for client-gen to fluentbit.fluent.io. [#95](https://github.com/fluent/fluent-operator/pull/95)

## Release 0.7.1 (2021-07-08)

### Features

- Add hostpath to sample configurations and manifests. [#72](https://github.com/fluent/fluent-operator/pull/72)

### Enhancements

- Improve goroutine synchronization of fluent-bit-watcher. [#74](https://github.com/fluent/fluent-operator/pull/74)

### Bug Fixes

- Fix the bug that Fluent Operator may crash during plugin loading. [#77](https://github.com/fluent/fluent-operator/pull/77)

## Release 0.7.0 (2021-06-29)

### Features

- Add fluent-bit-watcher. [#62](https://github.com/fluent/fluent-operator/pull/62).

### Enhancements

- Add support for plugin alias property in input and output specs. [#64](https://github.com/fluent/fluent-operator/pull/64).

## Release 0.6.2 (2021-06-11)

### Enhancements

- Update Kubernetes dependencies.
- Update Fluent Bit resources in manifest.

## Release 0.6.1 (2021-06-11

- Provide erroneous release with no changes from 0.6.0.

## Release 0.6.0 (2021-06-01)

### Features

- Update the default Fluent Bit version to v1.7.3.

### Enhancements

- Add Kubernetes Go client.
- Support syslog output.

## Release 0.5.0 (2021-04-14)

### Enhancements

- Support for audit logs.

## Release 0.4.0 (2021-04-01)

### Features

- Update the default Fluent Bit version to v1.6.9.

### Enhancements

- Support systemd input and Lua filter.
- Support Loki output.
- Support the ability to set affinity and resource for Fluent Bit DaemonSets.

### Bug Fixes

- Fix some bugs.

## Release 0.3.0 (2020-11-10)

### Features

- Support Parser plugin.

### Enhancements

- Support File, TCP, and HTTP outputs.

## Release 0.2.0 (2020-08-27)

### Features

- Rewrite relevant CRDs that are backwards incompatible with v0.1.0.
- Use kubebuilder as the building framework.

## Release 0.1.0 (2020-02-17)

- This is the first release of Fluent Operator.
