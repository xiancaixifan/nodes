# 熔断机制
![](http://assets.processon.com/chart_image/5c179fa3e4b05e0d06338764.png?_=1545061545060)


> 这就好比，一个汽车生产线，生产不同的汽车，需要使用不同的零件，如果某个零件因为种种原因无法使用，那么就会造成整台车无法装配，
> 陷入等待零件的状态，直到零件到位，才能继续组装。  
> 此时如果有很多个车型都需要这个零件，那么整个工厂都将陷入等待的状态，导致所有生产都陷入瘫痪。
> 一个零件的波及范围不断扩大。 


# 服务熔断 和 服务降级
## 服务熔断
> 服务熔断一般指的是软件系统应用中,由于某些原因导致服务过载,
> 为了防止整个系统出现奔溃的情况,从而采用的一种保护措施,也称过载保护;
>> **如果某个目标服务调用慢或者有大量超时，此时，熔断该服务的调用，对于后续调用请求，**
>> **不在继续调用目标服务，直接返回，快速释放资源。如果目标服务情况好转则恢复调用。**
>
## 服务降级
> 在系统整体资源不够用的情况下,比如某一时刻并发量过大,系统为了防止出现宕机从而导致雪崩情况,
> 会关闭一部分的服务从而减轻服务器压力,等资源够用了再开回来;


- **相同点**
1. **粒度一般都是服务级别** 最小单位一般都是应用服务,有的公司将粒度降低到持久层中
2. **目的一直** 都是为了防止系统出现大面积奔溃和系统瘫痪
3. **最终表现一致** 对于用户来说,用户体验到的最终都是某写应用不可用或不可达
4. **自治性要求高** 这两种保护手段,都是需要开发者在系统中配置的,不太可能手动开启,开发预置
5. **回升点一致** 当服务压力小,网络恢复正常时,系统会自动取消熔断和降级

- **不同点**
1. **出发原因不同**
    - 熔断服务一般是因为某个服务(下游服务)故障引起的
    - 服务降级是为了整个系统的稳定采用的

2. **管理目标不同**
    - 熔断服务的配置是基于整个系统的,不管哪个服务出现了问题,都会采用熔断
    - 服务降级是基于应用的,当系统资源不够时,一般先停止的服务是一些非核心服务,也就是说从外围服务开始降级




## ∷服务熔断配置
1. 如果要使用熔断和降级,需要导入他们所在的包
    ```xml
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
            </dependency>
    ```

2. 在入口函数中开启熔断和降级服务
    ```java
        @SpringCloudApplication
        public class ServiceConsumerApplication {

            public static void main(String[] args) {
                SpringApplication.run(ServiceConsumerApplication.class, args);
            }
    ```
    ![](https://s1.ax1x.com/2018/12/18/FBmwJP.jpg)



3. 在Controller调用层,配置具体的熔断信息
    1. 在消费方法上配置熔断信息,fallbackMethod表示被熔断后调用的方法名称
        ```java
        @HystrixCommand(fallbackMethod ="callBack")
        public String queryUserById(){
            String baseUrl = "http://provice/port/";
            String  str= this.restTemplate.getForObject(baseUrl, String.class);
            return str;
        }
        ```

    2. 可以在整个Controller消费类上面配置熔断信息
        - @DefaultProperties defaultFallback参数表名被熔断后调用的当前类方法名称,
        - @HystrixCommand 申明需要被熔断保护的消费方法

        ```java
        @DefaultProperties(defaultFallback ="callBack" )
        public class ConsumerController {
            @HystrixCommand
            public String queryUserById(){
                String baseUrl = "http://provice/port/";
                String  str= this.restTemplate.getForObject(baseUrl, String.class);
                return str;
            }
            ...
        }
        ```


4. 回调方法,不需要申明注解了
    ```java
        public String callBack() {
            return "系统繁忙,请稍后重试";
        }
    ```


5. 设置熔断机制的阈值(超时保护时间)
    ```yaml
    hystrix:
      command:
          default:
          execution:
              isolation:
              thread:
                  timeoutInMilliseconds: 6000 # 设置hystrix的超时时间为6000ms
    ```

## ∷服务降级配置
    - 模拟降级
        这个hystrix框架的降级是自动的
    ```java
        @GetMapping("port/{id}")
        @ResponseBody
        @HystrixCommand
        public String queryUserById(@PathVariable("id") Long id){
        if (id == 1) {
            throw new RuntimeException("忙碌");
        }
    ```
    发送 http://127.0.0.1:8090/port/1 进入异常中触发熔断,会自动调用回调方法
    发送 http://127.0.0.1:8090/port/2 正常调用提供者代码


    当 id 为1 ,调用失败20次时,在进行正常的调用提供者(id=2)时,也无法调用,因为该消费服务降级了