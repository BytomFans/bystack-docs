---
id: docs_46
title: BApp SDK初始化
sidebar_label: BApp SDK初始化
---

#### 介绍

本文档旨在帮助基于比原的分布式应用(Bapp)开发者使用Bapp SDK

一般来说，dapp需要一个主机环境和用户的钱包来进行交互。就像byone chrome扩展一样，bycoin在bycoin钱包应用程序中提供了这种环境。

bycoin的dapp浏览器可以让你的dapp以更复杂的方式进行交互，比使用byone chrome扩展等其他工具更为复杂。

#### 删除Bycoin同时bytom依赖注入

为了使用bycoin和bytom api调用，必须将eventlistener添加到文档中。

```javascript
document.addEventListener('chromeBytomLoaded', function () {
  //detect and use the relevant window.bytom and window.bycoin.
 })
```

#### 访问账户的信息

当您准备请求用户访问帐户信息时，可以调用以下简单方法：

```javascript
bytom.enable()
```

此承诺返回函数与当前帐户信息一起解析，在发送交易时可以用作一般帐户引用。

随着时间的推移，此方法将逐渐发展为包含各种附加参数，以帮助您的站点在安装过程中向用户请求所需的所有安装。

因为它返回一个promise，所以如果您使用的是异步函数，您可以这样登录：

```javascript
const default_ account = await bytom.enable()
```

