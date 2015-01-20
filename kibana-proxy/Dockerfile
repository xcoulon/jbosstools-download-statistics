# a Docker image Kibana 3.1.2, served via a custom reverse proxy written with undertow.io
# that also forwards elasticsearch requests to another container
FROM centos:7
MAINTAINER Xavier Coulon <xcoulon@redhat.com>

# install wget and other tools to subsequent downloads/install
RUN yum -y install wget tar java-1.7.0-openjdk.x86_64 maven

# Define commonly used JAVA_HOME variable
ENV JAVA_HOME /usr/lib/jvm/java-1.7.0-openjdk

# download and install Kibana
ENV KIBANA_VERSION 3.1.2
RUN wget --no-check-certificate https://download.elasticsearch.org/kibana/kibana/kibana-$KIBANA_VERSION.tar.gz && tar zxf kibana-$KIBANA_VERSION.tar.gz && rm kibana-3.1.2.tar.gz && mv kibana-$KIBANA_VERSION kibana
# custom config.js to access elasticsearch
ADD kibana-$KIBANA_VERSION/config.js /kibana-$KIBANA_VERSION/config.js

ADD . /ekproxy
WORKDIR /ekproxy
# build with maven using OpenShift profile to copy all dependencies in the target/dist folder
RUN mvn -P openshift clean package

# Define default command.
# assumes that there's a linked container with alias 'downloads_back' running
CMD java -DKIBANA_HOME=/kibana -DELASTICSEARCH_HOST=${ELASTICSEARCH_PORT_9200_TCP_ADDR} -DELASTICSEARCH_PORT=9200 -jar /ekproxy/target/dist/ek-reverse-proxy.jar

# Expose ports.
EXPOSE 8080