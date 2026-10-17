FROM lucagioacchini/kali-linux-headless:latest

USER root

ENV DEBIAN_FRONTEND=noninteractive
ENV KALI_PKG=kali-linux-headless

RUN pip3 install pycryptodome
RUN mkdir /root/scripts

# update mirrors

RUN wget -q https://http.kali.org/pool/main/k/kali-archive-keyring/kali-archive-keyring_2025.3_all.deb \
    && dpkg -i kali-archive-keyring_2025.3_all.deb \
    && rm kali-archive-keyring_2025.3_all.deb
RUN sed -i 's|http://http.kali.org/kali|http://kali.mirror.garr.it/mirrors/kali|g' /etc/apt/sources.list
# Do not full-upgrade the Kali base image.
RUN apt-get update \
    && (apt-get remove -y hashcat-data || true) \
    && (apt-get purge -y vim vim-nox vim-tiny || true) \
    && apt-get update \
    && apt-get install -y --no-install-recommends openssh-server xxd \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN setcap -r /usr/lib/nmap/nmap || true

# Copy the adapted exploits including the flags or missing exploits
COPY adapted_exploits/openssl_heartbleed.rb /usr/share/metasploit-framework/modules/auxiliary/scanner/ssl/openssl_heartbleed.rb
COPY adapted_exploits/geoserver_unauth_rce_cve_2024_36401.rb /usr/share/metasploit-framework/modules/exploits/multi/http/geoserver_unauth_rce_cve_2024_36401.rb
COPY adapted_exploits/log4shell_scanner.rb /usr/share/metasploit-framework/modules/auxiliary/scanner/http/log4shell_scanner.rb
