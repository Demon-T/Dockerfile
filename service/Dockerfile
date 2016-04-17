
#######################################################
# This dockerfile builds pocketmine                   #
#                                                     #
# Author: Demon Tsai                                  #
# Repository: demontsai/pocketmine:latest             #
# Version: 1.0                                        #
#                                                     #
#######################################################

FROM demontsai/ubuntu64:14.04

MAINTAINER Demon Tsai demontsai@estinet.com



##### Environment
ENV     POCKETMINE      /root/installation/PocketMine-MP
ENV     POCKETMINE_SRC  $POCKETMINE/src/pocketmine


##### Install pocketmine
RUN     wget -q -O - https://raw.githubusercontent.com/PocketMine/php-build-scripts/master/installer.sh | bash -s - -r -d installation/1.4.0
RUN     git clone --recursive https://github.com/PocketMine/PocketMine-MP.git $POCKETMINE
RUN     cp -rf installation/1.4.0/bin $POCKETMINE/

RUN     sed -i '33 i \ \ \ \ const\ ACCEPTED\_PROTOCOLS\ =\ [45,\ 46];' $POCKETMINE_SRC/network/protocol/Info.php
RUN     sed -i 's/v0.14.0.0 alpha/v0.14.x alpha/' $POCKETMINE_SRC/PocketMine.php
RUN     sed -i 's/0.14.0.0/0.14.0/' $POCKETMINE_SRC/PocketMine.php
RUN     sed -i 's/$packet->protocol1 !== ProtocolInfo::CURRENT_PROTOCOL/!in_array($packet->protocol1, ProtocolInfo::ACCEPTED_PROTOCOLS)/' $POCKETMINE_SRC/Player.php

RUN     echo "[ -f \"pocketmine/start.sh\" ] || cp -rf installation/PocketMine-MP/* pocketmine/" >> $ENTRYPOINT
RUN     echo "./pocketmine/start.sh --no-wizard --enable-rcon=on" >> $ENTRYPOINT

VOLUME  /root/pocketmine

EXPOSE 19132
EXPOSE 19132/udp


##### Clean
RUN     apt-get clean
RUN     rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 1.4.0

##### Command
ENTRYPOINT $ENTRYPOINT

