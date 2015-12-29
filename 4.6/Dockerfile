FROM phusion/ubuntu-lucid-32
MAINTAINER yamamuteki <yama@muteki.com>

# Reference to http://ftp.orca.med.or.jp/pub/data/receipt/download/lucid/lucid_install_46_03.pdf.
# Fix some isuees.
WORKDIR /root
RUN aptitude update
RUN aptitude install -y wget

# Upgrade distriburion.
RUN wget -q http://ftp.orca.med.or.jp/pub/ubuntu/archive.key && apt-key add archive.key
RUN wget -q -O /etc/apt/sources.list.d/jma-receipt-lucid46.list http://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-lucid46.list
RUN aptitude update
RUN aptitude dist-upgrade -y

# Install jma-receipt.
RUN aptitude install -y postgresql-8.4
RUN wget -q -O /etc/ssl/certs/orca-project-ca-1.crt http://ftp.orca.med.or.jp/pub/etc/orca-project-ca-2.crt
RUN service postgresql-8.4 restart && aptitude install -o Dpkg::Options::="--force-confold" -y jma-receipt

# CAUTION: Change no verify expired CA certificate for patches signed by orca-project-ca-1.crt in online upgrade.
RUN sed -i -E 's/(flag = OpenSSL::PKCS7::BINARY)/\1 | OpenSSL::PKCS7::NOVERIFY/' /usr/lib/jma-receipt/scripts/allways/orcadt_verify.rb

# Set password for ormaster.
RUN gluseradd -file /etc/jma-receipt/passwd -p ormaster ormaster

# Clean up.
RUN aptitude clean

EXPOSE 8000
CMD service postgresql-8.4 restart && service jma-receipt start && tail -f /dev/null
