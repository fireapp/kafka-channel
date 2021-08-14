# kafka-channel
flume kafka channel use for source and sink

# use
> source mode

kafka-channel -> other-sink
```properties
#flume conf

 a1.channels = c1
 a1.sinks = s1
 a1.sinks.s1.channel = c1 

# kafka channel source mode
 a1.channels.c1.type = org.apache.flume.ext.KafkaChannel
 a1.channels.c1.batchDurationMillis = 2000
 a1.channels.c1.pollTimeout = 500
 a1.channels.c1.kafka.bootstrap.servers = kafka-ip:port

# consumer topics, support regex
a1.channels.c1.kafka.topic = ^topic_name_[0-9]$

# When set to true, stores the topic of the retrieved message into a header, defined by the topicHeader property
 a1.channels.c1.setTopicHeader = true
 a1.channels.c1.topicHeader = topic
 a1.channels.c1.setPartitionHeader = true
 a1.channels.c1.partitionHeader = partition
 a1.channels.c1.setOffsetHeader = true
 a1.channels.c1.offsetHeader = offset 

 a1.channels.c1.kafka.consumer.group.id = group_id_001
 a1.channels.c1.kafka.consumer.auto.offset.reset = earliest
 a1.channels.c1.kafka.consumer.* = *

# file sink
 a1.sinks.s1.type = other-sink
 a1.sinks.s1.* = *

```

> sink mode

other-source -> other-interceptor -> kafka-channel
```properties
#flume conf

a1.sources = r1
a1.channels = c1
a1.sources.r1.channel = c1 

# other source
a1.sources.r1.type = other-source
a1.sources.r1.* = *

# other-interceptor used etl event
a1.sources.r1.interceptors = i1
a1.sources.r1.interceptors.i1.type = *

# kafka-channel sink mode

a1.channels.c1.type = org.apache.flume.ext.KafkaChannel
# use sink mode
a1.channels.c1.useSinkMode = true
a1.channels.c1.kafka.bootstrap.servers = kafka-ip:port
# event put default topic
a1.channels.c1.kafka.topic = topic_name

# event put custom topic, when event header set $topicHeader be used
a1.channels.c1.topicHeader = topic

a1.channels.c1.kafka.producer.* = *

```
