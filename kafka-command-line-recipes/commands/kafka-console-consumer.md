## Examples
- kafka-console-consumer --zookeeper localhost:2181 --topic test-topic --from-beginning
- kafka-console-consumer --new-consumer --bootstrap-server kafka1:9092 --topic mytopic --from-beginning
- kafka-console-consumer --group rarita --bootstrap-server kafka1:9092 --topic topic-2x2 --from-beginning
- kafka-console-consumer --bootstrap-server kafka1:9092 --from-beginning --whitelist "hello-producer-1|hello-producer2"

### Read from topics key & value, passing deserializers
kafka-console-consumer.sh --bootstrap-server kafka1:9092 --topic streams-wordcount-output --from-beginning
 --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property print.value=true
 --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
 --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer

### Read from __consumer_offsets topixc
- Add the following property to consumer.properties: exclude.internal.topics=false
- **kafka-console-consumer --consumer.config consumer.properties --from-beginning --topic __consumer_offsets --bootstrap-server kafka1:9092**

### Read from __consumer_offsets topic   ok
kafka-console-consumer --consumer.config consumer.properties --formatter "kafka.coordinator.group.GroupMetadataManager\$OffsetsMessageFormatter" --bootstrap-server kafka1:9092 --topic __consumer_offsets --from-beginning
````
[rarita,topic-2x2,0]::OffsetAndMetadata(offset=2, leaderEpoch=Optional.empty, metadata=, commitTimestamp=1581635048029, expireTimestamp=None)
[rarita,topic-2x2,1]::OffsetAndMetadata(offset=1, leaderEpoch=Optional.empty, metadata=, commitTimestamp=1581635048029, expireTimestamp=None)
[rarita,topic-2x2,2]::OffsetAndMetadata(offset=2, leaderEpoch=Optional.empty, metadata=, commitTimestamp=1581635048029, expireTimestamp=None)
```
