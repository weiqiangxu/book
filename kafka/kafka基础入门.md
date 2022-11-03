### 博客园-详细基础入门


> 高性能，低延迟提交日志存储，复制和传播的专用分布式文件系统 

```
注意是一个分布式文件系统
```

### 数据单位
```
数据以 topics 主题类别存储

每条记录有键、值、时间戳
```

### 核心API

```
Producer API（生产者API）
Consumer API（消费者API）
Streams API（流API）
Connector API（连接器API）
```
### 主题 topic 

> 逻辑分类，一类消息

### 分区 patitions

> 一个分区在存储层面可以是一个 append log 文件

> 一个 topic 可以分成多个分区 partition

```
有序、不可变的记录序列

分区中每个记录，有一个称谓偏移的顺序ID号码，唯一表示每个分区的记录
```


### kafka允许设置记录保留期（无论是否使用），但是Kafka的性能在数据大小方面实际上是恒定的，因此长时间存储数据不是问题


### consumer group 消费组

```
每个consumer属于一个consumer group，一个group里面可以有多个consumer，但是一个topic里面在group之中只会被消费一次，也就是同一个group的多个consumer不可能消费到同一个topic的同一个消息
```


### 分区

```
可以认为一个 append log 文件或者 一块磁盘
```

### 组

```
可以认为是一个用于访问 append log 的密钥 (这个密钥只能给一消费者用)
```

### 消费者 

```
可以认为是真正拿到 append log 文件内容的程序
```

### topic

```
可以认为是 append log 里面每一行内容的类别，比如有些是QQ日志、有些是微信日志
```


### 一个分区只能被同一个组的一个消费者消费 

```
可以简单地认为 一个 append log 文件，有多个组（密钥），在一个组内（1个密钥）只能给一个消费者使用
```

### 一个消费者可以消费同一个topic的多个分区

```
最终读取 append log 文件的程序，可以读取多个 append log 文件 (topic是微信日志，这个日志存在很多个地方)
```

# 关键点：持有一个group id消费1个分区的消息，只能被1个消费者消费，如果有2个消费者持有同一个 group id 消费同一个分区的数据，那么其中只有 1个 可以消费到

> 消费者数量大于分区数时候，多余的消费者会处于闲置的状态

### 术语

```
Record
Topic
Producer
Consumer
Broker
Partition
Leader/Follower  分区分本
Offset 偏移量
Consumer group  消费者组
Coodinator 协调者
Controller 控制器
```


### 参考博客

[超详细“零”基础kafka入门篇](https://www.cnblogs.com/along21/p/10278100.html)