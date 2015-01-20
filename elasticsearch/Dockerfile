# a Docker image running Elasticsearch 1.4.2 with extra plugins and /data and /logs volumes
FROM centos:7
MAINTAINER Xavier Coulon <xcoulon@redhat.com>

# install wget and other tools to subsequent downloads/install
RUN yum -y install wget tar java-1.7.0-openjdk.x86_64

WORKDIR /
ENV ES_VERSION 1.4.2
# download and install Elasticsearch
RUN wget --no-check-certificate https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-$ES_VERSION.tar.gz && tar zxf elasticsearch-$ES_VERSION.tar.gz && rm elasticsearch-$ES_VERSION.tar.gz && mv /elasticsearch-$ES_VERSION /elasticsearch
WORKDIR /elasticsearch
ADD elasticsearch-docker.yml /elasticsearch/config/elasticsearch.yml
RUN bin/plugin -v --install lmenezes/elasticsearch-kopf && bin/plugin -v --list

VOLUME ["/data", "/logs"]

# Define default command.
CMD ["bin/elasticsearch"]

# Expose ports.
#   - 9200: HTTP
#   - 9300: transport
EXPOSE 9200 
EXPOSE 9300

