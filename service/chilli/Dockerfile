#######################################################
# This dockerfile builds chilli                       #
#                                                     #
# Author: Demon Tsai                                  #
# Repository: demontsai/coovachilli:1.3.0             #
# Version: 1.0                                        #
#                                                     #
#######################################################

FROM ubuntu:14.04

MAINTAINER Demon Tsai demontsai@estinet.com


##### Adjust time zone
RUN     echo "Asia/Taipei" > /etc/timezone
RUN     dpkg-reconfigure --frontend noninteractive tzdata


##### Update system and install apps
RUN     apt-get update
RUN     apt-get install -y vim wget iptables libssl-dev devscripts haserl debhelper


##### Environment
ENV     CHILLI_VERSION  1.3.0
ENV     CHILLI_URL      https://coova.github.io/coova-chilli/coova-chilli-$CHILLI_VERSION.tar.gz


##### Create an user account and assign password to it
RUN     useradd -m  chilli && adduser chilli sudo && chsh -s /bin/bash chilli
RUN     echo "chilli:chilli" | chpasswd


##### Join the user to sudoers
RUN     echo 'chilli ALL=(ALL) NOPASSWD:ALL' > /tmp/chilli
RUN     echo 'Defaults:chilli !requiretty' >> /tmp/chilli
RUN     chmod 0440 /tmp/chilli
RUN     chown root:root /tmp/chilli
RUN     mv /tmp/chilli /etc/sudoers.d/chilli


##### Install chilli
WORKDIR /root
RUN     wget $CHILLI_URL
RUN     tar xzvf coova-chilli-$CHILLI_VERSION.tar.gz
WORKDIR /root/coova-chilli-$CHILLI_VERSION
RUN     debuild -b -us -uc
RUN     dpkg -i ../*.deb
WORKDIR /root
COPY    chilli_cfg  /usr/sbin/
COPY    chilli_init /usr/sbin/


#EXPOSE  18120/udp 1812/udp 1813/udp


##### Clean
RUN     apt-get clean
RUN     rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/*chilli*

##### User
WORKDIR /home/chilli
USER    chilli


#ENTRYPOINT  bash chilli_init


