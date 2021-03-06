# 作者简介:
冰河，高级软件架构师，Java编程专家，大数据架构师与编程专家，信息安全高级工程师，Mykit系列开源框架创始人、核心架构师和开发者，Android开源消息组件Android-MQ独立作者，精通Java, Python, Hadoop大数据生态体系，熟悉MySQL、Redis内核，Android底层架构。多年来致力于分布式系统架构、微服务、分布式数据库、分布式事务与大数据技术的研究，曾主导过众多分布式系统、微服务及大数据项目的架构设计、研发和实施落地。在高并发、高可用、高可扩展性、高可维护性和大数据等领域拥有丰富的架构经验。对Hadoop、Spark、Storm、Flink等大数据框架源码进行过深度分析并具有丰富的实战经验。《海量数据处理与大数据技术实战》、《MySQL开发、优化与运维实战》作者。

# NettyRpc
An RPC framework based on Netty, ZooKeeper and Spring  

### Features:
* Simple code and framework
* Non-blocking asynchronous call and Synchronous call support
* Long lived persistent connection
* High availability, load balance and failover
* Service Discovery support by ZooKeeper

#### How to use
1. Define an interface:

```
public interface HelloService { 
	String hello(String name); 
	String hello(Person person);
}
```

2. Implement the interface with annotation @RpcService:

```
@RpcService(HelloService.class)
public class HelloServiceImpl implements HelloService {
	@Override
	public String hello(String name) {
		return "Hello! " + name;
	}

	@Override
	public String hello(Person person) {
		return "Hello! " + person.getFirstName() + " " + person.getLastName();
	}
}
```

3. Run zookeeper

4. Start server:
```
RpcBootstrap
```
		
5. Use the client:
 
```
ServiceDiscovery serviceDiscovery = new ServiceDiscovery("127.0.0.1:2181");
final RpcClient rpcClient = new RpcClient(serviceDiscovery);
// Sync call
HelloService helloService = rpcClient.create(HelloService.class);
String result = helloService.hello("World");
// Async call
IAsyncObjectProxy client = rpcClient.createAsync(HelloService.class);
RPCFuture helloFuture = client.call("hello", "World");
String result = (String) helloFuture.get(3000, TimeUnit.MILLISECONDS);
```
