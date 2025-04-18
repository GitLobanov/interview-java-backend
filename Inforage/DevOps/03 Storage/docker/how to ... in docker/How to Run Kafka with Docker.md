# Docker-compose

```bash
version: '3.6'

networks:
  concifora_default:
    external: true 

volumes:
  zookeeper-data:
    driver: local
  zookeeper-log:
    driver: local
  kafka-data:
    driver: local

services:
  akhq:
    image: tchiotludo/akhq
    container_name: akhq-kafka-ui
    restart: unless-stopped
    environment:
      AKHQ_CONFIGURATION: |
        akhq:
          connections:
            docker-kafka-server:
              properties:
                bootstrap.servers: "kafka:9092"
              schema-registry:
                url: "http://schema-registry:8085"
              connect:
                - name: "connect"
                  url: "http://connect:8083"
    ports:
      - 22008:8080
    links:
      - kafka
      - schema-registry
    networks:
      - concifora_default  

  zookeeper:
    image: confluentinc/cp-zookeeper:7.5.0
    restart: unless-stopped
    container_name: zookeeper
    ports:
      - 2181:2181
    volumes:
      - zookeeper-data:/var/lib/zookeeper/data:Z
      - zookeeper-log:/var/lib/zookeeper/log:Z
    environment:
      ZOOKEEPER_CLIENT_PORT: '2181'
      ZOOKEEPER_ADMIN_ENABLE_SERVER: 'false'
    networks:
      - concifora_default   

  kafka:
    image: confluentinc/cp-kafka:7.5.0
    restart: unless-stopped
    container_name: kafka-master
    volumes:
      - kafka-data:/var/lib/kafka/data:Z
    environment:
      KAFKA_BROKER_ID: '0'
      KAFKA_ZOOKEEPER_CONNECT: 'zookeeper:2181'
      KAFKA_NUM_PARTITIONS: '12'
      KAFKA_COMPRESSION_TYPE: 'gzip'
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: '1'
      KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR: '1'
      KAFKA_TRANSACTION_STATE_LOG_MIN_ISR: '1'
      KAFKA_ADVERTISED_LISTENERS: 'PLAINTEXT://kafka:9092,PLAINTEXT_HOST://localhost:29092,PLAINTEXT_EXT://localhost:21041'
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: 'PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT,PLAINTEXT_EXT:PLAINTEXT'
      KAFKA_CONFLUENT_SUPPORT_METRICS_ENABLE: 'false'
      KAFKA_JMX_PORT: '9091'
      KAFKA_AUTO_CREATE_TOPICS_ENABLE: 'true'
      KAFKA_AUTHORIZER_CLASS_NAME: 'kafka.security.authorizer.AclAuthorizer'
      KAFKA_ALLOW_EVERYONE_IF_NO_ACL_FOUND: 'true'
    ports:
      - 21041:21041
    links:
      - zookeeper
    networks:
      - concifora_default   

  schema-registry:
    image: confluentinc/cp-schema-registry:7.5.0
    restart: unless-stopped
    container_name: schema-registry
    depends_on:
      - kafka
    ports:
     - 21042:8081
    environment:
      SCHEMA_REGISTRY_KAFKASTORE_BOOTSTRAP_SERVERS: 'PLAINTEXT://kafka:9092'
      SCHEMA_REGISTRY_HOST_NAME: 'schema-registry'
      SCHEMA_REGISTRY_LISTENERS: 'http://0.0.0.0:8085'
      SCHEMA_REGISTRY_LOG4J_ROOT_LOGLEVEL: 'INFO'
    networks:
      - concifora_default   

  connect:
    image: confluentinc/cp-kafka-connect:7.5.0
    restart: unless-stopped
    container_name: kafka-connect
    depends_on:
      - kafka
      - schema-registry
    ports:
      - 21043:8083
    environment:
      CONNECT_BOOTSTRAP_SERVERS: 'kafka:9092'
      CONNECT_REST_PORT: '8083'
      CONNECT_REST_LISTENERS: 'http://0.0.0.0:8083'
      CONNECT_REST_ADVERTISED_HOST_NAME: 'connect'
      CONNECT_CONFIG_STORAGE_TOPIC: '__connect-config'
      CONNECT_OFFSET_STORAGE_TOPIC: '__connect-offsets'
      CONNECT_STATUS_STORAGE_TOPIC: '__connect-status'
      CONNECT_GROUP_ID: 'kafka-connect'
      CONNECT_KEY_CONVERTER_SCHEMAS_ENABLE: 'true'
      CONNECT_KEY_CONVERTER: 'io.confluent.connect.avro.AvroConverter'
      CONNECT_KEY_CONVERTER_SCHEMA_REGISTRY_URL: 'http://schema-registry:8085'
      CONNECT_VALUE_CONVERTER_SCHEMAS_ENABLE: 'true'
      CONNECT_VALUE_CONVERTER: 'io.confluent.connect.avro.AvroConverter'
      CONNECT_VALUE_CONVERTER_SCHEMA_REGISTRY_URL: 'http://schema-registry:8085'
      CONNECT_INTERNAL_KEY_CONVERTER: 'org.apache.kafka.connect.json.JsonConverter'
      CONNECT_INTERNAL_VALUE_CONVERTER: 'org.apache.kafka.connect.json.JsonConverter'
      CONNECT_OFFSET_STORAGE_REPLICATION_FACTOR: '1'
      CONNECT_CONFIG_STORAGE_REPLICATION_FACTOR: '1'
      CONNECT_STATUS_STORAGE_REPLICATION_FACTOR: '1'
      CONNECT_PLUGIN_PATH: ' /usr/share/java/'
    networks:
      - concifora_default   

  kafka-exporter:
    image: danielqsj/kafka-exporter:latest
    # cpus: 0.4
    # mem_limit: 300m
    container_name: kafka-exporter
    command:
      - --kafka.server=localhost:21041
    ports:
      - 21044:9308
	networks:
      - concifora_default 

  ksqldb:
    image: confluentinc/cp-ksqldb-server:7.5.0
    restart: unless-stopped
    container_name: ksqldb
    depends_on:
      - kafka
      - connect
      - schema-registry
    ports:
      - 21004:8088
    environment:
      KSQL_BOOTSTRAP_SERVERS: 'kafka:9092'
      KSQL_LISTENERS: 'http://0.0.0.0:8088'
      KSQL_KSQL_SERVICE_ID: 'ksql'
      KSQL_KSQL_SCHEMA_REGISTRY_URL: 'http://schema-registry:8085'
      KSQL_KSQL_CONNECT_URL: 'http://connect:8083'
      KSQL_KSQL_SINK_PARTITIONS: '1'
      KSQL_KSQL_LOGGING_PROCESSING_TOPIC_REPLICATION_FACTOR: '1'
    networks:
      - concifora_default   
```

# Resources

- [Взаимодействие ksqlDB на Docker: примеры работы с CLI и REST API](https://bigdataschool.ru/blog/news/kafka/ksqldb-with-kafka-on-docker.html)
- [Доступ к Kafka на Docker извне: тунелирование портов](https://bigdataschool.ru/blog/news/kafka/publishing-from-external-client-to-kafka-on-docker.html)
- [Monitoring Kafka](https://danielmrosa.medium.com/monitoring-kafka-b97d2d5a5434)
- [Docker Compose example for Kafka, Zookeeper, and Schema Registry](https://jskim1991.medium.com/docker-docker-compose-example-for-kafka-zookeeper-and-schema-registry-c516422532e7)
- [Одна Kafka хорошо, а несколько — лучше](https://habr.com/ru/companies/X5Tech/articles/539396/)
- [How To Deploy Multiple Kafka Brokers With Docker Compose?](https://codersee.com/how-to-deploy-multiple-kafka-brokers-with-docker-compose/)
- [Running a Multi-Broker Kafka Cluster on Docker](https://ruan.dev/blog/2023/05/17/running-a-multi-broker-kafka-cluster-on-docker)
- [Kafka Single and Multi-Node Clusters using Docker Compose](https://howtodoinjava.com/kafka/kafka-cluster-setup-using-docker-compose/)
- [Kafka-workshop](https://github.com/gongled/kafka-workshop/blob/master/docker-compose.yml)
- 