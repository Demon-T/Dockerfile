#######################################################
# This dockerfile builds a opendaylight develop       #
# environment                                         #
#                                                     #
# Author: Demon Tsai                                  #
# Repository: demontsai/opendaylight                  #
# Version: 1.0                                        #
#                                                     #
#######################################################

FROM demontsai/maven:3.3.9

MAINTAINER Demon Tsai demontsai@estinet.com


##### Update system and install apps
RUN     sudo apt-get update -qq && sudo apt-get upgrade -qq


##### Environment
ENV     MVN_SETTING_URL     https://raw.githubusercontent.com/opendaylight/odlparent/master/settings.xml


##### Maven setting
RUN     mkdir $HOME/.m2
RUN     cp ~/.m2/settings.xml{,.orig} ; \wget -q -O - $MVN_SETTING_URL > ~/.m2/settings.xml


##### Expose port for controller
EXPOSE  6633 6653 8181


##### Sync source code folder
VOLUME  ["/home/estinet/code"]


##### Clean
RUN     sudo apt-get clean
RUN     sudo rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*



