# Staging of Pravega Helm Charts

The purpose of having this particular placeholder is for the staging of Pravega charts from where they can be tested before their official release.

To add the staging repository to your Helm repos, use the following command
```
helm repo add staging https://charts.pravega.io/staging
helm repo update
```
