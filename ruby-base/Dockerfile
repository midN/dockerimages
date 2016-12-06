FROM ruby:2.3.0
MAINTAINER andres@andres.wtf

RUN apt-get update > /dev/null && apt-get install -y \
  build-essential \
  nodejs \
  postgresql-client \
  vim cron gdebi htop > /dev/null

RUN wget http://download.gna.org/wkhtmltopdf/0.12/0.12.2.1/wkhtmltox-0.12.2.1_linux-jessie-amd64.deb && \
    gdebi --n wkhtmltox-0.12.2.1_linux-jessie-amd64.deb && \
    rm wkhtmltox-0.12.2.1_linux-jessie-amd64.deb
