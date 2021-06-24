# Pravega Helm Charts

This GitHub repository contains the source code for the helm charts for each of the following Pravega components and dependencies within the [charts](charts) folder

- Pravega Operator
- Pravega
- Bookkeeper Operator
- Bookkeeper
- Zookeeper Operator
- Zookeeper

It also contains the published charts for each of these components within the [gh-pages](https://github.com/pravega/charts/tree/gh-pages) branch.

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

Follow the commands provided in the links that each of the following steps guide you to, in the same order they have been mentioned, to quickly setup a running Pravega cluster.

- First install the zookeeper operator and a zookeeper cluster.
- Once that is done, you would need to install the [bookkeeper operator](charts/bookkeeper-operator/README.md#deploying-bookkeeper-operator) and a [bookkeeper cluster](charts/bookkeeper/README.md#deploying-bookkeeper).
- You then need to setup a [Long-Term Storage](https://github.com/pravega/pravega-operator/blob/master/doc/longtermstorage.md#use-nfs-as-longtermstorage) (or Tier 2) before moving to the next step.
- Finally install the pravega operator and a pravega cluster.

## Contributing
We thrive to build a welcoming and open community for anyone who wants to use the pravega helm charts or contribute to it. [Here](https://github.com/pravega/charts/wiki/Contributing) we describe how to contribute to the helm charts. Contact the developers and community on [slack](https://pravega-io.slack.com/) ([signup](https://pravega-slack-invite.herokuapp.com/)) if you need any help.

## License
Pravega Helm Charts come under Apache 2.0 license. See the [LICENSE](LICENSE) for details.
