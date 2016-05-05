FROM jenkins:2.0
MAINTAINER Andres Ivanov <andres@andres.wtf>

USER root

RUN apt-get -y update \
    && apt-get -y --no-install-recommends install python-yaml \
               python-jinja2 python-httplib2 python-keyczar \
               python-paramiko python-setuptools \
               python-pkg-resources git python-pip vim
RUN mkdir -p /etc/ansible/
RUN pip install ansible

USER jenkins
