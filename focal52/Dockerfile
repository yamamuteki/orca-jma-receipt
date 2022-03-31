FROM ubuntu:20.04
LABEL maintainer="to-lz1 <m.toriyama000@gmail.com>"

ENV ORMASTER_PASSWORD=ormaster
ENV TZ=Asia/Tokyo

# Reference: https://www.orca.med.or.jp/receipt/download/focal/focal_install_52.html
WORKDIR /tmp
RUN set -ex; \
  # jma-receipt depends on some packages (e.g. libqrencode3) distributed under 'universe'
  DEBIAN_FRONTEND=noninteractive; \
  apt-get update && apt-get install -y --no-install-recommends tzdata; \
  \
  repoTypes='main restricted universe'; \
  repoUrl='http://ftp.jaist.ac.jp/pub/Linux/ubuntu/'; \
  echo "deb $repoUrl focal $repoTypes" > /etc/apt/sources.list; \
  echo "deb $repoUrl focal-updates $repoTypes" >> /etc/apt/sources.list; \
  echo "deb $repoUrl focal-security $repoTypes" >> /etc/apt/sources.list; \
  \
  fetchDeps='ca-certificates wget gnupg'; \
  apt-get update; \
  apt-get install -y --no-install-recommends $fetchDeps; \
  rm -rf /var/lib/apt/lists/*; \
  \
  wget -q https://ftp.orca.med.or.jp/pub/ubuntu/archive.key; \
  apt-key add archive.key; \
  listUrl='https://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-focal52.list'; \
  wget -q -O /etc/apt/sources.list.d/jma-receipt-focal52.list $listUrl; \
  \
  apt-get update; \
  apt-get dist-upgrade -y; \
  apt-get install -y --no-install-recommends jma-receipt sudo; \
  rm -rf /var/lib/apt/lists/*

# we need this because jma-receipt doesn't provide System-V style init script anymore.
# see: https://github.com/gdraheim/docker-systemctl-replacement
RUN systemctlUrl='https://raw.githubusercontent.com/gdraheim/docker-systemctl-replacement/v1.5.4505/files/docker/systemctl3.py'; \
  wget -q -O /usr/bin/systemctl $systemctlUrl; \
  chmod +x /usr/bin/systemctl

RUN set -ex; \
  systemctl start postgresql@12-main; \
  jma-setup; \
  printf '%s\n' "$ORMASTER_PASSWORD" "$ORMASTER_PASSWORD" | script -q -c 'sudo -u orca /usr/lib/jma-receipt/bin/passwd_store.sh' /dev/null; \
  rm -rf /tmp/*; \
  systemctl stop postgresql@12-main

EXPOSE 8000
WORKDIR /

RUN systemctl enable postgresql@12-main
RUN systemctl enable jma-receipt
CMD systemctl --init
