# Create a topic
kafka-topics --create --zookeeper zk1:2181           --topic my-topic --replication-factor 2 --partitions 4  
kafka-topics --create --bootstrap-server kafka1:9092 --topic toto-3x2 --replication-factor 2 --partitions 3
kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic streams-wordcount-output --config cleanup.policy=compact

# List existing topics
kafka-topics --list  --zookeeper zk1:2181

# Describe existing topic(s)
kafka-topics  --describe  --topic toto-3x2   --zookeeper zk1:2181

kafka-topics  --describe  --topic toto-3x2   --bootstrap-server kafka1:9092

kafka-topics  --describe  --zookeeper zk1:2181

# Delete a topic:
kafka-topics --zookeeper zk1:2181 --delete --topic my-topic

# Remove a config:
kafka-topics --zookeeper zk1:2181 --alter --topic my-topic --deleteConfig x

# Alter the number of partitions (only increase)
kafka-topics --alter --zookeeper zk1:2181 --topic toto-3x2 --partitions 4

# Purge a topic
*kafka-topics --zookeeper zk1:2181 --alter --topic my-topic --config retention.ms=1000*

after a while ...

*kafka-topics --zookeeper localhost:2181 --alter --topic my-topic --delete-config retention.ms*

**Topic deletion option is disabled by default. To enable it set the server config**

*delete.topic.enable=true*


# Check the partitons a specific broker is leader

kafka-topics --describe --zookeeper zk1:2181 | egrep "Leader: 3|Replicas: 3"



# Retention
## Time based Retention Policy
**Broker properties**
> log.retention.ms=1680000 => Configures retention time in milliseconds <p/>
> log.retention.minutes=1680 => Used if log.retention.ms is not set<p/>
> log.retention.hours=168 => Used if log.retention.minutes is not set<p/>

**overrride to specific topic**
> kafka-topics --zookeeper zk1:2181 --alter --topic my-topic --config retention.ms=1680000

## Size based Retention Policy
**Broker properties**
> log.retention.bytes=104857600 => Configures maximum size of a Log <p/>

**overrride to specific topic(100MB)**
> kafka-topics --zookeeper zk1:2181 --alter --topic my-topic --config retention.bytes=104857600 <p/>

**delete**
> kafka-topics --zookeeper zk1:2181 --alter --topic my-topic  --delete-config retention.bytes

# Flush Interval
While optimal flush intervals will vary from one use case to another, we will talk about different settings provided by Apache Kafka to configure it.
 how to determine correct flush interval?
## Interval based on number of messages
**Broker properties**
>log.flush.interval.messages=10000 => **The number of messages to accept before forcing a flush of data to disk** <p/>

**overrride to specific topic**
> kafka-topics.sh --zookeeper localhost:2181 --alter --topic my-topic --config flush.messages=5000

## Interval based on time period
**Broker properties**
>log.flush.interval.ms=1000 => **The maximum amount of time a message can sit in a log before we force a flush** <p/>

**overrride to specific topic**
> kafka-topics.sh --zookeeper localhost:2181 --alter --topic my-topic --config flush.ms=60000



https://www.allprogrammingtutorials.com/tutorials/configuring-messages-retention-time-in-kafka.php
https://www.allprogrammingtutorials.com/tutorials/configuring-log-flush-interval-in-kafka.php
