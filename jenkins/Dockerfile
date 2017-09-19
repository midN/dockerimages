FROM jenkins:latest
MAINTAINER Andres Ivanov <andres@andres.wtf>

USER root

# Set correct timezone
ENV TZ=Europe/Tallinn
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN echo "deb http://cz.archive.ubuntu.com/ubuntu trusty main" >> /etc/apt/sources.list
RUN apt-get update && \
               apt-get -y --no-install-recommends --allow-unauthenticated install \
               python-yaml \
               python-jinja2 python-httplib2 python-keyczar \
               python-paramiko python-setuptools \
               python-pkg-resources git python-pip vim \
               apt-transport-https ca-certificates curl \
               gcc libc6-dev libssl-dev make postgresql-client libpq-dev \
               python-dev sudo libsystemd-journal0

RUN adduser jenkins sudo
RUN echo "jenkins ALL=(ALL) NOPASSWD: ALL" | (EDITOR="tee -a" visudo)

# Allow GPG to use passphrase
RUN echo "use-agent" >> /var/jenkins_home/.gnupg/gpg.conf
RUN echo "pinentry-mode loopback" >> /var/jenkins_home/.gnupg/gpg.conf

# Docker
COPY docker.list /etc/apt/sources.list.d/docker.list
RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D && \
    apt-get update && \
    apt-get -y --no-install-recommends install docker-engine

# Ansible
RUN mkdir -p /etc/ansible/
RUN pip install ansible==2.2.1.0 boto boto3 awscli psycopg2
RUN pip install --upgrade jinja2

# Oracle JDK
ENV VERSION 8
ENV UPDATE 131
ENV BUILD 11
ENV HASH d54c1d3a095b4ff2b6607d096fa80163

ENV JAVA_HOME /usr/lib/jvm/java-${VERSION}-oracle
ENV JRE_HOME ${JAVA_HOME}/jre

RUN curl --silent --location --retry 3 --cacert /etc/ssl/certs/GeoTrust_Global_CA.pem \
    --header "Cookie: oraclelicense=accept-securebackup-cookie;" \
      http://download.oracle.com/otn-pub/java/jdk/"${VERSION}"u"${UPDATE}"-b"${BUILD}"/"${HASH}"/jdk-"${VERSION}"u"${UPDATE}"-linux-x64.tar.gz \
      | tar xz -C /tmp && \
      mkdir -p /usr/lib/jvm && mv /tmp/jdk1.${VERSION}.0_${UPDATE} "${JAVA_HOME}" && \
      curl --silent --location --retry 3 --cacert /etc/ssl/certs/GlobalSign_Root_CA.pem \
      https://www.openssl.org/source/openssl-1.0.2g.tar.gz \
      | tar xz -C /tmp && \
      cd /tmp/openssl-1.0.2g && \
      ./config --prefix=/usr && \
      make clean && make && make install && \
      rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN usermod -aG docker jenkins

USER jenkins
