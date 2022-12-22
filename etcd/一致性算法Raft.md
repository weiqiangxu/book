# 分布式一致性算法Raft和Etcd原理解析

[分布式一致性算法Raft和Etcd原理解析](https://learn.lianglianglee.com/%E4%B8%93%E6%A0%8F/%E5%88%86%E5%B8%83%E5%BC%8F%E4%B8%AD%E9%97%B4%E4%BB%B6%E5%AE%9E%E8%B7%B5%E4%B9%8B%E8%B7%AF%EF%BC%88%E5%AE%8C%EF%BC%89/09%20%E5%88%86%E5%B8%83%E5%BC%8F%E4%B8%80%E8%87%B4%E6%80%A7%E7%AE%97%E6%B3%95%20Raft%20%E5%92%8C%20Etcd%20%E5%8E%9F%E7%90%86%E8%A7%A3%E6%9E%90.md)

### Raft角色

1. Leader（领导者）
2. Follower（追随者）
3. Candidate（候选者）

### Raft机制设定带来的问题有哪些

1. 选举（Leader Election）
2. 日志复制（Log Replication）
3. 安全性（Safety）


### Raft 算法之 Leader Election 原理

第一阶段：所有节点都是 Follower
第二阶段：Follower 转为 Candidate 并发起投票
第三阶段：投票策略
第四阶段：Candidate 转为 Leader

### Raft 算法之 Log Replication 原理

第一阶段：客户端请求提交到 Leader
第二阶段：Leader 将 Entry 发送到其它 Follower
第三阶段：Leader 等待 Followers 回应。
第四阶段：Leader 回应客户端。
第五阶段，Leader 通知 Followers Entry 已提交

### Raft 算法之安全性
1. 选举限制
2. 提交之前任期内的日志条目


### Etcd

1. 架构
2. 基本概念
3. 应用场景（服务发现、消息发布和订阅、分布式锁、集群监控与Leader竞选）