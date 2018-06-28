FROM openshift/redhat-openjdk18-openshift:1.2

USER root

ENV \
    EL_HOME=$APP_ROOT/elasticsearch

RUN \
    set -e ; \
    mkdir ${APP_ROOT} ;\
    curl -Lo /tmp/elasticsearch.zip https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/zip/elasticsearch/${EL_VERSION}/elasticsearch-${EL_VERSION}.zip ; \
	unzip -o -d ${APP_ROOT} /tmp/elasticsearch.zip ; \
	mkdir -p ${EL_HOME} ;\
	mv ${APP_ROOT}/elasticsearch-$EL_VERSION/* ${EL_HOME} ; \
	rm -rf ${APP_ROOT}/elasticsearch-$EL_VERSION

RUN useradd -r elastic

RUN chown -R elastic ${APP_ROOT} ; \
	chgrp -R 0 ${APP_ROOT} ; \
	chmod -R g+rw ${APP_ROOT} ; \
	chmod 775 ${EL_HOME}/bin/elasticsearch

USER elastic

RUN /opt/app-root/elasticsearch/bin/plugin install analysis-smartcn
RUN /opt/app-root/elasticsearch/bin/plugin install analysis-kuromoji

COPY elasticsearch.yml $EL_HOME/config

EXPOSE 9200 9300

CMD ["/opt/app-root/elasticsearch/bin/elasticsearch"]
