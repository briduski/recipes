

# Zookeeper 

- zookeeper-server-start.sh
**./bin/zookeeper-server-start.sh ./config/zookeeper.properties**
- zookeeper-server-stop.sh
**./bin/zookeeper-server-stop.sh ./config/zookeeper.properties**

- zookeeper-shell.sh
**zookeeper-shell zk1:2181**

[zk: zk1:2181(CONNECTED) 6] ls /brokers/topics
[cd488537-d2ae-4ea5-a989-334e7caf76f4-topic-events-v1-storage-changelog, interactive-subscription-topic-events-v1-storage-changelog, topic-3x3, test-compacto, topic-json, topic-2x2x2, hello-producer-2, 0c5c8248-c25e-454f-a482-082cae0eb62d-topic-events-v1-storage-changelog, hello-producer-1, topic-1, hello-producer-topic, 7d9ab5e8-56b1-4207-a811-e9b45769a57d-topic-events-v1-storage-repartition, 7d9ab5e8-56b1-4207-a811-e9b45769a57d-topic-events-v1-storage-changelog, TestLog, topic-2x2, nse-eod-topic, topic-events-v2, interactive-subscription-topic-events-v1-storage-repartition, test, topic-events-v1, _schemas, f54dd0d1-1852-4496-8b9b-bdc3108e3592-topic-events-v1-storage-changelog, 087ece1f-a79d-4d22-af89-647bc038ece5-topic-events-v1-storage-changelog, toto-3x2, 538a70b1-8284-4d3e-bcea-767f1668a439-topic-events-v1-storage-changelog, __transaction_state, 0c5c8248-c25e-454f-a482-082cae0eb62d-topic-events-v1-storage-repartition, 087ece1f-a79d-4d22-af89-647bc038ece5-topic-events-v1-storage-repartition, 538a70b1-8284-4d3e-bcea-767f1668a439-topic-events-v1-storage-repartition, f54dd0d1-1852-4496-8b9b-bdc3108e3592-topic-events-v1-storage-repartition, test-rar, __consumer_offsets, cd488537-d2ae-4ea5-a989-334e7caf76f4-topic-events-v1-storage-repartition]
[zk: zk1:2181(CONNECTED) 7] get /brokers/topics
null
cZxid = 0x10000000c
ctime = Fri Jan 24 13:33:54 UTC 2020
mZxid = 0x10000000c
mtime = Fri Jan 24 13:33:54 UTC 2020
pZxid = 0x12000002a8
cversion = 35
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 33
[zk: zk1:2181(CONNECTED) 8]


# Kafka 
kafka-server-start.sh
**./bin/kafka-server-start.sh ./config/server.properties**
kafka-server-stop.sh
**./bin/kafka-server-stop.sh**



https://gist.github.com/ursuad/e5b8542024a15e4db601f34906b30bb5

Others:
zookeeper-security-migration.sh
connect-distributed.sh
connect-mirror-maker.sh
connect-standalone.sh