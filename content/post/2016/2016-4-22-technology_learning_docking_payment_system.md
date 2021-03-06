---
categories:
- 技术学习
keywords:
- 支付系统
- 浅谈对接支付系统
tags:
- 支付系统
date: 2016-04-22T19:27:15+08:00
title: 浅谈对接支付系统 
url: 2016/04/22/technology_learning_docking_payment_system/
---

#### 前言
>工作中平台需要对接不同厂家的支付系统，以自己喜欢的方式整理好自己对，对接支付系统的理解，并形成相应的文档，在这里分享给大家，也便于自己日后继续学习。

##### 一、支付系统交易方式
支付系统的交易方式分为以下两种：

1. 同步交易
2. 异步交易

###### 1、同步交易
客户端发起交易请求，服务端即时响应，并返回响应的结果。

**优点：**

* 实时性强
* 易于理解、开发和对接

**缺点：**

* 实现的要求高（对网络、软件等要求高，不太现实）
* 容错性差
* 不易于维护
* 并发能力不足

###### 2、异步交易
客户端发起交易请求，服务端即时响应其请求的是否成功，至于交易处理结果通过客户端请求时发送的URL进行规则性通知，从而将响应的结果通知到客户端。

**优点：**

* 对网络、软件等要求相对同步交易要低不少
* 容错性高
* 开发灵活
* 易于维护
* 便于开发出并发能力好的软件

**缺点：**

* 实时性相比同步交易来说要差一些
* 开发难度较高

##### 二、对接支付系统
###### 1、对接支付系统的简化图
<img width="72%"  src="http://7xnflt.com1.z0.glb.clouddn.com/image%2Fblog%2Ftechnology%2Ftechnology_learning_docking_payment_system1.jpg"/>

###### 2、对接支付系统的基本策略
**基本策略：**

* 金融性交易采用异步交易方式
* 非金融性交易采用同步交易方式

**金融性交易**：

* 简单的理解就是和钱相关的交易（如支付）

**非金融性交易**：

* 简单的理解就是和钱没有直接关系的交易（如实名认证）

###### 3、对接支付系统过程
交易请求，存在以下三种状态：

* 成功
* 失败
* 未知（un-know）状态

通过API对接第三方支付系统，交易请求时，根据第三方支付系统返回状态进行相应处理：

* 返回成功状态：平台处理交易请求成功的业务逻辑。
* 返回失败状态：平台处理交易请求失败的业务逻辑。
* 返回未知（un-know）状态：平台可以通过交易订单号，主动通过API查询第三方支付系统中该交易的状态，可以制定主动查询的规则（如每隔一段时间，查询一次，如果还是未知状态，则进行每隔一段时间查询，直到超过平台规定的最大查询次数，平台再默认将其设置为失败状态[查询规则如： 最大查询次数为3次，第一次延迟1分钟查询，第二次延迟3分钟再查询，第三次延迟5分钟查询]）。

如果第三方支付系统既提供同步交易又提供异步交易接口，我们可以按照对接支付系统的基本策略（金融性交易采用异步交易方式，非金融性交易采用同步交易方式）去实现。

如果第三方支付系统只提供同步交易的话，对于金融性交易我们需要自己模拟实现异步交易的方式（先使用同步交易方式请求，如果结果为未知状态，主动通过API查询第三方支付系统中该交易的状态，再根据返回状态处理相应的业务逻辑。可以根据平台的自身业务制定根据查询规则处理该交易）。

##### 三、对支付系统的期待
第三方支付系统对接API没有统一的标准，差异化大。有些支付系统在金融性交易API中没有提供异步交易的方式，导致对接开发难度大。

**对支付系统的期待**：

* 形成统一的API标准，各厂商的API差异化尽可能的小
* 第三方支付系统对接API既提供同步交易的方式又提供异步交易的方式，方便别的平台灵活对接

##### 后续
>如果你有更多更好的**对接支付系统**方法和经验，可以通过发邮件至zhaokaiju@foxmail.com或者直接关注我微信公众号（**zhaokaiju**）分享给我，谢谢! 