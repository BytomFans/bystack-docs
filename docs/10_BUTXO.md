---
id: docs_10
title: BUTXO
sidebar_label: BUTXO
---

`Bytom`采用`BUTXO`结构，区块链上记录着由多种不同类型的`UTXO`构成的账本。每一笔`UTXO`都有两个重要属性：资产编号`assetID`和资产数量`amount`，一般将指定数量`amount`的资产`assetID`抽象地指代一笔`UTXO`。`BUTXO` 继承了比特币中的 UTXO 模型，具有无状态的特性，故而天然支持并发。`BUTXO` 在比特币中的 UTXO 模型的基础上添加多资产支持。

`BUTXO` 的数据结构如下：

    type UTXO struct {
	   OutputID                       hash
	   SourceID                       hash
	   AssetID                        hash
	   Amount                         integer
	   SourcePos                      integer
	   ControlProgram                 []byte
	   AccountID                      string
	   Address                        string
	   ControlProgramIndex            integer
	   ValidHeight                    integer
 	   Change                          bool
    }

`ControlProgram` 的结构：参见 “智能合约” 章节. 
