# 集群分片

> 主从只是扩展了读，但是写和存储能力并未得到扩展

### 关键模块

1. 哈希槽(Hash Slot)(数量是2^14=16384)，Cluster每个节点负责一部分哈希槽

2. Keys hash tags 将相关Key分配到相同的hash slot

3. Cluster nodes属性
```
redis-cli cluster nodes

node id, address:port, flags, last ping sent, last pong received, configuration epoch, link state, slots.
```
Cluster总线

4. Cluster总线
```
节点之间通讯使用集群总线和集群总线协议：有不同的类型和大小的帧组成的二进制协议
```

5. 集群拓扑

6. 节点握手

### 请求重定向

> 去中心化思想，集群主节点各自负责一部分槽

###  MOVED重定向

```
客户端发送key命令，节点检查不存在会返回Moved 重定向，客户端收到以后会根据Moved再一次发送找寻目标节点

redis-cli -c 

-c 是集群方式启动，即没加参数 -c，redis-cli不会自动重定向
```

### Ask重定向
```
集群伸缩会导致槽迁移，当我们去源节点访问时，此时数据已经可能已经迁移到了目标节点，使用Ask重定向来解决此种情况
```

### 扩容&缩容
```
扩容

1. 节点纳入，cluster meet new_node_ip:new_node_port 或者 redis-trib add node
2. 数据迁移，将槽迁移到目标节点

缩容

1. 槽迁移
2. 广播下线 luster forget nodeId
```

### 为什么Redis Cluster的Hash Slot 是2^14=16*1024

### 为什么Redis Cluster中不建议使用发布订阅
```
所有的publish命令都会向所有节点（包括从节点）进行广播，带宽消耗大
```

