# why write: broken pipe

[golang服务报错: write: broken pipe](https://blog.csdn.net/cljdsc/article/details/124134531)

[重现broken pipe: 讲解很清晰](https://piaohua.github.io/post/golang/20220731-connection-broken-pipe/)

[Gin Error Connection Write Broken Pipe](https://reid00.github.io/posts/gin-error-connection-write-broken-pipe/)

[nginx\api-gateway(golang server)\backend broken pipe](https://zyun.360.cn/blog/?p=1634)

1. ulimit -n 连接数过大
2. 调用者在接收到服务端响应之前断开连接

### client返回server RST包是什么意思

### 连接状态为CLOSE_WAIT的连接，是什么意思，怎么看

### 客户端设置的响应超时时间5秒，如何设置

### broken pipe 和 reset by peer 分别是什么

### 压测计量QPS的时候有很多broken pipe，加大连接数可以提升QPS吗

