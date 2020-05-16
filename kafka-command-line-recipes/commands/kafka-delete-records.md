
# Example
Insert 100 records, delete 90 records, read 10 records available

1.  seq 100 | kafka-console-producer --broker-list kafka1:9092 --topic mytest-1

2. Create file with offset =>  touch offsetfile.json

paste this :

```{"partitions": [{"topic": "mytest-1", "partition": 0, "offset": 90}], "version":1 }```


3. Delete records:

**kafka-delete-records --bootstrap-server kafka1:9092 --offset-json-file offsetfile.json**

4. Verify you read from offset 90
5. 
kafka-console-consumer --bootstrap-server kafka1:9092 --topic mytest-1 --from-beginning


https://stackoverflow.com/questions/46209666/how-do-i-delete-clean-kafka-queued-messages-without-deleting-topic