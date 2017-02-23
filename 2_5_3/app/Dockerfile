FROM ubuntu:14.04
LABEL maintainer="Michael Skrynski 'michael.skrynski@dkd.de'"

RUN apt-get -q -y update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -q -y nginx apache2 libapache2-mod-wsgi libpq5 wget supervisor

ENV CKAN_VERSION python-ckan_2.5-trusty_amd64.deb
ENV CKAN_REPO_URL http://packaging.ckan.org/$CKAN_VERSION
ENV CKAN_OPT /opt/ckan
ENV CKAN_HOME /usr/lib/ckan/default
ENV CKAN_CONFIG /etc/ckan/default/production.ini
ENV CKAN_DATA /var/lib/ckan

RUN mkdir -p $CKAN_OPT && \
    wget -nv $CKAN_REPO_URL -O $CKAN_OPT/$CKAN_VERSION && \
    dpkg -i $CKAN_OPT/$CKAN_VERSION && \
    mkdir -p /var/log/supervisor && \
    mkdir -p /etc/supervisor/conf.d && \
    mkdir -p $CKAN_DATA && \
    chown www-data:www-data $CKAN_DATA

ADD scripts/supervisor.conf /etc/supervisor/supervisor.conf
COPY services/* /etc/supervisor/conf.d/
ADD scripts/configure_ckan $CKAN_OPT

VOLUME $CKAN_DATA
EXPOSE 80
EXPOSE 9001

CMD ["supervisord", "-c", "/etc/supervisor/supervisor.conf"]
