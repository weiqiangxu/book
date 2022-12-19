# why write: broken pipe

[golang服务报错: write: broken pipe](https://blog.csdn.net/cljdsc/article/details/124134531)

[重现broken pipe: 讲解很清晰](https://piaohua.github.io/post/golang/20220731-connection-broken-pipe/)

[Gin Error Connection Write Broken Pipe](https://reid00.github.io/posts/gin-error-connection-write-broken-pipe/)

[nginx\api-gateway(golang server)\backend broken pipe](https://zyun.360.cn/blog/?p=1634)

[记一次connection-reset-by-peer问题定位-状态流转很详细](https://testerhome.com/articles/23296)

[如何在Golang中强制关闭TCP连接](https://itnext.io/forcefully-close-tcp-connections-in-golang-e5f5b1b14ce6)

[服务端大量close_wait 和 time_wait状态](https://www.cnblogs.com/taoshihan/p/14785384.html)

[Go 中如何强制关闭 TCP 连接](https://blog.csdn.net/EDDYCJY/article/details/120898217)


1. ulimit -n 连接数过大
2. 调用者在接收到服务端响应之前断开连接

### client返回server RST包是什么意思

### 连接状态为CLOSE_WAIT的连接，是什么意思，怎么看

### 客户端设置的响应超时时间5秒，如何设置

### broken pipe 和 reset by peer 分别是什么

### 压测计量QPS的时候有很多broken pipe，加大连接数可以提升QPS吗

### tcpdump工具的使用

### TCP SOCKET 状态表：

```
CLOSED: 关闭状态，没有连接活动
LISTEN: 监听状态，服务器正在等待连接进入
SYN_SENT: 已经发出连接请求，等待确认
SYN_RCVD: 收到一个连接请求，尚未确认
ESTABLISHED: 连接建立，正常数据传输状态
FIN_WAIT_1:（主动关闭）已经发送关闭请求，等待确认
FIN_WAIT_2:（主动关闭）收到对方关闭确认，等待对方关闭请求
CLOSE_WAIT:（被动关闭）收到对方关闭请求，已经确认
LAST_ACK:（被动关闭）等待最后一个关闭确认，并等待所有分组死掉
TIMED_WAIT: 完成双向关闭，等待所有分组死掉
CLOSING: 双方同时尝试关闭，等待对方确认
```

### 什么情况下会有非常多的CLOSE_WAIT

### Gin什么时候会在response之前关闭TCP连接