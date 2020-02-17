FROM ubuntu:18.04

ARG GRAALVM_VERSION=19.3.1
ARG JAVA_VERSION=java11
ARG GRAALVM_ARCH=linux-amd64
ARG GRAALVM_PKG=https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-$GRAALVM_VERSION/graalvm-ce-$JAVA_VERSION-$GRAALVM_ARCH-$GRAALVM_VERSION.tar.gz
ARG TARGETPLATFORM=linux/amd64

ENV LANG=en_US.UTF-8 \
    JAVA_HOME=/opt/graalvm-ce-$JAVA_VERSION-$GRAALVM_VERSION/

ADD gu-wrapper.sh /usr/local/bin/gu
ADD run-java.sh /deployments
RUN apt-get update
RUN apt-get install curl -y
RUN apt-get install wget -y

RUN wget ${GRAALVM_PKG}
RUN gunzip graalvm-ce-$JAVA_VERSION-$GRAALVM_ARCH-$GRAALVM_VERSION.tar.gz
RUN tar -xvf graalvm-ce-$JAVA_VERSION-$GRAALVM_ARCH-$GRAALVM_VERSION.tar
RUN mv graalvm-ce-$JAVA_VERSION-$GRAALVM_VERSION /opt/
RUN rm -rf graalvm-ce-$JAVA_VERSION-$GRAALVM_ARCH-$GRAALVM_VERSION.tar
RUN mkdir -p "/usr/java"
RUN ln -sfT "$JAVA_HOME" /usr/java/default
RUN ln -sfT "$JAVA_HOME" /usr/java/latest
# RUN for bin in "$JAVA_HOME/bin/"*; do base="$(basename "$bin")"; [ ! -e "/usr/bin/$base" ]; alternatives --install "/usr/bin/$base" "$base" "$bin" 20000; done
RUN chmod +x /usr/local/bin/gu
RUN echo "export PATH=$PATH:/usr/java/default/bin" >> /etc/profile

RUN apt-get install libssl1.0.0 -y
RUN apt-get install libasound2 -y
# Clean
RUN apt-get clean
RUN apt-get autoclean
RUN apt-get autoremove

RUN groupadd -r quarkus -g 1001 && useradd -u 1001 -r -g 1001 -m -d /home/quarkus -s /sbin/nologin -c "Quarkus user" quarkus
USER 1001
WORKDIR /project

CMD mvn
