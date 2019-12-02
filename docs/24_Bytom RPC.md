---
id: docs_24
title: Bytom RPC
sidebar_label: Bytom RPC
---

**API 调用点**

默认 JSON-RPC 调用点:

| Client | URL |
| :---: | :---: |
| Go | [http://localhost:9888](http://localhost:9888) |

一个完整的`curl`例子:

**Go**

```javascript
// curl -X POST url/method -d data
curl -X POST http://localhost:9888/create-key -d '{"alias": "alice", "password": "123456"}'
```
**API 方法**

#### 不论钱包是否打开都可用的方法

- [`create-key`](#create-key)
- [`list-keys`](#list-keys)
- [`update-key-alias`](#update-key-alias)
- [`delete-key`](#delete-key)
- [`check-key-password`](#check-key-password)
- [`reset-key-password`](#reset-key-password)
- [`create-account`](#create-account)
- [`list-accounts`](#list-accounts)
- [`update-account-alias`](#update-account-alias)
- [`delete-account`](#delete-account)
- [`create-account-receiver`](#create-account-receiver)
- [`list-addresses`](#list-addresses)
- [`validate-address`](#validate-address)
- [`get-mining-address`](#get-mining-address)
- [`set-mining-address`](#set-mining-address)
- [`get-coinbase-arbitrary`](#get-coinbase-arbitrary)
- [`set-coinbase-arbitrary`](#set-coinbase-arbitrary)
- [`list-pubkeys`](#list-pubkeys)
- [`create-asset`](#create-asset)
- [`get-asset`](#get-asset)
- [`list-assets`](#list-assets)
- [`update-asset-alias`](#update-asset-alias)
- [`list-balances`](#list-balances)
- [`list-unspent-outputs`](#list-unspent-outputs)
- [`backup-wallet`](#backup-wallet)
- [`restore-wallet`](#restore-wallet)
- [`rescan-wallet`](#rescan-wallet)
- [`recovery-wallet`](#recovery-wallet)
- [`wallet-info`](#wallet-info)
- [`sign-message`](#sign-message)
- [`decode-program`](#decode-program)
- [`get-transaction`](#get-transaction)
- [`list-transactions`](#list-transactions)
- [`build-transaction`](#build-transaction)
- [`build-chain-transactions`](#build-chain-transactions)
- [`sign-transaction`](#sign-transaction)
- [`sign-transactions`](#sign-transactions)

#### 钱包必须打开时的可用方法

- [`submit-transaction`](#submit-transaction)
- [`submit-transactions`](#submit-transactions)
- [`estimate-transaction-gas`](#estimate-transaction-gas)
- [`create-access-token`](#create-access-token)
- [`list-access-tokens`](#list-access-tokens)
- [`delete-access-token`](#delete-access-token)
- [`check-access-token`](#check-access-token)
- [`create-transaction-feed`](#create-transaction-feed)
- [`get-transaction-feed`](#get-transaction-feed)
- [`list-transaction-feeds`](#list-transaction-feeds)
- [`delete-transaction-feed`](#delete-transaction-feed)
- [`update-transaction-feed`](#update-transaction-feed)
- [`get-unconfirmed-transaction`](#get-unconfirmed-transaction)
- [`list-unconfirmed-transactions`](#list-unconfirmed-transactions)
- [`decode-raw-transaction`](#decode-raw-transaction)
- [`get-block-count`](#get-block-count)
- [`get-block-hash`](#get-block-hash)
- [`get-block`](#get-block)
- [`get-block-header`](#get-block-header)
- [`get-difficulty`](#get-difficulty)
- [`get-hash-rate`](#get-hash-rate)
- [`net-info`](#net-info)
- [`is-mining`](#is-mining)
- [`set-mining`](#set-mining)
- [`gas-rate`](#gas-rate)
- [`verify-message`](#verify-message)
- [`compile`](#compile)
- [`list-peers`](#list-peers)
- [`disconnect-peer`](#disconnect-peer)
- [`connect-peer`](#connect-peer)
- [`get-work`](#get-work)
- [`submit-work`](#submit-work)
- [`get-work-json`](#get-work-json)
- [`submit-work-json`](#submit-work-json)

---

#### create-key

它本质上是创建私钥，返回密钥的信息。私钥在文件中加密，对用户不可见。

**参数**

`Object:`

- `String` - _alias_,密钥名字.
- `String` - _password_, 私钥的密码.

**返回**

`Object`:

- `String` - _alias_, ，密钥的名字.
- `String` - _xpub_, 公钥.
- `String` - _file_, 私钥文件本地存放路径.

**例子**

```javascript
// Request
curl -X POST create-key -d '{"alias": "alice", "password": "123456"}'

// Result
{
  "alias": "alice",
  "xpub": "a7dae957c2d35b42efe7e6871cf5a75ebd2a0d0e51caffe767db42d3e6d69dbe211d1ca492ecf05908fe6fa625ad61b3253375ea744c9442dd5551613ba50aea",
  "file": "/Path/To/Library/Bytom/keystore/UTC--2018-04-22T06-30-27.609315219Z--0e34293c-8856-4f5f-b934-37456a3820fa"
}
```

#### list-keys

返回所有可用键的列表。

**参数**

none

**返回**

- `Array of Object`, 客户端所拥有的所有私钥信息.
  - `Object`:
    - `String` - _alias_, 密钥名字.
    - `String` - _xpub_, 公钥文件.
    - `String` - _file_, 密钥文件在客户端本地存放地址

**例子**

```javascript
// Request
curl -X POST list-keys

// Result
[
  {
    "alias": "alice",
    "xpub": "a7dae957c2d35b42efe7e6871cf5a75ebd2a0d0e51caffe767db42d3e6d69dbe211d1ca492ecf05908fe6fa625ad61b3253375ea744c9442dd5551613ba50aea",
    "file": "/Path/To/Library/Bytom/keystore/UTC--2018-04-21T02-35-15.035935116Z--4f2b8bd7-0576-4b82-8941-6cc6da05efe3"
  },
  {
    "alias": "bob",
    "xpub": "d30a810e88532f73816b7b5007d413cbd21e526ae9159023e5262511893adc1526b8eacd691b27c080201d7d79336a4f3d2cb4c167d997821cad445765916254",
    "file": "/Path/To/Library/Bytom/keystore/UTC--2018-04-22T06-30-27.609315219Z--0e34293c-8856-4f5f-b934-37456a3820fa"
  }
]
```

#### delete-key

删除现有密钥，请确保相关帐户中没有余额。

**参数**

`Object`:

- `String` - _xpub_, 公钥.
- `String` - _password_, 密钥的密码.

**返回**

如果密钥已成功删除，则为none。

否则：

**例子**

```javascript
// Request
curl -X POST delete-key -d '{"xpub": "a7dae957c2d35b42efe7e6871cf5a75ebd2a0d0e51caffe767db42d3e6d69dbe211d1ca492ecf05908fe6fa625ad61b3253375ea744c9442dd5551613ba50aea", "password": "123456"}'

// Result
```

#### reset-key-password

重置密钥密码。

**参数**

`Object`:

- `String` - _xpub_, 密钥的公钥.
- `String` - _old_password_, 密钥文件旧的密码.
- `String` - _new_password_, 密钥文件新的密码.

**返回**

`Object`:

- `Boolean` - _changed_, 返回重置的密钥密码，如果返回true表示返回成功，否则重置失败.

**例子**

```javascript
// Request
curl -X POST reset-key-password -d '{"xpub": "a7dae957c2d35b42efe7e6871cf5a75ebd2a0d0e51caffe767db42d3e6d69dbe211d1ca492ecf05908fe6fa625ad61b3253375ea744c9442dd5551613ba50aea", "old_password": "123456", "new_password": "654321"}'

// Result
{
  "changed": true
}
```

#### create-account

创建帐户以管理地址。单一标志帐户只包含一个root_xpubs和quorum;但是多符号帐户包含许多root_xpubs和quorum，quorum是验证签名的数量，范围是 [1, len(root_xpubs)]。

**参数**

`Object`:

- `Array of String` - _root_xpubs_, 公钥数组.
- `String` - _alias_, 账户名称.
- `Integer` - _quorum_, 默认值是`1`, 交易必需签名用来支出由账户控制的资产单位的秘钥阀值.

Optional:

- `String` - _access_token_, 如果在本地创建帐户时是可选的. 但是，如果您想远程创建帐户，它是必不可少的.

**返回**

`Object`:

- `String` - _id_, 账户id.
- `String` - _alias_, 账户名称.
- `Integer` - _key_index_, 账户key的索引.
- `Integer` - _quorom_,  交易必需签名用来支出由账户控制的资产单位的秘钥阀值.
- `Array of Object` - _xpubs_, 公钥数组.

**例子**

```javascript
// Request
curl -X POST create-account -d '{"root_xpubs":["2d6c07cb1ff7800b0793e300cd62b6ec5c0943d308799427615be451ef09c0304bee5dd492c6b13aaa854d303dc4f1dcb229f9578786e19c52d860803efa3b9a"],"quorum":1,"alias":"alice"}'

// Result
{
  "alias": "alice",
  "id": "08FO663C00A02",
  "key_index": 1,
  "quorum": 1,
  "xpubs": [
    "2d6c07cb1ff7800b0793e300cd62b6ec5c0943d308799427615be451ef09c0304bee5dd492c6b13aaa854d303dc4f1dcb229f9578786e19c52d860803efa3b9a"
  ]
}
```

#### list-accounts

返回所有可用帐户的列表。

**参数**

none

**返回**

- `Array of Object`, 账户数组.
  - `Object`:
    - `String` - _id_, 账户id.
    - `String` - _alias_, 公钥名称.
    - `Integer` - _key_index_, 账户密钥的索引.
    - `Integer` - _quorom_,  交易必需签名用来支出由账户控制的资产单位的秘钥阀值.
    - `Array of Object` - _xpubs_, 公钥数组.

**例子**

列出所有可用帐户:

```javascript
// Request
curl -X POST list-accounts -d {}

// Result
[
  {
    "alias": "alice",
    "id": "086KQD75G0A02",
    "key_index": 1,
    "quorum": 1,
    "xpubs": [
      "180aab8bf247932a7cf68da5cc9a873266279155097612f1e5fdda4add88d5e91e2e7ce5b736f3ac933824cdee9effcf1531b90dfcb388e5cc306d14e9a2c85e"
    ]
  },
  {
    "alias": "bob",
    "id": "086KQO67G0A04",
    "key_index": 2,
    "quorum": 1,
    "xpubs": [
      "180aab8bf247932a7cf68da5cc9a873266279155097612f1e5fdda4add88d5e91e2e7ce5b736f3ac933824cdee9effcf1531b90dfcb388e5cc306d14e9a2c85e"
    ]
  }
]
```

#### update-account-alias

按account id或account alias更新账户别名。

**参数**

`Object`:

- `String` - _account_id_, 账户id.
- `String` - _account_alias_, 账户别名.
- `String` - _new_alias_, 账户的新别名.

**返回**

如果账户别名更新成功，status为success。

**例子**

更新账户别名：

```javascript
// Request
curl -X POST update-account-alias -d '{"account_id":"0JA5BOS300A02","new_alias":"saber"}'

// Result
{"status":"success"}
```

#### delete-account

删除现有帐户，请确保相关地址中没有余额。

**参数**

`Object`:

- `String` - _account_info_, 账户的别名或者ID.

**返回**

如果帐户已成功删除，则为none。

**例子**

```javascript
// Request
curl -X POST delete-account -d '{"account_info": "alice"}'
or
curl -X POST delete-account -d '{"account_info": "08FO663C00A02"}'

// Result
```

#### create-account-receiver

创建地址和控制程序，地址和控制程序是一对一的关系。在构建事务API中，接收器是动作类型为control_address时的地址，接收器是动作类型为control_program时的控制程序，它们是相同的结果。

**参数**

`Object`: account_alias | account_id

可选:

- `String` - _account_alias_, 账户别名.
- `String` - _account_id_, 账户ID.

**返回**

`Object`:

- `String` - _address_, 账户地址.
- `String` - _control_program_,账户的控制程序.

**例子**

//Request

```bash
curl -X POST create-account-receiver -d '{"account_alias": "alice", "account_id": "0BDQARM800A02"}'
```

// Result

```json
{
    "address": "bm1q5u8u4eldhjf3lvnkmyl78jj8a75neuryzlknk0",
    "control_program": "0014a70fcae7edbc931fb276d93fe3ca47efa93cf064"
}
```

#### list-addresses

按帐户返回所有可用地址的列表。

**参数**

- `String` - _account_alias_, 账户别名.
- `String` - _account_id_, 账户id.

**返回**

- `Array of Object`, 账户所有地址的集合.
  - `Object`:
    - `String` - _account_alias_, 账户别名.
    - `String` - _account_id_, 账户id.
    - `String` - _address_, 账户地址.
    - `Boolean` - _change_, 账户地址是否改变.

**例子**

按account_id或account_alias列出所有地址:

```javascript
// Request
curl -X POST list-addresses -d '{"account_alias": "alice", "account_id": "086KQD75G0A02"}'

// Result
[
  {
    "account_alias": "alice",
    "account_id": "086KQD75G0A02",
    "address": "bm1qcn9lf7nxhswratvmg6d78nq7r7yupm36qgsv55",
    "change": false
  },
  {
    "account_alias": "alice",
    "account_id": "086KQD75G0A02",
    "address": "bm1qew4h5uvt5ssrtg2alms0j77r94c30m78ucrcxy",
    "change": false
  },
  {
    "account_alias": "alice",
    "account_id": "086KQD75G0A02",
    "address": "bm1qgnp4lte7wge0rsekevjlrdh39vkzz0c2alheue",
    "change": false
  }
]
```

#### validate-address

验证地址是否有效，并判断地址是否为自己。

**参数**

`Object`:

- `string` - _address_, 账户地址.

**返回**

`Object`:

- `Boolean` - _vaild_, 账户的地址是否有效.
- `Boolean` - _is_local_, 账户的地址是否是本地地址.

**例子**

检查地址是否有效。

```javascript
// Request
curl -X POST validate-address -d '{"address": "bm1qcn9lf7nxhswratvmg6d78nq7r7yupm36qgsv55"}'

// Result
{
   "vaild": true,
   "is_local": true,
}
```

#### create-asset

创建资产定义，准备发行资产。

**参数**

`Object`:

- `Array of String` - _root_xpubs_, 公钥集合.
- `String` - _alias_, 资产名称.
- `Integer` - _quorum_, 默认值是`1`, 交易必需签名用来支出由账户控制的资产单位的秘钥阀值.

Optional:

- `Object` - _definition_, 资产定义.
- `String` - _access_token_, 如果在本地创建资产时是可选的.但是，如果您想远程创建资产，它是必需要填写的.

**返回**

`Object`:

- `String` - _id_, 资产id.
- `String` - _alias_, 资产名称.
- `String` - _issuance_program_, 资产发行控制方案.
- `Array of Object` - _keys_, 资产公钥信息.
- `String` - _definition_, 资产定义.
- `Integer` - _quorum_,  交易必需签名用来支出由账户控制的资产单位的秘钥阀值.

**例子**

创建资产

```javascript
// Request
curl -X POST create-asset -d '{"alias": "GOLD", "root_xpubs": ["f6a16704f745a168642712060e6c5a69866147e21ec2447ae628f87d756bb68cc9b91405ad0a95f004090e864fde472f62ba97053ea109837bc89d63a64040d5"], "quorum":1}'

// Result
{
  "id": "3c1cf4c9436e3f942cb2f1d70a584f1c61df3697698dacccdc89e46f46a003d0",
  "alias": "GOLD",
  "issuance_program": "766baa209683b893483c0a5a317bf9868a8e2a09691f8aa8c1f3e2a7bb62b157e76712e05151ad696c00c0",
  "keys": [
    {
      "root_xpub": "f6a16704f745a168642712060e6c5a69866147e21ec2447ae628f87d756bb68cc9b91405ad0a95f004090e864fde472f62ba97053ea109837bc89d63a64040d5",
      "asset_pubkey": "9683b893483c0a5a317bf9868a8e2a09691f8aa8c1f3e2a7bb62b157e76712e012bd443fa7d56a0627df0a29dffcdc52641672a0f5cba54d104ad76ebeb8dfc3",
      "asset_derivation_path": [
        "000200000000000000"
      ]
    }
  ],
  "quorum": 1,
  "definition": {}
}
```

#### get-asset

按assetID查询详细信息资产。

**参数**

`Object`:

- `String` - _id_, 资产id.

**返回**

`Object`:

- `String` - _id_, 资产id.
- `String` - _alias_, 资产名字.
- `String` - _issuance_program_, 资产发行控制方案.
- `Integer` - _key_index_, xpub的密钥索引.
- `Integer` - _quorum_, 交易必需签名用来支出由账户控制的资产单位的秘钥阀值.
- `Array of Object` - _xpubs_, 公钥数组.
- `String` - _type_,资产类型.
- `Integer` - _vm_version_, 虚拟机版本.
- `String` - _raw_definition_byte_, 资产定义的字节.
- `Object` - _definition_, 资产描述.

**例子**

按assetID获取资产。

```javascript
// Request
curl -X POST get-asset -d '{"id": "50ec80b6bc48073f6aa8fa045131a71213c33f3681203b15ddc2e4b81f1f4730"}'

// Result
{
  "alias": "SILVER",
  "definition": null,
  "id": "50ec80b6bc48073f6aa8fa045131a71213c33f3681203b15ddc2e4b81f1f4730",
  "issue_program": "ae2029cd61d9ef31d40af7541f9a50831d6317fdb0870249d0564fcfa9a8f843589c5151ad",
  "key_index": 1,
  "quorum": 1,
  "raw_definition_byte": "",
  "type": "asset",
  "vm_version": 1,
  "xpubs": [
    "34b16ee500615cd325f8b84099f83c1ebecaca67977c5dc9b71ae32ceaf18207f996b0a9725b901d3792689b2babcb60febe3b81a684d9b56b65f67f307d453d"
  ]
}
```

#### list-assets

返回所有可用资产的列表。

**参数**

none

**返回**

- `Array of Object`, 资产数组.
  - `Object`:
    - `String` - _id_, 资产 id.
    - `String` - _alias_, 资产名称.
    - `String` - _issuance_program_, 资产发行控制程序.
    - `Integer` - _key_index_, 公钥索引.
    - `Integer` - _quorum_, 交易必需签名用来支出由账户控制的资产单位的秘钥阀值.
    - `Array of Object` - _xpubs_, 公钥数组.
    - `String` - _type_, 资产类型.
    - `Integer` - _vm_version_, 虚拟机版本.
    - `String` - _raw_definition_byte_, 资产定义的字节.
    - `Object` - _definition_, 资产的描述.

**例子**

列出所有可用资产:

```javascript
// Request
curl -X POST list-assets -d {}

// Result
[
  {
    "alias": "BTM",
    "definition": {
      "decimals": 8,
      "description": "Bytom Official Issue",
      "name": "BTM",
      "symbol": "BTM"
    },
    "id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "issue_program": "",
    "key_index": 0,
    "quorum": 0,
    "raw_definition_byte": "7b0a202022646563696d616c73223a20382c0a2020226465736372697074696f6e223a20224279746f6d204f6666696369616c204973737565222c0a2020226e616d65223a202262746d222c0a20202273796d626f6c223a202262746d220a7d",
    "type": "internal",
    "vm_version": 1,
    "xpubs": null
  },
  {
    "alias": "SILVER",
    "definition": null,
    "id": "50ec80b6bc48073f6aa8fa045131a71213c33f3681203b15ddc2e4b81f1f4730",
    "issue_program": "ae2029cd61d9ef31d40af7541f9a50831d6317fdb0870249d0564fcfa9a8f843589c5151ad",
    "key_index": 1,
    "quorum": 1,
    "raw_definition_byte": "",
    "type": "asset",
    "vm_version": 1,
    "xpubs": [
      "34b16ee500615cd325f8b84099f83c1ebecaca67977c5dc9b71ae32ceaf18207f996b0a9725b901d3792689b2babcb60febe3b81a684d9b56b65f67f307d453d"
    ]
  }
]
```

#### update-asset-alias

按assetID更新资产别名。

**参数**

`Object`:

- `String` - _id_, 资产id.
- `String` - _alias_, 资产的新别名.

**返回**

如果资产别名更新成功，则为none。

**例子**

更新资产别名。

```javascript
// Request
curl -X POST update-asset-alias -d '{"id":"50ec80b6bc48073f6aa8fa045131a71213c33f3681203b15ddc2e4b81f1f4730", "alias":"GOLD"}'

// Result
```

#### list-balances

返回所有可用帐户余额的列表。

**参数**

none

**返回**

- `Array of Object`,账户里面所有可用的余额.
  - `Object`:
    - `String` - _account_id_, 账户id.
    - `String` - _account_alias_,账户名称.
    - `String` - _asset_id_, 资产id.
    - `String` - _asset_alias_, 资产名称.
    - `Integer` - _amount_, 指定的帐户资产余额.

**例子**

列出所有可用的帐户余额。

```javascript
// Request
curl -X POST list-balances -d {}

// Result
[
    {
      "account_alias": "default",
      "account_id": "0BDQ9AP100A02",
      "amount": 35508000000000,
      "asset_alias": "BTM",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
    },
    {
      "account_alias": "alice",
      "account_id": "0BDQARM800A04",
      "amount": 60000000000,
      "asset_alias": "BTM",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
    }
]
```

#### list-unspent-outputs

返回钱包中所有帐户的所有可用未使用输出的列表。

**参数**

`Object`:
可选:

- `String` - _id_, 未花费的输出id.

**返回**

- `Array of Object`, 未花费的输出数组.
  - `Object`:
    - `String` - _account_id_, 账户id.
    - `String` - _account_alias_,账户名称.
    - `String` - _asset_id_, 资产id.
    - `String` - _asset_alias_, 资产名字.
    - `Integer` - _amount_, 指定的帐户资产余额.
    - `String` - _address_, 账户地址.
    - `Boolean` - _change_, 账户地址是否改变.
    - `String` - _id_, 未花费的输出id.
    - `String` - _program_, 账户程序.
    - `String` - _control_program_index_, 控制程序索引.
    - `String` - _source_id_, 未花费的输出源id
    - `String` - _source_pos_, 位花费的输出源id在区块中的位置.
    - `String` - _valid_height_, 有效高度.

**例子**

列出所有可用的未使用输出：

```javascript
// Request
curl -X POST list-unspent-outputs -d {}

// Result
[
  {
    "account_alias": "alice",
    "account_id": "0BKBR6VR00A06",
    "address": "bm1qv3htuvug7qdv46ywcvvzytrwrsyg0swltfa0dm",
    "amount": 2000,
    "asset_alias": "GOLD",
    "asset_id": "1883cce6aab82cf9af8cd085a3115dd4a92cdb8e6a9152acd73d7ae4adb9030a",
    "change": false,
    "control_program_index": 2,
    "id": "58f29f0f85f7bd2a91088bcbe536dee41cd0642dfb1480d3a88589bdbfd642d9",
    "program": "0014646ebe3388f01acae88ec318222c6e1c0887c1df",
    "source_id": "5988c1630c1f325e69bb92cb4b19af14286aa107311bc64b8f1a54629a33e0f4",
    "source_pos": 2,
    "valid_height": 0
  },
  {
    "account_alias": "default",
    "account_id": "0BKBR2D2G0A02",
    "address": "bm1qx7ylnhszg24995d5e0nftu9e87kt9vnxcn633r",
    "amount": 624000000000,
    "asset_alias": "BTM",
    "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "change": false,
    "control_program_index": 12,
    "id": "5af9d3c9b69470983377c1fc0c9125c4ac3bfd32c8d505f2a6042aade8503bc9",
    "program": "00143789f9de0242aa52d1b4cbe695f0b93facb2b266",
    "source_id": "233d1dd49e591980f98e11f333c6c28a867e78448e272011f045131df5aa260b",
    "source_pos": 0,
    "valid_height": 12
  }
]
```

列出与给定id匹配的未使用的输出：

```javascript
// Request
curl -X POST list-unspent-outputs -d '{"id": "58f29f0f85f7bd2a91088bcbe536dee41cd0642dfb1480d3a88589bdbfd642d9"}'

// Result
{
  "account_alias": "alice",
  "account_id": "0BKBR6VR00A06",
  "address": "bm1qv3htuvug7qdv46ywcvvzytrwrsyg0swltfa0dm",
  "amount": 2000,
  "asset_alias": "GOLD",
  "asset_id": "1883cce6aab82cf9af8cd085a3115dd4a92cdb8e6a9152acd73d7ae4adb9030a",
  "change": false,
  "control_program_index": 2,
  "id": "58f29f0f85f7bd2a91088bcbe536dee41cd0642dfb1480d3a88589bdbfd642d9",
  "program": "0014646ebe3388f01acae88ec318222c6e1c0887c1df",
  "source_id": "5988c1630c1f325e69bb92cb4b19af14286aa107311bc64b8f1a54629a33e0f4",
  "source_pos": 2,
  "valid_height": 0
}
```

#### backup-wallet

备份钱包到镜像文件，它包含帐户镜像，资产镜像和密钥镜像。

**参数**

none

**返回**

`Object`:

- `Object` - _account_image_, 账户镜像.
- `Object` - _asset_image_, 资产镜像.
- `Object` - _key_images_, 秘钥镜像.

**例子**

```javascript
// Request
curl -X backup-wallet -d {}

// Result
{
  "account_image": {
    "slices": [
      {
        "account": {
          "type": "account",
          "xpubs": [
            "395d6e0ac25978c3f52f9c7bdfdf75ce6af02639fd7875b4b1f40778ab1120c6dcf461b7ab6fd310983afb54a9a0fb3e09b6ec0d4364c4808c94383d50fb0681"
          ],
          "quorum": 1,
          "key_index": 1,
          "ID": "0CQTA3EOG0A02",
          "Alias": "def"
        },
        "contract_index": 2
      }
    ]
  },
  "asset_image": {
    "assets": []
  },
  "key_images": {
    "xkeys": [
      {
        "crypto": {
          "cipher": "aes-128-ctr",
          "ciphertext": "bf44766fec149478af9500e25ce0a6bc50bb2fa04e40465781da6ff64e9b3a4c9af3d214cd92c5a41d8498db5f4376526740f960ff429b16e52876aec6860e1d",
          "cipherparams": {
            "iv": "1b0fc61ae4dacb15f0f77d2b4ba67635"
          },
          "kdf": "scrypt",
          "kdfparams": {
            "dklen": 32,
            "n": 4096,
            "p": 6,
            "r": 8,
            "salt": "e133b1e7caae771ff1ab34b14824d6e27ef399f2b7ded4ad3500f080ede4a1dd"
          },
          "mac": "bc6bf411fb63e61a17bc15b94f29cf0d5a0f084c328955da1f7e2b26757cfc23"
        },
        "id": "1f40be59-7400-4fdc-b46b-15009f65363a",
        "type": "bytom_kd",
        "version": 1,
        "alias": "default",
        "xpub": "c4ec9bfd5df19d175e17ff7fed89193c37a4a64e1c0928387da01387ca76c3bfd99390e3373ec4d438522cc2d4644214cd2ec3b00965f7a1fa3546809583191c"
      },
      {
        "crypto": {
          "cipher": "aes-128-ctr",
          "ciphertext": "f0887c8603cbbafc0a66d5b45f71488e089708c7dea4342625a67858a49d6d08c79cd3f1800627e3c8b4668e8df34fcf0be9df5d9d4503acff05373976c312a9",
          "cipherparams": {
            "iv": "c111b46f9104f49f2c40aedb827e53b5"
          },
          "kdf": "scrypt",
          "kdfparams": {
            "dklen": 32,
            "n": 4096,
            "p": 6,
            "r": 8,
            "salt": "d9ef588b258b111dea1d99a4e4c5a4f968ab69072176bb95b111922e3bbea9e6"
          },
          "mac": "336f5fee643776e139f05ebe5e4f209d992ff97e16b906105fadac9e86133554"
        },
        "id": "611d407c-9e97-4297-a02a-13cd68e47983",
        "type": "bytom_kd",
        "version": 1,
        "alias": "def",
        "xpub": "395d6e0ac25978c3f52f9c7bdfdf75ce6af02639fd7875b4b1f40778ab1120c6dcf461b7ab6fd310983afb54a9a0fb3e09b6ec0d4364c4808c94383d50fb0681"
      }
    ]
  }
}
```

#### restore-wallet

按镜像文件还原钱包。

**参数**

`Object`:

- `Object` - _account_image_, 账户镜像.
- `Object` - _asset_image_, 资产镜像.
- `Object` - _key_images_, 秘钥镜像.

**返回**

如果恢复钱包成功，返回none

**例子**

```javascript
// Request
curl -X POST restore-wallet -d '{"account_image":{"slices":[{"account":{"type":"account","xpubs":["395d6e0ac25978c3f52f9c7bdfdf75ce6af02639fd7875b4b1f40778ab1120c6dcf461b7ab6fd310983afb54a9a0fb3e09b6ec0d4364c4808c94383d50fb0681"],"quorum":1,"key_index":1,"ID":"0CQTA3EOG0A02","Alias":"def"},"contract_index":2}]},"asset_image":{"assets":[]},"key_images":{"xkeys":[{"crypto":{"cipher":"aes-128-ctr","ciphertext":"bf44766fec149478af9500e25ce0a6bc50bb2fa04e40465781da6ff64e9b3a4c9af3d214cd92c5a41d8498db5f4376526740f960ff429b16e52876aec6860e1d","cipherparams":{"iv":"1b0fc61ae4dacb15f0f77d2b4ba67635"},"kdf":"scrypt","kdfparams":{"dklen":32,"n":4096,"p":6,"r":8,"salt":"e133b1e7caae771ff1ab34b14824d6e27ef399f2b7ded4ad3500f080ede4a1dd"},"mac":"bc6bf411fb63e61a17bc15b94f29cf0d5a0f084c328955da1f7e2b26757cfc23"},"id":"1f40be59-7400-4fdc-b46b-15009f65363a","type":"bytom_kd","version":1,"alias":"default","xpub":"c4ec9bfd5df19d175e17ff7fed89193c37a4a64e1c0928387da01387ca76c3bfd99390e3373ec4d438522cc2d4644214cd2ec3b00965f7a1fa3546809583191c"},{"crypto":{"cipher":"aes-128-ctr","ciphertext":"f0887c8603cbbafc0a66d5b45f71488e089708c7dea4342625a67858a49d6d08c79cd3f1800627e3c8b4668e8df34fcf0be9df5d9d4503acff05373976c312a9","cipherparams":{"iv":"c111b46f9104f49f2c40aedb827e53b5"},"kdf":"scrypt","kdfparams":{"dklen":32,"n":4096,"p":6,"r":8,"salt":"d9ef588b258b111dea1d99a4e4c5a4f968ab69072176bb95b111922e3bbea9e6"},"mac":"336f5fee643776e139f05ebe5e4f209d992ff97e16b906105fadac9e86133554"},"id":"611d407c-9e97-4297-a02a-13cd68e47983","type":"bytom_kd","version":1,"alias":"def","xpub":"395d6e0ac25978c3f52f9c7bdfdf75ce6af02639fd7875b4b1f40778ab1120c6dcf461b7ab6fd310983afb54a9a0fb3e09b6ec0d4364c4808c94383d50fb0681"}]}}'

// Result
```

#### sign-message

使用地址的密钥（私钥）对信息进行签名。

**参数**

`Object`:

- `String` - _address_, 账户地址.
- `String` - _message_, 地址xpub签名的消息.
- `String` - _password_, 账户密码.

**返回**

`Object`:

- `String` - _derived_xpub_, 派生的xpub.
- `String` - _signature_, 消息的签名.

**例子**

```javascript
// Request
curl -X POST sign-message -d '{"address":"bm1qx2qgvvjz734ur8x5lpfdtlau74aaa5djs0a5jn", "message":"this is a test message", "password":"123456"}'

// Result
{
  "signature": "74da3d6572233736e3a439166719244dab57dd0047f8751b1efa2da26eeab251d915c1211dcad77e8b013267b86d96e91ae67ff0be520ef4ec326e911410b609",
  "derived_xpub": "6ff8c3d1321ce39a3c3550f57ba70b67dcbcef821e9b85f6150edb7f2f3f91009e67f3075e6e76ed5f657ee4b1a5f4749b7a8c74c8e7e6a1b0e5918ebd5df4d0"
}
```

#### get-transaction

按transaction ID查询与帐户相关的事务。

**参数**

`Object`:

- `String` - _tx_id_, 交易 id, 交易hash.

**返回**

`Object`:

- `String` - _tx_id_, 交易id, 交易hash.
- `Integer` - _block_time_, 响应请求时的unix时间戳.
- `String` - _block_hash_, 交易所在区块的hash值.
- `Integer` - _block_height_, 交易所在的区块高度.
- `Integer` - _block_index_, 交易所在区块的位置.
- `Integer` - _block_transactions_count_, 此区块中的所有交易.
- `Boolean` - _status_fail_, 交易请求的状态是否失败.
- `Integer` - _size_, 交易占的内存大小.
- `Array of Object` - _inputs_, 交易的输入对象.
  - `String` - _type_, 输入类型,可选用的选项包括: 'spend'(), 'issue'(), 'coinbase'().
  - `String` - _asset_id_, 资产id.
  - `String` - _asset_alias_, 资产名字.
  - `Object` - _asset_definition_, 定义资产(json对象).
  - `Integer` - _amount_, 资产数量.
  - `Object` - _issuance_program_, 发行程序, 它仅仅当类型为'issue'时存在.
  - `Object` - _control_program_, 账户控制程序, 它仅仅存在当交易类型为'spend'时.
  - `String` - _address_, 账户地址, 它仅仅存在当交易类型为'spend'时.
  - `String` - _spent_output_id_, 在输入中前一笔交易的输出ID将被花费, 它仅仅存在当交易类型为'spend'时.
  - `String` - _account_id_, 账户id.
  - `String` - _account_alias_, 账户名称.
  - `Object` - _arbitrary_, 矿工可以设置任意信息, 只有当交易类型为 'coinbase'时存在.
- `Array of Object` - _outputs_, 交易的输出对象.
  - `String` - _type_, 输出动作类型, 可用选项包括: 'retire', 'control'.
  - `String` - _id_, 与utxo相关的输出id.
  - `Integer` - _position_, 交易输出的位置.
  - `String` - _asset_id_, 资产id.
  - `String` - _asset_alias_, 资产名称.
  - `Object` - _asset_definition_, 资产的定义(json对象).
  - `Integer` - _amount_, 资产数量.
  - `String` - _account_id_, 账户id.
  - `String` - _account_alias_, 账户名称.
  - `Object` - _control_program_, 账户的控制程序.
  - `String` - _address_, 账户地址.

**例子**

```javascript
// Request
curl -X POST get-transaction -d '{"tx_id": "15b8d66e227feff47b3de0f278934ea16d6c828371ec6c13c8f84713dd11703b"}'

// Result
{
  "block_hash": "1fa9bb389cf974a9b37b63ca38c0cf3453c30f394b9e8ae7f04f2d1b52c329b4",
  "block_height": 530,
  "block_index": 1,
  "block_time": 1528772056,
  "block_transactions_count": 2,
  "inputs": [
    {
      "account_alias": "default",
      "account_id": "0ER7MEFGG0A02",
      "address": "sm1q4pkg8msjumtep7ecsdzuct3tsuzk5pmnm3p8nr",
      "amount": 41250000000,
      "asset_alias": "BTM",
      "asset_definition": {
        "decimals": 8,
        "description": "Bytom Official Issue",
        "name": "BTM",
        "symbol": "BTM"
      },
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "control_program": "0014a86c83ee12e6d790fb388345cc2e2b87056a0773",
      "spent_output_id": "002025b727148d04197cc7b9cf7eafd9986041f07641ca82dc0a1d9e227b52f6",
      "type": "spend"
    }
  ],
  "outputs": [
    {
      "account_alias": "default",
      "account_id": "0ER7MEFGG0A02",
      "address": "sm1qmt6jxrr8etssufr8qp98emyaly3lknxyndh5cj",
      "amount": 29450000000,
      "asset_alias": "BTM",
      "asset_definition": {
        "decimals": 8,
        "description": "Bytom Official Issue",
        "name": "BTM",
        "symbol": "BTM"
      },
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "control_program": "0014daf5230c67cae10e2467004a7cec9df923fb4cc4",
      "id": "35a46dd36eb27b1ffdfdefbe5366175b6325e8f56e5bc3dd2aa1a47197e26e6c",
      "position": 0,
      "type": "control"
    },
    {
      "account_alias": "alice",
      "account_id": "0ER7OAK400A02",
      "address": "sm1qxe4jwhkekgnxkezu7xutu5gqnnpmyc8ppq98me",
      "amount": 11700000000,
      "asset_alias": "BTM",
      "asset_definition": {
        "decimals": 8,
        "description": "Bytom Official Issue",
        "name": "BTM",
        "symbol": "BTM"
      },
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "control_program": "0014366b275ed9b2266b645cf1b8be51009cc3b260e1",
      "id": "ae791bbde0cc5b370e28a505933b85082d67be8db81bdcc56b8202f200b883e7",
      "position": 1,
      "type": "control"
    }
  ],
  "size": 332,
  "status_fail": false,
  "tx_id": "15b8d66e227feff47b3de0f278934ea16d6c828371ec6c13c8f84713dd11703b"
}
```

#### list-transactions

返回所有与帐户相关的事务的列表。

**参数**

`Object`:

可选:

- `String` - _id_, 交易id,交易hash.
- `String` - _account_id_, 账户id.
- `Boolean` - _detail_ , 详细事务标志，默认为false（仅返回事务摘要).
- `Boolean` - _unconfirmed_, 确认事务的标志（查询结果包括所有已确认和未确认的事务），默认为false.

**返回**

`Array of Object`, 交易数组.

可选:

- `Object`:(交易汇总)
  - `String` - _tx_id_, 交易id,交易hash.
  - `Integer` - _block_time_, 响应请求时的unix时间戳.
  - `Array of Object` - _inputs_, 交易的主要输入对象.
    - `String` - _type_, 输入操作的类型，可用选项包括：'spend'，'issue'，'coinbase'.
    - `String` - _asset_id_, 资产id.
    - `String` - _asset_alias_, 资产名称.
    - `Integer` - _amount_, 资产数量.
    - `String` - _account_id_, 账户id.
    - `String` - _account_alias_, 账户名字.
    - `Object` - _arbitrary_, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
  - `Array of Object` - _outputs_, object of summary outputs for the transaction.
    - `String` - _type_, the type of output action, available option include: 'retire', 'control'.
    - `String` - _asset_id_, 资产id.
    - `String` - _asset_alias_, 资产名字.
    - `Integer` - _amount_, 资产数量.
    - `String` - _account_id_, 账户id.
    - `String` - _account_alias_, 账户名字.
    - `Object` - _arbitrary_, 矿工可以设置任意信息, 它仅存在当交易的输入类型'coinbase'(其他类型默认为空).
- `Object`:(交易详情)
  - `String` - _tx_id_, 交易id, 交易hash.
  - `Integer` - _block_time_, 请求的时间戳.
  - `String` - _block_hash_, 交易所在区块的hash.
  - `Integer` - _block_height_, 交易所在的区块高度.
  - `Integer` - _block_index_, 交易所在区块的索引.
  - `Integer` - _block_transactions_count_, 计算区块中的总交易笔数.
  - `Boolean` - _status_fail_, 交易的请求状态是否失败.
  - `Integer` - _size_, 交易所占内存大小.
  - `Array of Object` - _inputs_, 交易的输入对象.
    - `String` - _type_, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - _asset_id_, 资产id.
    - `String` - _asset_alias_, 资产名字.
    - `Object` - _asset_definition_, 资产定义(json对象).
    - `Integer` - _amount_, 资产数量.
    - `Object` - _issuance_program_, issuance program, it only exist when type is 'issue'.
    - `Object` - _control_program_, control program of account, it only exist when type is 'spend'.
    - `String` - _address_, 帐户地址，仅当类型为“花费”时才存在.
    - `String` - _spent_output_id_, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - _account_id_, 账户id.
    - `String` - _account_alias_, 账户名字.
    - `Object` - _arbitrary_, 矿工可以设置任意信息, 它仅存在当交易的输入类型'coinbase'.
  - `Array of Object` - _outputs_, 交易的输出对象.
    - `String` - _type_, 输出动作的类型,可用选项包括：’retire‘，’control‘.
    - `String` - _id_, 与utxo相关的输出id.
    - `Integer` - _position_, 输出的位置.
    - `String` - _asset_id_, 资产id.
    - `String` - _asset_alias_, 资产名称.
    - `Object` - _asset_definition_, 资产的定义(json 对象).
    - `Integer` - _amount_, 资产数量.
    - `String` - _account_id_, 账户id.
    - `String` - _account_alias_, 账户名字.
    - `Object` - _control_program_, 账户的控制程序.
    - `String` - _address_, 账户地址.


**例子**

列出所有可用的交易：

```javascript
// Request
curl -X POST list-transactions -d {}

// Result
[
  {
    "block_time": 1521771059,
    "inputs": [
      {
        "arbitrary": "06",
        "asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
        "type": "coinbase"
      }
    ],
    "outputs": [
      {
        "account_alias": "default",
        "account_id": "0BMHBOBVG0A02",
        "amount": 41250000000,
        "asset_alias": "BTM",
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "type": "control"
      }
    ],
    "tx_id": "c631a8de401913a512c338bcf4a61cb2de6cede12a7385d9d11637eaa6578f33"
  },
  {
    "block_time": 1521770515,
    "inputs": [
      {
        "account_alias": "default",
        "account_id": "0BMHBOBVG0A02",
        "amount": 41250000000,
        "asset_alias": "BTM",
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "type": "spend"
      }
    ],
    "outputs": [
      {
        "account_alias": "default",
        "account_id": "0BMHBOBVG0A02",
        "amount": 34649500000,
        "asset_alias": "BTM",
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "type": "control"
      },
      {
        "account_alias": "alice",
        "account_id": "0BMHDI1P00A04",
        "amount": 6600000000,
        "asset_alias": "BTM",
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "type": "control"
      }
    ],
    "tx_id": "1151ce5c7b32b8755b5e48109ec7ed956fb1783eaea9558bf5a2ad957825e4b7"
  }
]
```

列出与给定tx_id匹配的事务详细信息：

```javascript
// Request
curl -X POST list-transactions -d '{"id": "7e9f9b999381da936e3cae48b5bac2b9bc28bb56c6c862be6c110448f7e2f6b3","detail": true}'

// Result
[
  {
    "block_hash": "1b2d0efa025256603e9330273d37f5a900cd3dfb213e015ac53cf645e2315a6d",
    "block_height": 72,
    "block_index": 1,
    "block_time": 1528528584,
    "block_transactions_count": 2,
    "inputs": [
      {
        "account_alias": "default",
        "account_id": "0ER7MEFGG0A02",
        "address": "sm1q4pkg8msjumtep7ecsdzuct3tsuzk5pmnm3p8nr",
        "amount": 41250000000,
        "asset_alias": "BTM",
        "asset_definition": {
          "decimals": 8,
          "description": "Bytom Official Issue",
          "name": "BTM",
          "symbol": "BTM"
        },
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "control_program": "0014a86c83ee12e6d790fb388345cc2e2b87056a0773",
        "spent_output_id": "0072a2c1cee30a7c7b7b006ca08a48c2b479bc81c0ec6463fe4865ef37626ab6",
        "type": "spend"
      }
    ],
    "outputs": [
      {
        "account_alias": "default",
        "account_id": "0ER7MEFGG0A02",
        "address": "sm1qskj096x5w7ejcmk746g3djmv84dpxts62dewvd",
        "amount": 34649500000,
        "asset_alias": "BTM",
        "asset_definition": {
          "decimals": 8,
          "description": "Bytom Official Issue",
          "name": "BTM",
          "symbol": "BTM"
        },
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "control_program": "001485a4f2e8d477b32c6edeae9116cb6c3d5a132e1a",
        "id": "b08c9bfc816064ca33da8b569998229774fc9552da7d4f16870b2c5a8f645b3b",
        "position": 0,
        "type": "control"
      },
      {
        "account_alias": "alice",
        "account_id": "0ER7OAK400A02",
        "address": "sm1qxe4jwhkekgnxkezu7xutu5gqnnpmyc8ppq98me",
        "amount": 6600000000,
        "asset_alias": "BTM",
        "asset_definition": {
          "decimals": 8,
          "description": "Bytom Official Issue",
          "name": "BTM",
          "symbol": "BTM"
        },
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "control_program": "0014366b275ed9b2266b645cf1b8be51009cc3b260e1",
        "id": "0e8f8dc83a39b2b6d00a77759a797102d047f82f800fe21f5b1d80bb4d5e2e39",
        "position": 1,
        "type": "control"
      }
    ],
    "size": 333,
    "status_fail": false,
    "tx_id": "7e9f9b999381da936e3cae48b5bac2b9bc28bb56c6c862be6c110448f7e2f6b3"
  }
]
```

列出与给定account_id和未确认标志匹配的标签（未确认标签的block_hash，block_height和block_index默认为零）：

```javascript
// Request
curl -X POST list-transactions -d '{"account_id": "0F1MQVI500A02", "unconfirmed": true, "detail": true}'

// Result
[
  {
    "block_hash": "0000000000000000000000000000000000000000000000000000000000000000",
    "block_height": 0,
    "block_index": 0,
    "block_time": 1529032899,
    "inputs": [
      {
        "account_alias": "default",
        "account_id": "0F1L5Q3V00A02",
        "address": "sm1ql67n04pj8mfqzv3wjq8num3yrltdykemgrr45j",
        "amount": 41250000000,
        "asset_alias": "BTM",
        "asset_definition": {
          "decimals": 8,
          "description": "Bytom Official Issue",
          "name": "BTM",
          "symbol": "BTM"
        },
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "control_program": "0014febd37d4323ed201322e900f3e6e241fd6d25b3b",
        "spent_output_id": "00570443cbac4f68638ff565e8b04db2062800b9e23b7701913ddf6b190dbe65",
        "type": "spend"
      },
      {
        "account_alias": "default",
        "account_id": "0F1L5Q3V00A02",
        "address": "sm1ql67n04pj8mfqzv3wjq8num3yrltdykemgrr45j",
        "amount": 41250000000,
        "asset_alias": "BTM",
        "asset_definition": {
          "decimals": 8,
          "description": "Bytom Official Issue",
          "name": "BTM",
          "symbol": "BTM"
        },
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "control_program": "0014febd37d4323ed201322e900f3e6e241fd6d25b3b",
        "spent_output_id": "01df9011ca0bed4bb9b95dc84da4c5103fed06ca28c03d92d34ee3d61b945288",
        "type": "spend"
      }
    ],
    "outputs": [
      {
        "account_alias": "default",
        "account_id": "0F1L5Q3V00A02",
        "address": "sm1qdcfprk7wjy6flavkzhcjh3dxyrwlm935trrs5m",
        "amount": 41249100000,
        "asset_alias": "BTM",
        "asset_definition": {
          "decimals": 8,
          "description": "Bytom Official Issue",
          "name": "BTM",
          "symbol": "BTM"
        },
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "control_program": "00146e1211dbce91349ff59615f12bc5a620ddfd9634",
        "id": "09fabb1a2bac44c45054175453e23e81a764557147523d8df70d8a190cf2eb17",
        "position": 0,
        "type": "control"
      },
      {
        "account_alias": "default",
        "account_id": "0F1L5Q3V00A02",
        "address": "sm1qt92xx2f4ys63dyhy58jle87nttcf37zftweklh",
        "amount": 39150000000,
        "asset_alias": "BTM",
        "asset_definition": {
          "decimals": 8,
          "description": "Bytom Official Issue",
          "name": "BTM",
          "symbol": "BTM"
        },
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "control_program": "0014595463293524351692e4a1e5fc9fd35af098f849",
        "id": "6efae48663e872193e8a672eb85b8bbf29d8aee98e42816340fa0b2340cc355d",
        "position": 1,
        "type": "control"
      },
      {
        "account_alias": "alice",
        "account_id": "0F1MQVI500A02",
        "address": "sm1qum6ly8aq9u9k7xrkuck9pq64xg67gw40khnnxu",
        "amount": 2100000000,
        "asset_alias": "BTM",
        "asset_definition": {
          "decimals": 8,
          "description": "Bytom Official Issue",
          "name": "BTM",
          "symbol": "BTM"
        },
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "control_program": "0014e6f5f21fa02f0b6f1876e62c5083553235e43aaf",
        "id": "aca1ecc59d8bcf548e4f5afb8a97e38f0eb56e1387b17400fd3c693c074a703d",
        "position": 2,
        "type": "control"
      }
    ],
    "size": 1194,
    "status_fail": false,
    "tx_id": "9c28a6a2a039ed5bdebe81eea44cdb22a951c472bc25cb1e8188ae423a98f251"
  },
  {
    "block_hash": "474b9c28b225fba02278ad3b097d561bf8f5c562ff2a548226fc10fc1d75b7ed",
    "block_height": 255,
    "block_index": 1,
    "block_time": 1528963126,
    "block_transactions_count": 2,
    "inputs": [
      {
        "account_alias": "alice",
        "account_id": "0F1MQVI500A02",
        "address": "sm1qum6ly8aq9u9k7xrkuck9pq64xg67gw40khnnxu",
        "amount": 10000000000,
        "asset_alias": "BTM",
        "asset_definition": {
          "decimals": 8,
          "description": "Bytom Official Issue",
          "name": "BTM",
          "symbol": "BTM"
        },
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "control_program": "0014e6f5f21fa02f0b6f1876e62c5083553235e43aaf",
        "spent_output_id": "767649aafdfe2c22d46d641a5b74d934e2590330f7280b0fc55b978812a99a58",
        "type": "spend"
      },
      {
        "account_alias": "alice",
        "account_id": "0F1MQVI500A02",
        "address": "sm1qum6ly8aq9u9k7xrkuck9pq64xg67gw40khnnxu",
        "amount": 1000000000000,
        "asset_alias": "GOLD",
        "asset_definition": {
          "decimals": 8,
          "description": {},
          "name": "",
          "symobol": ""
        },
        "asset_id": "71deb74415f16a1f7bffb04c61d427bb1f93adfba257ffba2673f102d602e78f",
        "control_program": "0014e6f5f21fa02f0b6f1876e62c5083553235e43aaf",
        "spent_output_id": "5d7a88851f5696ded279cb9bc380e050024c555258ea7851dfdedc2797b0d820",
        "type": "spend"
      }
    ],
    "outputs": [
      {
        "account_alias": "alice",
        "account_id": "0F1MQVI500A02",
        "address": "sm1q39sztlh4jq5nknstn2udvvpm6v5ugussx2djc0",
        "amount": 9980000000,
        "asset_alias": "BTM",
        "asset_definition": {
          "decimals": 8,
          "description": "Bytom Official Issue",
          "name": "BTM",
          "symbol": "BTM"
        },
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "control_program": "0014896025fef590293b4e0b9ab8d6303bd329c47210",
        "id": "2b44969d28d79544006e792411d6cd1d245f9af20419f6138494b4b5aac2a72e",
        "position": 0,
        "type": "control"
      },
      {
        "account_alias": "alice",
        "account_id": "0F1MQVI500A02",
        "address": "sm1q258yd0gvatje4pn0qc8z9w8cdv45j9tvhfpjh8",
        "amount": 999999999901,
        "asset_alias": "GOLD",
        "asset_definition": {
          "decimals": 8,
          "description": {},
          "name": "",
          "symobol": ""
        },
        "asset_id": "71deb74415f16a1f7bffb04c61d427bb1f93adfba257ffba2673f102d602e78f",
        "control_program": "0014550e46bd0ceae59a866f060e22b8f86b2b49156c",
        "id": "54be1bc876d1deccb9845acec79eabf62d7eacd5935e337850233657914d0f9d",
        "position": 1,
        "type": "control"
      },
      {
        "amount": 99,
        "asset_alias": "GOLD",
        "asset_definition": {
          "decimals": 8,
          "description": {},
          "name": "",
          "symobol": ""
        },
        "asset_id": "71deb74415f16a1f7bffb04c61d427bb1f93adfba257ffba2673f102d602e78f",
        "control_program": "20e864761d8181103b6476435a805cba97361df9a05c40fae644c27f69ce045d3c16001464d928e181900d382fa33def66534c7323c778c4015820684d6683d014abb4e019878b50fbbb547bcbf9c4739498d8eeef565d37f9a82f741a547a6413000000007b7b51547ac1631a000000547a547aae7cac00c0",
        "id": "347553923bb550c236a703e46600d53f25161e3eb74ee3183884d398e5d894b0",
        "position": 2,
        "type": "control"
      }
    ],
    "size": 691,
    "status_fail": false,
    "tx_id": "383f8636842301b2fe292c5b8b2f540c6ed7867ba5751680b2e77827c300e41e"
  }
]
```

#### build-transaction

建立交易。

**参数**
`Object`:

- `String` - _base_transaction_, 交易基本数据, 默认为空.
- `Integer` - _ttl_, integer of the time to live in milliseconds, it means utxo will be reserved(locked) for builded transaction in this time range, if the transaction will not to be submitted into block, it will be auto unlocked for build transaction again after this ttl time. it will be set to 5 minutes(300 seconds) defaultly when ttl is 0.
- `Integer` - _time_range_, time stamp(block height)is maximum survival time for the transaction, the transaction will be not submit into block after this time stamp.
- `Arrary of Object` - _actions_:
  - `Object`:
    - `String` - _account_id_account_alias_, 别名或帐户ID.
    - `String` - _asset_id_asset_alias_, 资产的别名或ID.
    - `Integer` - _amount_, 与此交易一起发送的金额的指定资产.
    - `String`- _type_, type of transaction, valid types: 'spend_account', 'issue', 'spend_account_unspent_output', 'control_address', 'control_program', 'retire'.
    - `String` - _address_, (type is control_address) address of receiver, the style of address is P2PKH or P2SH.
    - `String` - _control_program_, (type is control_program) control program of receiver.

**返回**

- `Object of build-transaction` - _transaction_, builded transaction.

**例子**

- `spend` - transaction type is spend

```javascript
// Request
curl -X POST build-transaction -d '{"base_transaction":null,"actions":[{"account_id":"0BF63M2U00A04","amount":20000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","type":"spend_account"},{"account_id":"0BF63M2U00A04","amount":99,"asset_id":"3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680","type":"spend_account"},{"amount":99,"asset_id":"3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680","address":"bm1q50u3z8empm5ke0g3ngl2t3sqtr6sd7cepd3z68","type":"control_address"}],"ttl":0,"time_range": 1521625823}'
```

- `issue` - transaction type is issue

```javascript
// Request
curl -X POST build-transaction -d '{"base_transaction":null,"actions":[{"account_id":"0BF63M2U00A04","amount":20000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","type":"spend_account"},{"amount":10000,"asset_id":"3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680","type":"issue"},{"amount":10000,"asset_id":"3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680","address":"bm1q50u3z8empm5ke0g3ngl2t3sqtr6sd7cepd3z68","type":"control_address"}],"ttl":0,"time_range": 1521625823}'
```

- `address` - transaction type is address

```javascript
// Request
curl -X POST build-transaction -d '{"base_transaction":null,"actions":[{"account_id":"0BF63M2U00A04","amount":20000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","type":"spend_account"},{"account_id":"0BF63M2U00A04","amount":99,"asset_id":"3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680","type":"spend_account"},{"amount":99,"asset_id":"3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680","address":"bm1q50u3z8empm5ke0g3ngl2t3sqtr6sd7cepd3z68","type":"control_address"}],"ttl":0,"time_range": 1521625823}'
```

- `retire` - transaction type is retire

```javascript
// Request
curl -X POST build-transaction -d '{"base_transaction":null,"actions":[{"account_id":"0BF63M2U00A04","amount":20000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","type":"spend_account"},{"account_id":"0BF63M2U00A04","amount":99,"asset_id":"3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680","type":"spend_account"},{"account_id":"0BF63M2U00A04","amount":99,"asset_id":"3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680","type":"retire"}],"ttl":0,"time_range":1521625823}'
```

- `spend_account_unspent_output` - transaction type is spend_account_unspent_output(user can get UTXO information by call `list-unspent-outputs` API)


attention:
  - action field `output_id` correspond to UTXO result `id` field
  - UTXO asset and amount will be spent in this transaction
  - transaction fee is (utxo asset_amount - output asset_amount)

```javascript
// Request
curl -X POST build-transaction -d '{"base_transaction":null,"actions":[{"type":"spend_account_unspent_output","output_id":"01c6ccc6f522228cd4518bba87e9c43fbf55fdf7eb17f5aa300a037db7dca0cb"},{"amount":41243000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","address":"sm1qmw8c5s29zlexknfahrze3ghvlqrtn2huuntvpn","type":"control_address"}],"ttl":0,"time_range":0}'
```

```javascript
// Result(this type is spend, the other types are similar.)
{
  "allow_additional_actions": false,
  "local": true,
  "raw_transaction": "07010000020161015fb6a63a3361170afca03c9d5ce1f09fe510187d69545e09f95548b939cd7fffa3ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80fc93afdf01000116001426bd1b851cf6eb8a701c20c184352ad8720eeee90100015d015bb6a63a3361170afca03c9d5ce1f09fe510187d69545e09f95548b939cd7fffa33152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680c03e0101160014489a678741ccc844f9e5c502f7fac0a665bedb25010003013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80a2cfa5df0101160014948fb4f500e66d20fbacb903fe108ee81f9b6d9500013a3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680dd3d01160014cd5a822b34e3084413506076040d508bb12232c70001393152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc436806301160014a3f9111f3b0ee96cbd119a3ea5c60058f506fb1900",
  "signing_instructions": [
    {
      "position": 0,
      "witness_components": [
        {
          "keys": [
            {
              "derivation_path": [
                "010100000000000000",
                "0500000000000000"
              ],
              "xpub": "ee9dd8affdef7e0cacd0fbbf310217c7f588156c28e414db74c27afaedd8f876cf54547a672b431ff06ee8a146207df9595638a041b55ada1a764a8b5b30bda0"
            }
          ],
          "quorum": 1,
          "signatures": null,
          "type": "raw_tx_signature"
        },
        {
          "type": "data",
          "value": "62a73b6b7ffe52b6ad782b0e0efdc8309bf2f057d88f9a17d125e41bb11dbb88"
        }
      ]
    },
    {
      "position": 1,
      "witness_components": [
        {
          "keys": [
            {
              "derivation_path": [
                "010100000000000000",
                "0600000000000000"
              ],
              "xpub": "ee9dd8affdef7e0cacd0fbbf310217c7f588156c28e414db74c27afaedd8f876cf54547a672b431ff06ee8a146207df9595638a041b55ada1a764a8b5b30bda0"
            }
          ],
          "quorum": 1,
          "signatures": null,
          "type": "raw_tx_signature"
        },
        {
          "type": "data",
          "value": "ba5a63e7416caeb945eefc2ce874f40bc4aaf6005a1fc792557e41046f7e502f"
        }
      ]
    }
  ]
}
```

#### sign-transaction

签署交易。

**参数**

`Object`:

- `String` - _password_, 密码签名.
- `Object` - _transaction_, 构建好的交易.

**返回**

`Object`:

- `Boolean` - _sign_complete_, 如果签名成功返回true,false签名不成功.
- `Object of sign-transaction` - _transaction_, 交易签名.

**例子**

```javascript
// Request
curl -X POST sign-transaction -d '{"password":"123456","transaction":{"allow_additional_actions":false,"local":true,"raw_transaction":"07010000020161015fb6a63a3361170afca03c9d5ce1f09fe510187d69545e09f95548b939cd7fffa3ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80fc93afdf01000116001426bd1b851cf6eb8a701c20c184352ad8720eeee90100015d015bb6a63a3361170afca03c9d5ce1f09fe510187d69545e09f95548b939cd7fffa33152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680c03e0101160014489a678741ccc844f9e5c502f7fac0a665bedb25010003013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80a2cfa5df0101160014948fb4f500e66d20fbacb903fe108ee81f9b6d9500013a3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680dd3d01160014cd5a822b34e3084413506076040d508bb12232c70001393152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc436806301160014a3f9111f3b0ee96cbd119a3ea5c60058f506fb1900","signing_instructions":[{"position":0,"witness_components":[{"keys":[{"derivation_path":["010100000000000000","0500000000000000"],"xpub":"ee9dd8affdef7e0cacd0fbbf310217c7f588156c28e414db74c27afaedd8f876cf54547a672b431ff06ee8a146207df9595638a041b55ada1a764a8b5b30bda0"}],"quorum":1,"signatures":null,"type":"raw_tx_signature"},{"type":"data","value":"62a73b6b7ffe52b6ad782b0e0efdc8309bf2f057d88f9a17d125e41bb11dbb88"}]},{"position":1,"witness_components":[{"keys":[{"derivation_path":["010100000000000000","0600000000000000"],"xpub":"ee9dd8affdef7e0cacd0fbbf310217c7f588156c28e414db74c27afaedd8f876cf54547a672b431ff06ee8a146207df9595638a041b55ada1a764a8b5b30bda0"}],"quorum":1,"signatures":null,"type":"raw_tx_signature"},{"type":"data","value":"ba5a63e7416caeb945eefc2ce874f40bc4aaf6005a1fc792557e41046f7e502f"}]}]}}'

// Result
{
  "sign_complete": true,
  "transaction": {
    "allow_additional_actions": false,
    "local": true,
    "raw_transaction": "07010000020161015fb6a63a3361170afca03c9d5ce1f09fe510187d69545e09f95548b939cd7fffa3ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80fc93afdf01000116001426bd1b851cf6eb8a701c20c184352ad8720eeee96302400d432e6f0e22da3168d76552273e60d23d432d61b4dac53e6769d39a1097f1cd1bd8e54c7d39eda334803e5c904bc2de2f27ff29748166e0334dcfded20e980b2062a73b6b7ffe52b6ad782b0e0efdc8309bf2f057d88f9a17d125e41bb11dbb88015d015bb6a63a3361170afca03c9d5ce1f09fe510187d69545e09f95548b939cd7fffa33152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680c03e0101160014489a678741ccc844f9e5c502f7fac0a665bedb256302401eadd84ad07c3643f71a35cc5669a2c1def96ae98e790d287217e6a3543fe602dd90afffe853c729bd5237a28f33538df631572847d9870829fb1fd1100ff20820ba5a63e7416caeb945eefc2ce874f40bc4aaf6005a1fc792557e41046f7e502f03013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80a2cfa5df0101160014948fb4f500e66d20fbacb903fe108ee81f9b6d9500013a3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680dd3d01160014cd5a822b34e3084413506076040d508bb12232c70001393152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc436806301160014a3f9111f3b0ee96cbd119a3ea5c60058f506fb1900",
    "signing_instructions": [
      {
        "position": 0,
        "witness_components": [
          {
            "keys": [
              {
                "derivation_path": [
                  "010100000000000000",
                  "0500000000000000"
                ],
                "xpub": "ee9dd8affdef7e0cacd0fbbf310217c7f588156c28e414db74c27afaedd8f876cf54547a672b431ff06ee8a146207df9595638a041b55ada1a764a8b5b30bda0"
              }
            ],
            "quorum": 1,
            "signatures": [
              "0d432e6f0e22da3168d76552273e60d23d432d61b4dac53e6769d39a1097f1cd1bd8e54c7d39eda334803e5c904bc2de2f27ff29748166e0334dcfded20e980b"
            ],
            "type": "raw_tx_signature"
          },
          {
            "type": "data",
            "value": "62a73b6b7ffe52b6ad782b0e0efdc8309bf2f057d88f9a17d125e41bb11dbb88"
          }
        ]
      },
      {
        "position": 1,
        "witness_components": [
          {
            "keys": [
              {
                "derivation_path": [
                  "010100000000000000",
                  "0600000000000000"
                ],
                "xpub": "ee9dd8affdef7e0cacd0fbbf310217c7f588156c28e414db74c27afaedd8f876cf54547a672b431ff06ee8a146207df9595638a041b55ada1a764a8b5b30bda0"
              }
            ],
            "quorum": 1,
            "signatures": [
              "1eadd84ad07c3643f71a35cc5669a2c1def96ae98e790d287217e6a3543fe602dd90afffe853c729bd5237a28f33538df631572847d9870829fb1fd1100ff208"
            ],
            "type": "raw_tx_signature"
          },
          {
            "type": "data",
            "value": "ba5a63e7416caeb945eefc2ce874f40bc4aaf6005a1fc792557e41046f7e502f"
          }
        ]
      }
    ]
  }
}
```
#### submit-transaction

提交交易。

**参数**

Object:

- Object - raw_transaction, 已经签名好的交易.

**返回**

Object:

- String - tx_id, 交易id 交易hash.

**例子**

```javascript
// Request
curl -X POST submit-transaction -d '{"raw_transaction":"07010000010161015ffe8bdb49bbd08f711a54f0fbed4141b74c276de44c831999aac43bdd56f98309ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80c0b1ca94120001160014d61f9b3354771283461e6cb269bec98e7ee975f6630240403d7b35ec72a80816b21822d645adc98af2c1f07d5b379dda4527d8585ec12ef213ada312e3807ae0ccf7206575f313ffe2b405a286309d3172feabe07e7e0620d013706a314a57e84c2b262c2a291e8e2b063785aea9338476a66d41be4de4f202013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80bc82eb931201160014bdf260424fd2f322dbec9ce4ddbaed8618d8e959000139ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff6301160014a3f9111f3b0ee96cbd119a3ea5c60058f506fb1900"}'

// Result
{
  "tx_id": "2c0624a7d251c29d4d1ad14297c69919214e78d995affd57e73fbf84ece316cb"
}
```

#### estimate-transaction-gas

估计交易消耗的neu(1BTM = 10^8NEU)。

**参数**

Object:

- Object - transaction_template, 构建交易响应.

**返回**

Object:

- Integer - total_neu,发送交易时总共消耗的neo(1BTM = 10^8NEU),total_neu 是 storage_neu + vm_neu 四舍五入得到的.
- Integer - storage_neu, 存储交易时消耗的neu.
- Integer - vm_neu, 虚拟机执行消耗的neu.

**例子**

```javascript
// Request
curl -X POST estimate-transaction-gas -d '{"transaction_template":{"allow_additional_actions":false,"raw_transaction":"070100010161015ffe8a1209937a6a8b22e8c01f056fd5f1730734ba8964d6b79de4a639032cecddffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8099c4d59901000116001485eb6eee8023332da85df60157dc9b16cc553fb2010002013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80afa08b4f011600142b4fd033bc76b4ddf5cb00f625362c4bc7b10efa00013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8090dfc04a011600146eea1ce6cfa5b718ae8094376be9bc1a87c9c82700","signing_instructions":[{"position":0,"witness_components":[{"keys":[{"derivation_path":["010100000000000000","0100000000000000"],"xpub":"cb4e5932d808ee060df9552963d87f60edac42360b11d4ad89558ef2acea4d4aaf4818f2ebf5a599382b8dfce0a0c798c7e44ec2667b3a1d34c61ba57609de55"}],"quorum":1,"signatures":null,"type":"raw_tx_signature"},{"type":"data","value":"1c9b5c1db7f4afe31fd1b7e0495a8bb042a271d8d7924d4fc1ff7cf1bff15813"}]}]}}'

// Result
{
  "storage_neu": 3840000,
  "total_neu": 5259000,
  "vm_neu": 1419000
}
```

#### create-access-token

创建访问令牌，它为HTTP协议提供基本访问身份验证，返回token包含用户名和密码，它们用冒号分隔。

**参数**

Object:

- String - id, token ID.

optional:

- String - type, token类型.

**返回**

Object:

- String - token, 访问令牌，身份验证用户名和密码（用冒号分隔）.
- String - id, token ID.
- String - type, token类型.
- Object - created_at, token创建时间.


**例子**

创建访问token。

```javascript
// Request
curl -X POST create-access-token -d '{"id":"token1"}'

// Result
{
  "token": "token1:1fee70f537128a201338bd5f25a3adbf33dad02eae4f4c9ac43f336a069df8f3",
  "id": "token1",
  "created_at": "2018-03-20T18:56:01.043919771+08:00"
}
```

#### list-access-tokens

返回所有可用access token的列表。

**参数**

none

**返回**

- Array of Object, 访问令牌数组.
  - Object:
    - String - token, 访问令牌.
    - String - id, 令牌id.
    - String - type, 令牌类型.
    - Object - created_at, 令牌创建时间.

**例子**

列出所有可用的access token。

```javascript
// Request
curl -X POST list-access-tokens -d {}

// Result
[
  {
    "token": "token1:1fee70f537128a201338bd5f25a3adbf33dad02eae4f4c9ac43f336a069df8f3",
    "id": "token1",
    "created_at": "2018-03-20T18:56:01.043919771+08:00"
  },
  {
    "token": "alice:78598c6d9fb9e3258d01f78005d4e5725ad0d45e20af90a30b577b407d4a2edd",
    "id": "alice",
    "created_at": "2018-03-20T18:56:01.043919771+08:00"
  }
]
```

#### delete-access-token

删除已存在的access token.

**参数**

Object:

- String - id, 令牌 id.

**返回**

如果成功删除访问令牌，则为none。

**例子**

删除access token

```javascript
// Request
curl -X POST delete-access-token -d '{"id": "token1"}'

// Result
```

#### check-access-token

检查access token有效。

**参数**

Object:

- String - id, 令牌id.
- String - secret, secret of token, the second part of the colon division for token.

**返回**

如果access token被选中有效，则为none。

**例子**


检查access token是否有效。

```javascript
// Request
curl -X POST check-access-token -d '{"id": "token1", "secret": "1fee70f537128a201338bd5f25a3adbf33dad02eae4f4c9ac43f336a069df8f3"}'

// Result
```

#### create-transaction-feed

创建交易反馈。

**参数**

Object:

- String - alias, 交易反馈的名字.
- String - filter, 交易反馈的过滤器.

**返回**

如果交易反馈已成功创建，返回none。

**例子**

```javascript
// Request
curl -X POST create-transaction-feed -d '{"alias": "test1", "filter": "asset_id='84778a666fe453cf73b2e8c783dbc9e04bc4fd7cbb4f50caeaee99cf9967ebed' AND amount_lower_limit = 50 AND amount_upper_limit = 100"}'

// Result
```

#### get-transaction-feed

按名称查询交易反馈详细信息。

**参数**

Object:

- String - alias, 交易反馈的名字.

**返回**

Object:

- String - id, 交易反馈的id.
- String - alias, 交易反馈的名字.
- String - filter, 交易反馈的过滤器.
- Object - param, 交易反馈的参数.
  - String - assetid, 资产id.
  - Integer - lowerlimit, 资产数额下限.
  - Integer - upperlimit, 资产数额上限.
  - String - transtype, 交易类型.

**例子**

列出别名可用的txfeed：

```javascript
// Request
curl -X POST get-transaction-feed -d '{"alias": "test1"}'

// Result
{
  "alias": "test1",
  "filter": "asset_id='84778a666fe453cf73b2e8c783dbc9e04bc4fd7cbb4f50caeaee99cf9967ebed' AND amount_lower_limit = 50 AND amount_upper_limit = 100",
  "param": {}
}
```

list-transaction-feeds

返回所有可用交易反馈的列表。

**参数**

none

**返回**

- Array of Object, 交易反馈列表.
  - Object:
    - String - id, 交易反馈id.
    - String - alias, 交易反馈名字.
    - String - filter, 交易反馈过滤器.
    - Object - param, 交易反馈参数.
      - String - assetid, 资产id.
      - Integer - lowerlimit, 资产数额下限.
      - Integer - upperlimit, 资产数额上限.
      - String - transtype, 交易类型.

**例子**

列出所有可用的txfeed：

```javascript
// Request
curl -X POST list-transaction-feeds -d {}

// Result
[
  {
    "alias": "test1",
    "filter": "asset_id='84778a666fe453cf73b2e8c783dbc9e04bc4fd7cbb4f50caeaee99cf9967ebed' AND amount_lower_limit = 50 AND amount_upper_limit = 100",
    "param": {}
  },
  {
    "alias": "test2",
    "filter": "asset_id='cee6a588cc3fc280749021ef42d5209952a1e6feceada4e69dd8a424ad22b199' AND amount_lower_limit = 30 AND amount_upper_limit = 100",
    "param": {}
  }
]
```

---

#### delete-transaction-feed

根据交易名字删除交易反馈.

**参数**

Object:

- String - alias, 交易反馈名字.

**返回**

如果交易反馈删除成功，则无返回.

**例子**

```javascript
// Request
curl -X POST delete-transaction-feed -d '{"alias": "test1"}'

// Result
```
update-transaction-feed

更新交易反馈.

**参数**

Object:

- String - alias, 交易反馈名字.
- String - filter, 交易反馈过滤器.

**返回**

如果交易反馈更新成功无返回.

**例子**

当交易反馈存在的时候删除它，并使用别名和过滤器创建它:

```javascript
// Request
curl -X POST update-transaction-feed -d '{"alias": "test1", "filter": "asset_id='84778a666fe453cf73b2e8c783dbc9e04bc4fd7cbb4f50caeaee99cf9967ebed' AND amount_lower_limit = 60 AND amount_upper_limit = 80"}'

// Result
```

---

#### get-unconfirmed-transaction

根据交易ID查询在缓冲池里面查询未确认的交易.

**参数**

Object:

- String - tx_id, 交易id,交易hash.

**返回**

Object:

- String - id, 交易id,交易hash.
- Integer - version, 交易版本.
- Integer - size, 交易大小.
- Integer - time_range, 交易的时间范围.
- Boolean - status_fail, 请求的状态是否失败.
- String - mux_id, 前一笔交易的mux id(钱包启用可以获得，这个地方是空的).
- Array of Object - inputs, 交易的输入对象(输入结构请参考get-transaction API描述).
- Array of Object - outputs, 交易输出对象(输出结构请参考get-transction API描述).

**例子**

```javascript
// Request
curl -X POST get-unconfirmed-transaction -d '{"tx_id": "382090f24fbfc2f737fa7372b9d161a43f00d1c597a7130a56589d1f469d04b5"}'

// Result
{
  "id": "382090f24fbfc2f737fa7372b9d161a43f00d1c597a7130a56589d1f469d04b5",
  "inputs": [
    {
      "address": "bm1qqrm7ruecx7yrg9smtwmnmgj3umg9vcukgy5sdj",
      "amount": 41250000000,
      "asset_definition": {},
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "control_program": "001400f7e1f338378834161b5bb73da251e6d0566396",
      "spent_output_id": "161b44e547a6cc68d732eb64fa38031da98211a99319e088cfe632223f9ac6d8",
      "type": "spend"
    }
  ],
  "mux_id": "0000000000000000000000000000000000000000000000000000000000000000",
  "outputs": [
    {
      "address": "bm1qehxd5cdnepckh5jc72ggn30havd78lsgcqmt7k",
      "amount": 21230000000,
      "asset_definition": {},
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "control_program": "0014cdccda61b3c8716bd258f29089c5f7eb1be3fe08",
      "id": "a8f21ad24689c290634db85278f56d152efe6fe08bc194e5dee5127ed6d3ebee",
      "position": 0,
      "type": "control"
    },
    {
      "address": "bm1q2me9gwccnm3ehpnrcr99gcnj730js2zfucms3r",
      "amount": 20000000000,
      "asset_definition": {},
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "control_program": "001456f2543b189ee39b8663c0ca546272f45f282849",
      "id": "78219e422ea3257aeb32f6d952b5ce5560dab1d6440c9f3aebcdaad2a852d2a8",
      "position": 1,
      "type": "control"
    }
  ],
  "size": 664,
  "status_fail": false,
  "time_range": 0,
  "version": 1
}
```

list-unconfirmed-transactions

返回缓冲池交易的总数和交易ID列表.

**参数**

none

**返回**

Object:

- Integer - total, 交易版本.
- Array of Object - tx_ids, 列出所有交易id.

**例子**

```javascript
// Request
curl -X POST list-unconfirmed-transactions -d {}

// Result
{
  "total": 2,
  "tx_ids": [
    "382090f24fbfc2f737fa7372b9d161a43f00d1c597a7130a56589d1f469d04b5",
    "fc2da5dfa094c2170144f149fa07a312983157aec0ec95063a1319eedcb2d23b"
  ]
}
```

---

#### decode-raw-transaction

反序列化交易十六进制字符串为json对象.

**参数**

Object:

- String - raw_transaction, 交易的序列化字符串.

**返回**

Object:

- Integer - version,交易版本.
- String - size, 交易大小.
- String - time_range, 交易时间范围.
- String - fee, 发送交易的反馈.
- Array of Object - inputs, 交易的输入对象(输入结构请参考get-transaction API描述).
- Array of Object - outputs, 交易输出对象(输出结构请参考get-transction API描述).

**例子**

```javascript
// Request
curl -X POST decode-raw-transaction -d '{"raw_transaction": "070100010161015fc8215913a270d3d953ef431626b19a89adf38e2486bb235da732f0afed515299ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8099c4d59901000116001456ac170c7965eeac1cc34928c9f464e3f88c17d8630240b1e99a3590d7db80126b273088937a87ba1e8d2f91021a2fd2c36579f7713926e8c7b46c047a43933b008ff16ecc2eb8ee888b4ca1fe3fdf082824e0b3899b02202fb851c6ed665fcd9ebc259da1461a1e284ac3b27f5e86c84164aa518648222602013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80bbd0ec980101160014c3d320e1dc4fe787e9f13c1464e3ea5aae96a58f00013cffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8084af5f01160014bb93cdb4eca74b068321eeb84ac5d33686281b6500"}'

// Result
{
  "fee": 20000000,
  "inputs": [
    {
      "address": "tm1q26kpwrrevhh2c8xrfy5vnaryu0ugc97c4yxp66",
      "amount": 41250000000,
      "asset_definition": {},
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "control_program": "001456ac170c7965eeac1cc34928c9f464e3f88c17d8",
      "spent_output_id": "01bb3309666618a1507cb5be845b17dee5eb8028ee7e71b17d74b4dc97085bc8",
      "type": "spend"
    }
  ],
  "outputs": [
    {
      "address": "tm1qc0fjpcwuflnc06038s2xfcl2t2hfdfv06pfd70",
      "amount": 41030000000,
      "asset_definition": {},
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "control_program": "0014c3d320e1dc4fe787e9f13c1464e3ea5aae96a58f",
      "id": "567b34857614d16292220beaca16ce34b939c75023a49cc43fa432fff51ca0dd",
      "position": 0,
      "type": "control"
    },
    {
      "address": "tm1qhwfumd8v5a9sdqepa6uy43wnx6rzsxm9uhmk4q",
      "amount": 200000000,
      "asset_definition": {},
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "control_program": "0014bb93cdb4eca74b068321eeb84ac5d33686281b65",
      "id": "a8069d412e48c2b2994d2816758078cff46b215421706b4bad41f72a32928d92",
      "position": 1,
      "type": "control"
    }
  ],
  "size": 332,
  "time_range": 0,
  "version": 1
}
```

#### get-block-count

返回当前区块链的高度.

**参数**

none

**返回**

Object:

- Integer - block_count, 当前的区块高度.

**例子**

```javascript
// Request
curl -X POST get-block-count

// Result
{
    "block_count": 519
}
```

#### get-block-hash

返回当前区块的hash.

**参数**

none

**返回**

Object:

- String - block_hash, 最近的区块hash.

**例子**

```javascript
// Request
curl -X POST get-block-hash

// Result
{
  "block_hash": "997bf5cecb4df097991a7a121a7fd3cb5a404fa856e3d6032c791ac07bc7c74c"
}
```

#### get-block

区块高度或块哈希返回详细信息块。

**参数**

Object: block_height | block_hash

optional:

- String - block_hash, 区块hash.
- Integer - block_height, 区块高度.

**返回**

Object:

- String - hash, 区块hash.
- Integer - size, 区块大小.
- Integer - version, 区块版本.
- Integer - height, 区块高度.
- String - previous_block_hash, 前一个区块hash.
- Integer - timestamp, 区块时间戳.
- Integer - nonce, 随机数值.
- Integer - bits, 区块难度.
- String - difficulty, 难度值(String type).
- String - transaction_merkle_root, 交易的默克尔根.
- String - transaction_status_hash, 交易状态的默克尔根.
- Array of Object - transactions, 交易对象:
  - String - id, transaction id, 交易hash.
  - Integer - version, 交易版本.
  - Integer - size, 交易大小.
  - Integer - time_range, 响应请求时的unix时间戳.
  - Boolean - status_fail, 请求状态是否是失败的.
  - String - mux_id, 前一笔交易的mux id(utxo的源ID).
  - Array of Object - inputs, 交易对象的输入.
    - String - type, 输入操作的类型，可用选项包括: 'spend', 'issue', 'coinbase'.
    - String - asset_id, 资产id.
    - String - asset_alias, 资产名字.
    - Object - asset_definition, 资产定义(json 对象).
    - Integer - amount, 资产数量.
    - Object - issuance_program, issuance program, 当交易类型为'issue'.
    - Object - control_program, control program of account, 当交易类型为'spend'.
    - String - address, 账户地址, 当类型是'spend'时存在.
    - String - spent_output_id, the front of outputID to be spent in this input, 当交易类型为'spend'.
    - String - account_id, 账户id.
    - String - account_alias, 账户名字.
    - Object - arbitrary, 任意信息都可以由矿工设置，当交易类型为'coinbase'时.
  - Array of Object - outputs, 交易输出对象.
    - String - type, 输出action的类型，可选项包括:"retire","control".
    - String - id, 与utxo相关的交易输出id.
    - Integer - position, 输出的位置.
    - String - asset_id, 资产id.
    - String - asset_alias, 资产名字.
    - Object - asset_definition, 资产定义(json 对象).
    - Integer - amount, 资产数量.
    - String - account_id, 账户id.
    - String - account_alias, 账户名字.
    - Object - control_program, 账户的控制程序.
    - String - address, 账户地址.

**例子**

通过block_hash或block_height获取指定的块信息，如果两者都存在，则块结果通过哈希查询。

```javascript
// Request
curl -X POST get-block -d '{"block_height": 43, "block_hash": "886a8e85b275e7d65b569ba510875c0e63dece1a94569914d7624c0dac8002f9"}'

// Result
{
  "bits": 2305843009222082600,
  "difficulty": "5549086336",
  "hash": "886a8e85b275e7d65b569ba510875c0e63dece1a94569914d7624c0dac8002f9",
  "height": 43,
  "nonce": 3,
  "previous_block_hash": "2c72ccbd53b92a4f9423c5b610b4e106bbe8fbf3ecf2e16a1266b17ee323ff9d",
  "size": 386,
  "timestamp": 1521614189,
  "transaction_merkle_root": "77d40262cfeca3a16d4587132974552ef00951e43ce567a26801ebc3dbdb4d96",
  "transaction_status_hash": "53c0ab896cb7a3778cc1d35a271264d991792b7c44f5c334116bb0786dbc5635",
  "transactions": [
    {
      "id": "4576b1b1ec251da3263dbdd5486bcbf9a1cd1f712172dbe7a7a5fe46ab194629",
      "inputs": [
        {
          "amount": 0,
          "arbitrary": "09",
          "asset_definition": "7b7d",
          "asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
          "type": "coinbase"
        }
      ],
      "mux_id": "2383cefe8a34ea5810cc0706f2cf8cf08a106f90fc3eb3441f723cecdbc61331",
      "outputs": [
        {
          "address": "sm1q4pkg8msjumtep7ecsdzuct3tsuzk5pmnm3p8nr",
          "amount": 624000000000,
          "asset_definition": "7b7d",
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "control_program": "0014f3403bcd8b443d03882a280b50f6f98986e1a255",
          "id": "da87b40854a9b93be72ecdc24fe7bb03986ea3530e344b0f918f0788c3d83717",
          "position": 0,
          "type": "control"
        }
      ],
      "size": 77,
      "status_fail": false,
      "time_range": 0,
      "version": 1
    }
  ],
  "version": 1
}
```

---

#### get-block-header

按块高度或块哈希返回详细信息块标头。

**参数**

Object: block_height | block_hash

optional:

- String - block_hash, 区块hash.
- Integer - block_height, 区块高度.

**返回**

Object:

- Object - block_header, 区块头.
- Integer - reward, 区块奖励.


**例子**

```javascript
// Request
curl -X POST get-block-header -d '{"block_height": 43, "block_hash": "886a8e85b275e7d65b569ba510875c0e63dece1a94569914d7624c0dac8002f9"}'

// Result
{
  "block_header": "01019601e87da37e7d73f31d54304c719c9058ec7bc7de7819deda89a7c8834a99bc05b8fbdbe6d60540eba9e5d5cb79fd87b3c0fad32f6772c1e4483f2a070e093a6176d85226d986a8c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc00ad918480808080801e",
  "reward": 41250000000
}
```

#### get-difficulty

通过块高度或块散列返回块高度，当请求为空时返回当前块高度。

**参数**

Object:

optional:

- String - block_hash, 区块hash.
- Integer - block_height, 区块高度.

**返回**

Object:

- Integer - bits, 区块的位.
- String - difficulty, 区块难度.
- String - hash, 区块hash.
- Integer - height, 区块高度.

**例子**

为当前块或指定的块散列/高度获取困难。

```javascript
// Request
curl -X POST get-difficulty -d '{"block_height": 506, "block_hash": "d1fce60caea5466eae2b812e4586b55120c52aca27b6c92bd7c51e9cda82dcdf"}'

// Result
{
  "bits": 2161727821137910500,
  "difficulty": "15154807",
  "hash": "d1fce60caea5466eae2b812e4586b55120c52aca27b6c92bd7c51e9cda82dcdf",
  "height": 506
}
```

#### get-hash-rate

通过块高度或块散列返回块散列率，当请求为空时，它返回当前块散列率。

**参数**

Object:

optional:

- String - block_hash, 区块hash.
- Integer - block_height, 区块高度.

**返回**

Object:

- Integer - hash_rate, 区块的出块难度.
- String - hash, 区块 hash.
- Integer - height, 区块 height.

**例子**

获取当前块或指定块散列/高度的哈希率。

```javascript
// Request
curl -X POST get-hash-rate -d '{"block_height": 506, "block_hash": "d1fce60caea5466eae2b812e4586b55120c52aca27b6c92bd7c51e9cda82dcdf"}'

// Result
{
  "hash": "d1fce60caea5466eae2b812e4586b55120c52aca27b6c92bd7c51e9cda82dcdf",
  "hash_rate": 7577403,
  "height": 506
}
```

#### net-info

返回当前网络节点的信息。

**参数**

无

**返回**

Object:

- Boolean - listening, 节点是否正在监听.
- Boolean - syncing, 节点是否正在同步.
- Boolean - mining, 节点是否在挖矿.
- Integer - peer_count, 当前节点连接数.
- Integer - current_block, 当前节点中链的高度.
- Integer - highest_block, 当前节点的最新区块高度.
- String - network_id, 网络id.
- String - version, 比原版本.

**例子**

```javascript
// Request
curl -X POST net-info

// Result
{
  "listening": true,
  "syncing": true,
  "mining": true,
  "peer_count": 0,
  "current_block": 627,
  "highest_block": 0,
  "network_id": "mainnet",
  "version": "0.5.0"
}
```

#### is-mining

返回挖矿状态。

**参数**

无

**返回**

Object:

- Boolean - is_mining, 节点是否启动了挖矿.

**例子**

```javascript
// Request
curl -X POST is-mining

// Result
{
  "is_mining": true
}
```

#### set-mining

启动节点。

**参数**

Object:

- Boolean - is_mining, 节点是否启动了挖矿.

**例子**

```javascript
// Request
curl -X POST set-mining -d '{"is_mining": true}'

// Result
```

#### gas-rate

gas消耗率。

**参数**

无

**返回**

Object:

- Integer - gas_rate, gas 费率.

**例子**

```javascript
// Request
curl -X POST gas-rate -d '{}'

// Result
{
  "gas_rate": 1000
}
```

#### verify-message

用地址派生的pubkey验证已签名的消息。

**参数**

Object:

- String - address, 账户地址.
- String - derived_xpub, 派生的xpub.
- String - message, 来自derived_xpub的签名消息.
- String - signature, 消息签名.

**返回**

Object:

- Boolean - result, 验证结果.

**例子**

```javascript
// Request
curl -X POST verify-message -d '{"address":"bm1qx2qgvvjz734ur8x5lpfdtlau74aaa5djs0a5jn", "derived_xpub":"6ff8c3d1321ce39a3c3550f57ba70b67dcbcef821e9b85f6150edb7f2f3f91009e67f3075e6e76ed5f657ee4b1a5f4749b7a8c74c8e7e6a1b0e5918ebd5df4d0", "message":"this is a test message", "signature":"74da3d6572233736e3a439166719244dab57dd0047f8751b1efa2da26eeab251d915c1211dcad77e8b013267b86d96e91ae67ff0be520ef4ec326e911410b609"}'

// Result
{
  "result": true
}
```

#### decode-program

解码程序。

**参数**

Object:

- String - program, 账户程序.

**返回**

Object:

- String - instructions, 程序的说明和数据.

**例子**

```javascript
// Request
curl -X POST decode-program -d '{"program":"0014a86c83ee12e6d790fb388345cc2e2b87056a0773"}'

// Result
{
  "instructions": "DUP \nHASH160 \nDATA_20 a86c83ee12e6d790fb388345cc2e2b87056a0773\nEQUALVERIFY \nTXSIGHASH \nSWAP \nCHECKSIG \n"
}
```

#### get-work

获得工作证明。

**参数**

none

**返回**

Object:

- Object - block_header, 原始区块头.
- Object - seed, 随机种子.

```go
type GetWorkResp struct {
	BlockHeader *types.BlockHeader `json:"block_header"`
	Seed        *bc.Hash           `json:"seed"`
}
```

**例子**

```javascript
// Request
curl -X POST get-work -d '{}'

// Result
{
  "block_header": "0101870103f2c7495164c8f3af43697e81faa21dcb2d60aa5e10ce4f233491e62420742fbeadfcd50540bef2670a5fade2e58ad4955e2375a04ad1e4cb9c104faddab43f4a79e35be253c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc00ffffff838080808020",
  "seed": "702bef3f1707577fd0d75b6359a2919fa216487fe306771e27710acbaa9164ce"
}
```

#### submit-work

提交工作证明。

**参数**

Object:

- Object - block_header, 原始区块头.

```go
type SubmitWorkReq struct {
	BlockHeader *types.BlockHeader `json:"block_header"`
}
```

**返回**

如果成功返回true

**例子**

```javascript
// Request
curl -X POST submit-work -d '{"block_header": "0101870103f2c7495164c8f3af43697e81faa21dcb2d60aa5e10ce4f233491e62420742fbeadfcd50540bef2670a5fade2e58ad4955e2375a04ad1e4cb9c104faddab43f4a79e35be253c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc00ffffff838080808020"}'

// Result
true / error
```

