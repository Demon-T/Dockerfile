#######################################################
# This dockerfile builds freeradius                   #
#                                                     #
# Author: Demon Tsai                                  #
# Repository: demontsai/freeradius:3.0.11             #
# Version: 1.0                                        #
#                                                     #
#######################################################

FROM ubuntu:14.04

MAINTAINER Demon Tsai demontsai@estinet.com


##### Adjust time zone
RUN     echo "Asia/Taipei" > /etc/timezone
RUN     dpkg-reconfigure --frontend noninteractive tzdata

##### Update system and install apps
RUN     apt-get update && apt-get -y upgrade
RUN     apt-get install -y vim wget dpkg-dev ssl-cert quilt fakeroot\
        libcurl4-openssl-dev libcap-dev libjson0-dev libjson-c-dev \
        libreadline-dev libsqlite3-dev libtalloc-dev libwbclient-dev \
        libyubikey-dev libykclient-dev libmemcached-dev libhiredis-dev samba-dev
RUN     apt-get -y build-dep freeradius

##### Environment
ENV     FREERADIUS_VERSION  3.0.11
ENV     FREERADIUS_URL      ftp://ftp.freeradius.org/pub/freeradius/freeradius-server-$FREERADIUS_VERSION.tar.gz

##### Create an user account and assign password to it
RUN     useradd -m  radius && adduser radius sudo && chsh -s /bin/bash radius
RUN     echo "radius:radius" | chpasswd

##### Join the user to sudoers
RUN     echo 'radius ALL=(ALL) NOPASSWD:ALL' > /tmp/radius
RUN     echo 'Defaults:radius !requiretty' >> /tmp/radius
RUN     chmod 0440 /tmp/radius
RUN     chown root:root /tmp/radius
RUN     mv /tmp/radius /etc/sudoers.d/radius

##### Install freeradius
WORKDIR /root
RUN     wget $FREERADIUS_URL
RUN     tar xzvf freeradius-server-$FREERADIUS_VERSION.tar.gz
WORKDIR /root/freeradius-server-$FREERADIUS_VERSION
RUN     dpkg-buildpackage -b -uc
RUN     dpkg -i ../*.deb
RUN     touch /etc/freeradius/radius.log
RUN     chown freerad:freerad /etc/freeradius/radius.log

EXPOSE  18120/udp 1812/udp 1813/udp
VOLUME  ["/etc/freeradius/"]

##### Clean
RUN     apt-get clean
RUN     rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/*freeradius*

##### User
WORKDIR /home/radius
USER    radius

ENTRYPOINT  sudo freeradius -X -l /etc/freeradius/radius.log



