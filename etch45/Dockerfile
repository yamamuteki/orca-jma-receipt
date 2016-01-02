FROM yamamuteki/debian-etch-i386
MAINTAINER yamamuteki <yama@muteki.com>

ENV ORMASTER_PASSWORD=ormaster

# Reference to http://ftp.orca.med.or.jp/pub/data/receipt/download/etch/etch_install-4.pdf.
RUN set -xe \
  && cd /tmp \
  && apt-get update \
  && apt-get install -y apt-utils whiptail wget \
  && dpkg-reconfigure apt-utils whiptail wget \
  && wget -q http://ftp.orca.med.or.jp/pub/debian/archive.key \
  && apt-key add archive.key \
  && echo "deb http://archive.debian.org/debian/ etch main contrib non-free"> /etc/apt/sources.list \
  && echo "deb-src http://archive.debian.org/debian/ etch main contrib non-free">> /etc/apt/sources.list \
  && echo "deb http://ftp.orca.med.or.jp/pub/debian/ etch jma">> /etc/apt/sources.list \
  && echo "deb-src http://ftp.orca.med.or.jp/pub/debian/ etch jma">> /etc/apt/sources.list \
  && apt-get update \
  && apt-get dist-upgrade -y \
  && mkdir -p /etc/ssl/certs \
  && wget -q -O /etc/ssl/certs/orca-project-ca-1.crt http://ftp.orca.med.or.jp/pub/etc/orca-project-ca-2.crt \
  && apt-get install -y postgresql-8.1 \
  && /etc/init.d/postgresql-8.1 restart \
  && apt-get install -o Dpkg::Options::="--force-confold" -y jma-receipt \
  && gluseradd -file /etc/jma-receipt/passwd -p $ORMASTER_PASSWORD ormaster \
  && rm -rf /tmp/* /var/lib/apt/lists/* \
  && mkdir /var/lib/apt/lists/partial

# CAUTION: Change no verify expired CA certificate for patches signed by orca-project-ca-1.crt in online upgrade.
RUN sed -i -r -e 's/(flag = OpenSSL::PKCS7::BINARY)/\1 | OpenSSL::PKCS7::NOVERIFY/' /usr/lib/jma-receipt/scripts/allways/orcadt_verify.rb

EXPOSE 8000
CMD /etc/init.d/postgresql-8.1 restart && /etc/init.d/jma-receipt start && tail -f /dev/null
