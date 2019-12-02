---
id: docs_34
title: BlockCenter-SDK
sidebar_label: BlockCenter-SDK
---

**SDK作用**

blockcenter的提供的接口均需要密钥对，新建钱包需要公钥，构建交易签名需要私钥，SDK提供了生成密钥对的具体方法。

bytom和vapor均可使用以下的SDK

**java版本：** [github](https://github.com/Bytom/bytom-java-sdk)

**生成根私钥**

`tx-signer/src/main/java/io/bytom/offline/common/ExpandedPrivateKey.java`

```java
public static byte[] expandedPrivateKey(byte[] data)
```

**data**: 可以是助记词hash之后的数据；

**生成根公钥**

`tx-signer/src/main/java/io/bytom/offline/common/DeriveXpub.java`

```java
public static byte[] deriveXpub(byte[] xprv)
```

**xprv**: 是步骤1中返回的根私钥；

**签名交易数据**

a. 派生子私钥

`tx-signer/src/main/java/io/bytom/offline/common/NonHardenedChild.java`

```java
public static byte[] child(byte[] xprv, String[] hpaths)
```

**xprv**: 是步骤1中返回的根私钥；

**hpaths**: 调用API中构建交易接口（包含单签和多签）之后返回数据中的`derivation_path`字段

b. 派生子私钥签名

`tx-signer/src/main/java/io/bytom/offline/common/Signer.java`

```java
public static byte[] ed25519InnerSign(byte[] privateKey, byte[] message)
```

**privateKey**: 是步骤a中返回的子私钥；

**message**: 调用API中构建交易接口（包含单签和多签）之后返回数据中的`raw_transaction`字段

