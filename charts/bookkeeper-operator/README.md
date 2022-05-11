# Deploying Bookkeeper-Operator

Here, we briefly describe how to install [Bookkeeper Operator](https://github.com/pravega/bookkeeper-operator) that is used to create/configure/manage Bookkeeper clusters atop Kubernetes.

## Prerequisites
  - Kubernetes 1.16+ with Beta APIs
  - Helm 3.2.1+
  - Cert-Manager v1.0+ or some other certificate management solution in order to manage the webhook service certificates. This can be easily deployed by referring to [this](https://cert-manager.io/docs/installation/kubernetes/)
  - An Issuer and a Certificate (either self-signed or CA signed) in the same namespace that the Bookkeeper Operator will be installed (refer to [this](https://github.com/pravega/bookkeeper-operator/blob/master/config/certmanager/certificate.yaml) manifest to create a self-signed certificate in the default namespace)

## Installing Bookkeeper-Operator

> Note: If you are running on Google Kubernetes Engine (GKE), please [check this first](https://github.com/pravega/bookkeeper-operator/blob/master/doc/development.md#installation-on-google-kubernetes-engine).
To install the bookkeeper-operator chart, use the following commands:

```
$ helm repo add pravega https://charts.pravega.io
$ helm repo update
$ helm install [RELEASE_NAME] pravega/bookkeeper-operator --version=[VERSION] --set webhookCert.certName=[CERT_NAME] --set webhookCert.secretName=[SECRET_NAME]
```
where:

- **[RELEASE_NAME]** is the release name for the bookkeeper-operator chart.
- **[VERSION]** can be any stable chart version for bookkeeper-operator from 0.1.3 onwards.
- **[CERT_NAME]** is the name of the certificate created as a prerequisite
- **[SECRET_NAME]** is the name of the secret created by the above certificate

This command deploys a bookkeeper-operator on the Kubernetes cluster in its default configuration. The [configuration](#operator-configuration) section lists the parameters that can be configured during installation.

> Note: If we provide [RELEASE_NAME] same as chart name, deployment name will be same as release-name. But if we are providing a different name for release (other than bookkeeper-operator in this case), deployment name will be [RELEASE_NAME]-[chart-name]. However, deployment name can be overridden by providing `--set  fullnameOverride=[DEPLOYMENT_NAME]` along with helm install command


> Note: If the bookkeeper-operator version is 0.1.2, webhookCert.certName and webhookCert.secretName should not be set. Also in this case, cert-manager and the certificate/issuer do not need to be deployed as prerequisites.

## Upgrading Bookkeeper-Operator

For upgrading bookkeeper-operator, please refer [upgrade guide](https://github.com/pravega/bookkeeper-operator/blob/master/doc/operator-upgrade.md)

## Uninstalling  Bookkeeper-Operator

To uninstall/delete the bookkeeper-operator chart, use the following command:

```
$ helm uninstall [RELEASE_NAME]
```

This command removes all the Kubernetes components associated with the chart and deletes the release.

## Operator Configuration

The following table lists the configurable parameters of the bookkeeper-operator chart and their default values.

| Parameter | Description | Default |
| ----- | ----------- | ------ |
| `image.repository` | Image repository | `pravega/bookkeeper-operator` |
| `image.tag` | Image tag | `0.1.7` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `crd.create` | Create bookkeeper CRD | `true` |
| `rbac.create` | Create RBAC resources | `true` |
| `serviceAccount.create` | Create service account | `true` |
| `serviceAccount.name` | Name for the service account | `bookkeeper-operator` |
| `testmode.enabled` | Enable test mode | `false` |
| `webhookCert.crt` | tls.crt value corresponding to the certificate | |
| `webhookCert.key` | tls.key value corresponding to the certificate | |
| `webhookCert.generate` | Whether to generate the certificate and the issuer (set to false while using self-signed certificates) | `false` |
| `webhookCert.certName` | Name of the certificate, if generate is set to false | `selfsigned-cert-bk` |
| `webhookCert.secretName` | Name of the secret created by the certificate, if generate is set to false | `selfsigned-cert-tls-bk` |
| `watchNamespace` | Namespaces to be watched  | `""` |
