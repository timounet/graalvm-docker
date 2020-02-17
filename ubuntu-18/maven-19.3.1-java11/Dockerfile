FROM tperdriau/ubuntu-graalvm:19.3.1-java11
USER root
RUN apt-get update

RUN apt-get install maven -y
# Clean
RUN apt-get clean
RUN apt-get autoclean
RUN apt-get autoremove

USER 1001
WORKDIR /project

CMD mvn
