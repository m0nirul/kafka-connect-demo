
# Overview
This is a quick demo of how to work on the confluent platform
> Note: This is not an official repo. Just tried to get the things working from confluent opensource repo


The codes used are all Apache 2.0 licence

| Description | Source | Licence |
| ------ | ------ | ------------ |
| cp-zookeeper | [docker hub](https://hub.docker.com/r/confluentinc/cp-zookeeper) | Apache 2 |
| cp-kafka | [docker hub](https://hub.docker.com/r/confluentinc/cp-kafka) | Apache 2 |
| cp-kafka-rest | [docker hub](https://hub.docker.com/r/confluentinc/cp-kafka-rest) | Apache 2 |
| cp-schema-registry | [docker hub](https://hub.docker.com/r/confluentinc/cp-schema-registry) | Apache 2 |
| cp-kafka-connect | [docker hub](https://hub.docker.com/r/confluentinc/cp-kafka-connect) | Apache 2 |
| Connector Jars | [Github/Landoop](https://github.com/Landoop/stream-reactor/releases) | Apache 2 |
| Confluent connect datagen | [Github/confluentinc](https://github.com/confluentinc/kafka-connect-datagen) | Apache 2 |

For an example of how to use this Docker setup, refer to the Confluent Platform quickstart: https://docs.confluent.io/current/quickstart/index.html

## Steps
### 1. Install necessary packages
```
docker pull confluentinc/cp-zookeeper
docker pull confluentinc/cp-kafka
docker pull confluentinc/cp-schema-registry
docker pull confluentinc/cp-kafka-rest
```

### 2. Download extra jars and put it in ./quickstart/jars/*


### 3. Build the connect Image
```
docker build --rm -t cp-kafka-connect-new .
```
### 4. Deploy the stack on the swarm
```
docker stack deploy -c docker-composev2.yml kafka
```
### 5. Access the services
The services wil l be deployed as per the details in ``docker-composev2.yml``. As per default configuration this are some servcice urls

* Kafka - host:9092
* zookeeper - host:2181
* kafka-connect - host:8083
* kafka-rest - host:8082

### 6. Create a sink/source
To use the created service and create connectors make a post request to ``http://host:8083/connectors`` and the body will be name and the config details. Please check the respective websites for the config details. For cassandra it will be 
```json
{
  "name" : "cassandraSinkConnector1",
  "config" : {
    "connector.class" : "com.datamountaineer.streamreactor.connect.cassandra.sink.CassandraSinkConnector",
    "tasks.max" : "1",
    "topics" : "swipesRecord",
    "connect.cassandra.kcql" : "INSERT INTO swipes SELECT * FROM swipesRecord;",
    "connect.cassandra.contact.points":"cassandrahost",
    "connect.cassandra.port":9042,
    "connect.cassandra.key.space":"swipes"
  }
}
```


## List of available connector lists

| class|name|version|encodedVersion|type|typeName|location|
| ------ | ------ | ------- | ------ | ------ | ------ | ------ |
| com.datamountaineer.streamreactor.connect.cassandra.sink.CassandraSinkConnector|com.datamountaineer.streamreactor.connect.cassandra.sink.CassandraSinkConnector|1.2.2|1.2.2|sink|sink|file:/var/jars/kafka-connect-cassandra-1.2.2/|
|com.datamountaineer.streamreactor.connect.cassandra.source.CassandraSourceConnector|com.datamountaineer.streamreactor.connect.cassandra.source.CassandraSourceConnector|1.2.2|1.2.2|source|source|file:/var/jars/kafka-connect-cassandra-1.2.2/|
|io.confluent.connect.activemq.ActiveMQSourceConnector|io.confluent.connect.activemq.ActiveMQSourceConnector|5.1.2|5.1.2|source|source|file:/usr/share/java/kafka-connect-activemq/|
|io.confluent.connect.elasticsearch.ElasticsearchSinkConnector|io.confluent.connect.elasticsearch.ElasticsearchSinkConnector|5.1.2|5.1.2|sink|sink|file:/usr/share/java/kafka-connect-elasticsearch/|
|io.confluent.connect.gcs.GcsSinkConnector|io.confluent.connect.gcs.GcsSinkConnector|5.0.1|5.0.1|sink|sink|file:/usr/share/confluent-hub-components/confluentinc-kafka-connect-gcs/|
|io.confluent.connect.hdfs.HdfsSinkConnector|io.confluent.connect.hdfs.HdfsSinkConnector|5.1.2|5.1.2|sink|sink|file:/usr/share/java/kafka-connect-hdfs/|
|io.confluent.connect.hdfs.tools.SchemaSourceConnector|io.confluent.connect.hdfs.tools.SchemaSourceConnector|2.1.1-cp1|2.1.1-cp1|source|source|file:/usr/share/java/kafka-connect-hdfs/|
|io.confluent.connect.ibm.mq.IbmMQSourceConnector|io.confluent.connect.ibm.mq.IbmMQSourceConnector|5.1.2|5.1.2|source|source|file:/usr/share/java/kafka-connect-ibmmq/|
|io.confluent.connect.jdbc.JdbcSinkConnector|io.confluent.connect.jdbc.JdbcSinkConnector|5.1.2|5.1.2|sink|sink|file:/usr/share/java/kafka-connect-jdbc/|
|io.confluent.connect.jdbc.JdbcSourceConnector|io.confluent.connect.jdbc.JdbcSourceConnector|5.1.2|5.1.2|source|source|file:/usr/share/java/kafka-connect-jdbc/|
|io.confluent.connect.jms.JmsSourceConnector|io.confluent.connect.jms.JmsSourceConnector|5.1.2|5.1.2|source|source|file:/usr/share/java/kafka-connect-jms/|
|io.confluent.connect.s3.S3SinkConnector|io.confluent.connect.s3.S3SinkConnector|5.1.2|5.1.2|sink|sink|file:/usr/share/java/kafka-connect-s3/|
|io.confluent.connect.storage.tools.SchemaSourceConnector|io.confluent.connect.storage.tools.SchemaSourceConnector|2.1.1-cp1|2.1.1-cp1|source|source|file:/usr/share/java/kafka-connect-hdfs/|
|io.confluent.kafka.connect.datagen.DatagenConnector|io.confluent.kafka.connect.datagen.DatagenConnector|null|null|source|source|file:/usr/share/confluent-hub-components/confluentinc-kafka-connect-datagen/|
|org.apache.kafka.connect.file.FileStreamSinkConnector|org.apache.kafka.connect.file.FileStreamSinkConnector|2.1.1-cp1|2.1.1-cp1|sink|sink|file:/usr/share/java/kafka/|
|org.apache.kafka.connect.file.FileStreamSourceConnector|org.apache.kafka.connect.file.FileStreamSourceConnector|2.1.1-cp1|2.1.1-cp1|source|source|file:/usr/share/java/kafka/|
|org.apache.kafka.connect.tools.MockConnector|org.apache.kafka.connect.tools.MockConnector|2.1.1-cp1|2.1.1-cp1|connector|connector|file:/usr/share/java/confluent-control-center/|
|org.apache.kafka.connect.tools.MockSinkConnector|org.apache.kafka.connect.tools.MockSinkConnector|2.1.1-cp1|2.1.1-cp1|sink|sink|file:/usr/share/java/confluent-control-center/|
|org.apache.kafka.connect.tools.MockSourceConnector|org.apache.kafka.connect.tools.MockSourceConnector|2.1.1-cp1|2.1.1-cp1|source|source|file:/usr/share/java/confluent-control-center/|
|org.apache.kafka.connect.tools.SchemaSourceConnector|org.apache.kafka.connect.tools.SchemaSourceConnector|2.1.1-cp1|2.1.1-cp1|source|source|file:/usr/share/java/confluent-control-center/|
|org.apache.kafka.connect.tools.VerifiableSinkConnector|org.apache.kafka.connect.tools.VerifiableSinkConnector|2.1.1-cp1|2.1.1-cp1|source|source|file:/usr/share/java/confluent-control-center/|
|org.apache.kafka.connect.tools.VerifiableSourceConnector|org.apache.kafka.connect.tools.VerifiableSourceConnector|2.1.1-cp1|2.1.1-cp1|source|source|file:/usr/share/java/confluent-control-center/|
