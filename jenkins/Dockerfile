FROM chaiyd/openjdk:jdk8-alpine

LABEL maintainer="Jenkins Docker Maintainers <chaiyd.cn@gmail.com>"

RUN apk add --no-cache git openssh-client curl unzip bash ttf-dejavu coreutils tini npm

ARG user=jenkins
ARG group=jenkins
ARG uid=1000
ARG gid=1000
ARG http_port=8080
ARG agent_port=50000
ARG JENKINS_HOME=/var/jenkins_home
ARG REF=/usr/share/jenkins/ref
ARG maven=maven-3.6.1
ARG JENKINS_VERSION=2.235.3

ENV JENKINS_HOME $JENKINS_HOME
ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
ENV REF $REF

RUN mkdir -p $JENKINS_HOME \
  && chown ${uid}:${gid} $JENKINS_HOME \
  && addgroup -g ${gid} ${group} \
  && adduser -h "$JENKINS_HOME" -u ${uid} -G ${group} -s /bin/bash -D ${user}

VOLUME $JENKINS_HOME

RUN mkdir -p /usr/share/jenkins/ref/init.groovy.d

# jenkins version being bundled in this docker image
ENV JENKINS_VERSION ${JENKINS_VERSION}

# Can be used to customize where jenkins.war get downloaded from
#ARG JENKINS_URL=https://mirrors.tuna.tsinghua.edu.cn/jenkins/war-stable/${JENKINS_VERSION}/jenkins.war  #china url
ARG JENKINS_URL=http://mirror.xmission.com/jenkins/war-stable/${JENKINS_VERSION}/jenkins.war

RUN curl -fsSL ${JENKINS_URL} -o /usr/share/jenkins/jenkins.war 

ENV JENKINS_UC https://updates.jenkins.io
ENV JENKINS_UC_EXPERIMENTAL=https://updates.jenkins.io/experimental
ENV JENKINS_INCREMENTALS_REPO_MIRROR=https://repo.jenkins-ci.org/incrementals
RUN chown -R ${user} "$JENKINS_HOME" "$REF"

# for main web interface:
EXPOSE ${http_port}

# will be used by attached slave agents:
EXPOSE ${agent_port}

ENV COPY_REFERENCE_FILE_LOG $JENKINS_HOME/copy_reference_file.log

#USER ${user}

COPY jenkins-support /usr/local/bin/jenkins-support
COPY jenkins.sh /usr/local/bin/jenkins.sh
COPY tini-shim.sh /bin/tini
ENTRYPOINT ["/sbin/tini", "--", "/usr/local/bin/jenkins.sh"]

# from a derived Dockerfile, can use `RUN plugins.sh active.txt` to setup /usr/share/jenkins/ref/plugins from a support bundle
COPY plugins.sh /usr/local/bin/plugins.sh
COPY install-plugins.sh /usr/local/bin/install-plugins.sh
RUN chmod a+x -R /usr/local/bin/

ADD $maven /usr/local/$maven
ENV MAVEN_HOME /usr/local/$maven
ENV PATH $MAVEN_HOME/bin:$PATH
RUN npm install -g cnpm --registry=https://registry.npm.taobao.org
USER ${user}
