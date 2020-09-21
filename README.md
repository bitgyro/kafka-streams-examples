## Introduction
This project is to explore Kafka Stream Library. The java based streaming application named ```employee_data_stream_app``` accepts
unbounded employee data (k,v) separated by ``,``. 
* The streaming application doubles salary of each message posted to input topic ```employee_data_input```
and posts it to output topic```employee_data_output```.
* Employee whose salary remains less than 1000 (post double) will be posted to output topic ```casual_employee_data_output```.
  While posting (k,v) to this topic, "CASUAL" is concatenated to key value.
* Casual Employees whose salary is below 500 will be posted to casual_employee_data_output_with_below_500_salary topic
* Casual Employees whose salary is greater than equal to 500 will be posted to casual_employee_data_output_with_above_500_salary  

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
#### Output topic creation
```
kafka-topics.sh --zookeeper localhost:2181 \
                 --topic casual_employee_data_output   \
                 --create  --partitions 3 --replication-factor 3 
```

#### Output topic creation ( casual employees with even salary)
```
kafka-topics.sh --zookeeper localhost:2181 \
                 --topic casual_employee_data_output_with_above_500_salary   \
                 --create  --partitions 3 --replication-factor 3 
```
#### Output topic creation ( casual employees with salary divisible by 5)
```
kafka-topics.sh --zookeeper localhost:2181 \
                 --topic casual_employee_data_output_with_below_500_salary   \
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
All employees with doubles salary will appear here.
```
kafka-console-consumer.sh --bootstrap-server localhost:9092 \
                          --topic employee_data_output  \
                          --from-beginning \
                          --property print.key=true \
                          --property print.value=true \
                          --key-deserializer "org.apache.kafka.common.serialization.StringDeserializer" \
                          --value-deserializer "org.apache.kafka.common.serialization.LongDeserializer"   
```

#### Console Consumer to render messages from casual_employee_data_output.
All employees with salary less than 1000 will appear here.
```
kafka-console-consumer.sh --bootstrap-server localhost:9092 \
                          --topic casual_employee_data_output  \
                          --from-beginning \
                          --property print.key=true \
                          --property print.value=true \
                          --key-deserializer "org.apache.kafka.common.serialization.StringDeserializer" \
                          --value-deserializer "org.apache.kafka.common.serialization.LongDeserializer"   
```

#### Console Consumer to render messages from casual_employee_data_output.
All employees with salary less than 1000 will appear here.
```
kafka-console-consumer.sh --bootstrap-server localhost:9092 \
                          --topic casual_employee_data_output_with_below_500_salary  \
                          --from-beginning \
                          --property print.key=true \
                          --property print.value=true \
                          --key-deserializer "org.apache.kafka.common.serialization.StringDeserializer" \
                          --value-deserializer "org.apache.kafka.common.serialization.LongDeserializer"   
```

#### Console Consumer to render messages from casual_employee_data_output.
All employees with salary less than 1000 will appear here.
```
kafka-console-consumer.sh --bootstrap-server localhost:9092 \
                          --topic casual_employee_data_output_with_above_500_salary  \
                          --from-beginning \
                          --property print.key=true \
                          --property print.value=true \
                          --key-deserializer "org.apache.kafka.common.serialization.StringDeserializer" \
                          --value-deserializer "org.apache.kafka.common.serialization.LongDeserializer"   
```

#### Run Streaming application
./gradlew clean run