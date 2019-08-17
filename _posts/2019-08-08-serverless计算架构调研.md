---
layout:     post
title:      "Serverless计算架构调研"
# subtitle:   ""
date:       2019-08-08
author:     "Paul"
header-img: "img/post-bg-2019.jpg"
tags:
    - 技术
---

1. Serverless 计算架构是什么
   - 函数级云服务：Serverless 计算是一种通用的函数级云服务。目前通用的云计算服务一般提供硬件级抽象（IaaS）、操作系统级抽象（PaaS）或是应用软件级抽象（SaaS）。而 Serverless 计算则将计算服务抽象到函数级，提供函数服务。
   
   - 业务开发者的阿拉丁神灯：我把 Serverless 计算比喻为业务开发者的阿拉丁神灯（ 待商榷 :) :) ），因为开发者只需要编写核心业务代码，并在 Serverless 平台上以函数配合（平台是否提供一些常用函数暂不清楚，但是根据具体业务编写自定义函数这一点我是确定的）。在 Serverless 上定义和编写的函数又可以调用平台提供的丰富的后端功能如存储服务、云应用市场等。本质上，Serverless 计算提供的是一种更彻底的外包解决方案：服务器、数据库、应用程序、甚至是逻辑外包。
   
   - 事件驱动：Serverless 计算提供多种触发器，由事件触发的设计有益于实现服务性质的系统（因为服务一般都是源自请求等事件的触发）。目前市场上的Serverless或函数计算服务通常提供如下触发器：HTTP请求触发、数据库变更触发、定时触发、API网关触发、消息队列触发。
   
   - Server != No server：产生这种观点的原因主要是因为在Serverless提供的服务中，开发人员不需要关心服务器的环境搭建、性能、运维等等。负载均衡由云平台负责，函数服务在每次触发调用后，可能被路由到不同的容器中执行。并发量高的请求，Serverless平台会自动扩张计算资源，请求后自动降低计算并发数。
   
   - 使管理运维对开发者透明
     
      - 友好的服务器环境配置。
   - 可扩展性（资源、容量分配）—— Serverless提供弹性资源扩缩。
   - 维护操作——Serverless提供高可用服务，同时由Serverless方面提供运维。
     
      - 解放了运维人员的大量工作，降低开发成本，提高开发效率。
      
   - 对比 Micro-Service (一种角度)：目前微服务架构使用 BFF (Backend For Frontend)，BFF将后端的微服务进行适配，将后端接口通过聚合裁剪和格式适配等逻辑，向客户端（前端）提供友好、统一的API，便于用户访问；在BFF处将承担较大并发压力，而Serverless服务完全是分布式的，无瓶颈。缓解了微服务开发架构中BFF层的压力，从而承担更大流量的业务。
     ![传统 C/S 架构设计模式](https://raw.githubusercontent.com/Paul-HIT/my-pictures/master/c-s.png)
     
     <center>C/S架构</center>
   ![Serverless架构](https://raw.githubusercontent.com/Paul-HIT/my-pictures/master/serverless.png)
   <center>Serverless架构</center>
   - 在传统的 C/S 架构的设计模式下，需要安排后台开发人员搭建业务服务器环境，配置数据库等等；但是在Serverless架构下，开发人员只需要编写函数并设置触发即可。服务器环境以及通用服务如认证服务、数据库等都由平台提供，简化开发任务。
   
   
   
   ![Serverless物联网应用场景](https://raw.githubusercontent.com/Paul-HIT/my-pictures/master/iot.png)
   <center>物联网场景应用举例</center>
   - 用Serverless服务在物联网场景的应用举例：
     - 只有灰色矩形框内的部分是我们需要编写的函数和配置的触发器，深绿色是触发器，(浅) 绿色是函数。
     - 对于一个需要更换过滤器的物联网冰箱，冰箱检测到过滤器需要更换，发送一个请求到云端。
     - 该请求触发分析函数，理解到是一个更换过滤器的请求，则更新 service 数据库。
     - 数据库变更激活触发器，触发器触发了创建订单的函数，函数创建订单包括过滤器型号、收货地址等等，并把订单信息写入 order 数据库。
     - order 数据库变更触发通知函数，通知函数以邮件或短信的方式通知用户，用户根据订单选择手动下单或者配置自动下单来购进新的过滤器。
2. Serverless 计算的优势
   - 无需管理服务器：无需了解和管理运维服务器、运行环境，只需编写代码并设置运行条件即可以安全可靠的方式运行。
   - 资源弹性伸缩：函数计算会根据客户的请求动态的伸缩运行环境，会根据运行时候需要的资源来动态调整。
   - 事件触发执行：客户以简单的时间来触发函数运行，例如云存储中各种事件，消息通道的信息以及各种监控事件。
   - 不执行不付费：客户无需为闲置资源付费，无需为计算和存储等资源预置或超额预置资源。代码不运行，不付费。
   
3. 商业 Serverless 平台
  
   各大商业云计算平台目前的 Serverless 计算服务没有形成统一的标准，因此目前在不同的 Serverless 平台上迁移项目可能需要大量修改代码。而且云服务商都在围绕 Serverless 服务打造自己的生态服务。使用商业 Serverless 平台的服务，往往会依赖更多的云平台提供的服务。
   
   - AWS Lambda
     - 第一个 Serverless 计算平台
     - 优势：无需管理服务器，持续扩展，次秒级计量
   - 阿里云 Serverless 服务
     - Serverless 生态：函数计算，对象存储，API网关，表格存储，日志服务
     - 特点：事件触发，完备的监控和日志，细粒度计量收费，支持多种编程语言
   - Apache Openwhisk
     - 开源的Serverless框架，搭建自有Serverless平台的可选框架
     - IBM Cloud Functions 基于 OpenWhisk 搭建
   - Google Cloud Functions, Azure Functions,  华为，腾讯 ...
4. 应用场景

   - [这里以腾讯云 Serverless 服务提供的应用场景介绍为例](https://cloud.tencent.com/product/scf)