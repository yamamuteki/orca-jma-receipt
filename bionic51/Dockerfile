FROM ubuntu:18.04
LABEL maintainer="typewriter / takuya <takuyaxp@gmail.com>"

ENV ORMASTER_PASSWORD=ormaster
ENV TZ=Asia/Tokyo

# Reference: https://www.orca.med.or.jp/receipt/download/bionic/bionic_install_51.html#receipt
WORKDIR /tmp
RUN set -ex; \
  # jma-receipt depends on some packages (e.g. libqrencode3) distributed under 'universe'
  DEBIAN_FRONTEND=noninteractive; \
  apt-get update && apt-get install -y --no-install-recommends tzdata; \
  \
  repoTypes='main restricted universe'; \
  repoUrl='http://ftp.jaist.ac.jp/pub/Linux/ubuntu/'; \
  echo "deb $repoUrl bionic $repoTypes" > /etc/apt/sources.list; \
  echo "deb $repoUrl bionic-updates $repoTypes" >> /etc/apt/sources.list; \
  echo "deb $repoUrl bionic-security $repoTypes" >> /etc/apt/sources.list; \
  \
  fetchDeps='ca-certificates wget gnupg'; \
  apt-get update; \
  apt-get install -y --no-install-recommends $fetchDeps; \
  rm -rf /var/lib/apt/lists/*; \
  \
  wget -q https://ftp.orca.med.or.jp/pub/ubuntu/archive.key; \
  apt-key add archive.key; \
  listUrl='https://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-bionic51.list'; \
  wget -q -O /etc/apt/sources.list.d/jma-receipt-bionic51.list $listUrl; \
  apt-get update; \
  apt-get dist-upgrade -y; \
  apt-get install -y --no-install-recommends jma-receipt; \
  apt-get install sudo

# ref: https://www.orca.med.or.jp/news/program_update_error_201905.html
RUN wget https://ftp.orca.med.or.jp/pub/etc/install_modules_for_ftp.tgz; \
  tar xvzf install_modules_for_ftp.tgz; \
  cd install_modules_for_ftp; \
  sudo -u orca ./install_modules.sh; \
  rm -rf /var/lib/apt/lists/*

RUN set -ex; \
  service postgresql start; \
  jma-setup; \
  printf '%s\n' "$ORMASTER_PASSWORD" "$ORMASTER_PASSWORD" | script -q -c 'sudo -u orca /usr/lib/jma-receipt/bin/passwd_store.sh' /dev/null; \
  # don't remove 'sudo' here because of warnings
  rm -rf /tmp/*

EXPOSE 8000
WORKDIR /
CMD service postgresql restart && service jma-receipt start && service jma-receipt stop && service jma-receipt start && tail -f /dev/null
