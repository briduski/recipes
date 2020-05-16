https://www.allprogrammingtutorials.com/tutorials/enforcing-quotas-in-apache-kafka.php

# Alter configuration
kafka-configs.sh --zookeeper zk1:2181 --entity-type topics --entity-name topic --alter --add-config min.insync.replicas=2


# Quotas
Limiting to a kafka client the rate bytes/second
Kafka attempts to slow down a client exceeding quota by delaying the response. Amount of delay is computed by brokers in a way that client does not violate its quota.
Quotas are enforced at broker level and by default, there is a fixed quota assigned to each unique client.
These default quotas can be changed in Kafka configuration file and require Kafka broker restart.
For example, below configuration parameters can be used to set producer/consumer quota to 50 MB/sec -

## Network quota

**Broker properties**
> quota.producer.default=52428800  => **Sets producer quota to 50 MB**<p/>
> quota.consumer.default=52428800 => **Sets consumer quota to 50 MB**<p/>

**override quota to a specific client**

>kafka-configs  --zookeeper zk1:2181 --alter --add-config 'producer_byte_rate=10485760,consumer_byte_rate=20971520' --entity-name test-client --entity-type clients

**Adds configuration for client with id test-client  with producer quota as 10 MB and consumer quota as 20 MB**

> kafka-configs  --zookeeper zk1:2181 --describe --entity-name test-client --entity-type clients

**verify the change**

**Output of above command will be as belowConfigs for clients:test-client are producer_byte_rate=10485760,consumer_byte_rate=20971520**


## Request rate quota
$ kafka-configs --zookeeper zk1:2181  --alter  --add-config 'request_percentage=50`  --entity-name test-client --entity-type clients --entity-name user1 --entity-type users


Describe quota
kafka-configs --zookeeper zk_host:2181 --describe --entity-name test-client --entity-type clients --entity-name user1 --entity-type users