FROM ubuntu:12.04
MAINTAINER yamamuteki <yama@muteki.com>

# Reference to http://www.orca.med.or.jp/receipt/download/precise/precise_install_47.html.
# Fix some isuees.
WORKDIR /root
RUN apt-get update
RUN apt-get install -y apt-utils whiptail && dpkg-reconfigure apt-utils whiptail
RUN apt-get install -y wget ca-certificates

# Upgrade distriburion.
RUN wget -q http://ftp.orca.med.or.jp/pub/ubuntu/archive.key && apt-key add archive.key
RUN wget -q -O /etc/apt/sources.list.d/jma-receipt-precise47.list http://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-precise47.list
RUN apt-get update
RUN apt-get dist-upgrade -y

# Install jma-receipt.
RUN apt-get install -y jma-receipt
RUN apt-get install -y orca-ca-cert
RUN /bin/echo -e "if [ -f /etc/ssl/certs/orca-project-ca-2.crt ]; then\nCACERTFILE=/etc/ssl/certs/orca-project-ca-2.crt\nfi">> /etc/jma-receipt/jma-receipt.conf
RUN service postgresql restart && jma-setup

# Set password for ormaster.
RUN gluseradd -file /etc/jma-receipt/passwd -p ormaster ormaster

# Clean up.
RUN apt-get clean

EXPOSE 8000
CMD service postgresql restart && service jma-receipt start && tail -f /dev/null
