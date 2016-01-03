FROM yamamuteki/ubuntu-lucid-i386
MAINTAINER yamamuteki <yama@muteki.com>

ENV ORMASTER_PASSWORD=ormaster

# Reference to http://ftp.orca.med.or.jp/pub/data/receipt/download/lucid/lucid_install_47_01_20130430.pdf.
RUN set -xe \
  && cd /tmp \
  && apt-get update \
  && apt-get install -y apt-utils whiptail wget gnupg \
  && dpkg-reconfigure apt-utils whiptail wget gnupg \
  && wget -q http://ftp.orca.med.or.jp/pub/ubuntu/archive.key \
  && apt-key add archive.key \
  && wget -q -O /etc/apt/sources.list.d/jma-receipt-lucid47.list http://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-lucid47.list \
  && apt-get update \
  && apt-get dist-upgrade -y \
  && apt-get install -y jma-receipt \
  && wget -P /usr/lib/jma-receipt/patch-lib/data http://ftp.orca.med.or.jp/pub/etc/orca-project-ca-2.crt \
  && /bin/echo -e "if [ -f /usr/lib/jma-receipt/patch-lib/data/orca-project-ca-2.crt ]; then\nCACERTFILE=/usr/lib/jma-receipt/patch-lib/data/orca-project-ca-2.crt\nfi">> /etc/jma-receipt/jma-receipt.conf \
  && service postgresql-8.4 restart \
  && jma-setup \
  && gluseradd -file /etc/jma-receipt/passwd -p $ORMASTER_PASSWORD ormaster \
  && rm -rf /tmp/* /var/lib/apt/lists/*

EXPOSE 8000
CMD service postgresql-8.4 restart && service jma-receipt start && tail -f /dev/null
