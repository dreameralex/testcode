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