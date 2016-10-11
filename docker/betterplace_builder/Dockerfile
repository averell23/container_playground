FROM betterplace/base
MAINTAINER "Daniel Hahn" <averell@newgods.org>
USER betterplace
WORKDIR /var/apps/betterplace/current
RUN bundle exec rake assets:precompile
