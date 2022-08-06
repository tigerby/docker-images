ARG IMAGE_TAG=2.12-2.8.1-0.1

FROM tigerby/kafka:$IMAGE_TAG

EXPOSE 8083
VOLUME ["/kafka/data","/kafka/logs","/kafka/config"]

COPY docker-entrypoint.sh /
COPY --chown=kafka:kafka log4j.properties $KAFKA_HOME/config/log4j.properties

#
# Set up the plugins directory ...
#
ENV KAFKA_CONNECT_PLUGINS_DIR=$KAFKA_HOME/connect \
    KAFKA_ETC=$KAFKA_HOME/etc \
    CONNECT_PLUGIN_PATH=$KAFKA_CONNECT_PLUGINS_DIR \
    MAVEN_DEP_DESTINATION=$KAFKA_HOME/libs \
    CONFLUENT_VERSION=6.0.3 \
    AVRO_VERSION=1.9.2 \
    JMX_PROMETHEUS_AGENT_VERSION=0.17.0 \
#    JMXPORT=9091 \
    ENABLE_JMX_EXPORTER=true \
    CONFLUENT_S3_SINK_VERSION=10.0.11

RUN mkdir "$KAFKA_CONNECT_PLUGINS_DIR" "$KAFKA_ETC"

RUN curl -so /tmp/confluentinc-kafka-connect-s3-$CONFLUENT_S3_SINK_VERSION.zip https://SOMEWHERE_ZIPFILE_UPLOADED/confluentinc-kafka-connect-s3-$CONFLUENT_S3_SINK_VERSION.zip && \
    unzip /tmp/confluentinc-kafka-connect-s3-$CONFLUENT_S3_SINK_VERSION.zip -d $KAFKA_CONNECT_PLUGINS_DIR && \
    rm -f /tmp/confluentinc-kafka-connect-s3-$CONFLUENT_S3_SINK_VERSION.zip

# JMX exporter
RUN curl -so $KAFKA_ETC/jmx_prometheus_javaagent.jar https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/$JMX_PROMETHEUS_AGENT_VERSION/jmx_prometheus_javaagent-$JMX_PROMETHEUS_AGENT_VERSION.jar
COPY --chown=kafka:kafka jmx_exporter_config.yaml $KAFKA_ETC/jmx_exporter_config.yaml

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["start"]
