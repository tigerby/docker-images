# docker-connect
Base Kafka Connect image. These files are based on [Debezium kafka connect](https://github.com/debezium/container-images/tree/main/connect-base) and [Confluent JMX monitoring stack](https://github.com/confluentinc/jmx-monitoring-stacks).

## Monitoring 
* [Confluent JMX exporter](https://github.com/confluentinc/jmx-monitoring-stacks/blob/7.2-post/shared-assets/jmx-exporter/kafka_connect.yml)
* [Confluent Grafana metric](https://github.com/confluentinc/jmx-monitoring-stacks/blob/7.2-post/jmxexporter-prometheus-grafana/assets/grafana/provisioning/dashboards/kafka-connect-cluster.json)

## References
https://github.com/debezium/container-images
https://github.com/confluentinc/jmx-monitoring-stacks
