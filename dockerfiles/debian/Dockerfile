# https://github.com/olivergregorius/zabbix-docker-monitoring

# Define required Debian version by using FROM tag. Avalaible: wheezy/jessie/...
FROM debian:stretch

# Define required Zabbix version (<version>) or release (branches/<version>), e.g. release/4.4
ENV ZABBIX_VERSION=release/4.4

WORKDIR /root

RUN \
   apt-get -qq update  1>/dev/null && \
   apt-get -qq --assume-yes install git automake autoconf gcc make pkg-config libpcre3-dev 1>/dev/null && \
   git clone -q https://github.com/realhidden/zabbix-docker-monitoring

RUN \
   git clone -b ${ZABBIX_VERSION} --depth 1 https://github.com/zabbix/zabbix.git ~/zabbix && \
   cd ~/zabbix/ && \
   ./bootstrap.sh 1>/dev/null && \
   ./configure --enable-agent 1>/dev/null && \
   cp -R ~/zabbix-docker-monitoring/src/modules/zabbix_module_docker/ ~/zabbix/src/modules/ && \
   cd ~/zabbix/src/modules/zabbix_module_docker && \
   make

## Dockerized compilation (build from remote URL or local PATH):
# docker build --rm=true -t local/zabbix-docker-module-compilation https://github.com/olivergregorius/zabbix-docker-monitoring.git#master:dockerfiles/debian/
# docker build --rm=true -t local/zabbix-docker-module-compilation .
# docker run  --rm -v /tmp:/tmp local/zabbix-docker-module-compilation cp /root/zabbix/src/modules/zabbix_module_docker/zabbix_module_docker.so /tmp/zabbix_module_docker.so
# docker rmi -f local/zabbix-docker-module-compilation
## use/copy /tmp/zabbix_module_docker.so in your Zabbix
