ARG JAVA_IMAGE_TAG=alpine-slim

FROM adoptopenjdk/openjdk11:$JAVA_IMAGE_TAG

ARG KAFKA_VERSION=2.8.1
ARG SCALA_VERSION=2.12
#ARG SHA512HASH="287CF3B43CC723E9A2DACC83E153152A74CF9C39D86EB0702CC8D237BE95577098E0B984687F3F7EA37BB2782F7DF23DD597FD618AF03A48460E6B3F4931B6C2"

LABEL org.label-schema.name="kafka" \
      org.label-schema.description="Apache Kafka" \
      org.label-schema.build-date="${build_date}" \
      org.label-schema.version="${SCALA_VERSION}_${KAFKA_VERSION}" \
      org.label-schema.schema-version="1.0"

RUN apk add --no-cache bash curl zip

ENV KAFKA_VERSION=$KAFKA_VERSION \
    SCALA_VERSION=$SCALA_VERSION \
    KAFKA_HOME=/opt/kafka \
    GLIBC_VERSION=$glibc_version \
    KAFKA_URL_PATH=kafka/$KAFKA_VERSION/kafka_$SCALA_VERSION-$KAFKA_VERSION.tgz

ENV KAFKA_DATA=$KAFKA_HOME/data

USER root

RUN addgroup -g 1001 -S kafka && adduser -u 1001 -G kafka -h $KAFKA_HOME -s /sbin/nologin -S kafka && \
    chmod 755 $KAFKA_HOME
RUN mkdir $KAFKA_DATA && \
    mkdir $KAFKA_HOME/logs

RUN chown -R kafka $KAFKA_HOME && \
    chgrp -R kafka $KAFKA_HOME

RUN curl -fSL -o /tmp/kafka.tgz $(curl --stderr /dev/null https://www.apache.org/dyn/closer.cgi\?as_json\=1 | sed -rn 's/.*"preferred":.*"(.*)"/\1/p')$KAFKA_URL_PATH || curl -fSL -o /tmp/kafka.tgz https://archive.apache.org/dist/$KAFKA_URL_PATH && \
#    echo "$SHA512HASH /tmp/kafka.tgz" | sha512sum -c - && \
    tar -xzf /tmp/kafka.tgz -C $KAFKA_HOME --strip-components 1 && \
    rm -f /tmp/kafka.tgz

# RUN zip -d /kafka/libs/log4j-1.2.17.jar org/apache/log4j/net/JMSAppender.class org/apache/log4j/net/SocketServer.class

COPY ./log4j.properties $KAFKA_HOME/config/log4j.properties
RUN mkdir $KAFKA_HOME/config.orig &&\
    mv $KAFKA_HOME/config/* $KAFKA_HOME/config.orig &&\
    chown -R kafka:kafka $KAFKA_HOME/config.orig

RUN rm -f $KAFKA_HOME/libs/*-{sources,javadoc,scaladoc}.jar* &&\
    rm -r $KAFKA_HOME/site-docs

RUN chmod -R g+w,o+w $KAFKA_HOME

USER kafka

WORKDIR $KAFKA_HOME

EXPOSE 9092
VOLUME ["/kafka/data","/kafka/logs","/kafka/config"]

COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["start"]
