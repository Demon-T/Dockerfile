#######################################################
# This dockerfile builds a clean ubuntu 14.04 and     #
# including ssh, git, vim, etc..., basic apps.        #
#                                                     #
# Author: Demon Tsai                                  #
# Repository: demontsai/ubuntu64:14.04                #
# Version: 1.0                                        #
#                                                     #
#######################################################

FROM ubuntu:14.04

MAINTAINER Demon Tsai demontsai@estinet.com


##### Adjust time zone
RUN     echo "Asia/Taipei" > /etc/timezone
RUN     dpkg-reconfigure --frontend noninteractive tzdata


##### Update system and install apps
RUN     apt-get update -qq && apt-get upgrade -qq
RUN     apt-get install -qqy openssh-server screen vim git gitk wget curl


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
EXPOSE  22


##### Screen setting
COPY    screenrc /etc/
RUN     mkdir -p /var/run/screen
RUN     chmod 0777 /var/run/screen


##### Vim
COPY    vimrc.local /etc/vim/


##### Clean
RUN     apt-get clean
RUN     rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


##### User
USER    estinet
ENV     HOME            /home/estinet
ENV     CODE_PATH       $HOME/code
ENV     INSTALL_PATH    $HOME/installation 
ENV     RUN_BASH        $HOME/start/run.bash
WORKDIR $HOME
COPY    bashrc $HOME/.bashrc
RUN     sudo chmod 0644 $HOME/.bashrc
RUN     sudo chown estinet:estinet $HOME/.bashrc
RUN     mkdir $CODE_PATH
RUN     mkdir $INSTALL_PATH
RUN     mkdir $HOME/start
RUN     touch $RUN_BASH
RUN     echo "#!/bin/bash" >> $RUN_BASH


