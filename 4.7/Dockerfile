FROM ubuntu:12.04
MAINTAINER yamamuteki <yama@muteki.com>

# Reference to http://www.orca.med.or.jp/receipt/download/precise/precise_install_47.html.
# Fix some isuees.
WORKDIR /root
RUN apt-get update
RUN apt-get install -y apt-utils whiptail && dpkg-reconfigure apt-utils whiptail
RUN apt-get install -y wget sudo ca-certificates

# Upgrade distriburion.
RUN wget -q http://ftp.orca.med.or.jp/pub/ubuntu/archive.key && apt-key add archive.key
RUN wget -q -O /etc/apt/sources.list.d/jma-receipt-precise47.list http://ftp.orca.med.or.jp/pub/ubuntu/jma-receipt-precise47.list
RUN apt-get update
RUN apt-get dist-upgrade -y

# Add oruser.
RUN useradd -ms /bin/bash oruser && echo "oruser ALL=(ALL:ALL) NOPASSWD:ALL">> /etc/sudoers
USER oruser
WORKDIR /home/oruser

# Install jma-receipt.
RUN sudo apt-get install -y jma-receipt
RUN sudo apt-get install -y orca-ca-cert
RUN sudo echo -e "if [ -f /etc/ssl/certs/orca-project-ca-2.crt ]; then\nCACERTFILE=/etc/ssl/certs/orca-project-ca-2.crt\nfi" | sudo tee -a /etc/jma-receipt/jma-receipt.conf
RUN sudo service postgresql restart && sudo jma-setup

# Set password for ormaster.
RUN sudo gluseradd -file /etc/jma-receipt/passwd -p ormaster ormaster

# Clean up.
RUN sudo apt-get clean

EXPOSE 8000 5432
CMD sudo service postgresql restart && sudo service jma-receipt start && tail -f /dev/null
