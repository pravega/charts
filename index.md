# Pravega Helm Charts

Here you will find the helm charts for each of the components mentioned below, which are required to setup a pravega cluster.
- Pravega Operator
- Pravega
- Bookkeeper Operator
- Bookkeeper
- Zookeeper Operator
- Zookeeper

## Prerequisites

- Kubernetes 1.15+
- Helm 3.2.1+
- [Cert-Manager](https://cert-manager.io/docs/installation/kubernetes/) v0.15.0+

To add the Pravega charts repository to your Helm repos, use the following command
```
helm repo add pravega https://charts.pravega.io
helm repo update
```
You can also run the following command to see all the available charts
```
helm search repo pravega -l
```

## Deploying Pravega using charts

We now show you step by step how to deploy Pravega, which involves the deployment of Zookeeper, Bookkeeper and Pravega (as well as their respective Operators).
Follow the following steps to quickly setup a running Pravega cluster.

First install the zookeeper operator and a zookeeper cluster
```
helm install zookeeper-operator pravega/zookeeper-operator
helm install zookeeper pravega/zookeeper
```

Before installing the bookkeeper operator, you would need to install an issuer and a certificate (can be either self-signed or CA signed). Refer to [this](https://github.com/pravega/bookkeeper-operator/blob/master/deploy/certificate.yaml) manifest to create a self-signed issuer and certificate.

Then install the bookkeeper operator and a bookkeeper cluster
```
helm install bookkeeper-operator pravega/bookkeeper-operator --set webhookCert.certName=[CERT_NAME] --set webhookCert.secretName=[SECRET_NAME]
helm install bookkeeper pravega/bookkeeper
```

Just like in case of the bookkeeper operator, you would need to install another issuer and certificate before installing the pravega-operator. Refer to [this](https://github.com/pravega/pravega-operator/blob/master/deploy/certificate.yaml) manifest to create a self-signed issuer and certificate.

You would also need to setup a [Long-Term Storage](https://github.com/pravega/pravega-operator#set-up-tier-2-storage) (or Tier 2) before you proceed with the next step.

Finally install the pravega operator and a pravega cluster using the following commands
```
helm install pravega-operator pravega/pravega-operator --set webhookCert.certName=[CERT_NAME] --set webhookCert.secretName=[SECRET_NAME]
helm install pravega pravega/pravega
```

> Note: Here [CERT_NAME] is the name of the certificate and [SECRET_NAME] is the name of the secret created by the certificate that was installed in the previous step.
