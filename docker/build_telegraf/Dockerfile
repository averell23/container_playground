FROM alanfranz/fwd-centos-6:latest

RUN mkdir /build_target && mkdir /usr/go && mkdir /go
VOLUME ["/build_target"]

ENV GOLANG_VERSION 1.5.4
ENV GOLANG_DOWNLOAD_URL https://golang.org/dl/go$GOLANG_VERSION.linux-amd64.tar.gz
ENV GOLANG_DOWNLOAD_SHA256 a3358721210787dc1e06f5ea1460ae0564f22a0fbd91be9dcd947fb1d19b9560
ENV INFLUX_PATH /go/src/github.com/influxdata
ENV GOROOT /usr/go
ENV GOPATH /go
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/go/bin:/go/bin

# Pull down slightly more recent git, go get will barf otherwise
RUN rpm -i 'http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm' \
    && rpm --import http://apt.sw.be/RPM-GPG-KEY.dag.txt \
    && yum -y --disablerepo=base,updates --enablerepo=rpmforge-extras update git \
    && yum clean all

RUN curl -fsSL "$GOLANG_DOWNLOAD_URL" -o golang.tar.gz \
	&& echo "$GOLANG_DOWNLOAD_SHA256  golang.tar.gz" | sha256sum -c - \
	&& tar -C /usr -xzf golang.tar.gz \
	&& rm golang.tar.gz

RUN  mkdir -p $INFLUX_PATH

WORKDIR $INFLUX_PATH
RUN git clone https://github.com/averell23/telegraf.git
WORKDIR $INFLUX_PATH/telegraf
# RUN make

CMD ["/bin/bash"]
