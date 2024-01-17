# Deploying Pravega-Operator

Here, we briefly describe how to install [pravega-operator](https://github.com/pravega/pravega-operator) that is used to create/configure/manage Pravega clusters atop Kubernetes.

## Prerequisites
  - Kubernetes 1.16+ with Beta APIs
  - Helm 3.2.1+
  - Cert-Manager v1.0+ or some other certificate management solution in order to manage the webhook service certificates. This can be easily deployed by referring to [this](https://cert-manager.io/docs/installation/kubernetes/)
  - An Issuer and a Certificate (either self-signed or CA signed) in the same namespace that the Pravega Operator will be installed (refer to [this certificate.yaml file](https://github.com/pravega/pravega-operator/blob/master/config/certmanager/certificate.yaml) manifest to create a self-signed certificate in the default namespace)

## Installing Pravega-Operator

> Note: If you are running on Google Kubernetes Engine (GKE), please [check this first](https://github.com/pravega/pravega-operator/blob/master/doc/development.md#installation-on-google-kubernetes-engine).

To install the pravega-operator chart, use the following commands:

```
$ helm repo add pravega https://charts.pravega.io
$ helm repo update
$ helm install [RELEASE_NAME] pravega/pravega-operator --version=[VERSION] --set webhookCert.certName=[CERT_NAME] --set webhookCert.secretName=[SECRET_NAME]
```
where:

- **[RELEASE_NAME]** is the release name for the pravega-operator chart.
- **[VERSION]** can be any stable release version for pravega-operator from 0.5.0 onwards.
- **[CERT_NAME]** is the name of the certificate created as a prerequisite
- **[SECRET_NAME]** is the name of the secret created by the above certificate

This command deploys a pravega-operator on the Kubernetes cluster in its default configuration. The [configuration](#operator-configuration) section lists the parameters that can be configured during installation.

>Note: If we provide [RELEASE_NAME] same as chart name, deployment name will be same as release-name. But if we are providing a different name for release(other than pravega-operator in this case), deployment name will be [RELEASE_NAME]-[chart-name]. However, deployment name can be overridden by providing `--set  fullnameOverride=[DEPLOYMENT_NAME]` along with helm install command

>Note: If the pravega-operator version is 0.4.5, webhookCert.certName and webhookCert.secretName should not be set. Also in this case, bookkeeper operator, cert-manager and the certificate/issuer do not need to be deployed as prerequisites.

## Version Compatibility

The following table lists which version of the pravega operator chart would install which version of the pravega operator by default.

| Chart Version | App Version |
| :-----------: | :---------: |
| 0.4.5 | 0.4.5 |
| 0.5.0 | 0.5.0 |
| 0.5.1 | 0.5.1 |
| 0.5.2 | 0.5.2 |
| 0.5.3 | 0.5.3 |
| 0.6.0 | 0.5.4 |

The following table captures compatibility between the App Versions i.e. the pravega operator versions vs. the Chart Versions.
> NOTE: From Chart Version 0.6.0 onwards, the App Version and the Chart Version fields have been decoupled.

| Chart Version \ Operator Version | 0.4.x | 0.5.x |
| :------------------------------: | :---: | :---: |
| 0.4.x | Yes | No |
| 0.5.x | No | Yes |
| 0.6.x | No | Yes |

The following table captures compatibility between pravega operator charts with pravega charts

| Pravega Chart \ Pravega Operator Chart Version | 0.4.x | 0.5.x | 0.6.x |
| :----------------------------------------------------: | :---: | :---: | :---: |
| pravega-bk-0.5.x | Yes | No | No |
| pravega-bk-0.6.x | Yes | No | No |
| pravega-bk-0.7.x | Yes | No | No |
| pravega-0.5.x | No | Yes | Yes |
| pravega-0.6.x | No | Yes | Yes |
| pravega-0.7.x | No | Yes | Yes |
| pravega-0.8.x | No | Yes | Yes |
| pravega-0.9.x | No | Yes | Yes |
| pravega-0.10.x | No | Yes | Yes |

## Upgrading Pravega-Operator

For upgrading pravega-operator, please refer [upgrade guide](https://github.com/pravega/pravega-operator/blob/master/doc/operator-upgrade.md)

## Uninstalling Pravega-Operator

To uninstall/delete the pravega-operator chart, use the following command:

```
$ helm uninstall [RELEASE_NAME]
```

This command removes all the Kubernetes components associated with the chart and deletes the release.

## Operator Configuration

The following table lists the configurable parameters of the pravega-operator chart and their default values.

| Parameter | Description | Default |
| ----- | ----------- | ------ |
| `image.repository` | Image repository | `pravega/pravega-operator` |
| `image.tag` | Image tag | `0.5.7` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `crd.create` | Create pravega CRD | `true` |
| `rbac.create` | Create RBAC resources | `true` |
| `serviceAccount.create` | Create service account | `true` |
| `serviceAccount.name` | Name for the service account | `pravega-operator` |
| `testmode.enabled` | Enable test mode | `false` |
| `webhookCert.crt` | tls.crt value corresponding to the certificate | |
| `webhookCert.key` | tls.key value corresponding to the certificate | |
| `webhookCert.generate` | Whether to generate the certificate and the issuer (set to false while using self-signed certificates) | `false` |
| `webhookCert.certName` | Name of the certificate, if generate is set to false | `selfsigned-cert` |
| `webhookCert.secretName` | Name of the secret created by the certificate, if generate is set to false | `selfsigned-cert-tls` |
| `watchNamespace` | Namespaces to be watched  | `""` |
