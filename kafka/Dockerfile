FROM alpine

ENV SCALA_VERSION 2.12
ENV KAFKA_VERSION 2.1.0
ENV ZOOKEEPER_VERSION 3.4.13

WORKDIR /kafka
ADD install.sh /kafka/install
RUN /kafka/install

ADD wait-for-zookeeper.sh /kafka/bin
ADD start.sh /kafka/bin

# Share the same base image to reduce layers used in testing
FROM gcr.io/distroless/java:11-debug
MAINTAINER OpenZipkin "http://zipkin.io/"

ENV SCALA_VERSION 2.12
ENV KAFKA_VERSION 2.1.0
ENV ZOOKEEPER_VERSION 3.4.13

WORKDIR /kafka

COPY --from=0 /kafka /kafka

# Port 19092 is for connections from the Docker host
EXPOSE 2181 9092 19092

ENTRYPOINT ["/busybox/sh", "/kafka/bin/start.sh"]
