# See https://github.com/phusion/baseimage-docker/blob/master/Changelog.md for
# a list of version numbers.
FROM phusion/baseimage:0.9.19

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

ENV RUBY_VERSION 2.3.2
ENV MYSQL_VERSION 5.7
ENV ELASTICSEARCH_VERSION 1.7.5

ENV DEBIAN_FRONTEND noninteractive
ENV PATH /usr/local/ruby/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

COPY elasticsearch_run /etc/service/elasticsearch/run
COPY mysql_run         /etc/service/mysql/run
COPY redis_run         /etc/service/redis/run
COPY ssh_key           /root/.ssh/id_rsa

RUN apt-get update \
    && apt-get install -y -q wget openjdk-8-jre mysql-server-$MYSQL_VERSION redis-server imagemagick git \
               autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev \
               libncurses5-dev libffi-dev libgdbm3 libgdbm-dev libmysqlclient-dev libmagic-dev \
               libcurl4-openssl-dev \
    && wget --quiet https://download.elastic.co/elasticsearch/elasticsearch/elasticsearch-$ELASTICSEARCH_VERSION.deb \
    && dpkg -i elasticsearch-$ELASTICSEARCH_VERSION.deb \
    && /usr/share/elasticsearch/bin/plugin -install decompound -url http://xbib.org/repository/org/xbib/elasticsearch/plugin/elasticsearch-analysis-decompound/1.7.1.3/elasticsearch-analysis-decompound-1.7.1.3-plugin.zip \
    && echo 'script.disable_dynamic: false' >> /etc/elasticsearch/elasticsearch.yml \
    && git clone https://github.com/rbenv/ruby-build.git && ./ruby-build/install.sh \
    && ruby-build $RUBY_VERSION /usr/local/ruby \
    && gem install bundler \
    && mkdir /var/apps/ \
    && chmod 0600 /root/.ssh/id_rsa \
    && ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime

# Clean up APT when done.
# RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
