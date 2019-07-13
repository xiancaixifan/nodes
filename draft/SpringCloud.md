- RestTemplate:微服务应用之间的通信桥梁

- eureka注册中心:微服务应用的容器,有统筹,分配,组合不同微服务的功能
    - 心跳机制


- 什么是SpringCloud
    > Spring提供的微服务架构,类似于bubbo,Http协议 

- 远程调用
    - rpc :远程过程调用,自定义数据格式,基于TCP协议,效率更高 -> dubo
    - http:网络通信协议,规定了数据格式,基于TCP协议, ->SpringCloud


- eureka
    - eureka-server
        > 1. 引入依赖
        > 2. 开始配置
        >> spring.application.name= 服务名
        >> eureka.client.service= K V
        > 3.在引导类上添加注解 @EnableEurekaServer
    - eureka-client
        > 1.引入依赖
        > 2.开始配置
        >> 和服务端的差不多,要配置当前服务名,和服务器地址
        > 3.添加注解@EnableDiscoveryClient 并且配置通信桥梁RestTemplate对象
