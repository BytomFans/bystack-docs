---
id: docs_53
title:  GPU/ASIC
sidebar_label:  GPU/ASIC
---

**注意：** GPU Miner会提取一定比例的手续费

**Miner内核**

HSPMiner: [http://www.hspminer.com/](http://www.hspminer.com/)

BMiner: [https://www.bminer.me/](https://www.bminer.me/)

NBMiner: [https://github.com/NebuTech/NBMiner](https://github.com/NebuTech/NBMiner)


**HSPMiner**

```
HSPMiner.exe -bpool btm.matpool.io:8118 -bwal bm1qyfm5mw8k07g6wftfpx2fjksag50uzhh96wntpp -bworker default -bpsw passd -logfile -api 127.0.0.1:16666
```

##### 参数说明

**Bytom相关:**

    -bwal       挖Bytom钱包地址
    -bworker	挖Bytom旷工名称
    -bpool      挖Bytom矿池地址,可加`tls://pool_url:port`,启用TLS连接
    -bpsw       挖Bytom矿池密码
    -bdevice	挖Bytom启用的设备,默认为全部设备,可用-bdevice 0,2,4来限制只在GPU0,2,4上运行_

**运行相关:**

    -api        启用网络监控的地址,比如:192.168.1.2:16666,可用浏览器访问http://192.168.1.2:16666,监控矿机运行
    -logfile	启用log文件,默认为根据时间生成文件名称,后面可跟文件名称来指定文件,比如-logfile hspminer.log
    -hide       启动程序后立即隐藏界面,注意会在后台运行,如果开了api接口，可点击WebMonitor.cmd启动浏览器监控(Linux下不可用)


##### BMiner

用Bminer挖BTM的步骤，您需要在案例的基础上，对下面的字段做出调整：

- 用您的钱包地址替换bm1qyfm5mw8k07g6wftfpx2fjksag50uzhh96wntpp。
- 用您定义的矿工名字替换worker。
- 用您选择的矿池替换btm.matpool.io:8118。

```
bminer -uri tensority://bm1qyfm5mw8k07g6wftfpx2fjksag50uzhh96wntpp.worker@btm.matpool.io:8118
```
您可以用`tensority+ssl://`替换`tensority://`来通过SSL方式连接矿池(请注意端口的变化)。

```
bminer -uri tensority+ssl://bm1qyfm5mw8k07g6wftfpx2fjksag50uzhh96wntpp.worker@btm.f2pool.com:9443
```

文档详情：[https://www.bminer.me/zh/references/](https://www.bminer.me/zh/references/)

##### NBMiner

f2pool

```
nbminer -a tensority -o stratum+tcp://btm.f2pool.com:9221 -u bm1xxxxxxxxxx.worker
```

antpool
**

```
nbminer -a tensority -o stratum+tcp://stratum-btm.antpool.com:6666 -u username.worker
```

matpool.io

```
nbminer -a tensority -o stratum+tcp://btm.matpool.io:8118 -u bm1xxxxxxxxxxx.worker
```

文档详情：[命令行参数](https://github.com/NebuTech/NBMiner/blob/master/readme_zh.md#%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0)
