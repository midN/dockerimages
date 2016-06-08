FROM jenkins:latest
MAINTAINER Andres Ivanov <andres@andres.wtf>

USER root

RUN apt-get update && \
               apt-get -y --no-install-recommends install python-yaml \
               python-jinja2 python-httplib2 python-keyczar \
               python-paramiko python-setuptools \
               python-pkg-resources git python-pip vim \
               apt-transport-https ca-certificates

# Docker
COPY docker.list /etc/apt/sources.list.d/docker.list
RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D && \
    apt-get update && \
    apt-get -y --no-install-recommends install docker-engine

# Ansible
RUN mkdir -p /etc/ansible/
RUN pip install ansible boto boto3 awscli

RUN usermod -aG docker jenkins

USER jenkins
