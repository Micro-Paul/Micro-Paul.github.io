---
title: spring-boot2
date: 2019-03-28 14:04:19
tags: springboot
categories: 语言
 - Spring
---
使用springboot作为微服务开发框架，可以很好地提高开发的效率和保证微服务开发的质量，springboot中嵌入了tomcat,可直接运行一个jar包来发布微服务，此外还提供了一系列“开箱即用”的插件，可大大提高开发的效率，同时也可以去扩展更多的插件。
### 微服务技术选型
在发布微服务时，可连接zookeeper来注册微服务，实现“服务注册”。实际上zookeeper中有一个名为ZNode的内存树状模型，树上的节点用于存放微服务的配置信息。使用Node.js处理浏览器的请求，在Node.js中连接zookeeper，发现服务配置，实现“服务发现”，有大量的Node.js的zookeeper客户端可以完成这个任务。   
通过Node.js将请求转发到tomcat上，实现“反向代理”，同样也有大量的Node.js库可供我们自由选择。Node.js的“单线程模型”且“非阻塞异步式I/O”特性，通过“事件循环”的方式来支撑大量的高并发请求，此外Node.js原生也提供了集群特性，可确保高可用性。  
为了实现微服务自动化部署，我们可通过Jenkins搭建自动化部署系统，并使用Docker将服务进行容器化封装。   
综上所述，微服务架构技术选型如下所示。
Springboot：<https://spring.io/projects/spring-boot/>
ZooKeeper: <http://zookeeper.apache.org/>
Node.js: <https://nodejs.org/>
Jenkins: <https://jenkins.io/>
Docker: <https://www.docker.com/>
下面通过一张关系图来归纳一下微服务架构技术选型，如下图所示：
![微服务架构选型.png](/uploads/images/微服务架构选型.png "微服务架构选型")  
1. 使用Jenkins部署服务
2. 使用springboot开发服务
3. 使用Docker封装服务
4. 使用ZooKeeper注册服务
5. 使用Node.js调用服务
