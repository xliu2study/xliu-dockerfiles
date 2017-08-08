FROM ubuntu:14.04
MAINTAINER Xiaodong "xiaodong.liu@intel.com"
ENV REFRESHED_AT 2017-08-07

ENV http_proxy http://proxy-prc.intel.com:911/
ADD sources.list.trusty /etc/apt/sources.list
RUN apt-get -yqq update

RUN apt-get install -yqq curl apt-transport-https
RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
RUN echo deb https://apt.dockerproject.org/repo ubuntu-trusty main > /etc/apt/sources.list.d/docker.list
RUN apt-get update -qq
RUN apt-get install -qqy iptables ca-certificates openjdk-7-jdk git-core docker-engine wget

ENV JENKINS_HOME /opt/jenkins/data
ENV JENKINS_MIRROR http://mirrors.jenkins-ci.org

RUN mkdir -p $JENKINS_HOME/plugins
RUN curl -sf -o /opt/jenkins/jenkins.war -L $JENKINS_MIRROR/war/2.50/jenkins.war

RUN for plugin in chucknorris greenballs scm-api git-client git ws-cleanup ; \
	do curl -sl -o $JENKINS_HOME/plugins/${plugin}.hpi \
		-L $JENKINS_MIRROR/plugins/${plugin}/2.50/${plugin}.hpi; done

ADD ./dockerjenkins.sh /usr/local/bin/dockerjenkins.sh
RUN chmod +x /usr/local/bin/dockerjenkins.sh

VOLUME /var/lib/docker

EXPOSE 8080

ENTRYPOINT [ "/usr/local/bin/dockerjenkins.sh" ]

