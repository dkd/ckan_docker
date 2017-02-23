FROM openjdk:8-jre-alpine
LABEL maintainer="Michael Skrynski 'michael.skrynski@dkd.de'"

RUN apk add --no-cache tar

# Install Solr
ENV SOLR_HOME /opt/solr/example/solr
ENV SOLR_VERSION 4.8.1
ENV SOLR solr-$SOLR_VERSION

RUN mkdir -p /opt/solr

ADD https://archive.apache.org/dist/lucene/solr/$SOLR_VERSION/$SOLR.tgz /opt/$SOLR.tgz

RUN tar zxf /opt/$SOLR.tgz -C /opt/solr --strip-components 1 && \
    rm /opt/$SOLR.tgz && \
    rm -Rf /opt/solr/docs/ && \
    mv $SOLR_HOME/collection1/ $SOLR_HOME/ckan/ && \
    echo name=ckan > $SOLR_HOME/ckan/core.properties

COPY schema.xml $SOLR_HOME/ckan/conf/schema.xml

EXPOSE 8983
WORKDIR /opt/solr/example
CMD ["java", "-jar", "start.jar"]
