FROM openjdk:17.0.2-slim

RUN set -ex \
    && sed -i 's|http://deb.debian.org/debian|http://archive.debian.org/debian|g' /etc/apt/sources.list \
    && sed -i 's|http://security.debian.org/debian-security|http://archive.debian.org/debian-security|g' /etc/apt/sources.list \
    && sed -i '/bullseye-updates/d' /etc/apt/sources.list \
    && printf 'Acquire::Check-Valid-Until "false";\n' > /etc/apt/apt.conf.d/99archive \
    && apt-get update \
    && apt-get install -y --no-install-recommends curl unzip ca-certificates \
    && curl -#SL https://sourceforge.net/projects/geoserver/files/GeoServer/2.23.2/geoserver-2.23.2-bin.zip/download -o /tmp/geoserver.zip \
    && mkdir -p /mnt/geoserver \
    && unzip /tmp/geoserver.zip -d /mnt/geoserver \
    && rm -f /tmp/geoserver.zip

EXPOSE 8080
WORKDIR /mnt/geoserver

ENV JAVA_OPTS="-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005"

CMD ["/mnt/geoserver/bin/startup.sh"]

USER root

COPY flag.txt /root/flag.txt
