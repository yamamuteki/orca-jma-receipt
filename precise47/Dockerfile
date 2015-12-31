FROM ubuntu:12.04
MAINTAINER yamamuteki <yama@muteki.com>

ENV ORMASTER_PASSWORD=ormaster

# Reference to http://www.orca.med.or.jp/receipt/download/precise/precise_install_47.html.
RUN set -xe \
  && cd /tmp \
  && apt-get install -y apt-utils whiptail wget ca-certificates \
  && dpkg-reconfigure apt-utils whiptail wget ca-certificates \
  && wget -q http://ftp.orca.med.or.jp/pub/ubuntu/archive.key \
  && apt-key add archive.key \
  && wget -q -O /etc/apt/sources.list.d/jma-receipt-precise47.list http://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-precise47.list \
  && apt-get update \
  && apt-get dist-upgrade -y \
  && apt-get install -y jma-receipt \
  && apt-get install -y orca-ca-cert \
  && /bin/echo -e "if [ -f /etc/ssl/certs/orca-project-ca-2.crt ]; then\nCACERTFILE=/etc/ssl/certs/orca-project-ca-2.crt\nfi">> /etc/jma-receipt/jma-receipt.conf \
  && service postgresql restart \
  && jma-setup \
  && gluseradd -file /etc/jma-receipt/passwd -p $ORMASTER_PASSWORD ormaster \
  && rm -rf /tmp/* /var/lib/apt/lists/*

EXPOSE 8000
CMD service postgresql restart && service jma-receipt start && tail -f /dev/null
