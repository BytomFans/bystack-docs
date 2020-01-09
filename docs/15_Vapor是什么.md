---
id: docs_15
title: Vapor是什么
sidebar_label: Vapor是什么
---

Vapor是平台最重要的区块链基础设施之一，采用创新的DPoS+[BBFT](http://localhost:3000/bystack-docs/docs/docs_22) 的共识算法，具有高性能、高安全、可扩展等特点，用于搭建规模化的商业应用。

**设计理念**

存储层去掉了传统公链本地KV数据库Leveldb， Rocksdb等，替换为更通用和强大的数据库层接口，满足使用高性能MySQL、PostgreSQL、MongoDB等企业级数据库。
对于数据分析的需求也同时兼容HDFS和HIVE，融入Hadoop或Spark生态系统。

Vapor的TPS超过10万，可以满足目前企业级服务平台的基本应用。平均出块速度约0.5秒，交易及区块大小经过压缩，从而减少带宽开支，让全节点可以更快的同步区块。

基于BBFT的共识技术提供高可用的拜占庭容错能力，支持共识状态自动恢复，区块数据互备恢复等。合约采用 [BUTXO](http://localhost:3000/bystack-docs/docs/docs_23) 模型，只能获取该合约锁定部分的资产，从而保障资产安全性。