FROM jenkins/jenkins:2.326-slim-jdk8

ARG MAVEN_VER=3.8.4

USER root

RUN apt update && \
    apt install wget ca-certificates software-properties-common curl lsb-release -y && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"

RUN apt update && \
    apt install  docker-ce-cli -y && \
    wget -c https://dlcdn.apache.org/maven/maven-3/3.8.4/binaries/apache-maven-${MAVEN_VER}-bin.tar.gz && \
    tar -zxvf apache-maven-${MAVEN_VER}-bin.tar.gz -C /usr/local/ && \
    rm -rf apache-maven-${MAVEN_VER}-bin.tar.gz

ENV MAVEN_HOME /usr/local/apache-maven-${MAVEN_VER}
ENV PATH $MAVEN_HOME/bin:$PATH
