FROM openshift/wildfly-120-centos7

ENV \
    LIFERAY_HOME=$HOME/../liferay \
    WILDFLY_HOME=/wildfly \
    BUILD_VERSION=20171018150113838 \
    DL_PREFIX="https://sourceforge.net/projects/lportal/files/Liferay%20Portal/7.0.4%20GA5" \
    LF_DEPS="liferay-ce-portal-dependencies-7.0-ga5" \
    LF_OSGI_DEPS="liferay-ce-portal-osgi-7.0-ga5" \
    LF_PORTAL="liferay-ce-portal-7.0-ga5"

RUN \
    mkdir -p $WILDFLY_HOME/modules/com/liferay/portal/main

COPY module.xml $WILDFLY_HOME/modules/com/liferay/portal/main

RUN \
    for d in {${LF_DEPS},${LF_OSGI_DEPS}}-${BUILD_VERSION}.zip ${LF_PORTAL}-${BUILD_VERSION}.war ; do \
	curl -Lo /tmp/$d ${DL_PREFIX}/$d/download ; \
    done
#COPY liferay-ce-portal-* /tmp/

RUN \
    unzip -j -d $WILDFLY_HOME/modules/com/liferay/portal/main /tmp/${LF_DEPS}-${BUILD_VERSION}.zip ; \
    mkdir -p $LIFERAY_HOME/osgi ; \
    unzip -d $LIFERAY_HOME/osgi /tmp/$LF_OSGI_DEPS-${BUILD_VERSION}.zip ; \
    set -x ; \
    mv $LIFERAY_HOME/osgi/$LF_OSGI_DEPS/* $LIFERAY_HOME/osgi ; \
    rm -rf $LIFERAY_HOME/osgi/$LF_OSGI_DEPS/
