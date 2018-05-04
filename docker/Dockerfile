FROM openshift/wildfly-120-centos7

ARG LF_REPO_PREFIX

ENV \
    LIFERAY_HOME=$APP_ROOT/liferay \
    WILDFLY_HOME=/wildfly \
    LF_BUILD=20171018150113838 \
    LF_VERSION=7.0-ga5 \
    DL_PREFIX="https://sourceforge.net/projects/lportal/files/Liferay%20Portal/7.0.4%20GA5" \
    LF_DEPS="liferay-ce-portal-dependencies" \
    LF_OSGI_DEPS="liferay-ce-portal-osgi" \
    LF_PORTAL="liferay-ce-portal"

RUN \
    set -e ; \
    export DL_PREFIX=${LF_REPO_PREFIX:-$DL_PREFIX} ; \
    mkdir -p $WILDFLY_HOME/modules/com/liferay/portal/main ; \
    curl -Lo $WILDFLY_HOME/standalone/deployments/ROOT.war ${DL_PREFIX}/${LF_PORTAL}-${LF_VERSION}-${LF_BUILD}.war ; \
    for d in {${LF_DEPS},${LF_OSGI_DEPS}}-${LF_VERSION}-${LF_BUILD}.zip ; do \
	curl -Lo /tmp/$d ${DL_PREFIX}/$d ; \
    done ; \
    unzip -j -d $WILDFLY_HOME/modules/com/liferay/portal/main /tmp/${LF_DEPS}-${LF_VERSION}-${LF_BUILD}.zip ; \
    mkdir -p $LIFERAY_HOME/osgi ; \
    unzip -d $LIFERAY_HOME/osgi /tmp/${LF_OSGI_DEPS}-${LF_VERSION}-${LF_BUILD}.zip ; \
    mv $LIFERAY_HOME/osgi/${LF_OSGI_DEPS}-${LF_VERSION}/* $LIFERAY_HOME/osgi ; \
    rm -rf $LIFERAY_HOME/osgi/${LF_OSGI_DEPS}-${LF_VERSION}/ ; \
    rm -rf /tmp/liferay-ce-*.zip ; \
    ln -s $WILDFLY_HOME ${LIFERAY_HOME} ; \
    $WILDFLY_HOME/bin/jboss-cli.sh --echo-command --commands="\
	embed-server --admin-only --std-out=echo,\
	/subsystem=undertow/server=default-server/http-listener=default:read-attribute(name=url-charset),\
	/subsystem=deployment-scanner/scanner=default:read-attribute(name=deployment-timeout),\
	/subsystem=security/security-domain=PortalRealm:add(cache-type=default),\
	/subsystem=security/security-domain=PortalRealm/authentication=classic:add(),\
	/subsystem=security/security-domain=PortalRealm/authentication=classic/login-module=liferay:add(\
	 code=com.liferay.portal.security.jaas.PortalLoginModule, flag=required),\
	/subsystem=undertow/server=default-server/host=default-host/location=\/:remove()\
    " ; \
    rm -rf $WILDFLY_HOME/standalone/configuration/standalone_xml_history ; \
    chmod g+w $LIFERAY_HOME/osgi $WILDFLY_HOME/standalone/{configuration,data,deployments,log,tmp}

COPY module.xml $WILDFLY_HOME/modules/com/liferay/portal/main

CMD [ "/wildfly/bin/standalone.sh", "-b" , "0.0.0.0" ]
