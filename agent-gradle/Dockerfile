FROM arnaudeprez/jenkins-slave-base:latest

LABEL maintainer="Arnaud Deprez <arnaudeprez@gmail.com>"

ENV GRADLE_VERSION=4.6 \
    GRADLE_HOME=/opt/gradle

ARG GRADLE_DOWNLOAD_SHA256=98bd5fd2b30e070517e03c51cbb32beee3e2ee1a84003a5a5d748996d4b1b915
RUN set -o errexit -o nounset \
    && INSTALL_PKGS="java-1.8.0-openjdk-devel.x86_64 java-1.8.0-openjdk-devel.i686" \
    && yum install -y --enablerepo=centosplus $INSTALL_PKGS \
	&& echo "Downloading Gradle" \
	&& wget -O gradle.zip "https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip" \
	\
	&& echo "Checking download hash" \
	&& echo "${GRADLE_DOWNLOAD_SHA256} *gradle.zip" | sha256sum -c - \
	\
	&& echo "Installing Gradle" \
	&& unzip gradle.zip \
	&& rm gradle.zip \
	&& mkdir -p /opt \
	&& mv "gradle-${GRADLE_VERSION}" "${GRADLE_HOME}/" \
    && ln --symbolic "${GRADLE_HOME}/bin/gradle" /usr/bin/gradle \
    && mkdir -p $HOME/.gradle \
    && rpm -V $INSTALL_PKGS \
    && yum clean all -y

ADD ./contrib/gradle/* $HOME/.gradle/
ADD test $HOME/test/

RUN chown -R 1001:0 $HOME && \
    chmod -R g+rw $HOME

USER 1001
