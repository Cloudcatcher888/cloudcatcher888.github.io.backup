---
layout: post
title:  "Pyflink Kafka Intro"
date:   2022-05-22
categories: jekyll update
---
# 使用Pyflink和Kafka处理流式数据

## requirement

[kafka 3.2.0](https://dlcdn.apache.org/kafka/3.2.0/kafka_2.12-3.2.0.tgz)

[flink1.15](https://www.apache.org/dyn/closer.lua/flink/flink-1.15.0/flink-1.15.0-bin-scala_2.12.tgz)

[jdk 11](http://121.4.16.168:5000/jdk-11.0.15.jdk.zip)

[flink-sql-connector-kafka-1.15.0.jar](https://repo.maven.apache.org/maven2/org/apache/flink/flink-sql-connector-kafka/1.15.0/flink-sql-connector-kafka-1.15.0.jar)

## 安装与配置

设置java版本：在每个终端内设置

```bash
$ export JAVA_HOME=/Users/wangzhikai/jdk-11.0.15.jdk/Contents/Home
$ java -version
output->
java version "11.0.15" 2022-04-19 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.15+8-LTS-149)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.15+8-LTS-149, mixed mode)
```

安装kafka：

```bash
$ tar -xzf kafka_2.13-3.2.0.tgz
$ cd kafka_2.13-3.2.0
```

启动zookeeper（kafka内置）：

```bash
# Start the ZooKeeper service
# Note: Soon, ZooKeeper will no longer be required by Apache Kafka.
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

启动kafka：

```bash
# Start the Kafka broker service
$ bin/kafka-server-start.sh config/server.properties
```

创建topic（例子里是quickstart-events）并启动生产者：

```bash
$ /Users/wangzhikai/kafka_2.12-3.2.0/bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
$ /Users/wangzhikai/kafka_2.12-3.2.0/bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
$ /Users/wangzhikai/kafka_2.12-3.2.0/bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
```

optional：（可启动消费者查看是否kafka运行正常）

```bash
$ /Users/wangzhikai/kafka_2.12-3.2.0/bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```

安装flink：同kafka，解压即可

启动flink：

```bash
$ ~/flink-1.15.0/bin/start-cluster.sh 
```

结束flink：

```bash
$ ~/flink-1.15.0/bin/stop-cluster.sh 
```

任务：

```python
from pyflink.common.serialization import JsonRowDeserializationSchema
from pyflink.common.typeinfo import Types
from pyflink.datastream import StreamExecutionEnvironment
from pyflink.datastream.connectors import FlinkKafkaConsumer

env = StreamExecutionEnvironment.get_execution_environment()
# the sql connector for kafka is used here as it's a fat jar and could avoid dependency issues
env.add_jars("file:///Users/wangzhikai/flink-sql-connector-kafka-1.15.0.jar")

deserialization_schema = JsonRowDeserializationSchema.builder() \
    .type_info(type_info=Types.ROW_NAMED(
                             ["a","b"], [Types.STRING(), Types.STRING()])).build()

kafka_consumer = FlinkKafkaConsumer(
    topics='quickstart-events',
    deserialization_schema=deserialization_schema,
    properties={'bootstrap.servers': 'localhost:9092', 'group.id': 'test_group'})

ds = env.add_source(kafka_consumer)
```

对ds进行变换：

```python
ds = ds.map(lambda a: a + "d")
```

（参考可用的操作[operator](https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/dev/datastream/operators/overview/)）



kafka写入消息：("a","b"等列名按照JsonRowDeserializationSchema里定义的来，否则会输出Row(None, None))

```bash
# kafka-console-producer中 > 后面写入消息，格式如下：
> {"a":1,"b":"dfajdslkfj"}
> {"a":5,"b":"gajgsjd"}
> {"a":2,"b":"dsfjalj"}
> ...
```

在jupyter notebook中查看打印结果：

```python
with ds.execute_and_collect() as results:
    for result in results:
        print(result)
        
<Row('1', 'dfajdslkfjd')>
<Row('5', 'gajgsjdd')>
<Row('2', 'dsfjaljd')>
...
```

