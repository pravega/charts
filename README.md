# Pravega Helm Charts

This GitHub repository contains the source for the packaged and versioned charts released for each of the following pravega components inside the [docs](docs/) folder.
- Pravega Operator
- Pravega
- Bookkeeper Operator
- Bookkeeper
- Zookeeper Operator
- Zookeeper
- Schema Registry

The purpose of this repository is to provide a place for maintaining the official Pravega Charts.

To add the Pravega charts repository to your Helm repos :
```
helm repo add pravega charts.pravega.io
```

# Deploying Pravega using charts

We now show you step by step how to deploy Pravega, which involves the deployment of Zookeeper, Bookkeeper and Pravega (as well as their respective Operators).
Follow the following steps to quickly setup a running pravega cluster.

First install the zookeeper operator and a zookeeper cluster
```
helm install zookeeper-operator pravega/zookeeper-operator
helm install zookeeper pravega/zookeeper
```

Then install the bookkeeper operator and a bookkeeper cluster
```
helm install bookkeeper-operator pravega/bookkeeper-operator
helm install bookkeeper pravega/bookkeeper --set zookeeperUri=zookeeper-client:2181
```

Before installing the pravega operator, you would need to install [cert-manager](https://cert-manager.io/docs/installation/kubernetes/), an issuer and the certificate which can be either self-signed or CA signed (refer to [this](https://github.com/pravega/pravega-operator/blob/master/deploy/certificate.yaml) manifest to create a self-signed certificate in the default namespace). For more details refer to [this](https://github.com/pravega/pravega-operator/tree/master/charts/pravega-operator#prerequisites).

Finally install the pravega operator and a pravega-cluster
```
helm install pravega-operator pravega/pravega-operator
helm install pravega pravega/pravega --set zookeeperUri=zookeeper-client:2181 --set bookkeeperUri=bookkeeper-bookie-headless:3181 –set webhookCert.crt=<tls.crt> --set webhookCert.certName=<crt-name> --set webhookCert.secretName=<secret-name>
```
> Note: The values *tls.crt*, *cert-name* and *secret-name* will be obtained from the certificate that you have installed earlier.
