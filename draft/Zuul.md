# Zuul 网关,服务路由

> 作为微服务引用的入口,在这里对所有的请求进行分类封装发送

## 基本配置
- 导入配置文件
```java
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
        </dependency>
```


- 入口类上加入注解 @EnableZuulProxy 开启网段代理服务


- 覆盖配置
```ymal
    zuul:
        routes:
            provice: /lh/**
            consumer: /co/**
        prefix: /api

```
> 其中 **provice** 和 **consumer** 是要被连接的服务名, 后面的是服务所要被替代成的别名
>> 如果不配置,默认配置的别名就是服务名


## 过滤器
```java
public abstract ZuulFilter implements IZuulFilter{

    abstract public String filterType();

    abstract public int filterOrder();

    boolean shouldFilter();// 来自IZuulFilter

    Object run() throws ZuulException;// IZuulFilter
}
```


- shouldFilter: 返回一个boolean值,来判定当前过滤器是否要生效
- run:过滤器的具体业务逻辑实现
- filterType:返回字符串,包含以下四种类型
    - pre: 请求在路由之前执行
    - route: 请求在正被路由时执行
    - post: 在route和error过滤器之后执行(请求结束时执行)
    - error:处理请求发生错误时执行
- filterOrder: 通过返回值来确定当前过滤器的执行优先级别




- 过滤器的生命周期
    ![](https://s1.ax1x.com/2018/12/22/FsqzZD.png)

    - 正常流程
        > 请求首先到达pre,然后是route,执行后返回结果会到达post,然后返回响应


    - 异常流程
        > pre或者route出现异常,直接进入error过滤器,error处理完毕后,请求会到达post过滤器,然后返回给用户
        > 如果是post出现异常,请求会到达error返回


- 过滤器使用场景
    - 服务鉴权:一般放在pre中,如果发现没有权限则直接拦截
    - 异常处理:一般放在error和post中结合来处理
    - 服务调用时长统计:pre和post结合使用




## 负载均衡和熔断
    - 配置熔断信息
    ```ymal
        hystrix:
        command:
            default:
            execution:
                isolation:
                thread:
                    timeoutInMilliseconds: 2000 # 设置hystrix的超时时间为6000ms
    ```