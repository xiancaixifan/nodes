![](http://assets.processon.com/chart_image/5c175ca5e4b05e0d0632f78d.png?_=1545046645989)
- Eureka 服务端和客户端
    - 创建一个Eureka项目,引入EurekaServer
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.0.7.RELEASE</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>
        <groupId>org.msk</groupId>
        <artifactId>eureka</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <name>eureka</name>
        <description>Demo project for Spring Boot</description>

        <properties>
            <java.version>1.8</java.version>
            <spring-cloud.version>Finchley.SR2</spring-cloud.version>
        </properties>

        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
            </dependency>

            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <scope>test</scope>
            </dependency>
        </dependencies>

        <dependencyManagement>
            <dependencies>
                <dependency>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-dependencies</artifactId>
                    <version>${spring-cloud.version}</version>
                    <type>pom</type>
                    <scope>import</scope>
                </dependency>
            </dependencies>
        </dependencyManagement>

        <build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                </plugin>
            </plugins>
        </build>

    </project>

    ```

- 配置application.yml全局配置文件


    ```yaml
    server:
        port: 11113
    spring:
      application:
        name: eureka # 会作为客户端名称,显示在eureka服务器中
    # Eureka对外客户端
    eureka:
      client:
        service-url:
        defaultZone: http://127.0.0.1:11113/eureka
    ```



- 在入口初始化类中打上Eureka服务器注解,打上这个注解就表名这个项目是一个Eureka服务器项目
    ```java
    package com.lh;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

    @SpringBootApplication
    @EnableEurekaServer//启用Eureka服务端
    public class EurekaApplication {

        public static void main(String[] args) {
            SpringApplication.run(EurekaApplication.class, args);
        }

    }

    ```



- Eureka集群搭建
    - 在application配置中,EurekaServer的对应对外客户端分别指向对方的服务端,实现交叉调用,组成集群




- 心跳机制调整

    - 服务续约(检测服务是否存活)<br/>
    **该配置项配置在微服务应用的*提供方*中,被EurekaServer检测**
    ```yaml
    eureka:
      instance:
        # 心跳检测的间隔时间
        lease-renewal-interval-in-seconds: 5
        # 发现服务死亡,清除前的等待时间
        lease-expiration-duration-in-seconds: 30 
    ```
    
    - 拉取服务
    **配置在微服务应用的*消费方*中,也被EurekaServer检测**
    ```yaml
    eureka:
     client:
       service-url:
       defaultZone: http://127.0.0.1:11113/eureka
       # 自动检测所需提供方的间隔时间
       registry-fetch-interval-seconds: 10
    ```

- 服务下线
    - 正常状态下,微服务应用本身要求下线,在EurekaServer中注销服务

    - 非正常状态下,微服务应用宕机<br>
        在EurekaServer的对外客户端中配置
        ```yaml
        #剔除失效服务的时间 10秒, 当服务器检测到失效服务时,10秒后将其下线
        eureka.server.eviction-interval-timer-in-ms=10000
        ```
        


- 自我保护
    当出现以下信息时
    ![](https://s1.ax1x.com/2018/12/17/F0gQ5d.png)
    > 这是触发了Eureka的自我保护机制。当一个服务未按时进行心跳续约时，Eureka会统计最近15分钟心跳失败的服务实例的比例是否超过了85%。
    > 在生产环境下，因为网络延迟等原因，心跳失败实例的比例很有可能超标，但是此时就把服务剔除列表并不妥当，
    > 因为服务可能没有宕机。Eureka就会把当前实例的注册信息保护起来，不予剔除。
    > 生产环境下这很有效，保证了大多数服务依然可用。


    **这回浪费时间,在开发阶段,自我保护机制要等待,所以在开发阶段一般关闭该机制**

    ```yaml
    eureka:
      server:
        enable-self-preservation: false # 关闭自我保护模式（缺省为打开）
        eviction-interval-timer-in-ms: 1000 # 扫描失效服务的间隔时间（缺省为60*1000ms）
    ```