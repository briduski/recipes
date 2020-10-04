
# Kafkacat

- Install on mac
    brew install kafkacat

- Examples
  + Check(offset) a consumer group of a topic
  > kafkacat -b 127.0.0.1:19092 -G rarity topic-streams-words

  + Consumer, since -o offset: beginning, end, or specific offset #15
  > kafkacat -C -b 127.0.0.1:19092 -t topic-streams-words -o beginning
    
  +  Consumer based on timestamp
  > kafkacat -C -b 127.0.0.1:19092 -t topic-streams-words -o s@start_timestamp
    >> kafkacat -C -b 127.0.0.1:19092 -t topic-streams-words -o s@1590000000000
  > kafkacat -C -b 127.0.0.1:19092 -t topic-streams-words -o e@end_timestamp
    >> kafkacat -C -b 127.0.0.1:19092 -t topic-streams-words -o e@1690000000000

  + Consumer, partion #0, last #o (100) messages, exit (e)
  > kafkacat -C -b 127.0.0.1:19092 -t topic-streams-words -p 0 -o -100 -e

  + Consumer, 50 messages
  > kafkacat -C -b 127.0.0.1:19092 -t topic-streams-words -c 50
  
  + Consumer %h => headers, %p => partitions, %o => offset, %s => record.value, %k => key
     > kafkacat -b 127.0.0.1:19092 -C -t topic-events-v1 -f ' %h %p %o %s %k \n’
     > kafkacat -b 127.0.0.1:19092 -C -t topic-events-v1 -f 'Key is %k, and message payload is: %s \n'

  + Consumer, exit after consuming 1 message
    > kafkacat -b 127.0.0.1:19092 -C -t orders -K: -f '\nKey (%K bytes): %k\t\nValue (%S bytes): %s\n\Partition: %p\tOffset: %o\n--\n'  -c 1

  + Consumer with a property file
    > kafkacat -F kafkacat.config -C -t topic-streams-words -f '\nReceived event\nheader: \t %h \npartition:  %p\noffset: \t  %o \nkey: \t %k \n*********\n'

  + Producer
    > kafkacat -b 127.0.0.1:19092 -X enable.idempotence=true -P -t employees

    > kafkacat -b 127.0.0.1:19092 -K: -P -t employees
      key1:val1
      key2:val2

    > kafkacat -b 127.0.0.1:19092 -P -t orders -K:   -l /data/orders.txt

    > kafkacat -P -b 127.0.0.1:19092 -t topic1 -H header1=valHeader1 -H header2=valHeader2

    > kafkacat -b 127.0.0.1:19092 -t orders -K: -P <<EOF

    >  

4:{"order_id":4,"order_ts":1534772801276,"total_amount":11.50,"customer_name":"Alina Smith"}
5:{"order_id":5,"order_ts":1534772905276,"total_amount":13.32,"customer_name":"Alex Black"}
6:{"order_id":6,"order_ts":1534773042276,"total_amount":31.00,"customer_name":"Emma Watson"}
EOF

  + Query mode (kafkacat -Q -b broker -t <topic>:<partition>:<timestamp>)
  > kafkacat -Q -b 127.0.0.1:19092 -t orders:0:-1
  > kafkacat -q -b 127.0.0.1:19092 -t orders -p 0 -o 5
  > kafkacat -q -b 127.0.0.1:19092 -t topic-streams-words -p 0 -o 5
  https://www.oxymorus.com/

  + Metadata
    > kafkacat -L -b 127.0.0.1:19092 -t topic-streams-words 

   + Serdes: i:integer, s:string
    > kafkacat -C -b localhost:9092 -t topic1 -s key=i -s value=s 
   + Avro
   > kafkacat -C -b localhost:9092 -t avro-topic -s key=s -s value=avro -r http://localhost:8081

  > kafkacat -b 127.0.0.1:19092 -t topic-streams-words -s value=avro -r http://schema-registry-url:8080 | jq .payload.age

  > kafkacat -b 127.0.0.1:19092 -t topic-streams-words  -s value=avro -r http://127.0.0.1:8081 | jq .errorInformation.transactionId

  > kafkacat -b 127.0.0.1:19092 -t topic-streams-words  -s value=avro -r http://127.0.0.1:8081 | jq .errorInformation.transactionId | xargs -I {} echo "transactionId "{}



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
     docker run -it --network=host edenhill/kafkacat:1.5.0  -b 127.0.0.1:19092 -G rarity topic-streams-words topic-streams-wordsx2
