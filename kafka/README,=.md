# kafka消费

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



### 参考博客

[超详细“零”基础kafka入门篇](https://www.cnblogs.com/along21/p/10278100.html)