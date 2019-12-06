---
id: docs_49
title: Bapp开发流程
sidebar_label: Bapp开发流程
---

##### 安装使用插件钱包

1. 打开Google浏览器的应用商店，搜索Bystore

![image.png](https://ibb.co/CvpVk0x)

下载链接：[http://t.cn/E6cFFwb](http://t.cn/E6cFFwb)


2. 然后点击添加到Chrome,就可以添加到我们的：

![image.png](https://ibb.co/C95MXZZ)


3. 使用google插件钱包

如果你使用的是测试网，可以去测试网水龙头领取BTM。
测试网水龙头：[https://bytom.io/zh/dev/](https://bytom.io/zh/dev/)


##### 搭建Dapp demo

Dapp demo是一个基于比原的储蓄合约，该demo可以进行资产的锁仓储蓄，到期返还资产并给一定的利息。这个dapp很适合的场景就是股息分红，内部通过智能合约自动锁仓操作，到期资产自动解锁。所以我个人对这个dapp应用场景表示非常看好。

项目源码地址：[https://github.com/Bytom/Bytom-Dapp-Demo](https://github.com/Bytom/Bytom-Dapp-Demo)

根据源码里面的readme.md文件进行搭建dapp,然后我们在本地打开[http://127.0.0.1:8080](http://127.0.0.1:8080) 后就可以看该dapp应用。然后我们点开我们的账户如下图：

![64.png](https://i.loli.net/2019/12/06/RUtzPhZESiQxHNM.png)

点击saving，我们看到的是储蓄资产界面，用户可以设置资产的金额，并储蓄资产

![image.png](https://ibb.co/5R6fPnw)

下图是我们收益的页面，我们可以看到自己储蓄的收益，如果是到期的话我们可以提出自己的收益。

![image.png](https://ibb.co/GcbZS1w)


##### Dapp调起Google插件的实现


###### 初始化注入

![image.png](https://ibb.co/yS1z4YQ)


###### 检查插件，账户

![image.png](https://ibb.co/Hq58Mxp)


###### 调交易接口

下图是发送交易的API接口，接口的具体文档参考:[https://github.com/Bytom/Bystore/wiki/API-reference](https://github.com/Bytom/Bystore/wiki/API-reference)。还有其他的API接口都在该文档里面。监听事件接口bytom.request(eventName, options)。

![image.png](https://ibb.co/HCz873c)


##### 后端服务器接口

由于比原链采用的UTXO模型，该模型没有状态。但是在开发dapp的过程中需要关联用户的的地址。所以后端服务器主要是封装一层类似账户模型，方便dapp跟链进行交互。开发者开发dapp可以搭建改项目作为与链交互的服务器，自己搭建参考项目的readme.

后端服务器项目地址：[https://github.com/oysheng/bufferserver](https://github.com/oysheng/bufferserver)


##### Dapp开发流程梳理

通过上面的一系列步骤，我们已经大概明白基于比原链开发dapp的一个大致流程。流程总结就是：


step1: 下载安装Chrome插件钱包，如果自己的dapp不需要跳过这一步。


step2: 如果需要自己搭建BlockCenter后端服务器，参考项目说明文件安装。不想搭建的话，直接用官方的服务，直接远程调用即可。

step3: 开发智能合约，并编译。然后将编译后的合约参数配置在dapp的配置文件，如下图：（全红部分是测试网合约配置参数）

![image.png](https://ibb.co/gmh6FMb)

step4: 调用Chrome插件钱包。

到此，在比原链上开发dapp的整套流程都已经梳理清楚，欢迎大家快速上手试试。开发出优秀的dapp应用。

Github： [https://github.com/bycoinio/Bystore/](https://github.com/bycoinio/Bystore/)
