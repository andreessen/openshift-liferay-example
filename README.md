# openshift-liferay-example

## Build from scratch

```console
$ oc new-project liferay-example
$ oc new-build openshift/wildfly:12.0~https://github.com/dsevost/openshift-liferay-example --context-dir=/docker
