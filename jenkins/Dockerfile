#this is the base image we use to create our image from
FROM jenkins/jenkins:2.138
MAINTAINER Navapon Tantechasa (navapon.dev@gmail.com)
#get rid of admin password setup
# ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

USER root

RUN \
    apt-get update && \
    apt-get install -y build-essential && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

#automatically installing all plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# set permission use docker host agent
CMD DOCKER_GID=$(stat -c '%g' /var/run/docker.sock) && \
    groupadd -for -g ${DOCKER_GID} docker && \
    usermod -aG docker jenkins && \
    /usr/local/bin/jenkins.sh