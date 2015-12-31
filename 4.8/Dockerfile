FROM ubuntu:14.04
MAINTAINER yamamuteki <yama@muteki.com>

# Reference to http://www.orca.med.or.jp/receipt/download/trusty/trusty_install_48.html.
# Fix some isuees.
WORKDIR /root
RUN apt-get update
RUN apt-get install -y wget syslinux-common

# Upgrade distriburion.
RUN wget -q https://ftp.orca.med.or.jp/pub/ubuntu/archive.key && apt-key add archive.key
RUN wget -q -O /etc/apt/sources.list.d/jma-receipt-trusty48.list https://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-trusty48.list
RUN apt-get update
RUN apt-get dist-upgrade -y

# Install jma-receipt.
RUN apt-get install -y jma-receipt
RUN wget https://ftp.orca.med.or.jp/pub/data/receipt/outline/update/claim_update.tar.gz && tar xvzf claim_update.tar.gz && bash claim_update.sh
RUN service postgresql restart && jma-setup

# Set password for ormaster.
RUN service postgresql restart && sleep 10 && cd /tmp && /bin/echo -e "ormaster\normaster" | sudo -u orca /usr/lib/jma-receipt/bin/passwd_store.sh

# Clean up.
RUN apt-get clean

EXPOSE 8000
CMD service postgresql restart && service jma-receipt start && tail -f /dev/null
