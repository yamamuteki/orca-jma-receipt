FROM yamamuteki/ubuntu-lucid-i386
MAINTAINER yamamuteki <yama@muteki.com>

ENV ORMASTER_PASSWORD=ormaster

# Reference to http://ftp.orca.med.or.jp/pub/data/receipt/download/lucid/lucid_install_46_03.pdf.
RUN set -xe \
  && cd /tmp \
  && apt-get update \
  && apt-get install -y apt-utils whiptail wget gnupg \
  && dpkg-reconfigure apt-utils whiptail wget gnupg \
  && wget -q http://ftp.orca.med.or.jp/pub/ubuntu/archive.key \
  && apt-key add archive.key \
  && wget -q -O /etc/apt/sources.list.d/jma-receipt-lucid46.list http://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-lucid46.list \
  && apt-get update \
  && apt-get dist-upgrade -y \
  && wget -q -O /etc/ssl/certs/orca-project-ca-1.crt http://ftp.orca.med.or.jp/pub/etc/orca-project-ca-2.crt \
  && apt-get install -y postgresql-8.4 \
  && service postgresql-8.4 restart \
  && apt-get install -o Dpkg::Options::="--force-confold" -y jma-receipt \
  && gluseradd -file /etc/jma-receipt/passwd -p $ORMASTER_PASSWORD ormaster \
  && rm -rf /tmp/* /var/lib/apt/lists/* \
  && mkdir /var/lib/apt/lists/partial

# CAUTION: Change no verify expired CA certificate for patches signed by orca-project-ca-1.crt in online upgrade.
RUN sed -i -E 's/(flag = OpenSSL::PKCS7::BINARY)/\1 | OpenSSL::PKCS7::NOVERIFY/' /usr/lib/jma-receipt/scripts/allways/orcadt_verify.rb

EXPOSE 8000
CMD service postgresql-8.4 restart && service jma-receipt start && tail -f /dev/null
