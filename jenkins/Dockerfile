FROM chaiyd/openjdk:jdk8-alpine

LABEL maintainer="Jenkins Docker Maintainers <chaiyd.cn@gmail.com>"

RUN apk add --no-cache \
  bash \
  coreutils \
  curl \
  git \
  git-lfs \
  openssh-client \
  tini \
  ttf-dejavu \
  tzdata \
  unzip \
#  npm \
  docker

ARG user=jenkins
ARG group=jenkins
ARG uid=1000
ARG gid=1000
ARG http_port=8080
ARG agent_port=50000
ARG JENKINS_HOME=/var/jenkins_home
ARG REF=/usr/share/jenkins/ref
#ARG maven=maven-3.6.3
ARG JENKINS_VERSION=2.289.3

ENV JENKINS_HOME $JENKINS_HOME
ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
ENV REF $REF

# Jenkins is run with user `jenkins`, uid = 1000
# If you bind mount a volume from the host or a data container,
# ensure you use the same uid
RUN mkdir -p $JENKINS_HOME \
  && chown ${uid}:${gid} $JENKINS_HOME \
  && addgroup -g ${gid} ${group} \
  && adduser -h "$JENKINS_HOME" -u ${uid} -G ${group} -s /bin/bash -D ${user}

# Jenkins home directory is a volume, so configuration and build history
# can be persisted and survive image upgrades
VOLUME $JENKINS_HOME

# $REF (defaults to `/usr/share/jenkins/ref/`) contains all reference configuration we want
# to set on a fresh new installation. Use it to bundle additional plugins
# or config file with your custom jenkins Docker image.
RUN mkdir -p ${REF}/init.groovy.d

# jenkins version being bundled in this docker image
ENV JENKINS_VERSION ${JENKINS_VERSION}

# jenkins.war download will be validated using it

#ARG JENKINS_URL=http://mirror.xmission.com/jenkins/${JENKINS_VERSION}/jenkins.war
#ARG JENKINS_URL=https://mirrors.tuna.tsinghua.edu.cn/jenkins/war-stable/${JENKINS_VERSION}/jenkins.war  #china url
# LTS war
ARG JENKINS_URL=http://mirror.xmission.com/jenkins/war-stable/${JENKINS_VERSION}/jenkins.war
# Regular releases (Weekly)
#ARG JENKINS_URL=http://mirror.xmission.com/jenkins/war/${JENKINS_VERSION}/jenkins.war

# could use ADD but this one does not check Last-Modified header neither does it allow to control checksum
# see https://github.com/docker/docker/issues/8331
RUN curl -fsSL ${JENKINS_URL} -o /usr/share/jenkins/jenkins.war

ENV JENKINS_UC https://updates.jenkins.io
ENV JENKINS_UC_EXPERIMENTAL=https://updates.jenkins.io/experimental
ENV JENKINS_INCREMENTALS_REPO_MIRROR=https://repo.jenkins-ci.org/incrementals
RUN chown -R ${user} "$JENKINS_HOME" "$REF"

ARG PLUGIN_CLI_URL=https://github.com/jenkinsci/plugin-installation-manager-tool/releases/download/2.9.0/jenkins-plugin-manager-2.9.0.jar
RUN curl -fsSL ${PLUGIN_CLI_URL} -o /usr/lib/jenkins-plugin-manager.jar

# for main web interface:
EXPOSE ${http_port}

# will be used by attached slave agents:
EXPOSE ${agent_port}

ENV COPY_REFERENCE_FILE_LOG $JENKINS_HOME/copy_reference_file.log

COPY jenkins-support /usr/local/bin/jenkins-support
COPY jenkins.sh /usr/local/bin/jenkins.sh
COPY tini-shim.sh /bin/tini
COPY jenkins-plugin-cli.sh /bin/jenkins-plugin-cli

ENTRYPOINT ["/sbin/tini", "--", "/usr/local/bin/jenkins.sh"]

# from a derived Dockerfile, can use `RUN install-plugins.sh active.txt` to setup $REF/plugins from a support bundle
COPY install-plugins.sh /usr/local/bin/install-plugins.sh
#ADD $maven /usr/local/$maven
RUN chmod a+x -R /usr/local/bin/
#    && \
#    chmod a+x -R /usr/local/$maven/bin

#ENV MAVEN_HOME /usr/local/$maven
#ENV PATH $MAVEN_HOME/bin:$PATH
#RUN npm install -g cnpm --registry=https://registry.npm.taobao.org
USER ${user}
