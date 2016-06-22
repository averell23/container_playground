FROM centos:7
MAINTAINER "Daniel Hahn" <averell@newgods.org>
ENV RUBY_MAJOR 2.3
ENV RUBY_VERSION 2.3.1
ENV RAILS_ENV production
RUN yum install -y git-core zlib zlib-devel gcc-c++ patch readline readline-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison curl mysql-devel file-devel \
&& curl -fSL -o ruby.tar.gz "http://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.gz" \
&& mkdir -p /usr/src/ruby \
&& tar -xvzf ruby.tar.gz -C /usr/src/ruby --strip-components=1 \
&& rm ruby.tar.gz \
&& cd /usr/src/ruby \
&& autoconf \
&& ./configure --disable-install-doc \
&& make && make install
RUN gem install bundler --version 1.12.5
ADD bp_source/ /var/apps/betterplace/current
RUN echo ZONE='"Europe/Berlin"' > /etc/sysconfig/clock
RUN cd /var/apps/betterplace/current && bundle install --path /var/apps/betterplace/shared/bundle/2.3.1 --without [:development, :test, :deployment] --deployment \
&& bundle exec rake assets:precompile
