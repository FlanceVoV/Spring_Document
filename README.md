# Spring_Document
# spring相关文档

# 1)activity工作流相关文档  
activity数据库说明:  
Activiti工作流总共包含23张数据表，所有的表名默认以“ACT_”开头。  
并且表名的第二部分用两个字母表明表的用例，而这个用例也基本上跟Service API匹配。  
u  ACT_GE_* : “GE”代表“General”（通用），用在各种情况下；  
u  ACT_HI_* : “HI”代表“History”（历史），这些表中保存的都是历史数据，比如执行过的流程实例、变量、任务，等等。Activit默认提供了4种历史级别：  
Ø  none: 不保存任何历史记录，可以提高系统性能；  
Ø  activity：保存所有的流程实例、任务、活动信息；  
Ø  audit：也是Activiti的默认级别，保存所有的流程实例、任务、活动、表单属性；  
Ø  full：最完整的历史记录，除了包含audit级别的信息之外还能保存详细，例如：流程变量。  
对于几种级别根据对功能的要求选择，如果需要日后跟踪详细可以开启full。  
  
u  ACT_ID_* : “ID”代表“Identity”（身份），这些表中保存的都是身份信息，如用户和组以及两者之间的关系。如果Activiti被集成在某一系统当中的话，这些表可以不用，可以直接使用现有系统中的用户或组信息；  
u  ACT_RE_* : “RE”代表“Repository”（仓库），这些表中保存一些‘静态’信息，如流程定义和流程资源（如图片、规则等）；  
u  ACT_RU_* : “RU”代表“Runtime”（运行时），这些表中保存一些流程实例、用户任务、变量等的运行时数据。Activiti只保存流程实例在执行过程中的运行时数据，并且当流程结束后会立即移除这些数据，这是为了保证运行时表尽量的小并运行的足够快；  



# 2)springCloud相关文档  
# springCloud简介:
springcloud为开发人员提供了快速构建分布式系统的一些工具，包括配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策竞选、分布式会话等等。它运行环境简单，可以在开发人员的电脑上跑。  
## 服务的注册与发现（Eureka） 
eureka是一个高可用的组件，它没有后端缓存，每一个实例注册之后需要向注册中心发送心跳（因此可以在内存中完成）。  
当client向server注册时，它会提供一些元数据，例如主机和端口，URL，主页等。可以在server中对所有注册的client进行监控与管理，Eureka server 从每个client实例接收心跳消息。 如果心跳超时，则通常将该实例从注册server中删除。  
## 服务消费者（Feign）   
Feign是一个声明式的伪Http客户端，它使得写Http客户端变得更简单。使用Feign，只需要创建一个接口并注解。它具有可插拔的注解特性，可使用Feign 注解和JAX-RS注解。Feign支持可插拔的编码器和解码器。Feign默认集成了Ribbon，并和Eureka结合，默认实现了负载均衡的效果。
并且可以@FeignClient注解消费指定的模块或服务。  
## 断路器（Hystrix）
在微服务架构中，根据业务来拆分成一个个的服务，服务与服务之间可以相互调用（RPC），在Spring Cloud可以用RestTemplate+Ribbon和Feign来调用。为了保证其高可用，单个服务通常会集群部署。由于网络原因或者自身的原因，服务并不能保证100%可用，如果单个服务出现问题，调用这个服务就会出现线程阻塞，此时若有大量的请求涌入，Servlet容器的线程资源会被消耗完毕，导致服务瘫痪。服务与服务之间的依赖性，故障会传播，会对整个微服务系统造成灾难性的严重后果，这就是服务故障的“雪崩”效应。
较底层的服务如果出现故障，会导致连锁故障。当对特定的服务的调用的不可用达到一个阀值（Hystric 是5秒20次） 断路器将会被打开。断路打开后，可用避免连锁故障，fallback方法可以直接返回一个固定值。
## 路由网关(zuul)
在微服务架构中，需要几个基础的服务治理组件，包括服务注册与发现、服务消费、负载均衡、断路器、智能路由、配置管理等，由这几个基础组件相互协作，共同组建了一个简单的微服务系统。
Zuul的主要功能是路由转发和过滤器。路由功能是微服务的一部分，比如／api/user转发到到user服务，/api/shop转发到到shop服务。zuul默认和Ribbon结合实现了负载均衡的功能。


