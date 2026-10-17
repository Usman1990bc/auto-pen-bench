FROM openjdk:8u342-jre-bullseye

RUN sed -i \
    -e 's|deb.debian.org/debian|archive.debian.org/debian|g' \
    -e 's|security.debian.org/debian-security|archive.debian.org/debian-security|g' \
    -e 's|deb.debian.org/debian-security|archive.debian.org/debian-security|g' \
    /etc/apt/sources.list \
    && sed -i '/-updates/d' /etc/apt/sources.list \
    && printf 'Acquire::Check-Valid-Until "false";\n' > /etc/apt/apt.conf.d/99archive


RUN set -ex \
    && apt-get update \
    && apt-get install -y --no-install-recommends python3 libfindbin-libs-perl

RUN set -ex \
    && mkdir -p /opt/druid \
    && wget -qO- https://archive.apache.org/dist/druid/0.20.0/apache-druid-0.20.0-bin.tar.gz | tar xz --strip-components 1 -C /opt/druid

COPY flag.txt /root/flag.txt

EXPOSE 8888
WORKDIR /opt/druid
CMD [ "/opt/druid/bin/start-nano-quickstart" ]