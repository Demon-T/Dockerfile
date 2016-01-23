#######################################################
# This dockerfile builds a mininet environment        #
#                                                     #
#                                                     #
# Author: Demon Tsai                                  #
# Repository: demontsai/mininet:2.2.1                 #
# Version: 1.0                                        #
#                                                     #
#######################################################

FROM demontsai/ubuntu64:14.04

MAINTAINER Demon Tsai demontsai@estinet.com


##### Update system and install apps
RUN     sudo apt-get -qq update && sudo apt-get -qq upgrade


##### Environment
ENV     MININET_REPO git://github.com/mininet/mininet
ENV     MININET_VERSION 2.2.1
ENV     MININET_INSTALLER util/install.sh
ENV     MININET_OPT -nfv


##### Mininet install
RUN     git clone $MININET_REPO $INSTALL_PATH/mininet
WORKDIR $INSTALL_PATH/mininet
RUN     git checkout -b $MININET_VERSION $MININET_VERSION
RUN     sudo $MININET_INSTALLER $MININET_OPT
RUN     echo "sudo service openvswitch-switch start" >> $RUN_BASH
RUN     echo "/bin/bash" >> $RUN_BASH
WORKDIR $HOME


##### Sync source code folder
VOLUME  ["/home/estinet/code"]


##### Clean
RUN     sudo apt-get clean
RUN     sudo rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


##### Command
CMD     ["bash", "/home/estinet/start/run.bash"]
