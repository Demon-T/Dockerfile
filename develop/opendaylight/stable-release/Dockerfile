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
RUN     apt-get -qq update

##### Environment
ENV     MVN_SETTING_URL     	https://raw.githubusercontent.com/opendaylight/odlparent/master/settings.xml
ENV 	CONTROLLER_VERSION  	1.0.2
ENV	CONTROLLER_NAME		E-Central-Enterprise-Controller
ENV 	CONTROLLER_URL	    	'http://jenkins.estinet.com/job/controller-stable/lastStableBuild/com.estinet.controller$'$CONTROLLER_NAME/artifact/com.estinet.controller/$CONTROLLER_NAME

##### Maven setting
RUN     mkdir ~/.m2
RUN 	wget -q -O - $MVN_SETTING_URL > ~/.m2/settings.xml

##### Controller
RUN	echo '[ -d \"$CONTROLLER_NAME-$CONTROLLER_VERSION\" ] || {' >> $ENTRYPOINT
RUN 	echo '    wget $CONTROLLER_URL/$CONTROLLER_VERSION/$CONTROLLER_NAME-$CONTROLLER_VERSION.tar.gz' >> $ENTRYPOINT
RUN 	echo '    tar xzvf $CONTROLLER_NAME-$CONTROLLER_VERSION.tar.gz' >> $ENTRYPOINT
RUN	echo '}' >> $ENTRYPOINT
RUN 	echo './$CONTROLLER_NAME-$CONTROLLER_VERSION/bin/karaf clean' >> $ENTRYPOINT

##### Expose port for controller
EXPOSE  6633 8181

##### Clean
RUN     apt-get clean
RUN     rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* *.tar.gz

##### Command
ENTRYPOINT $ENTRYPOINT

