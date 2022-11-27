# telepresence

```
telepresence version
telepresence connect
```

[github telepresence](https://github.com/telepresenceio/telepresence)


### connect your cluster

[本机连接到远程 Kubernetes 集群](https://kubernetes.io/zh-cn/docs/tasks/debug/debug-cluster/local-debugging/)

```
$ telepresence connect
```

### 开发和调试现有的服务

```
# 远程流量发送到本地服务

# $SERVICE_NAME 是本地服务名称
# $LOCAL_PORT 是服务在本地工作站上运行的端口
# $REMOTE_PORT 是服务在集群中侦听的端口

telepresence intercept $SERVICE_NAME --port $LOCAL_PORT:REMOTE_PORT
```

### 验证集群是否连接成功

```
ping [$service_name].[$namespace].svc.cluster.local

ping login.user.svc.cluster.local
```



