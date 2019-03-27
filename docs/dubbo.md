[一. zooleeper宕机还可以调用服务吗？](#1)

[二. dubbo负载均衡机制？](#2)

[三. 服务降级？](#3)

[四. 集群容错？](#4)

[五. RPC原理？](#5)

[六. dubbo原理？](#6)

#### <span id="1">一. zooleeper宕机还可以调用服务吗？</span>

（1）只要消费者调用过服务提供者，注册中心全部宕机，服务提供者和消费者仍能通过本地缓存通讯

（2）还可以通过dubbo直连的方式，绕过注册中心。@Refence(url="127.0.0.1:20882")



#### <span id="2">二. dubbo负载均衡机制？</span>

（@Reference（loadbalance="roundrobin"））

2.随机负载均衡 （random）：按权重设置随机概率

1.基于权重的轮询负载均衡 （roundrobin）：按权重设置轮询比率（7次中调3次等）

2.最少活跃数（leastactive）：挑选相应速度最快的服务器

3.一致性哈希（consistenhash）：相同参数的请求总是发送到同一个提供者



#### <span id="3">三. 服务降级？</span>

当服务器压力剧增的情况下，对一些服务和页面不处理。释放服务器资源

（可以再zookeeper控制台进行勾选）



#### <span id="4">四. 集群容错？</span>

1.在集群调用失败时，dubbo默认采用failover容错模式

（1）failover：当出现失败，重试其它服务器 <dubbo:service retries="2" />

（2）failfase:快速失败。调用失败，直接报错（用于写入操作，比如新增）

（3）failsafe：安全失败，出现异常，直接忽略，写入日志

2.第三方容错方案 Hystrix

（1）引入依赖

```xml

<dependency>
    <groupId>com.netflix.hystrix</groupId>
    <artifactId>hystrix-core</artifactId>
    <version>1.4.4</version>
</dependency>

```

（2）application类上添加注解： @EnableHystrix

（3）对方法进行异常代理：@HystrixCommand



#### <span id="5">五. RPC原理？</span>

1.RPC调用流程？

（1）消费者代理在收到调用后负责将方法，参数组装成能进行网络传输的消息体（序列化）

（2）消费者代理找到服务地址，将消息发送到服务端

（3）服务端代理收到消息后进行反序列化，调用本地服务

（4）服务端代理将结果序列化返回至消费者端

（5）消费者端反序列化得到结果



#### <span id="6">六. dubbo原理？</span>

