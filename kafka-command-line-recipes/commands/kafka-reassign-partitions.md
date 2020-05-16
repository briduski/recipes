# Reassigning partitions 
Reassigning partitions is done when you want to scale up/down (aka decommission) brokers, or when you simple want to move selectively replicas to specific set of brokers
It can also be used to increase the replication factor  
(When creting the file reassignment.json, you can add new replicas to the proposed assignment  
 That is if data is partitioned by hash(key) % number_of_partitions then this partitioning will potentially be shuffled by adding partitions  
 but Kafka will not attempt to automatically redistribute data in any way.)


## Example
Topic with 2 partitions and 2 replicas. Cluster with 3 brokers. Ids = (1,2,3). I want to move all the partitions allocated by broker id=3

#### Start docker for kafka cluster: 3 brokers, 3 zk, 1 sr (kafka-doc/docker-compose)
**docker-compose up -d.**

### Move to the kafka1 container
**docker-compose exec --privileged kafka1 bash**

### Create topic 2x2
**kafka-topics --create --bootstrap-server kafka1:9092 --replication-factor 2  --partitions 2 --topic topic-2x2**

### See topic list
a)
**kafka-topics  --zookeeper zk1:2181 --describe --topic topic-2x2**

Topic:topic-2x2	PartitionCount:2	ReplicationFactor:2	Configs:            <p/>
	Topic: topic-2x2	Partition: 0	Leader: 2	Replicas: 2,3	Isr: 2,3<p/>
	Topic: topic-2x2	Partition: 1	Leader: 3	Replicas: 3,1	Isr: 3,1<p/>

*** only for the topic I want to move topic-2x2

b)
**kafka-topics --describe --zookeeper zk1:2181 | egrep "Leader: 3|Replicas: 3"**

filtering topics where the leader is the broker id = 3, in case you want to do for all the topics

### Create a file to put the topics wanted to reassign partitions

 **touch topics.json.** <p/>
**apt-get update                    >>>      create file** <p/>
**apt-get -y install vim            >>>      install vim** <p/>
**vim topics.json                   >>>      edit file** <p/>

paste this     <p/>
```
{ "version": 1,
  "topics": [
     {"topic": "topic-2x2"}
  ]
}  
```

**ESC, :wq	       				  >>>      save & exit** <p/>

### Run the command kafka-reassign-partitions with --generate for getting a suggestion of reassignment, without the broker you want avoid (or delete after)
**kafka-reassign-partitions --zookeeper zk1:2181 --generate --topics-to-move-json-file topics.json --broker-list 1,2**

```
{"version":1,"partitions":[{"topic":"topic-2x2","partition":1,"replicas":[3,1],"log_dirs":["any","any"]},{"topic":"topic-2x2","partition":0,"replicas":[2,3],"log_dirs":["any","any"]}]}
```

Proposed partition reassignment configuration
```
{"version":1,"partitions":[{"topic":"topic-2x2","partition":1,"replicas":[2,1],"log_dirs":["any","any"]},{"topic":"topic-2x2","partition":0,"replicas":[1,2],"log_dirs":["any","any"]}]}
```

### Create a file to put the Proposed partition reassignment configuration
**touch reassignment.json** <p/>
**vim reassignment.json** <p/>  
    			           	  >>>  paste the proposed configuration
```
{
   "version":1,
   "partitions":[ 
      { 
         "topic":"topic-2x2",
         "partition":1,
         "replicas":[ 
            2,
            1
         ],
         "log_dirs":[ 
            "any",
            "any"
         ]
      },
      { 
         "topic":"topic-2x2",
         "partition":0,
         "replicas":[ 
            1,
            2
         ],
         "log_dirs":[ 
            "any",
            "any"
         ]
      }
   ]
}
```

**ESC, :wq	       				  >>>      save & exit** <p/>

### Run the command kafka-reassign-partitions with --execute  => execute the assignment
**kafka-reassign-partitions --zookeeper zk1:2181 --execute --reassignment-json-file reassignment.json** <p/>

Current partition replica assignment
```
{"version":1,"partitions":[{"topic":"topic-2x2","partition":1,"replicas":[3,1],"log_dirs":["any","any"]},{"topic":"topic-2x2","partition":0,"replicas":[2,3],"log_dirs":["any","any"]}]}
```
Save this to use as the --reassignment-json-file option during rollback

### Run the command kafka-reassign-partitions with --verify  => Verify the assigment
**kafka-reassign-partitions --zookeeper zk1:2181 --verify --reassignment-json-file reassignment.json** <p/>

Status of partition reassignment: 
Reassignment of partition topic-2x2-1 completed successfully
Reassignment of partition topic-2x2-0 completed successfully

### Describe the topic(s) to make a double check of the partition allocation
**kafka-topics  --zookeeper zk1:2181 --describe --topic topic-2x2** <p/>
Topic:topic-2x2	PartitionCount:2	ReplicationFactor:2	Configs:
	Topic: topic-2x2	Partition: 0	Leader: 2	Replicas: 1,2	Isr: 2,1
	Topic: topic-2x2	Partition: 1	Leader: 2	Replicas: 2,1	Isr: 1,2
	
### Extra
now, just for fun, I want to expand the topic to 3 brokers, supossing I want to scale up the cluster with three nodes <p/>
**kafka-reassign-partitions --zookeeper zk1:2181 --generate --topics-to-move-json-file topics.json --broker-list 1,2,3**
<p/> got a new proposed assignment including the broker id =3
```
Current partition replica assignment
{"version":1,"partitions":[{"topic":"topic-2x2x2","partition":1,"replicas":[2,1],"log_dirs":["any","any"]},{"topic":"topic-2x2x2","partition":0,"replicas":[1,2],"log_dirs":["any","any"]}]}
Proposed partition reassignment configuration
{"version":1,"partitions":[{"topic":"topic-2x2x2","partition":1,"replicas":[2,3],"log_dirs":["any","any"]},{"topic":"topic-2x2x2","partition":0,"replicas":[1,2],"log_dirs":["any","any"]}]}
```
**touch reassignment-up.json**
<p/> paste the proposed assignment

**kafka-reassign-partitions --zookeeper zk1:2181 --execute --reassignment-json-file reassignment-up.json** <p/>
```
Current partition replica assignment

{"version":1,"partitions":[{"topic":"topic-2x2x2","partition":1,"replicas":[2,1],"log_dirs":["any","any"]},{"topic":"topic-2x2x2","partition":0,"replicas":[1,2],"log_dirs":["any","any"]}]}

Save this to use as the --reassignment-json-file option during rollback
```
**kafka-reassign-partitions --zookeeper zk1:2181 --verify --reassignment-json-file reassignment.json**

**kafka-topics  --zookeeper zk1:2181 --describe --topic topic-2x2** <p/>

#### Broker shutdown

Settings related:
 - controlled.shutdown.enable - controlled.shutdown.max.retries - controlled.shutdown.retry.backoff.ms
controlled.shutdown.enable: This setting, if set to true, makes sure that when a shutdown is called on the broker, and if it’s the leader of any topic, then it will gracefully move all leaders to a different broker before it shuts down. This increases the availability of the system, overall.
Controlled shutdown:
- Reduce the unavailability
- Data in page cache will be flushed to disk
• How to:
    -Send a SIGTERM to the process, => kill -SIGTERM <pid> or kill -15 <pid>  
    -If the Broker java process is running in the foreground, issue Ctrl-C


##### OTHERS
##### CONFLUENT
https://docs.confluent.io/current/kafka/post-deployment.html#rolling-restart
https://docs.confluent.io/current/kafka/rebalancer/rebalancer.html#decommissioning-brokers
<p/>
The confluent-rebalancer tool automatically generates a rebalance plan for decommissioning brokers using the --remove-broker-ids option. The cluster is balanced in the same execution.
Feature	Auto Data Balancer	`kafka-reassign- partitions`
Easy to decommission brokers	Yes, with --remove -broker-ids option	Possible by assignment

**./bin/confluent-rebalancer execute --zookeeper localhost:2181 --metrics-bootstrap-server localhost:9092 --throttle 100000 --remove-broker-ids 1**

##### From PACT cookbook
1- After you have gracefully shut down the node you are going to decommission, the logs for all the lead partitions on that node are flushed to disk.
<p/>2- After that the transfer of the lead replica for the partitions happens and the node is finally shut down.
<p/>3- First step you are creating a new JSON file in which you specify which partition should be part of which replicas. You remove reference to the decommissioned node from this JSON file.
<p/>4- Execute the command for reassigning partitions so that it will update the partition replication info in the Kafka cluster.
<p/>5- The nodes are reassigned in line with the instructions in your JSON file.


##### IBM

https://ibm.github.io/event-streams/administering/scaling/         => NOTHING ABOUT KAFKA TOOLS
https://www.ibm.com/support/knowledgecenter/SSCVHB_1.2.1/admin/tnpi_decommission_kafka_service.html?view=embed

<p/>1-Identify the list of topics and partitions that require leadership and replicas reassignment

**./kafka-topics.sh --zookeeper <zookeeper_server>:2182 --describe**

Topic partitions that require reassignment are identified with Leader and Replicas values that are equal to the broker ID of the node that is to be decommissioned

<p/>2-Isolate the topics for a specific broker ID that you want to delete by running the following commands:
For example, to decommission, broker ID 1004:
./kafka-topics.sh --zookeeper `hostname`:2182 --describe | egrep "Leader: 1004|Replicas: 1004"

<p/>3- Reassign partition
https://www.ibm.com/developerworks/community/blogs/eaa9489e-3122-440a-93a3-ee5cd204c603/entry/Reassigning_Kafka_topic_partitions?lang=en



