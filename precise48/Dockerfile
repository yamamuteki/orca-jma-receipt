FROM ubuntu:12.04
MAINTAINER yamamuteki <yama@muteki.com>

ENV ORMASTER_PASSWORD=ormaster

# Reference to http://www.orca.med.or.jp/receipt/download/precise/precise_install_48.html.
RUN set -xe \
  && cd /tmp \
  && apt-get install -y apt-utils whiptail wget sudo syslinux-common \
  && dpkg-reconfigure apt-utils whiptail wget sudo syslinux-common \
  && wget -q http://ftp.orca.med.or.jp/pub/ubuntu/archive.key \
  && apt-key add archive.key \
  && wget -q -O /etc/apt/sources.list.d/jma-receipt-precise48.list http://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-precise48.list \
  && apt-get update \
  && apt-get dist-upgrade -y \
  && apt-get install -y jma-receipt \
  && wget https://ftp.orca.med.or.jp/pub/data/receipt/outline/update/claim_update.tar.gz \
  && tar xvzf claim_update.tar.gz \
  && bash claim_update.sh \
  && service postgresql restart \
  && jma-setup \
  && /bin/echo -e "$ORMASTER_PASSWORD\n$ORMASTER_PASSWORD" | sudo -u orca /usr/lib/jma-receipt/bin/passwd_store.sh \
  && rm -rf /tmp/* /var/lib/apt/lists/*

EXPOSE 8000
CMD service postgresql restart && service jma-receipt start && tail -f /dev/null
