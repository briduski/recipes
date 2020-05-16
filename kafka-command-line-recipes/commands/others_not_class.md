# kafka-replica-verification.sh**
Validate that all replicas for a set of topics have the same data.

**kafka-replica-verification --broker-list kafka1:9092 --topic-white-list topic-2x2**
```
2020-02-14 18:35:44,896: verification process is started.
2020-02-14 18:36:15,082: max lag is 0 for partition topic-2x2-0 at offset 1335 among 3 partitions
2020-02-14 18:36:45,197: max lag is 0 for partition topic-2x2-0 at offset 1335 among 3 partitions
```

# kafka-leader-election.sh
this tool attempts to elect a new leader for a set of topic partitions. The type of elections supported are preferred replicas and unclean replicas.
 
**./kafka-leader-election.sh --bootstrap-server 127.0.0.1:19092 --topic  mytest-1 --election-type PREFERRED --partition 0**
```
@Successfully completed leader election (PREFERRED) for partitions mytest-1-0
```

# kafka-verifiable-producer.sh
This tool produces increasing integers to the specified topic and prints JSON metadata to stdout on each "send" request, making externally visible which messages have been acked and which have not.


p1mac078:bin a01422$ ./kafka-verifiable-producer.sh --broker-list 127.0.0.1:19092 --topic mytest-1 --max-messages 3 --acks 1
{"timestamp":1581706325305,"name":"startup_complete"}
{"timestamp":1581706325488,"name":"producer_send_success","key":null,"value":"0","offset":100,"topic":"mytest-1","partition":0}
{"timestamp":1581706325490,"name":"producer_send_success","key":null,"value":"1","offset":101,"topic":"mytest-1","partition":0}
{"timestamp":1581706325490,"name":"producer_send_success","key":null,"value":"2","offset":102,"topic":"mytest-1","partition":0}
{"timestamp":1581706325496,"name":"shutdown_complete"}
{"timestamp":1581706325496,"name":"tool_data","sent":3,"acked":3,"target_throughput":-1,"avg_throughput":15.625}


# kafka-verifiable-consumer.sh   *TODO*
This tool consumes messages from a specific topic and emits consumer events (e.g. group rebalances, received messages, and offsets committed) as JSON objects to STDOUT.


p1mac078:bin a01422$ ./kafka-verifiable-consumer.sh  --broker-list 127.0.0.1:19092 --topic mytest-1 --group-id c1-1 --max-messages 4 --group-instance-id c1-1-v1
{"timestamp":1581706461664,"name":"startup_complete"}
[2020-02-14 19:54:22,014] ERROR Error during processing, terminating consumer process:  (org.apache.kafka.tools.VerifiableConsumer)
org.apache.kafka.common.errors.UnsupportedVersionException: The broker join group protocol version 4 does not support usage of config group.instance.id.
{"timestamp":1581706462020,"name":"shutdown_complete"}


# kafka-log-dirs
**kafka-log-dirs --bootstrap-server kafka1:9092 --describe --topic-list topic-2x2x2**

{"version":1,"brokers":[{"broker":1,"logDirs":[{"logDir":"/var/lib/kafka/data","error":null,"partitions":[{"partition":"topic-2x2x2-0","size":0,"offsetLag":0,"isFuture":false}]}]},{"broker":2,"logDirs":[{"logDir":"/var/lib/kafka/data","error":null,"partitions":[{"partition":"topic-2x2x2-0","size":0,"offsetLag":0,"isFuture":false},{"partition":"topic-2x2x2-1","size":0,"offsetLag":0,"isFuture":false}]}]},{"broker":3,"logDirs":[{"logDir":"/var/lib/kafka/data","error":null,"partitions":[{"partition":"topic-2x2x2-1","size":0,"offsetLag":0,"isFuture":false}]}]}]}



# NOT USED YET
<p/>kafka-log-dirs.sh
<p/>kafka-mirror-maker.sh
<p/>kafka-streams-application-reset.sh
<p/>kafka-acls.sh
<p/>kafka-delegation-tokens.sh
<p/>kafka-broker-api-versions.sh
<p/>kafka-dump-log.sh



