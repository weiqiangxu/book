# golang的一些包应用

1. 日志统一使用zap [日志级别\日志appender\抽象]
2. 鉴权用jwt[无状态]
3. 业务垂直划分是领域驱动模型解耦
4. 业务水平划分有仓储层 Persistant Object 持久化对象，领域层 Domain Object ,业务层 Business Object，业务模块输出 DTO
5. 微服务部署 openapi开放网关 \ user center用户中心 \ Distributed File System文件服务中心 \ 业务模块[订单\商品\支付\客服\广告\消息]
6. CI\CD构建、docker hub私服、helm私服构建
