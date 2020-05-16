# Example 
kafka-consumer-groups --list --bootstrap-server kafka1:9092 - list consumer groups
kafka-consumer-groups --group groopy--describe --bootstrap-server=kafka1:9092,kafka2:9092,kafka3:9092 - describe consumer group

# List the consumer groups known to Kafka
kafka-consumer-groups --zookeeper zk1:2181 --list (old api)
kafka-consumer-groups --new-consumer --bootstrap-server kafka1:2181 --list (new api)

# View the details of a consumer group
kafka-consumer-groups --zookeeper zk1:2181 --describe --group <group name>

# new consumer group
kafka-consumer-groups --new-consumer --bootstrap-server kafka1:9092 --describe --group flume