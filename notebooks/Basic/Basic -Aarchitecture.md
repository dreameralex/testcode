# 微服务

但通常在其而言，微服务架构是一种架构模式或者说是一种架构风格，它提倡将单一应用程序划分成一组小的服务，每个服务运行独立的自己的进程中，服务之间互相协调、互相配合，为用户提供最终价值。  
服务之间采用轻量级的通信机制互相沟通（通常是基于 HTTP 的 RESTful API ) 。每个服务都围绕着具体业务进行构建，并且能够被独立地部署到生产环境、类生产环境等。  
另外，应尽量避免统一的、集中式的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言、工具对其进行构建，可以有一个非常轻量级的集中式管理来协调这些服务。可以使用不同的语言来编写服务，也可以使用不同的数据存储。

## **2.微服务架构带来的运维挑战**

（1）单服务流量激增时扩容  
（2）调用链条变长，调用关系更加复杂  
（3）微服务拆分导致故障点增多

---

（1）单服务变更性能影响如何评估？  
（2）性能瓶颈在各微服务间漂移，如何做好性能测试？  
（3）应对突发流量需求，扩容能否解决问题，如何扩容？  
（4）服务实例数量众多，如何收集信息，快速定位性能问题？





## **定位问题 - 链路跟踪**

在微服务架构下，一个用户的请求往往涉及多个内部服务调用。为了方便定位问题，需要能够记录每个用户请求时，微服务内部产生了多少服务调用，及其调用关系。这个叫做链路跟踪。

## **分析问题 - 日志分析**

日志分析组件应该在微服务兴起之前就被广泛使用了。即使单体应用架构，当访问数变大、或服务器规模增多时，日志文件的大小会膨胀到难以用文本编辑器进行访问，更糟的是它们分散在多台服务器上面。排查一个问题，需要登录到各台服务器去获取日志文件，一个一个地查找（而且打开、查找都很慢）想要的日志信息。

<img src="https://pic1.zhimg.com/v2-b721b26bd974169cffd107ba4eec54f0_r.jpg?source=1940ef5c" title="" alt="" width="387">



## **网关 - 权限控制，服务治理**

拆分成微服务后，出现大量的服务，大量的接口，使得整个调用关系乱糟糟的。经常在开发过程中，写着写着，忽然想不起某个数据应该调用哪个服务。或者写歪了，调用了不该调用的服务，本来一个只读的功能结果修改了数据……

为了应对这些情况，微服务的调用需要一个把关的东西，也就是网关。在调用者和被调用者中间加一层网关，每次调用时进行权限校验。另外，网关也可以作为一个提供服务接口文档的平台。

使用网关有一个问题就是要决定在多大粒度上使用：最粗粒度的方案是整个微服务一个网关，微服务外部通过网关访问微服务，微服务内部则直接调用；最细粒度则是所有调用，不管是微服务内部调用或者来自外部的调用，都必须通过网关。折中的方案是按照业务领域将微服务分成几个区，区内直接调用，区间通过网关调用。

<img src="https://pic1.zhimg.com/v2-84a6eee84a5dc3f2e97f56b4148bbc66_r.jpg?source=1940ef5c" title="" alt="" width="353">
