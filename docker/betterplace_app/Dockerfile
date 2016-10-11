FROM ruby:2.3.1-alpine
MAINTAINER "Daniel Hahn" <averell@newgods.org>
ENV RUBY_MAJOR 2.3
ENV RUBY_VERSION 2.3.1
ENV RAILS_ENV production
ENV DB_ADAPTER=nulldb
# We need edge for libmagic at the moment
# RUN echo "http://dl-3.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories
# RUN apk upgrade --update-cache --available
ENV SMTP_SERVER='127.0.0.1'
RUN apk add --no-cache --virtual .bundle-deps\
    bash \
    bzip2 \
    ca-certificates \
    coreutils \
    curl \
    libmagic \
    nodejs \
    git \
    mariadb-client-libs \
    openssl \
  && apk add --no-cache --virtual .bundle-builddeps\
    autoconf \
		bison \
		bzip2-dev \
		gcc \
    g++ \
		gdbm-dev \
		glib-dev \
		libc-dev \
		libffi-dev \
		libxml2-dev \
		libxslt-dev \
    libstdc++ \
    file-dev \
    mariadb-dev \
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
  && mkdir /etc/sysconfig \
  && echo ZONE='"Europe/Berlin"' > /etc/sysconfig/clock
ADD bp_source/ /var/apps/betterplace/current
RUN adduser betterplace -D -H \
  && chown -R betterplace: /var/apps/betterplace \
  && mkdir /home/betterplace && chown -R betterplace: /home/betterplace
WORKDIR /var/apps/betterplace/current
RUN bundle install --path /var/apps/betterplace/shared/bundle/2.3.1 --without [:development, :test, :deployment] --deployment --retry 3 \
  && bundle exec rake setup:copy
USER root
RUN apk del .bundle-builddeps
