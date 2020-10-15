# Pravega Helm Charts

This GitHub repository contains the source for the packaged and versioned charts released for each of the following pravega components inside the [docs](https://github.com/pravega/charts/tree/master/docs) folder.
- Pravega Operator
- Pravega
- Bookkeeper Operator
- Bookkeeper
- Zookeeper Operator
- Zookeeper
- Schema Registry

The purpose of this repository is to provide a place to maintain the official Pravega Charts.

To add the Pravega charts repository to your Helm repos, use the following command
```
helm repo add pravega https://charts.pravega.io
helm repo update
```

# Deploying Pravega using charts

We now show you step by step how to deploy Pravega, which involves the deployment of Zookeeper, Bookkeeper and Pravega (as well as their respective Operators).
Follow the following steps to quickly setup a running Pravega cluster.

First install the zookeeper operator and a zookeeper cluster
```
helm install zookeeper-operator pravega/zookeeper-operator
helm install zookeeper pravega/zookeeper
```

Then install the bookkeeper operator and a bookkeeper cluster
```
helm install bookkeeper-operator pravega/bookkeeper-operator
helm install bookkeeper pravega/bookkeeper
```

Before installing the pravega operator, you would need to install [cert-manager](https://cert-manager.io/docs/installation/kubernetes/), an issuer and the certificate (can be either self-signed or CA signed). For more details, refer to [this](https://github.com/pravega/pravega-operator/tree/master/charts/pravega-operator#prerequisites).

You would also need to setup a [Long-Term Storage](https://github.com/pravega/pravega-operator#set-up-tier-2-storage) (or Tier 2).

Finally install the pravega operator and a pravega cluster
```
helm install pravega-operator pravega/pravega-operator --set webhookCert.certName=[CERT_NAME] --set webhookCert.secretName=[SECRET_NAME]
helm install pravega pravega/pravega
```
> Note: Here [CERT_NAME] is the name of the certificate and [SECRET_NAME] is the name of the secret created by the certificate that was installed in the previous step.
