## Introduction
This project is to explore Kafka Stream Library. The java based streaming application named ```employee_data_stream_app``` accepts
unbounded employee data (k,v) separated by ``,``. 
The streaming application doubles salary of each message posted to input topic ```employee_data_input```
and posts it to output topic```employee_data_output```.

#### Input topic creation
```
kafka-topics.sh --zookeeper localhost:2181 \
                 --topic employee_data_input   \
                 --create  --partitions 3 --replication-factor 3 
```
#### Output topic creation
```
kafka-topics.sh --zookeeper localhost:2181 \
                 --topic employee_data_output   \
                 --create  --partitions 3 --replication-factor 3 
```
#### Console Producer to pump messages to employee_data_input.
```
kafka-console-producer.sh --bootstrap-server localhost:9092 \
                          --topic employee_data_input \
                          --property parse.key=true \
                          --property key.separator=, 
                          
```

#### Console Consumer to render messages from employee_data_output.
```
kafka-console-consumer.sh --bootstrap-server localhost:9092 \
                          --topic employee_data_output  \
                          --from-beginning \
                          --property print.key=true \
                          --property print.value=true \
                          --key-deserializer "org.apache.kafka.common.serialization.StringDeserializer" \
                          --value-deserializer "org.apache.kafka.common.serialization.LongDeserializer"   
```
