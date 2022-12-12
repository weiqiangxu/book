# opentracing

1. 什么是opentracing
2. 什么是jaeger
3. 如何实现服务可观测


### 启动jaeger

```
docker run -d -p 5775:5775/udp -p 16686:16686 jaegertracing/all-in-one:latest
```

[http://127.0.0.1:16686/search](http://127.0.0.1:16686/search)


### 相关地址

[github.com/yurishkuro/opentracing-tutorial/](https://github.com/yurishkuro/opentracing-tutorial/)

[opentracing.io/docs/getting-started/][https://opentracing.io/docs/getting-started/]

[jaegertracing.io/](https://www.jaegertracing.io/)

[https://www.cnblogs.com/whuanle/p/14598049.html](https://www.cnblogs.com/whuanle/p/14598049.html)

[go-gin-api 路由中间件 - Jaeger 链路追踪](https://www.cnblogs.com/xinliangcoder/p/11604880.html)

[github.com/xinliangnote/go-jaeger-demo](https://github.com/xinliangnote/go-jaeger-demo)