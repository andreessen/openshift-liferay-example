# openshift-liferay-example

## Build from scratch

```console
$ export APP_NAME=wildfly12-liferay7

$ oc new-project liferay-example

$ oc new-app mysql-persistent \
    -p DATABASE_SERVICE_NAME=mysql-lportal -p MYSQL_DATABASE=lportal -p MYSQL_USER=lportal -p MYSQL_PASSWORD=lportal

$ # export LF_REPO_PREFIX=http://httpd-ex-liferay-distr.apps.172.20.10.5.xip.io/liferay/
$ oc new-app \
    openshift/wildfly:12.0~https://github.com/dsevost/openshift-liferay-example \
    --context-dir=/src \
    --name=$APP_NAME \
    -e MYSQL_DATASOURCE=LiferayDS \
    -e MYSQL_DATABASE=lportal \
    -e MYSQL_USER=lportal \
    -e MYSQL_PASSWORD=lportal \
    -e MYSQL_SERVICE_HOST=mysql-lportal \
    -e MYSQL_SERVICE_PORT=3306 \
    --build-env=LF_REPO_PREFIX=$LF_REPO_PREFIX \
    -l liferay=

$ oc set resources dc/$APP_NAME --limits=cpu=2,memory=1536Mi --requests=cpu=1,memory=1535Mi

$ oc create cm liferay-portal-ext --from-file=docker/portal-ext.properties

$ oc set volume dc/$APP_NAME --add --name=portal-ext-props --type=configmap --mount-path=/opt/app-root/liferay/portal-ext.properties \
    --configmap-name=liferay-portal-ext --sub-path=portal-ext.properties # --overwrite

```

## Build from S2I template

```
$ oc new-project liferay-example
$ oc --as=system:admin -n liferay-example create quota resources --hard=cpu=4,memory=4G,pods=10,services=4,secrets=15,persistentvolumeclaims=5
$ oc create -f liferay-on-wildfly-s2i.yaml
$ oc new-app liferay-on-wildfly-s2i -p APP_NAME=liferay-ex
```

