
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
docker stack deploy -c docker-composev2.yml connect
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
	"connect.cassandra.contact.points":"localhost",
	"connect.cassandra.port":9042,
	"connect.cassandra.key.space":"swipes"
  }
}
```


## List of available connector lists
<table>

<tbody>

<tr>

<td>

<div class="td_head">class</div>

</td>

<td>

<div class="td_head">name</div>

</td>

<td>

<div class="td_head">version</div>

</td>

<td>

<div class="td_head">encodedVersion</div>

</td>

<td>

<div class="td_head">type</div>

</td>

<td>

<div class="td_head">typeName</div>

</td>

<td>

<div class="td_head">location</div>

</td>

</tr>

<tr>

<td class="td_row_even">

<div class="td_row_even">com.datamountaineer.streamreactor.connect.cassandra.sink.CassandraSinkConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">com.datamountaineer.streamreactor.connect.cassandra.sink.CassandraSinkConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">1.2.2</div>

</td>

<td class="td_row_even">

<div class="td_row_even">1.2.2</div>

</td>

<td class="td_row_even">

<div class="td_row_even">sink</div>

</td>

<td class="td_row_even">

<div class="td_row_even">sink</div>

</td>

<td class="td_row_even">

<div class="td_row_even">file:/var/jars/kafka-connect-cassandra-1.2.2/</div>

</td>

</tr>

<tr>

<td class="td_row_odd">

<div class="td_row_odd">com.datamountaineer.streamreactor.connect.cassandra.source.CassandraSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">com.datamountaineer.streamreactor.connect.cassandra.source.CassandraSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">1.2.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">1.2.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">file:/var/jars/kafka-connect-cassandra-1.2.2/</div>

</td>

</tr>

<tr>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.activemq.ActiveMQSourceConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.activemq.ActiveMQSourceConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">5.1.2</div>

</td>

<td class="td_row_even">

<div class="td_row_even">5.1.2</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">file:/usr/share/java/kafka-connect-activemq/</div>

</td>

</tr>

<tr>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.connect.elasticsearch.ElasticsearchSinkConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.connect.elasticsearch.ElasticsearchSinkConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">5.1.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">5.1.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">sink</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">sink</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">file:/usr/share/java/kafka-connect-elasticsearch/</div>

</td>

</tr>

<tr>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.gcs.GcsSinkConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.gcs.GcsSinkConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">5.0.1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">5.0.1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">sink</div>

</td>

<td class="td_row_even">

<div class="td_row_even">sink</div>

</td>

<td class="td_row_even">

<div class="td_row_even">file:/usr/share/confluent-hub-components/confluentinc-kafka-connect-gcs/</div>

</td>

</tr>

<tr>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.connect.hdfs.HdfsSinkConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.connect.hdfs.HdfsSinkConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">5.1.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">5.1.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">sink</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">sink</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">file:/usr/share/java/kafka-connect-hdfs/</div>

</td>

</tr>

<tr>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.hdfs.tools.SchemaSourceConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.hdfs.tools.SchemaSourceConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">file:/usr/share/java/kafka-connect-hdfs/</div>

</td>

</tr>

<tr>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.connect.ibm.mq.IbmMQSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.connect.ibm.mq.IbmMQSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">5.1.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">5.1.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">file:/usr/share/java/kafka-connect-ibmmq/</div>

</td>

</tr>

<tr>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.jdbc.JdbcSinkConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.jdbc.JdbcSinkConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">5.1.2</div>

</td>

<td class="td_row_even">

<div class="td_row_even">5.1.2</div>

</td>

<td class="td_row_even">

<div class="td_row_even">sink</div>

</td>

<td class="td_row_even">

<div class="td_row_even">sink</div>

</td>

<td class="td_row_even">

<div class="td_row_even">file:/usr/share/java/kafka-connect-jdbc/</div>

</td>

</tr>

<tr>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.connect.jdbc.JdbcSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.connect.jdbc.JdbcSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">5.1.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">5.1.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">file:/usr/share/java/kafka-connect-jdbc/</div>

</td>

</tr>

<tr>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.jms.JmsSourceConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.jms.JmsSourceConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">5.1.2</div>

</td>

<td class="td_row_even">

<div class="td_row_even">5.1.2</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">file:/usr/share/java/kafka-connect-jms/</div>

</td>

</tr>

<tr>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.connect.s3.S3SinkConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.connect.s3.S3SinkConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">5.1.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">5.1.2</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">sink</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">sink</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">file:/usr/share/java/kafka-connect-s3/</div>

</td>

</tr>

<tr>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.storage.tools.SchemaSourceConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">io.confluent.connect.storage.tools.SchemaSourceConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">file:/usr/share/java/kafka-connect-hdfs/</div>

</td>

</tr>

<tr>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.kafka.connect.datagen.DatagenConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">io.confluent.kafka.connect.datagen.DatagenConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">null</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">null</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">file:/usr/share/confluent-hub-components/confluentinc-kafka-connect-datagen/</div>

</td>

</tr>

<tr>

<td class="td_row_even">

<div class="td_row_even">org.apache.kafka.connect.file.FileStreamSinkConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">org.apache.kafka.connect.file.FileStreamSinkConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">sink</div>

</td>

<td class="td_row_even">

<div class="td_row_even">sink</div>

</td>

<td class="td_row_even">

<div class="td_row_even">file:/usr/share/java/kafka/</div>

</td>

</tr>

<tr>

<td class="td_row_odd">

<div class="td_row_odd">org.apache.kafka.connect.file.FileStreamSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">org.apache.kafka.connect.file.FileStreamSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">2.1.1-cp1</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">2.1.1-cp1</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">file:/usr/share/java/kafka/</div>

</td>

</tr>

<tr>

<td class="td_row_even">

<div class="td_row_even">org.apache.kafka.connect.tools.MockConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">org.apache.kafka.connect.tools.MockConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">connector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">connector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">file:/usr/share/java/confluent-control-center/</div>

</td>

</tr>

<tr>

<td class="td_row_odd">

<div class="td_row_odd">org.apache.kafka.connect.tools.MockSinkConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">org.apache.kafka.connect.tools.MockSinkConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">2.1.1-cp1</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">2.1.1-cp1</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">sink</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">sink</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">file:/usr/share/java/confluent-control-center/</div>

</td>

</tr>

<tr>

<td class="td_row_even">

<div class="td_row_even">org.apache.kafka.connect.tools.MockSourceConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">org.apache.kafka.connect.tools.MockSourceConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">file:/usr/share/java/confluent-control-center/</div>

</td>

</tr>

<tr>

<td class="td_row_odd">

<div class="td_row_odd">org.apache.kafka.connect.tools.SchemaSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">org.apache.kafka.connect.tools.SchemaSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">2.1.1-cp1</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">2.1.1-cp1</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">file:/usr/share/java/confluent-control-center/</div>

</td>

</tr>

<tr>

<td class="td_row_even">

<div class="td_row_even">org.apache.kafka.connect.tools.VerifiableSinkConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">org.apache.kafka.connect.tools.VerifiableSinkConnector</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">2.1.1-cp1</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">source</div>

</td>

<td class="td_row_even">

<div class="td_row_even">file:/usr/share/java/confluent-control-center/</div>

</td>

</tr>

<tr>

<td class="td_row_odd">

<div class="td_row_odd">org.apache.kafka.connect.tools.VerifiableSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">org.apache.kafka.connect.tools.VerifiableSourceConnector</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">2.1.1-cp1</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">2.1.1-cp1</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">source</div>

</td>

<td class="td_row_odd">

<div class="td_row_odd">file:/usr/share/java/confluent-control-center/</div>

</td>

</tr>

</tbody>

</table>
