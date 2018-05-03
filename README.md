# openshift-liferay-example

## Build from scratch

```console
$ oc new-project liferay-example

$ oc new-app mysql -p DATABASE_SERVICE_NAME=mysql-lportal -p MYSQL_DATABASE=lportal -p MYSQL_USER=lportal -p MYSQL_PASSWORD=lportal

$ oc new-app \
    openshift/wildfly:12.0~https://github.com/dsevost/openshift-liferay-example \
    --context-dir=/src \
    --name=wildfly12-liferay7 \
    -l liferay= \
    -e MYSQL_DATABASE=lportal \
    -e MYSQL_DATASOURCE=LiferayDS \
    -e MYSQL_USER=lportal -e MYSQL_PASSWORD=lportal

$ oc create cm liferay-portal-ext --from-file=portal-ext.properties

```
