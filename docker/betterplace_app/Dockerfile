FROM ruby:2.3.1-alpine
MAINTAINER "Daniel Hahn" <averell@newgods.org>
ENV RUBY_MAJOR 2.3
ENV RUBY_VERSION 2.3.1
ENV RAILS_ENV production
ADD bp_source/ /var/apps/betterplace/current
RUN adduser betterplace -D -H
RUN chown -R betterplace: /var/apps/betterplace
# We need edge for libmagic at the moment
RUN echo "http://dl-3.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories
RUN apk upgrade --update-cache --available
# This does not actully do anything except make our current
# Centos-Based Rails app happy
RUN mkdir /etc/sysconfig && echo ZONE='"Europe/Berlin"' > /etc/sysconfig/clock
RUN apk add --no-cache --virtual .bundle-builddeps\
    autoconf \
    bash \
		bison \
		bzip2 \
		bzip2-dev \
		ca-certificates \
		coreutils \
		curl \
		gcc \
    g++ \
		gdbm-dev \
		glib-dev \
		libc-dev \
		libffi-dev \
		libxml2-dev \
		libxslt-dev \
    libstdc++ \
    libmagic \
    file-dev \
    nodejs-dev \
    mysql-dev \
		linux-headers \
		make \
		ncurses-dev \
		openssl-dev \
		procps \
# https://bugs.ruby-lang.org/issues/11869 and https://github.com/docker-library/ruby/issues/75
		readline-dev \
		ruby \
		yaml-dev \
		zlib-dev \
    git
RUN mkdir /home/betterplace && chown -R betterplace: /home/betterplace
USER betterplace
RUN bundle config build.therubyracer --use-system-libraries
WORKDIR /var/apps/betterplace/current
RUN bundle install --path /var/apps/betterplace/shared/bundle/2.3.1 --without [:development, :test, :deployment] --deployment
RUN bundle exec rake setup:copy
