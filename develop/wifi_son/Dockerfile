#######################################################
# This dockerfile builds a clean ubuntu 14.04 and     #
# including samba, ssh, etc..., basic apps.           #
#                                                     #
# Author: Demon Tsai                                  #
#                                                     #
# Version: 1.0                                        #
#                                                     #
#######################################################

FROM ubuntu:14.04

MAINTAINER Demon Tsai demontsai@estinet.com


##### Update system and install apps
RUN     apt-get upgrade -qq
RUN     apt-get update -qq
RUN     apt-get install -qqy openssh-server screen vim git gitk gnome-terminal gcc make autoconf


##### Create an user account and assign password to it
RUN     useradd -m  estinet && adduser estinet sudo && chsh -s /bin/bash estinet
RUN     echo "estinet:estinet" | chpasswd


##### Join the user to sudoers
RUN     echo 'estinet ALL=(ALL) NOPASSWD:ALL' > /tmp/estinet
RUN     echo 'Defaults:estinet !requiretty' >> /tmp/estinet
RUN     chmod 0440 /tmp/estinet
RUN     chown root:root /tmp/estinet
RUN     mv /tmp/estinet /etc/sudoers.d/estinet


##### SSH setting
RUN     sudo -u estinet /usr/bin/ssh-keygen -t ecdsa -N "" -f /home/estinet/.ssh/id_ecdsa
RUN     sudo -u estinet /usr/bin/ssh-keygen -t rsa -N "" -f /home/estinet/.ssh/id_rsa
RUN     mkdir -p /var/run/sshd


##### Screen setting
COPY    screenrc /etc/
RUN     mkdir -p /var/run/screen
RUN     chmod 0777 /var/run/screen


##### Vim
COPY    vimrc.local /etc/vim/


##### Bashrc
COPY    bashrc /home/estinet/.bashrc
RUN     chmod 0644 /home/estinet/.bashrc
