---
id: docs_31
title: CPU/ASIC
sidebar_label: CPU/ASIC
---

#### 使用命令行开启挖矿

**初始化**

初始化节点:

```
$ cd ./cmd/bytomd
$ ./bytomd init --chain_id mainnet
```

`--chain_id`三个选项:

- `mainnet`: 连接到主网
- `testnet`: 连接到测试网络
- `solonet`: 单机模式

**启动并挖矿**

```
$ ./bytomd node --mining
```
#### 使用API配置挖矿

**API**

默认的 JSON-RPC :

| Client | URL |
| --- | --- |
| Go | [http://localhost:9888](http://localhost:9888/) |

**set-mining**

开启节点挖矿

**参数**

`Object`:
- `Boolean` - _is_mining_, 节点是否在挖矿.


**例子**

```
// Request
curl -X POST set-mining -d '{"is_mining": true}'
// Result
```


**Dashboard**

你可以在dashboard看到挖矿结果

```
$ open http://localhost:9888/
```

