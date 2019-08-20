1.cloud体系中，分为三种角色
	服务注册中心，服务提供者，服务消费者
也可以说分为两部分：
	服务注册中心，客户端
	
2.
“服务提供者”在启动的时候会通过发送REST请求的方式将自己注册到EurekaServer上，同时带上了自身服务的一些元数据信息。
EurekaServer接收到这个REST请求之后，将元数据信息存储在一个双层结构Map中，其中第一层的key是服务名，第二层的key是具体服务的实例名。
（我们可以回想一下之前在实现Ribbon负载均衡的例子中，Eureka信息面板中一个服务有多个实例的情况，这些内容就是以这样的双层Map形式存储的。）

服务提供者
	服务注册
	服务同步
	服务续约
服务消费者
	获取服务
	服务调用
	服务下线
服务注册中心
	失效剔除
	自我保护


EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.
eureka.server.enable-self-preservation
服务注册到EurekaServer之后，会维护一个心跳连接，告诉EurekaServer自己还活着。
EurekaServer在运行期间，会统计心跳失败的比例在15分钟之内是否低于85%，如果出现低于的情况（在单机调试的时候很容易满足，实际在生产环境上通常是由于网络不稳定导致），
EurekaServer会将当前的实例注册信息保护起来，让这些实例不会过期，尽可能保护这些注册信息。

