FROM jenkins/jenkins:2.326-slim-jdk8

ARG MAVEN_VER=3.8.4

RUN apt update && \
    apt install wget docker-ce-cli -y && \
    wget -c https://dlcdn.apache.org/maven/maven-3/3.8.4/binaries/apache-maven-${MAVEN_VER}-bin.tar.gz && \
    tar -zxvf apache-maven-${MAVEN_VER}-bin.tar.gz -C /usr/local/ && \
    rm -rf apache-maven-${MAVEN_VER}-bin.tar.gz


ENV MAVEN_HOME /usr/local/apache-maven-${MAVEN_VER}
ENV PATH $MAVEN_HOME/bin:$PATH

USER root
