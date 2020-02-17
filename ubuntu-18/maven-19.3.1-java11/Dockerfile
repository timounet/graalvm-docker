FROM tperdriau/ubuntu-graalvm:19.3.1-java11

RUN apt-get update

RUN apt-get install maven -y
# Clean
RUN apt-get clean
RUN apt-get autoclean
RUN apt-get autoremove

RUN groupadd -r quarkus -g 1001 && useradd -u 1001 -r -g 1001 -m -d /home/quarkus -s /sbin/nologin -c "Quarkus user" quarkus
USER 1001
WORKDIR /project

CMD mvn
