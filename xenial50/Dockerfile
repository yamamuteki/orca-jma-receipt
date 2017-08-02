FROM ubuntu:16.04
MAINTAINER Ryo Kato <ryo_kato@hashedhyphen.com>

ENV ORMASTER_PASSWORD=ormaster

# Reference: https://www.orca.med.or.jp/receipt/download/xenial/xenial_install_50.html#receipt
WORKDIR /tmp
RUN set -ex; \
  # jma-receipt depends on some packages (e.g. libqrencode3) distributed under 'universe'
  repoTypes='main restricted universe'; \
  repoUrl='http://ftp.jaist.ac.jp/pub/Linux/ubuntu/'; \
  echo "deb $repoUrl xenial $repoTypes" > /etc/apt/sources.list; \
  echo "deb $repoUrl xenial-updates $repoTypes" >> /etc/apt/sources.list; \
  echo "deb $repoUrl xenial-security $repoTypes" >> /etc/apt/sources.list; \
  \
  fetchDeps='ca-certificates wget'; \
  apt-get update; \
  apt-get install -y --no-install-recommends $fetchDeps; \
  rm -rf /var/lib/apt/lists/*; \
  \
  wget -q https://ftp.orca.med.or.jp/pub/ubuntu/archive.key; \
  apt-key add archive.key; \
  listUrl='https://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-xenial50.list'; \
  wget -q -O /etc/apt/sources.list.d/jma-receipt-xenial50.list $listUrl; \
  apt-get purge -y --auto-remove $fetchDeps; \
  \
  apt-get update; \
  apt-get dist-upgrade -y; \
  apt-get install -y --no-install-recommends jma-receipt; \
  rm -rf /var/lib/apt/lists/*; \
  \
  apt-get update; \
  apt-get install sudo; \
  rm -rf /var/lib/apt/lists/*; \
  \
  service postgresql start; \
  jma-setup; \
  printf '%s\n' "$ORMASTER_PASSWORD" "$ORMASTER_PASSWORD" | script -q -c 'sudo -u orca /usr/lib/jma-receipt/bin/passwd_store.sh' /dev/null; \
  # don't remove 'sudo' here because of warnings
  rm -rf /tmp/*

EXPOSE 8000
WORKDIR /
CMD service postgresql restart && service jma-receipt start && tail -f /dev/null
