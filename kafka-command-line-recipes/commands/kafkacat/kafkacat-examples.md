
# Kafkacat

- Install on mac
    brew install kafkacat

- Examples
  + Check(offset) a consumer group of a topic
  > kafkacat -b 127.0.0.1:19092 -G rarity topic-2x2

  + Consumer, partion, last #o (100) messages, exit
  > kafkacat -C -b 127.0.0.1:19092 -t topic-2x2 -p 0 -o -100 -e

  + Consumer %h => headers, %p => partitions, %o => offset, %s => record.value, %k => key
     > kafkacat -b 127.0.0.1:19092 -C -t topic-events-v1 -f ' %h %p %o %s %k \n’
     > kafkacat -b localhost:19092 -C -t orders -K: -f '\nKey (%K bytes): %k\t\nValue (%S bytes): %s\n\Partition: %p\tOffset: %o\n--\n'  -c 1

     exit after producing(consuming) 1 message

  + Consumer with a property file
    > kafkacat -F kafkacat.config -C -t nt-events-subscription-v1 -f '\nReceived event\nheader: \t %h \npartition:  %p\noffset: \t  %o \nkey: \t %k \n*********\n'    

  + Producer
    > kafkacat -b 127.0.0.1:19092 -X enable.idempotence=true -P -t employees

    > kafkacat -b 127.0.0.1:19092 -K: -P -t employees
    > kafkacat -b 127.0.0.1:19092 -K: -P -t employees -t orders -P -l /data/orders.txt
    >kafkacat -b localhost:9092 -t orders -K: -P <<EOF
4:{"order_id":4,"order_ts":1534772801276,"total_amount":11.50,"customer_name":"Alina Smith"}
5:{"order_id":5,"order_ts":1534772905276,"total_amount":13.32,"customer_name":"Alex Black"}
6:{"order_id":6,"order_ts":1534773042276,"total_amount":31.00,"customer_name":"Emma Watson"}
EOF

  + Query mode (kafkacat -Q -b broker -t <topic>:<partition>:<timestamp>)
  > kafkacat -Q -b localhost:9092 -t orders:0:-1
  > kafkacat -q -b localhost:9092 -t orders -p 0 -o 5
  https://www.oxymorus.com/

  + Metadata
    > kafkacat -L -b 127.0.0.1:19092 -t topic-2x2

   + Avro
  > kafkacat -b 127.0.0.1:19092 -t nt-events-subscription-v1 -s value=avro -r http://schema-registry-url:8080 | jq .payload.age

  > kafkacat -b 127.0.0.1:19092 -t nt-events-subscription-v1  -s value=avro -r http://localhost:8081 | jq .errorInformation.transactionId

  > kafkacat -b 127.0.0.1:19092 -t nt-events-subscription-v1  -s value=avro -r http://localhost:8081 | jq .errorInformation.transactionId | xargs -I {} echo "transactionId "{}



### Others:

 Confluent

 https://github.com/confluentinc/demo-scene/blob/master/ksql-workshop/docker-compose.yml


    kafkacat:
        image: confluentinc/cp-kafkacat:latest
        container_name: kafkacat
        depends_on:
        - kafka
        entrypoint: sleep infinity

### Kafkacat docker

__https://github.com/edenhill/kafkacat#running-in-docker__

     docker run -it --network=host edenhill/kafkacat:1.5.0 -b 127.0.0.1:19092 -L
     docker run -it --network=host edenhill/kafkacat:1.5.0  -b 127.0.0.1:19092 -G rarity topic-2x2 topic-2x2x2
