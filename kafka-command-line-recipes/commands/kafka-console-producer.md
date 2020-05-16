kafka-console-producer --broker-list localhost:9092 --topic topic-2x2


## Write with a key
kafka-console-producer --broker-list kafka1:9092 --topic employees --property parse.key=true --property key.separator=":"
