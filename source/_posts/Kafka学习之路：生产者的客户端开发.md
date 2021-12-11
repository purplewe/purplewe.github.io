---
title: Kafka学习之路：生产者——客户端开发
date: 2020-12-06
tags:
---

#### 客户端开发


**正常的客户端开发会有下面几步**

(1). 配置客户端参数，创建生产者实例
<question>KafkaProducer是线程安全的，为什么是线程安全的，如何池化KafkaProducer？KafkaTemplate中的producerFactory</question>
(2). 构建待发送消息
(3). 发送消息
(4). 关闭实例

**必要的参数配置**
(1). bootstrap.servers：指定生产者客户端连接的Kafka集群broker地址清单，用逗号分隔或者写成数组的形式。如果连接的是Kafka集群，最好是设置两个以上的broker地址性息，当其中一个宕机时客户端仍然可以连接到Kafka集群
(2). key.serializer、value.serializer：为ProducerRecord<K, V>中的key、value指定序列化操作，将其转为字节数组
(3). clent.id：设定生产者客户端id

更多参数配置可以查看类org.apache.kafka.clients.producer.ProducerConfig

```
spring:
  kafka:
    bootstrap-servers:
      - 192.168.88.137:9092
      - 192.168.88.138:9092
    client-id: producer-learnKafka
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.apache.kafka.common.serialization.StringSerializer
    consumer:
      group-id: consumer-learnKafka
```

**消息的发送**
发送消息有三种模式
(1). 发后即忘：只管发送消息，不管是否正确到达Kafka。这种方式性能最高，可靠性最差 
(2). 同步：KafkaProducer的send方法的返回值时Future类型的，可以执行完send方法后直接链式调用get方法来阻塞等待Kafka响应，直到消息发送成功或者发生异常
(3). 异步：