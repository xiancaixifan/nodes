# 负载均衡
![](http://assets.processon.com/chart_image/5c179fa3e4b05e0d06338764.png?_=1545061545060)
- 为了减轻服务器压力,更好的发挥服务器集群的价值

- Ribbon有七大负载均衡规则

|内置规则|规则描述|
|:--|:--|
|RoundRobinRule|默认规则,轮询|
|AvailabilityFilteringRule|对以下两种服务器进行忽略：<br><br>（1）在默认情况下，这台服务器如果3次连接失败，这台服务器就会被设置为“短路”状态。短路状态将持续30秒，如果再次连接失败，短路的持续时间就会几何级地增加 <br><br>注意：可以通过修改配置loadbalancer.<clientName>.connectionFailureCountThreshold来修改连接失败多少次之后被设置为短路状态。默认是3次。<br><br>（2）并发数过高的服务器。如果一个服务器的并发连接数过高，配置了AvailabilityFilteringRule规则的客户端也会将其忽略。并发连接数的上线，可以由客户端的<clientName>.<clientConfigNameSpace>.ActiveConnectionsLimit属性进行配置。|
|WeightedResponseTimeRule|为每一个服务器赋予一个权重值。服务器响应时间越长，这个服务器的权重就越小。这个规则会随机选择服务器，这个权重值会影响服务器的选择。|
|ZoneAvoidanceRule|以区域可用的服务器为基础进行服务器的选择。使用Zone对服务器进行分类，这个Zone可以理解为一个机房、一个机架等。|
|BestAvailableRule|	忽略哪些短路的服务器，并选择并发数较低的服务器。|
|RandomRule|随机选择一个可用的服务器。|
|Retry|重试机制的选择逻辑|


- 在application中配置选择轮询策略
```yaml
{服务名称}:
  ribbon:
    NFLoadBalancerRuleClassName: IRule的实现类
```

|策略名|	策略声明|	策略描述|	实现说明|
|:--|:--|:--|:--|
|BestAvailableRule|	public class BestAvailableRule extends ClientConfigEnabledRoundRobinRule	|选择一个最小的并发请求的server|	逐个考察Server，如果Server被tripped了，则忽略，在选择其中ActiveRequestsCount最小的server|
|AvailabilityFilteringRule|	public class AvailabilityFilteringRule extends PredicateBasedRule|	过滤掉那些因为一直连接失败的被标记为circuit tripped的后端server，并过滤掉那些高并发的的后端server（active connections 超过配置的阈值）|	使用一个AvailabilityPredicate来包含过滤server的逻辑，其实就就是检查status里记录的各个server的运行状态|
|WeightedResponseTimeRule|	public class WeightedResponseTimeRule extends RoundRobinRule	|根据响应时间分配一个weight，响应时间越长，weight越小，被选中的可能性越低。|	一个后台线程定期的从status里面读取评价响应时间，为每个server计算一个weight。Weight的计算也比较简单responsetime 减去每个server自己平均的responsetime是server的权重。当刚开始运行，没有形成status时，使用roubine策略选择server。|
|RetryRule|	public class RetryRule extends AbstractLoadBalancerRule|	对选定的负载均衡策略机上重试机制。	|在一个配置时间段内当选择server不成功，则一直尝试使用subRule的方式选择一个可用的server|
|RoundRobinRule|	public class RoundRobinRule extends AbstractLoadBalancerRule	|roundRobin方式轮询选择server|	轮询index，选择index对应位置的server|
|RandomRule	public| class RandomRule extends AbstractLoadBalancerRule	|随机选择一个server	|在index上随机，选择index对应位置的server|
|ZoneAvoidanceRule|	public class ZoneAvoidanceRule extends PredicateBasedRule|	复合判断server所在区域的性能和server的可用性选择server|	使用ZoneAvoidancePredicate和AvailabilityPredicate来判断是否选择某个server，前一个判断判定一个zone的运行性能是否可用，剔除不可用的zone（的所有server），AvailabilityPredicate用于过滤掉连接数过多的Server。|
