#### Version Apache Kafka 2.5

## ExportZkOffsets.scala 
 A utility that retrieves the offsets of broker partitions in ZK and prints to an output file in the following format


**kafka-run-class kafka.tools.ExportZkOffsets --zkconnect zk1:2181 --group test-consumer-group --output-file /tmp/out.txt**
  ```
$ cat /tmp/out.txt
/consumers/test-consumer-group/offsets/mytesttopic/3:0
/consumers/testconsumer-group/offsets/mytesttopic/2:6
/consumers/test-consumergroup/offsets/mytesttopic/1:0
/consumers/test-consumergroup/offsets/mytesttopic/0:0
``` 

## ImportZkOffsets.scala  
 It can import offsets for a topic partitions


**kafka-run-class kafka.tools.ImportZkOffsets --inputfile /tmp/zkoffset.txt --zkconnect zk1:2181**

## ConsoleConsumer.scala

**kafka-run-class kafka.tools.ConsoleConsumer --bootstrap-server kafka1:9092 --topic topic-2x2 --from-beginning**  
  
## ConsoleProducer.scala

**kafka-run-class kafka.tools.ConsoleProducer --broker-list kafka1:9092 --topic topic-2x2**

## DumpLogSegments.scala 
 For debug purposes, status of the segments.  Change your current directory to the Kafka folder.

**kafka-run-class kafka.tools.DumpLogSegments --deep-iteration -files /tmp/kafka-logs/my-replicated-topic-2/00000000000000000000.log**

## GetOffsetShell.scala

 - GetOffsetShell.scala => get offsets for a topic
    - Get number of messages in a topic per partition
       **kafka-run-class kafka.tools.GetOffsetShell --broker-list kaf
       **kafka-run-class kafka.tools.GetOffsetShell --broker-list kafka1:9092 --topic topic-2x2 --time -1 --offsets 1 | awk -F ":" '{sum += $3} END {print sum}'ka1:9092 --topic topic-2x2**
    - Get number of messages in a topic
       **kafka-run-class kafka.tools.GetOffsetShell --broker-list kafka1:9092 --topic topic-2x2 --time -1 --offsets 1 | awk -F ":" '{sum += $3} END {print sum}'**
        9
    - Get the earliest offset still in a topic
    
         **kafka-run-class kafka.tools.GetOffsetShell --broker-list kafka1:9092 --topic topic-2x2 --time -2**
       ```
       topic-2x2:0:0
          topic-2x2:1:0
          topic-2x2:2:0
          ```
    - Get the latest offset still in a topic
       **kafka-run-class kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic mytopic --time -1**
     ```topic-2x2:0:2
        topic-2x2:1:2
        topic-2x2:2:2 
    ```
     - Get the number of records of a topic   
        
```
     newest= kafka-run-class kafka.tools.GetOffsetShell --broker-list kafka1:9092 --topic topic-2x2 --time -1 --offsets 1 | awk -F ":" '{sum += $3} END {print sum}'
     oldest= kafka-run-class kafka.tools.GetOffsetShell --broker-list kafka1:9092 --topic topic-2x2 --time -2 --offsets 1 | awk -F ":" '{sum += $3} END {print sum}'
     total=`expr $newest - $oldest`
     echo Latest Total $newest
     echo Earliest Total $oldest
     echo InQueue Total $total  
``` 
 
*Doc*
   
   *--time <Long: timestamp/-1(latest)/-2  timestamp of the offsets before that  (earliest)  (default: -1)*
   
     

##  JmxTool.scala  
It prints metrics via JMX
  
**kafka-run-class kafka.tools.JmxTool --jmx-url service:jmx:rmi:///jndi/rmi://127.0.0.1:9999/jmxrmi**


## MirrorMaker.scala
You have two different instances of Kafka up and running and you are ready to replicate data on one to the other.

**kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config config/consumer.config --producer.config config/producer.config -whitelist mytesttopic**

## ConsumerPerformance.scala
	This tool helps in performance test for the full zookeeper consumer
    
```
 kafka-run-class kafka.tools.ConsumerPerformance --broker-list kafka1:9092 --messages 3 --topic topic-2x2
start.time, end.time, data.consumed.in.MB, MB.sec, data.consumed.in.nMsg, nMsg.sec, rebalance.time.ms, fetch.time.ms, fetch.MB.sec, fetch.nMsg.sec
2020-02-14 12:01:16:275, 2020-02-14 12:01:19:737, 0.0000, 0.0000, 6, 1.7331, 3041, 421, 0.0001, 14.2518`
```


## EndToEndLatency.scala

  This class records the average end to end latency for a single message to travel through Kafka e.g. [localhost:9092 test 10000 1 20]
  **kafka-run-class kafka.tools.EndToEndLatency kafka1:9092 topic-2x2 100 1 4**
```
    val brokerList = args(0), val topic = args(1),  val numMessages = args(2).toInt, val producerAcks = args(3) [1, all], val messageLen = args(4).toInt (randomBytesOfLen)
    val propsFile = if (args.length > 5) Some(args(5)).filter(_.nonEmpty) else None
```

## ReplicaVerificationTool.scala 

**kafka-run-class kafka.tools.ReplicaVerificationTool --broker-list kafka1:9092 --topic-white-list topic-2x2**
```
2020-02-13 23:48:34,633: verification process is started.
2020-02-13 23:49:04,819: max lag is 0 for partition topic-2x2-0 at offset 2 among 3 partitions
2020-02-13 23:49:34,932: max lag is 0 for partition topic-2x2-0 at offset 2 among 3 partitions
2020-02-13 23:50:05,058: max lag is 0 for partition topic-2x2-0 at offset 2 among 3 partitions
```

## StateChangeLogMerger.scala  
 A utility that merges the state change logs (possibly obtained from different brokers and over multiple days).
 <p/>

**kafka-run-class kafka.tools.StateChangeLogMerger --log-regex /tmp/state-change.log* --partitions 0,1,2 --topic statelog**


## StreamsResetter

StreamsResetter.java
this tool helps to quickly reset an application in order to reprocess its data from scratch.
* This tool resets offsets of input topics to the earliest available offset and it skips to the end of intermediate topics (topics used in the through() method).
* This tool deletes the internal topics that were created by Kafka Streams (topics starting with "<application.id>-").
You do not need to specify internal topics because the tool finds them automatically.
* This tool will not delete output topics (if you want to delete them, you need to do it yourself with the bin/kafka-topics.sh command).
* This tool will not clean up the local state on the stream application instances (the persisted stores used to cache aggregation results).
You need to call KafkaStreams#cleanUp() in your application or manually delete them from the directory specified by "state.dir" configuration (/tmp/kafka-streams/<application.id> by default).

***Important! You will get wrong output if you don't clean up the local stores after running the reset tool!**



**kafka-run-class kafka.tools.StreamsResetter --bootstrap-servers kafka1:9092 --application-id rar-1 --input-topics topic-events-v1**
```
Reset-offsets for input topics [topic-events-v1]
Following input topics offsets will be reset to (for consumer group rar-1)
Topic: topic-events-v1 Partition: 4 Offset: 26
Topic: topic-events-v1 Partition: 5 Offset: 2
Topic: topic-events-v1 Partition: 6 Offset: 0
Topic: topic-events-v1 Partition: 7 Offset: 36
Topic: topic-events-v1 Partition: 0 Offset: 76
Topic: topic-events-v1 Partition: 1 Offset: 14
Topic: topic-events-v1 Partition: 2 Offset: 0
Topic: topic-events-v1 Partition: 3 Offset: 24
Done.
Deleting all internal/auto-created topics for application rar-1
Done.
```
 

## ConsumerGroupCommand  (Kafka Admin )
kafka-run-class kafka.admin.ConsumerGroupCommand --bootstrap-server kafka1:9092 --group kafkaGroup --describe
kafka-run-class kafka.admin.ConsumerGroupCommand --bootstrap-server kafka1:9092 --group rarita --describe


https://github.com/apache/kafka/tree/2.5/core/src/main/scala/kafka/admin
https://www.sderosiaux.com/articles/2017/08/07/looking-at-kafka-s-consumers-offsets/


# Others

## PerfConfig.scala 
-kafka-run-class kafka.tools.PerfConfig  ???? 
- ConsumerOffsetChecker.scala   => @deprecated / Use kafka-consumer-groups.sh to get consumer group details.
- KafkaMigrationTool.java  => Migrates a 0.7 broker to 0.8
- SimpleConsumerShell.scala  => removed
- ReplayLogProducer.scala => Consume from one topic and replay those messages and produce to another topic
**kafka-run-class kafka.tools.ReplayLogProducer --broker-list kafka1:9092 --inputtopic topic-2x2 --outputtopic toto-2x2 --zookeeper zk1:2181**
**kafka-run-class kafka.tools.ReplayLogProducer --sync --brokerlist kafka1:9092 --inputtopic topic-2x2 --outputtopic toto-2x2 --zookeeper zk1:2181** => removed
- UpdateOffsetsInZK.scala  => removed
-
- VerifyConsumerRebalance.scala  =>Make sure there is an owner for every partition. A successful rebalancing operation would select an owner for each available partition.
  This means that for each partition registered under /brokers/topics/[topic]/[broker-id], an owner exists under /consumers/[consumer_group]/owners/[topic]/[broker_id-partition_id]
After the rebalancing operation, each partition must have selected an owner.
$ bin/kafka-run-class kafka.tools.VerifyConsumerRebalance -zookeeper.connect localhost:2181 --group mytestconsumer   --removed