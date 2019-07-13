# Feign 
- Feign可以把Rest请求进行隐藏,伪装成类似SpringMvc的Contgroller一样. 不需要再自己拼接url,拼接参数等



## 1. 引入依赖
```xml
        <dependency>
            <groupId>org.sp bnm8,9ringframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
```


## 2.覆盖默认配置
    `暂无`

## 3. 在引导类中添加@EnableFeignClients注解,开启Feign
    - 在代码中:
        - 定义一个接口
        - 在接口上添加@FeignClient("要被调用的服务名")注解
        - 在接口中定义方法,要求方法签名和提供方的方法一致
        - 通过Autowride注入接口的动态代理实现类到Controller中(也是通过)