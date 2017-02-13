FROM centos:latest
MAINTAINER François Hellebuyck

ARG HBASE_VERSION=1.2.4


ENV http_proxy http://fr-proxy:3128
ENV https_proxy http://fr-proxy:3128

LABEL Description="HBase", "HBase Version"="$HBASE_VERSION"

RUN yum update -y && \
yum install -y wget && \ 
wget --no-cookies --no-check-certificate \
  --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" \
   "http://download.oracle.com/otn-pub/java/jdk/8u121-b13/e9e7ea248e2c4826b92b3f075a80e441/jre-8u121-linux-x64.rpm" && \
yum localinstall -y /jre-8u121-linux-x64.rpm && \
rm -f /jre-8u121-linux-x64.rpm && \
yum clean all

WORKDIR /opt

RUN wget http://apache.mindstudios.com/hbase/${HBASE_VERSION}/hbase-$HBASE_VERSION-bin.tar.gz &&\
mkdir hbase-$HBASE_VERSION && \
tar zxf hbase-$HBASE_VERSION-bin.tar.gz -C hbase-$HBASE_VERSION --strip 1 && \
ln -sv hbase-$HBASE_VERSION hbase && \
rm -fv hbase-$HBASE_VERSION-bin.tar.gz && \
{ rm -rf hbase/{docs,src}; : ; }

ENV JAVA_HOME=/usr
ENV PATH /opt/hbase/bin:$PATH
RUN mkdir /opt/hbase/zkdata
COPY conf/hbase-site.xml /opt/hbase/conf
# EXPOSE PORTS
# zookeeper
EXPOSE 2181

# hbase.master.port
EXPOSE 16000

# hbase.master.info.port (http)
EXPOSE 16010

# hbase.regionserver.port
EXPOSE 16020

# hbase.regionserver.info.port (http)
EXPOSE 16030

# hbase.rest.port
EXPOSE 8080

ADD start-pseudo-distributed.sh /opt/hbase/bin/start-pseudo-distributed.sh
# Start HBase
CMD /opt/hbase/bin/start-pseudo-distributed.sh

