FROM ubuntu:14.04
MAINTAINER yamamuteki <yama@muteki.com>

# Reference to http://www.orca.med.or.jp/receipt/download/trusty/trusty_install_48.html.
# Fix some isuees.
WORKDIR /root
RUN echo '#!/bin/sh' > /usr/sbin/policy-rc.d && echo 'exit 0' >> /usr/sbin/policy-rc.d
RUN touch /etc/init.d/systemd-logind
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get install -y wget

# Upgrade distriburion.
RUN wget -q https://ftp.orca.med.or.jp/pub/ubuntu/archive.key && apt-key add archive.key
RUN wget -q -O /etc/apt/sources.list.d/jma-receipt-trusty48.list https://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-trusty48.list
RUN apt-get update
RUN apt-get dist-upgrade -y

# Add oruser.
RUN useradd -ms /bin/bash oruser && echo "oruser ALL=(ALL:ALL) NOPASSWD:ALL">> /etc/sudoers
USER oruser
WORKDIR /home/oruser

# Install jma-receipt.
RUN sudo apt-get install -y jma-receipt
RUN wget https://ftp.orca.med.or.jp/pub/data/receipt/outline/update/claim_update.tar.gz && tar xvzf claim_update.tar.gz && sudo bash claim_update.sh
RUN sudo service postgresql restart && sudo jma-setup

# Set password for ormaster.
RUN sudo apt-get install -y syslinux-common
RUN sudo service postgresql restart && sudo -u orca echo -e "ormaster\normaster" | sudo -u orca /usr/lib/jma-receipt/bin/passwd_store.sh

# Clean up.
RUN sudo apt-get clean

EXPOSE 8000
CMD sudo service postgresql restart && sudo service jma-receipt start && tail -f /dev/null
