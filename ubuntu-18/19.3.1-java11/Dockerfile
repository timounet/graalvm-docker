FROM ubuntu:18.04

ARG GRAALVM_VERSION=19.3.1
ARG JAVA_VERSION=java11
ARG GRAALVM_ARCH=linux-amd64
ARG GRAALVM_PKG=https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-$GRAALVM_VERSION/graalvm-ce-$JAVA_VERSION-$GRAALVM_ARCH-$GRAALVM_VERSION.tar.gz
ARG TARGETPLATFORM=linux/amd64

ENV JAVA_HOME=/opt/graalvm-ce-$JAVA_VERSION-$GRAALVM_VERSION/ \
    PATH=$PATH:/usr/java/default/bin

RUN mkdir -p /deployments

COPY gu-wrapper.sh /usr/local/bin/gu
COPY run-java.sh /deployments/

RUN apt-get -q update

RUN apt-get -q install curl -y
RUN apt-get -q install wget -y

RUN wget --quiet ${GRAALVM_PKG}
RUN tar -zxf graalvm-ce-$JAVA_VERSION-$GRAALVM_ARCH-$GRAALVM_VERSION.tar.gz
RUN mv graalvm-ce-$JAVA_VERSION-$GRAALVM_VERSION /opt/
RUN rm -rf graalvm-ce-$JAVA_VERSION-$GRAALVM_ARCH-$GRAALVM_VERSION.tar.gz
RUN ls
RUN mkdir -p "/usr/java"
RUN ln -sfT "$JAVA_HOME" /usr/java/default
RUN ln -sfT "$JAVA_HOME" /usr/java/latest

# /dev/urandom is used as random source, which is perfectly safe
# according to http://www.2uo.de/myths-about-urandom/
RUN echo "securerandom.source=file:/dev/urandom" >> /usr/java/default/lib/security/java.security

# RUN for bin in "$JAVA_HOME/bin/"*; do base="$(basename "$bin")"; [ ! -e "/usr/bin/$base" ]; alternatives --install "/usr/bin/$base" "$base" "$bin" 20000; done
RUN chmod +x /usr/local/bin/gu
# RUN echo "export PATH=$PATH:/usr/java/default/bin" >> /etc/profile
COPY run-java.sh /deployments/
RUN chmod 755 /deployments/run-java.sh

RUN apt-get install libssl1.0.0 -y
RUN apt-get install libasound2 -y
# Clean
RUN apt-get clean
RUN apt-get autoclean
RUN apt-get autoremove

RUN groupadd -r quarkus -g 1001 && useradd -u 1001 -r -g 1001 -m -d /home/quarkus -s /sbin/nologin -c "Quarkus user" quarkus
USER 1001
WORKDIR /project

CMD [ "/deployments/run-java.sh" ]
