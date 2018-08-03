# Zookeeper Operator Chart

## TL;DR;

Add the repo (if you haven't already):
```bash
$ helm repo add pravega https://pravega.github.io/charts
```

Install the driver:
```bash
$ helm install --name zookeeper-operator --namespace public
```

## Introduction

This chart installs the [Zookeeper Operator](http://github.com/pravega/zookeeper-operator) [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager. You can use the operator create and manage zookeeper clusters. The operator will install the associated resources (stateful-sets, config-maps, etc.) and handles changes made to the cluster. For more information check the [Zookeeper Operator README](http://github.com/pravega/zookeeper-operator).

## Prerequisites

- Kubernetes 1.9+ with Beta APIs enabled

If you're running on GKE, be sure to enable cluster role bindings:

```shell
$ kubectl create clusterrolebinding your-user-cluster-admin-binding --clusterrole=cluster-admin --user=<your.google.cloud.email@example.org>
```

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release zookeeper-operator
```
> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release [--purge]
```

The command removes all the Kubernetes components associated with the chart and deletes the release. The purge option also removes the provisioned release name, so that the name itself can also be reused.
