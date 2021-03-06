# SpringCloud 
## SpringCloud是什么 

<img src="https://spring.io/images/cloud-diagram-1a4cad7294b4452864b5ff57175dd983.svg" width="80%">  

Spring Cloud是分布式微服务架构下的一站式解决方案，是各个微服务架构落地技术的集合体，俗称微服务全家桶  
Spring Cloud为开发人员提供了工具，以快速构建分布式系统中的一些常见模式（例如，配置管理，服务发现，断路器，智能路由，微代理，控制总线，一次性令牌，全局锁，领导选举，分布式会话，群集状态）。分布式系统的协调导致样板式样，并且使用Spring Cloud开发人员可以快速实现这些样板的服务和应用程序。   
Spring Cloud 离不开Spring Boot,属于依赖关系，Spring Boot专注于快速、方便的开发单个个体微服务，Spring Cloud关注全局的服务治理框架  

### Spring Boot
Spring Boot本身并不提供Spring框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于Spring框架的应用程序。也就是说，它并不是用来替代Spring 的解决方案，而是和Spring 框架紧密结合用于提升Spring 开发者体验的工具。  

### 集群 分布式

<img src="https://github.com/qingjiaowolwh/Learning/blob/main/SpringCloud/img/fenbushi%20jiqun.jpg?raw=true" width="40%">

#### 集群  
单机处理到达瓶颈的时候，你就把单机复制几份，这样就构成了一个“集群”。
集群中每台服务器就叫做这个集群的一个“节点”，所有节点构成了一个集群。
每个节点都提供相同的服务，那么这样系统的处理能力就相当于提升了好几倍（有几个节点就相当于提升了这么多倍）。
但问题是用户的请求究竟由哪个节点来处理呢？最好能够让此时此刻负载较小的节点来处理，这样使得每个节点的压力都比较平均。
要实现这个功能，就需要在所有节点之前增加一个“调度者”的角色，用户的所有请求都先交给它，
然后它根据当前所有节点的负载情况，决定将这个请求交给哪个节点处理。这个“调度者”有个牛逼了名字——负载均衡服务器。
#### 分布式  
分布式结构就是将一个完整的系统，按照业务功能，拆分成一个个独立的子系统，在分布式结构中，每个子系统就被称为“服务”
。这些子系统能够独立运行在web容器中，它们之间通过RPC（Remote Procedure Call 远程过程调用）方式通信。

## eureka服务注册与发现  
### 什么是 eureka
eureka是netflix的一个子模块，也是核心模块之一。  
eureka是一个基于rest的服务，用于定位服务，以实现云端中间层服务发现和故障，
服务注册与发现对于微服务来说是非常重要的，有了服务注册与发现，只需要使用服务的
标识符，就可以访问到服务，而不需要修改服务调用的配置文件了

### eureka的基本架构
spring cloud封装了netflix公司开发的eureka模块来实现服务注册与发现  
eureka采用了c-s的架构设计，eureka server作为服务注册功能的服务器，他是服务注册中心  
而系统的其他问服务。使用eureka的客户端连接到eureka server并维持心跳连接。
这样系统的维护人员就可以通过eureka server来监控系统中各个微服务是否正常运行，
spring cloud的一些其他模块（比如zuul）就可以通过eureka server来发现
系统中的其他微服务，并执行相关的逻辑；  
eureka client是一个java客户端，用于简化eureka server的交互，客户端同时也具备一个内置的，使用轮询
负载算法的负载均衡器。在启动后，将会向eureka server发送心跳（默认周期为30秒）
如果eureka server在多个心跳周期内没有接收到某个节点的心跳，eureka server将会
从服务注册表中把这个服务节点移除掉（默认周期为90秒）

### CAP是什么  
C(Consistency) 强一致性  
A(Availability) 可用性  
P(Partition tolerance) 分区容错性  

### CAP 核心原理
一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求  
根据CAP原理，分成了满足CA原则，满足CP原则和满足AP原则三大类：  
CA：单点集群，满足一致性，可用性的系统，通常可扩展性较差
CP：满足一致性，分区容错性的系统，通常性能不是特别高  
AP：满足可用性，分区容错性的系统，通常对一致性要求低一些  

### eureka保证的是AP
eureka在设计时优先保证可用性。eureka各个节点都是平等的，几个节点挂掉不会影响正常节点的工作，
剩余的节点依然可以提供注册和查询服务。而eureka的客户端在向某个eureka注册时，
如果发现连接失败，则会自动切换至其他节点，只要有一台eureka
还在，就能保证注册服务的可用性，只不过查到的信息可能不是最新的，除此之外，eureka还有一种自我保护机制，如果在十五分钟内超过
85%的节点都没有正常的心跳，那么eureka就认为客户端与注册中心出现了网络故障，此时会出现以下几种情况：  
1、eureka不会从注册列表中移除因为长时间没收到心跳而应该过期的服务  
2、eureka任然能够接收新服务的注册和查询请求，但是不会被同步到其他节点上（即保证当前节点依然可用）  
3、当网络稳定时，当前实例新的注册信息会被同步到其他节点中

因此，eureka可以很好的应对因网络故障导致部分节点失去联系的情况   

### 实践

<img src="https://github.com/qingjiaowolwh/Learning/blob/main/SpringCloud/img/fuwuzhucexiaofei.jpg?raw=true" width="50%">  


<img src="https://github.com/qingjiaowolwh/Learning/blob/main/SpringCloud/img/jiandanjuqun.jpg?raw=true" width="50%">

## Feign  
Feign是声明式的Web Service客户端，它让微服务之间的调用变得更简单了，类似Controller调用 Service， Spring Cloud集成了Ribbon 和 Eureka，可在使用Feign时提供负载均衡的http客户端  
Feign做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义，在Feign的实现下，只需要创建一个接口并使用注解的方式来配置它(类似于以前Dao接口标注Mapper注解，现在是一个微服务接口上面标注一个Feign注解即可)，即可完成服务提供方的接口绑定，简化了使用Spring Cloud Ribbon时，自动封装服务调用客户端的开发量

## Ribbon  
Spring Cloud Ribbon是基于Netflix ribbon实现的一套客户端负载均衡的工具。
  
### 负载均衡分类  
#### 集中式LB  
即在服务的消费方和提供方之间使用独立的LB设施，如Nginx:
方向代理服务器，由该设施负责把访问请求通过某种策略转发至服务的提供方  

#### 进程式LB  
将LB逻辑集成到消费方，消费方从服务注册中心获知哪些地址可用，然后自己再从这些地址中选出一个合适的服务器

### 负载均衡策略 

<img src="https://github.com/qingjiaowolwh/Learning/blob/main/SpringCloud/img/Robbon%20rule.jpg?raw=true" width="80%"> 

## Hystrix   (https://github.com/Netflix/Hystrix/wiki)
是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时，异常等，Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。  
本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控(类似熔断保险丝)，向调用方返回一个服务预期的，可处理的备选响应(FallBack)，而不是长时间的等待或者抛出调用方法无法处理的异常，这样就可以保证了服务调用方的线程不会被长时间，不必要的占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩  

### Hystrix工作流

<img src="https://github.com/qingjiaowolwh/Learning/blob/main/SpringCloud/img/hystrix-work-flow.png?raw=true" width="60%">

### Hystrix具备哪些能力/优点？  
在通过网络依赖服务出现高延迟或者失败时，为系统提供保护和控制  
可以进行快速失败，缩短延迟等待时间和快速恢复：当异常的依赖回复正常后，失败的请求所占用的线程会被快速清理，不需要额外等待  
提供失败回退（Fallback）和相对优雅的服务降级机制  
提供有效的服务容错监控、报警和运维控制手段  

### 服务雪崩、服务熔断、服务降级 (https://www.cnblogs.com/rjzheng/p/10340176.html)

### Hystrix 如何解决依赖隔离的  （https://www.cnblogs.com/rickiyang/p/11853315.html）  
Hystrix使用命令模式HystrixCommand(Command)包装依赖调用逻辑，每个命令在单独线程中/信号授权下执行。  
可配置依赖调用超时时间,超时时间一般设为比99.5%平均时间略高即可.当调用超时时，直接返回或执行fallback逻辑。  
为每个依赖提供一个小的线程池（或信号），如果线程池已满调用将被立即拒绝，默认不采用排队，加速失败判定时间。  
依赖调用结果分:成功，失败（抛出异常），超时，线程拒绝，短路。 请求失败(异常，拒绝，超时，短路)时执行fallback(降级)逻辑。  
提供熔断器组件,可以自动运行或手动调用,停止当前依赖一段时间     

#### 断路器  
#### 断路器开启条件  
1.当满足一定阈值的时候（默认10秒内超过20个请求此时）  
2.当失败率达到一定的时候（默认10秒内超过50%请求失败）  
3.当达到以上阈值，断路器将会开启  
4.当开启的时候，所有请求都不会进行转发  
5.一段时间之后（默认五秒），这个时候断路器是半开状态，会让其中一个请求进行转发。如果成功，断路器会关闭，若失败继续开启。重复4和5  

### Dashboard

<img src="https://github.com/qingjiaowolwh/Learning/blob/main/SpringCloud/img/Hystrix.jpg?raw=true" width="50%">

http://localhost:8001/actuator/hystrix.stream  
http://localhost:9001/hystrix  

## Zuul
Zuul是Spring Cloud全家桶中的微服务API网关。  
所有从设备或网站来的请求都会经过Zuul到达后端的Netflix应用程序。作为一个边界性质的应用程序，Zuul提供了动态路由、监控、弹性负载和安全功能。Zuul底层利用各种filter实现如下功能：  
认证和安全 识别每个需要认证的资源，拒绝不符合要求的请求。
性能监测 在服务边界追踪并统计数据，提供精确的生产视图。
动态路由 根据需要将请求动态路由到后端集群。
压力测试 逐渐增加对集群的流量以了解其性能。
负载卸载 预先为每种类型的请求分配容量，当请求超过容量时自动丢弃。
静态资源处理 直接在边界返回某些响应。

### zuul的工作原理  
#### 1、过滤器机制  

<img src="https://github.com/qingjiaowolwh/Learning/blob/main/SpringCloud/img/zuul%20yuanlitu.png?raw=true">  

zuul的核心是一系列的filters, 其作用可以类比Servlet框架的Filter，或者AOP。
zuul把Request route到 用户处理逻辑 的过程中，这些filter参与一些过滤处理，比如Authentication，Load Shedding等。  

#### 2、过滤器的生命周期  

<img src="https://github.com/qingjiaowolwh/Learning/blob/main/SpringCloud/img/zuul%20shengmingzhouqi.png?raw=true">

Zuul请求的生命周期如图，该图详细描述了各种类型的过滤器的执行顺序。  
Zuul大部分功能都是通过过滤器来实现的。Zuul中定义了四种标准过滤器类型，这些过滤器类型对应于请求的典型生命周期  
1、PRE：这种过滤器在请求被路由之前调用。我们可利用这种过滤器实现身份验证、在集群中选择请求的微服务、记录调试信息等  
2、ROUTING：这种过滤器将请求路由到微服务。这种过滤器用于构建发送给微服务的请求，并使用Apache HttpClient或Netfilx Ribbon请求微服务  
3、POST：这种过滤器在路由到微服务以后执行。这种过滤器可用来为响应添加标准的HTTP Header、收集统计信息和指标、将响应从微服务发送给客户端等  
4、ERROR：在其他阶段发生错误时执行该过滤器  

### 操作
http://zuul9527.com:9527/mydept/dept/get/1
