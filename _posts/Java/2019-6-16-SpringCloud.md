---
layout: post
title: SpringCloud
categories: Java
description: 微服务架构
keywords: 微服务架构 SpringCloud
---



# 单体架构
单体架构也称之为单体系统或者是单体应用。就是一种把系统中所有的功能、模块耦合 在一个应用中的架构方式

特点
- 打包成一个独立的单元(导成一个唯一的jar包或者是war包)
- 会一个进程的方式来运行

单体架构的优点
- 项目易于管理
- 部署简单

单体架构的缺点
- 测试成本高
- 可伸缩性差
- 可靠性差
- 迭代困难
- 跨语言程度差
- 团队协作难


# 架构

架构风格：项目的一种设计模式



常见的架构风格
- 客户端与服务端的
- 基于组件模型的架构（EJB）
- 分层架构(MVC)
- 面向服务架构(SOA)

![project architecture](/images/java/project architecture.png)

## MVC架构
其实 MVC 架构就是一个单体架构。 代表技术：Struts2、SpringMVC、Spring、Mybatis 等等。

## RPC架构
RPC(RemoteProcedureCall)：远程过程调用。一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议。代表技术：Thrift、Hessian 等等

## SOA架构
SOA(ServiceorientedArchitecture):面向服务架构 
ESB(Enterparise Servce Bus):企业服务总线，服务中介。主要是提供了一个服务于服务之间的交互。ESB包含的功能如：负载均衡，流量控制，加密处理，服务的监控，异常处理，监控告急等等。 
代表技术：Mule、WSO2

## 微服务架构

微服务是一种架构风格。一个大型的复杂软件应用，由一个或多个微服务组成。系统中的各个微服务可被独立部署，各个微服务之间是松耦合的。每个微服务仅关注于完成一件任务并很好的完成该任务。

微服务就是一个轻量级的服务治理方案。 代表技术：SpringCloud、dubbo 等等

微服务特点：
- 系统是由多个服务构成
- 每个服务可以单独独立部署
- 每个服务之间是松耦合的。服务内部是高内聚的，外部是低耦合的。高内聚就是每个服务只关注完成一个功能

微服务的优点
- 测试容易
- 可伸缩性强
- 可靠性强
- 跨语言程度会更加灵活
- 团队协作容易
- 系统迭代容易

微服务的缺点
- 运维成本过高，部署数量较多
- 接口兼容多版本
- 分布式系统的复杂性
- 分布式事务

### 微服务设计原则

#### AKF拆分原则


#### 前后端分离原则

#### 无状态服务

#### RestFul的通信风格

# SpringCloud

SpringCloud：是一个服务治理平台，提供了一些服务框架。包含了：服务注册与发现、配置中心、消息中心、负载均衡、数据监控等等。

Spring Cloud 是一个微服务框架，相比 Dubbo 等 RPC 框架, Spring Cloud 提 供的全套的分布式系统解决方案。

Spring Cloud 对微服务基础框架 Netflix 的多个开源组件进行了封装，同时又实现 了和云端平台以及和 Spring Boot 开发框架的集成。SpringCloud为微服务架构开发涉及的配置管理，服务治理，熔断机制，智能路由，微代理，控制总线，一次性token，全局一致性锁，leader 选举，分布式 session，集 群状态管理等操作提供了一种简单的开发方式。

SpringCloud为开发者提供了快速构建分布式系统的工具，开发者可以快速的启动服务或构建应用、同时能够快速和云平台资源进行对接。

## SpringCloud的子项目
### Spring Cloud Config
配置管理工具，支持使用Git存储配置内容，支持应用配置的外部化存储，支持客户端配置信息刷新、加解密配置内容等
### Spring Cloud Bus
事件、消息总线，用于在集群（例如，配置变化事件）中 传播状态变化，可与 Spring Cloud Config 联合实现热部署。
### Spring Cloud Netflix
针对多种 Netflix 组件提供的开发工具包，其中包括 Eureka、Hystrix、Zuul、Archaius等。
- Netflix Eureka：一个基于rest服务的服务治理组件，包括服务注册中心、服务注册与服务发现机制的实现，实现了云端负载均衡和中间层服务器 的故障转移。
- NetflixHystrix：容错管理工具，实现断路器模式，通过控制服务的节点,
从而对延迟和故障提供更强大的容错能力。
- Netflix Ribbon：客户端负载均衡的服务调用组件。
- Netflix Feign：基于 Ribbon 和 Hystrix 的声明式服务调用组件。
- Netflix Zuul：微服务网关，提供动态路由，访问过滤等服务。
- Netflix Archaius：配置管理API，包含一系列配置管理API，提供动态类型化属性、线程安全配置操作、轮询框架、回调机制等功能。 

### Spring Cloud for Cloud Foundry
通过Oauth2协议绑定服务到CloudFoundry，CloudFoundry 是VMware推出的开源PaaS云平台。
### SpringCloud Sleuth
日志收集工具包，封装了 Dapper,Zipkin 和 HTrace操作。
### Spring Cloud Data Flow
大数据操作工具，通过命令行方式操作数据流。
### Spring Cloud Security
安全工具包，为你的应用程序添加安全控制，主要是指OAuth2。
### Spring Cloud Consul
封装了 Consul 操作，consul 是一个服务发现与配置工具，与Docker容器可以无缝集成。
### Spring Cloud Zookeeper 
操作Zookeeper的工具包，用于使用zookeeper方式的服务注册和发现。
### Spring Cloud Stream
数据流操作开发包，封装了与 Redis,Rabbit、 Kafka 等发送接收消息。
### Spring Cloud CLI
基于 Spring Boot CLI，可以让你以命令行方式快速建立云组件。
