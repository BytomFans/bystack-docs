---
id: docs_47
title: Bytom API
sidebar_label: Bytom API
---

#### Bapp开发者通用指南

**初始化 Bapps**

一旦建立了基本的开发环境，就可以开始与byone或bycoin进行交互了。在发送交易的时候，无论你需要调用那种依赖库都非常方便。

**检查bytom环境**

You can detect bytom browser by !!window.bytom, it returns true if current browser is bytom DApp browser.
你可以通过!window.bytom检查bytom浏览器，如果当前的浏览器是比原的Bapp浏览器，返回true.

#### API

**bytom.send_transaction**

发送正常的事物
发送参数对象：

| Params | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| from | String | true | null | sender account id or guid |
| to | String | true | null | the receiver address |
| amount | Number | true | null | the amount that do the transaction, in the atom mode |
| asset | String | true | null | asset Id |
| confirmations | Number | false | 1 | confirmations |


相应对象:

| property | Type | Description |
| --- | --- | --- |
| transaction_hash | String | a successful transaction hash |


```javascript
var params = {
   "to":"tm1qv3mjueqvdael5m706axlzu0mx9evdy4exrfkt0",
   "from": window.bytom.default_account.accountId,
   "amount":20000000,
   "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
}

window.bytom.send_transaction(params).then(function (resp) {
    alert(resp)
  }).catch(function (err){
    alert(err)
  })
```

**bytom.send_advanced_transaction**

发送高级交易或合约交易。

发送参数对象：

| Params | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| input | Array of Object | true | null | type: "spend_utxo", |
| type: "spend_wallet" | suggest that the Dapp developer should put spend_wallet at the very end | since spend_wallet will encounter multiple UTXO |  |  |
| output | Array of Object | true | null | type:"control_address" |
| gas | Number | true | null | gas should be in the atom mode |
| args | Array of Object | false | null | arg type. Args needed for contract transaction |
| confirmations | Number | false | 1 | confirmations |



输入的例子:

```javascript
[ 
   { 
      "type":"spend_utxo",
      "output_id":"37921eee9453d2598b6357dfeddd2cf351850f5be917bdac0d1cf208859ece06"
   },
   { 
      "type":"spend_wallet",
      "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount":10000000000
   }
]
```

输出的例子:

```javascript
[ 
   { 
      "type":"control_address",
      "amount":99,
      "asset":"3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680",
      "address":"bm1q50u3z8empm5ke0g3ngl2t3sqtr6sd7cepd3z68"
   },
   { 
      "type":"control_program",
      "amount":99,
      "asset":"3152a15da72be51b330e1c0f8e1c0db669269809da4f16443ff266e07cc43680",
      "control_program":"bm1q50u3z8empm5ke0g3ngl2t3sqtr6sd7cepd3z68"
   }
]
```

参数的例子:

```javascript
[ 
   { 
      "type":"data",
      "raw_data":{ 
         "value":"ba5a63e7416caeb945eefc2ce874f40bc4aaf6005a1fc792557e41046f7e502f"
      }
   },
   { 
      "type":"integer",
      "raw_data":{ 
         "value":100
      }
   },
   { 
      "type":"String",
      "raw_data":{ 
         "value":"String"
      }
   },
   { 
      "type":"boolean",
      "raw_data":{ 
         "value":true
      }
   },
   { 
      "type":"address",
      "raw_data":{ 
         "value":"bm1q5u8u4eldhjf3lvnkmyl78jj8a75neuryzlknk0"
      }
   }
]
```

相应的对象:

| property | Type | Description |
| --- | --- | --- |
| transaction_hash | String | a successful transaction hash |


```javascript
var params = { 
   "input":[ 
      { 
         "amount":1750000000,
         "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
         "type":"spend_wallet"
      }
   ],
   "output":[ 
      { 
         "amount":1750000000,
         "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
         "address":"tm1qv3mjueqvdael5m706axlzu0mx9evdy4exrfkt0",
         "type":"control_address"
      }
   ],
   "gas":400000
}

window.bytom.send_advanced_transaction(params).then(function (resp) {
    alert(resp)
  }).catch(function (err){
    alert(err)
  })
```

**bytom.sign_message**

使用当前账户的地址对消息签名

| Params | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| address | String | true | null | Account address |
| message | String | true | null | Message that want to sign |


相应对象:

| property | Type | Description |
| --- | --- | --- |
| signature | String | Sign successful will return an signature |


```javascript
var params = {
   address:window.bytom.default_account.address,
   message:"Hello World"
}

window.bytom.sign_message(params).then(function (resp) {
    alert(resp)
  }).catch(function (err){
    alert(err)
  })
```

**bytom.default_account**

返回当前账户信息

返回一个账户的信息:

| property | Type | Description |
| --- | --- | --- |
| address | String | account address |
| alias | String | account alias |
| balances | Array of Object | Object with asset, balance, availableBalance(**only in Vapor Chain**), decimals and alias properties. |
| accountId | String | Account ID |
| rootXpub | String | account root xpub |
| derivedXpub | String | account derived xpub |


返回格式示例:

```javascript
{ 
   "address":"tm1q2pd6vkw60a6lmyh5mqjkxmdqrxvmweh5dts0xk",
   "alias":"test",
   "balances":[ 
      { 
         "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
         "balance":1489673000,
         "availableBalance":1389673000, // available only in Vapor Chain
        "decimals":8,
         "alias":"BTM",
         "symbol":"BTM"
      }
   ],
   "accountId":"c54475ee-2548-4a6d-b327-2e740bece2c3",
   "rootXPub":"404f3826f69aebef28a47b1eda...",
   "derivedXpub":"xxxxxxxx..."
}
```

**bytom.net**

返回当前比原网络ID，例如:"testnet"

**bytom.chain**

返回链类型。例如“bytom”或“vapor”
