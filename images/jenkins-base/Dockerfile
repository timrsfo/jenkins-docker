# images/jenkins-base/Dockerfile
FROM centos:7
MAINTAINER timr@gmail.com

LABEL dockerfile_location=https://github.com/timrfso/jenkins-practice/tree/master/images/jenkins-base/Dockerfile \
      image_name=practicejenkins/jenkins-base \
      base_image=centos:7


# Jenkins' Environment
ENV JENKINS_HOME /var/jenkins_home
ENV JENKINS_ROOT /usr/share/jenkins
ENV JENKINS_WAR /usr/share/jenkins/ref/warfile/jenkins.war
ENV JENKINS_SLAVE_AGENT_PORT 50000
ENV user=jenkins
ENV group=jenkins
ENV uid=1000
ENV gid=1000

# Jenkins Version info
ENV JENKINS_VERSION 2.108
ENV JENKINS_SHA 396406c34da9d7f7710a1d5d60057a67c2e97b6ff77e1516585b4b40d9c566b6

# These URLs can be swapped out for internal repos if needed. Secrets required may vary :)
ENV JENKINS_UC https://updates.jenkins.io
ENV JENKINS_URL http://mirrors.jenkins.io/war/${JENKINS_VERSION}/jenkins.war

# Jenkins is run with user `jenkins`, uid = 1000
# If you bind mount a volume from the host or a data container,
# ensure you use the same uid
RUN groupadd -g ${gid} ${group} \
    && useradd -d "$JENKINS_HOME" -u ${uid} -g ${group} -s /bin/bash ${user}

# Install our tools and make them executable
COPY files/jenkins-support /usr/local/bin/jenkins-support
RUN mkdir -p ${JENKINS_ROOT}
RUN chown jenkins /usr/local/bin/* ${JENKINS_ROOT} \
    && chmod +x /usr/local/bin/*

# Configure to Los Angeles timezone. I dislike debugging failures in UTC
RUN unlink /etc/localtime && ln -s /usr/share/zoneinfo/America/Los_Angeles /etc/localtime

# Install Java, Git, and Unzip
RUN yum install -y java-1.8.0-openjdk-devel tzdata-java git unzip \
    && yum clean all

