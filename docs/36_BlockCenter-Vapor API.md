---
id: docs_36
title: BlockCenter-Vapor API
sidebar_label: BlockCenter-Vapor API
---

### BlockCenter-Vapor API

**BaseURL：[https://bcapi.bystack.com/api/v2/vapor](https://bcapi.bystack.com/api/v2/vapor)**
**

---

#### Account

**/account/copy 【POST】**

Create a wallet using a public key. Each wallet is identified by a guid.

**Body JSON:**

Object:

- String- guid, 钱包guid.
- String- coin, 指定资产.

```json
{
    "guid":"b2889fc3-444e-447b-8d2b-69e357bdd341",
    "coin":"btm"
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Object- data, 返回结果数据
    - String- guid, 钱包唯一标识.
    - String- address, 钱包的Bytom地址.
    - String- label, 地址的标签.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "data":{
            "guid":"b2889fc3-444e-447b-8d2b-69e357bdd341",
            "address":"vp1qlldkdzrwqdd3rg037zxaeqcwnpdfqdjj040gnf",
            "label":"mock"
        }
    }
}
```

**/account/list-addresses 【POST】**

List all addresses and the corresponding balances of a wallet.

**Body JSON:**

Object(必选):

- String- guid, 钱包的唯一标识.

Optional(可选):

- `Integer`- start, 开始数量，默认0.
- `Integer`- _limit_, 查询数量，默认10.

```json
{
    "guid":"e276c676-ad38-4879-9790-87e5a1be1111"
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - String- _links, 下一页的URL
  - Object- data, 返回结果数据
    - String- guid, 钱包唯一标识.
    - String- address, 钱包的Bytom地址.
    - String- label, 地址的标签.
    - Array of object- balances, 金额相关信息
      - String- asset, 资产的唯一标识.
      - String- balance, 资产金额数.
      - String- total_received, 资产总接收金额数.
      - String- total_sent,资产总发送金额数.
      - Integer- decimals, 资产精度.
      - String- alias, 资产别名.
      - Integer- symbol, 资产标识名.
      - String- icon, 资产图标.
      - String- in_usd, 资产转换成usdt的价值.
      - String- in_cny, 资产转换成cny的价值.
      - String- in_btc, 资产转换成btc的价值.
    - Array of object- votes, 投票信息
      - String- vote, 投给的公钥.
      - String- total, 投票数.
      - String- locked, 锁定的金额.
  - Integer- start, 分页开始数.
  - Integer- limit, 分页结束数.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "_links":{

        },
        "data":[
            {
                "guid":"b5fe58d1-f065-414d-82c8-9be117bd6d66",
                "address":"tm1qz0vktvj0tv7wfkdku93pzul90stp5fefrkxcgm",
                "label":"mock",
                "balances":[
                    {
                        "asset":"a853110464934ba4cb35f1f98512f9f8700ae24da4bf18eb8b216bb38a3ffd1f",
                        "balance":"42400000001",
                        "total_received":"44400000000",
                        "total_sent":"1999999999",
                        "decimals":8,
                        "icon": "icon.jpg",
                        "alias":"btm",
                        "symbol": "btm",
                        "in_usd":"0.00",
                        "in_cny":"0.00",
                        "in_btc":"0.000000"
                    },
                    {
                        "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "balance":"3262476859534000",
                        "total_received":"3263535568514000",
                        "total_sent":"1058708980000",
                        "decimals":8,
                        "icon": "icon.jpg",
                        "alias":"btm",
                        "symbol": "btm",
                        "in_usd":"2424020.31",
                        "in_cny":"16725740.12",
                        "in_btc":"704.042506"
                    }
                ],
                "votes":[
                    {
                        "vote":"0f17286fb02772c606e5fb504194426494aae786602821e37bcebe7a17f61d753796848dce2e0469bb01d21d3aecc1dba95fabbc1fa16aa332b75ac723e8038d",
                        "total":500000000000,
                        "locked":350000000000
                    }
                ]
            }
        ],
        "limit":10,
        "start":0
    }
}
```

**/account/list-wallets 【POST】**

List created wallet of a public key and coin pair.
**Body JSON:**

Object(必选):

- String- pubkey, 公钥.

Optional(可选):

- `Integer`- start, 开始数量，默认0.
- `Integer`- _limit_, 查询数量，默认10.
- Object- filter, 过滤条件.
  - String- type, 地址类型, pay-to-pubkey (P2PK), pay-to-pubkey-hash (P2PKH), and pay-to-script-hash (P2SH).
  - String- guid, 钱包guid.
  - String- account_index, 账户索引

```json
{
    "pubkey":"82b5c28d07a5f7442d564d25b2f95a9bf46c826bff4b27593f37250cd9639a797d1f501b69f895b830138fabc6f91e2b3b3c8df26642a34be4af27886b9134dc",
    "filter":{
        "type":"P2PKH",
        "guid":"mock",
        "account_index":1
    }
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - String- _links, 下一页的URL
  - Array of object- data, 返回结果数据
    - String- access_token, 加入多钱钱包的验证码.
    - String- guid, 钱包唯一标识.
    - Array of String- address, 钱包的Vapor地址.
    - String- status, 结果状态，0 等待，1 完成.
    - Array of object- signers, 最小签名数.
      - String- pubkey, 签名者的公钥（唯一）.
      - String- name, 签名者的名字（唯一）.
  - Integer- start, 分页开始数.
  - Integer- limit, 分页结束数.

```json
{
    "code": 200,
    "msg": "",
    "result": {
        "_links": {},
        "data": [
            {
                "access_token": "",
                "guid": "78e2afde-7073-4f1f-ada6-fe2c7dc634b3",
                "m": 1,
                "n": 1,
                "status": 1,
                "signers": [
                    {
                        "pubkey": "b7f463446a31b3792cd168d52b7a89b3657bca3e25d6854db1488c389ab6fc8d538155c25c1ee6975cc7def19710908c7d9b7463ca34a22058b456b45e498db9",
                        "name": ""
                    }
                ],
                "addresses": [
                    "tp1qqqztfvm47h6rwhv82al87rq3w26npr5r9jhm3q",
                    "tp1q2dydpj7ewm408tdd3v8awc5f6ehszszd0hxu9f"
                ]
            }
        ],
        "limit": 10,
        "start": 0
    }
}
```

**/account/new-address 【POST】**

Create a new address for a wallet.

**Body JSON:**

Object(必选):

- String- guid, 钱包的唯一标识.

Optional(可选):

- String- label, 地址标签.

```json
{
    "guid":"e276c676-ad38-4879-9790-87e5a1be1111",
    "label":""2nd addr""
}
```

**Result JSON:**

Object

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Object- data, 返回结果数据
    - String- guid, 钱包唯一标识.
    - String- address, 钱包的Vapor地址.
    - String- label, 地址的标签.

```json
{
    "code":200,
    "msg":"",
    "result":{
        "data":{
            "guid":"e276c676-ad38-4879-9790-87e5a1be1111",
            "address":"tm1qeuwdh3d4crfzatya9y3gpvnu4vtqrdhk5ayn39",
            "label":"2nd addr"
        }
    }
}
```

---

#### Merchant

**/merchant/build-payment 【POST】**

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.
- String- asset, 资产类型.
- Object- recipients, 接收地址和金额组成的键值对.
- Bool- forbid_chain_tx, 是否禁止链式交易，true 禁止，false 非禁止.
- String- memo, 备注.
- String- confirmations, 确认需要的交易确认数.

```json
{
    "guid":"b5fe58d1-f065-414d-82c8-9be117bd6d66",
    "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "confirmations":1,
    "memo":"mock",
    "recipients": {
    	"tm1q9jpad933ddjfm3ryena49f0fajg955ckjnjuhn": 10000000000
    },
    "forbid_chain_tx": false,
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - String- _links, 下一页的URL
  - Array of object- data, 返回结果数据
    - String- tx, 构建的交易信息.
      - String- hash, 交易哈希.
      - Bool- status_fail, 构建交易状态（成功 false，失败true）.
      - Integer- size, 交易大小.
      - String- memo,  交易备注.
      - Integer- submission_timestamp, 构建时间.
      - Array of Object- inputs, 交易输入.
      - Array of Object- outputs, 交易输出.
      - Array of Object- balances, 交易资产金额信息.
      - Bool- status_fail, 构建交易状态（成功 false，失败true）.
      - Array of string- types, 交易类型.
    - String- raw_transaction, 原生交易信息.
    - Integer-fee, 交易费.
    - String- signing_instructions, 签名信息.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "data":[
        	{
                "tx":{
                    "inputs":[
                        {
                            "script":"00147a8a16c665568c2f52e33c5fb373a3bd1fb7f2c1",
                            "address":"tm1q029pd3n926xz75hr830mxuarh50m0ukpw8g27u",
                            "asset":"0000000000000000000000000000000000000000000000000000000000000000",
                            "amount":0,
                            "type":"spend"
                        }
                    ],
                    "outputs":[
                        {
                            "script":"00147a8a16c665568c2f52e33c5fb373a3bd1fb7f2c1",
                            "address":"tm1q029pd3n926xz75hr830mxuarh50m0ukpw8g27u",
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":41250000000,
                            "type":"control"
                        }
                    ],
                    "fee":0,
                    "balances":[
                        {
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":"41250000000"
                        }
                    ],
                    "hash":"eb4daea5edc379b6ab024c0e4ef963feafe6affcfe581d66f7ad3ec6d1828463"
                },
                "raw_transaction":"070100010160015e5978c52e0508cbf1cd901919277e4dba80fb4440b4771bbaa3b6c483f9264d21ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8094ebdc0301011600143c21d88332683d060ccf905a0f26ce82907ac132220120f3a597b7a1f8b7b210790d5ceef145ae8616d025a6fdb4aec67338bf937af6b9020139ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff6401160014f0928dc5f8878a4289b981d7c66386ff74be7fc300013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffb4dfcfdc03011600143c21d88332683d060ccf905a0f26ce82907ac13200",
                "signing_instructions":[
                    {
                        "derivation_path":[
                            "2c000000",
                            "99000000",
                            "01000000",
                            "00000000",
                            "01000000"
                        ],
                        "sign_data":[
                            "b9fb5dc0e7b7ee0e9c252439ec32fe6b222e0d170aee6b562634dfad3638560e"
                        ]
                    }
                ]
        	}
        ]
    }
}
```

**/merchant/get-transaction 【POST】**

Get the transaction by tx_id

**Body JSON:**

Object(必选):

- String- tx_id, 交易哈希.

```javascript
{ "tx_id":"0f44a0af521b7993e72512a2fadc9736aaaf70f67fab16c7ae3f55494a7a3536" }
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Array of object- data, 返回结果数据
    - String- hash, 交易哈希.
    - Bool- status_fail, 构建交易状态（成功 false，失败true）.
    - Integer- size, 交易大小.
    - String- memo,  交易备注.
    - Integer- submission_timestamp, 构建时间.
    - Array of Object- inputs, 交易输入.
    - Array of Object- outputs, 交易输出.
    - Integer- block_height, 交易所属的区块高度.
    - Integer- block_timestamp, 交易所属的区块时间戳.
    - Array of Object- balances, 交易资产金额信息.
    - Bool- status_fail, 构建交易状态（成功 false，失败true）.
    - Integer-fee, 交易费.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "data":{
            "hash":"7de59581513c471ef4cf2cd205de2608d2d7e9dd1d598251d75f8df112bd1ecd",
            "status_fail":false,
            "size":2231,
            "submission_timestamp":1559720537,
            "block_height":195118,
            "block_timestamp":1559720542,
            "memo":"mock",
            "inputs":[
                {
                    "script":"2097c36cdeb2fb82c971580f03d632ad5c09187d35e0e3e937cf9fd4894c35dd01160014489ac931be795efecb50a9bd2fcd569425f00be4060040e59c30120600a0724e1809202f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f4dfc01567a64f5010000c358797ca153795579a19a6957790400e1f5059653790400e1f505967c00a07c00a09a69c358797c9f91616429010000005879c2547951005b79895a7989597989587989537a894c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac890274787e008901c07ec169515879c2515a79c16952c3597994c251005a79895979895879895779895679895579890274787e008901c07ec16963f0010000005879c2547951005b79895a7989597989587989537a894c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac890274787e008901c07ec16951c3c2515a79c16963fc010000567a567aae7cac747800c0",
                    "address":"smart contract",
                    "asset":"80c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba3400",
                    "amount":9995000000000,
                    "type":"spend"
                },
                {
                    "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                    "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                    "asset":"2f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f",
                    "amount":1000000000,
                    "type":"spend"
                },
                {
                    "vote":"50ef22b3a3fca7bc08916187cc9ec2f4005c9c6b1353aa1decbd4be3f3bb0fbe1967589f0d9dec13a388c0412002d2c267bdf3b920864e1ddc50581be5604ce1",
                    "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                    "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                    "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount":1000000000,
                    "type":"veto"
                },
                {
                    "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount":1000000000,
                    "type":"crosschain_input"
                }
            ],
            "outputs":[
                {
                    "utxo_id":"b4645bf3232bbb971fd7378d41bd74134a43cc7c108cce53e7fc045f641b3159",
                    "script":"2097c36cdeb2fb82c971580f03d632ad5c09187d35e0e3e937cf9fd4894c35dd01160014489ac931be795efecb50a9bd2fcd569425f00be4060040e59c30120600a0724e18092080c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba34004c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac747800c0",
                    "address":"smart contract",
                    "asset":"2f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f",
                    "amount":1000000000,
                    "type":"control"
                },
                {
                    "utxo_id":"3beb18d5e24f38c2742fca12e5468a8a36551607288fcc2d0799f09da182d645",
                    "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                    "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                    "asset":"80c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba3400",
                    "amount":1000000000,
                    "type":"control"
                },
                {
                    "vote":"50ef22b3a3fca7bc08916187cc9ec2f4005c9c6b1353aa1decbd4be3f3bb0fbe1967589f0d9dec13a388c0412002d2c267bdf3b920864e1ddc50581be5604ce1",
                    "utxo_id":"28d807f13c854fc07639539cf4442a42e32dc33ed9f7becebf061c6af044f153",
                    "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                    "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                    "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount":960000000,
                    "type":"vote"
                },
                {
                    "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                    "address":"bm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                    "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount":1000000000,
                    "type":"crosschain_output"
                }
            ],
            "fee":40000000,
            "balances":[
                {
                    "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount":"41250000000"
                }
            ]
        }
    }
}
```

---

**/merchant/list-transactions 【POST】**

List all the transactions related to a wallet or an address.

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.

Optional(可选):

- `Integer`- start, 开始数量，默认0.
- `Integer`- _limit_, 查询数量，默认100.
- Object- filter, 过滤条件.
  - String- asset_id, 资产ID.
  - String- tx_types, 交易类型 ["veto"/投票, "vote"/取消投票, "in_crosschain"/跨链输入, "out_crosschain"/跨链输出].
  - Object- sort, 排序方式.
    - String- by, 排序的键.
    - String- order, 顺序 desc，asc.

```json
{
    "guid":"b5fe58d1-f065-414d-82c8-9be117bd6d66",
    "filter":{
        "asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "tx_types": ["veto"] // 支持"veto", "vote", "in_crosschain", "out_crosschain"
    },
    "sort":{
        "by":"amount",
        "order":"desc"
    }
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - String- _links, 下一页的URL
  - Array of object- data, 返回结果数据
    - String- hash, 交易哈希.
    - Bool- status_fail, 构建交易状态（成功 false，失败true）.
    - Integer- size, 交易大小.
    - String- memo,  交易备注.
    - Integer- submission_timestamp, 构建时间.
    - Array of Object- inputs, 交易输入.
    - Array of Object- outputs, 交易输出.
    - Array of Object- balances, 交易资产金额信息.
    - Bool- status_fail, 构建交易状态（成功 false，失败true）.
    - Array of string- types, 交易类型.
    - Integer-fee, 交易费.
  - `Integer`- start, 开始数量.
  - `Integer`- _limit_, 查询数量.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "_links":{
            "next":"/api/v1/vapor/merchant/list-transactions?limit=100&start=100"
        },
        "data":[
            {
                "hash":"7de59581513c471ef4cf2cd205de2608d2d7e9dd1d598251d75f8df112bd1ecd",
                "status_fail":false,
                "size":2231,
                "submission_timestamp":1559720537,
                "block_height":195118,
                "block_timestamp":1559720542,
                "memo":"mock",
                "inputs":[
                    {
                        "script":"2097c36cdeb2fb82c971580f03d632ad5c09187d35e0e3e937cf9fd4894c35dd01160014489ac931be795efecb50a9bd2fcd569425f00be4060040e59c30120600a0724e1809202f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f4dfc01567a64f5010000c358797ca153795579a19a6957790400e1f5059653790400e1f505967c00a07c00a09a69c358797c9f91616429010000005879c2547951005b79895a7989597989587989537a894c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac890274787e008901c07ec169515879c2515a79c16952c3597994c251005a79895979895879895779895679895579890274787e008901c07ec16963f0010000005879c2547951005b79895a7989597989587989537a894c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac890274787e008901c07ec16951c3c2515a79c16963fc010000567a567aae7cac747800c0",
                        "address":"smart contract",
                        "asset":"80c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba3400",
                        "amount":9995000000000,
                        "type":"spend"
                    },
                    {
                        "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                        "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                        "asset":"2f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f",
                        "amount":1000000000,
                        "type":"spend"
                    },
                    {
                        "vote":"50ef22b3a3fca7bc08916187cc9ec2f4005c9c6b1353aa1decbd4be3f3bb0fbe1967589f0d9dec13a388c0412002d2c267bdf3b920864e1ddc50581be5604ce1",
                        "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                        "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                        "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "amount":1000000000,
                        "type":"veto"
                    }
                    //OR
                    {
                        "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "amount":1000000000,
                        "type":"crosschain_input"
                    }
                ],
                "outputs":[
                    {
                        "utxo_id":"b4645bf3232bbb971fd7378d41bd74134a43cc7c108cce53e7fc045f641b3159",
                        "script":"2097c36cdeb2fb82c971580f03d632ad5c09187d35e0e3e937cf9fd4894c35dd01160014489ac931be795efecb50a9bd2fcd569425f00be4060040e59c30120600a0724e18092080c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba34004c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac747800c0",
                        "address":"smart contract",
                        "asset":"2f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f",
                        "amount":1000000000,
                        "type":"control"
                    },
                    {
                        "utxo_id":"3beb18d5e24f38c2742fca12e5468a8a36551607288fcc2d0799f09da182d645",
                        "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                        "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                        "asset":"80c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba3400",
                        "amount":1000000000,
                        "type":"control"
                    },
                    {
                        "vote":"50ef22b3a3fca7bc08916187cc9ec2f4005c9c6b1353aa1decbd4be3f3bb0fbe1967589f0d9dec13a388c0412002d2c267bdf3b920864e1ddc50581be5604ce1",
                        "utxo_id":"28d807f13c854fc07639539cf4442a42e32dc33ed9f7becebf061c6af044f153",
                        "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                        "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                        "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "amount":960000000,
                        "type":"vote"
                    },
                    {
                        "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                        "address":"bm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                        "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "amount":1000000000,
                        "type":"crosschain_output"
                    }
                ],
                "fee":40000000,
                "balances":[
                    {
                        "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "amount":"41250000000"
                    }
                ]
            }
        ],
        "limit":100,
        "start":0
    }
}
```

**/merchant/list-txproposals 【POST】**

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.

Optional(可选):

- Object- filter, 过滤条件.
  - String- tx_hash, 交易哈希.
  - Object- status, 排序方式.
- Object- sort, 排序方式.
  - String- by, 排序的键.
  - String- order, 顺序 desc，asc.

```json
{
    "guid":"b5fe58d1-f065-414d-82c8-9be117bd6d66",
    "filter":{
        "tx_hash":"mock",
        "status":1
    },
   "sort":{
        "by":"proposal_timestamp",
        "order":"desc"
    }
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Array of object- data, 返回结果数据
    - Object- tx, 构建的交易信息.
    - String- raw_transaction, 原生交易信息.
    - Integer-status, 交易状态.
    - String- signing_instructions, 签名信息.
    - String- signatures, 签名者信息.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "data":[
            {
                "tx":{
                    "inputs":[
                        {
                            "script":"003132",
                            "address":"coinbase",
                            "asset":"0000000000000000000000000000000000000000000000000000000000000000",
                            "amount":0
                        }
                    ],
                    "outputs":[
                        {
                            "script":"00147a8a16c665568c2f52e33c5fb373a3bd1fb7f2c1",
                            "address":"tm1q029pd3n926xz75hr830mxuarh50m0ukpw8g27u",
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":41250000000
                        }
                    ],
                    "fee":0,
                    "balances":[
                        {
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":"41250000000"
                        }
                    ]
                },
                "status":1,
                "signatures":[
                    {
                        "name":"中本聪",
                        "pubkey":"mock",
                        "rejected":true
                    }
                ],
                "raw_transaction":"070100010160015e5978c52e0508cbf1cd901919277e4dba80fb4440b4771bbaa3b6c483f9264d21ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8094ebdc0301011600143c21d88332683d060ccf905a0f26ce82907ac132220120f3a597b7a1f8b7b210790d5ceef145ae8616d025a6fdb4aec67338bf937af6b9020139ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff6401160014f0928dc5f8878a4289b981d7c66386ff74be7fc300013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffb4dfcfdc03011600143c21d88332683d060ccf905a0f26ce82907ac13200",
                "signing_instructions":[
                    {
                        "derivation_path":[
                            "2c000000",
                            "99000000",
                            "01000000",
                            "00000000",
                            "01000000"
                        ],
                        "sign_data":[
                            "b9fb5dc0e7b7ee0e9c252439ec32fe6b222e0d170aee6b562634dfad3638560e"
                        ]
                    }
                ]
            }
        ]
    }
}
```

**/merchant/sign-txproposal 【POST】**

Submit a signed transaction to the chain.

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.
- String- tx_hash, build-payment 返回的数据.
- Array of array- signatures, 包含签名数据的数组.
- String- pubkey, 备注.

```json
{
    "guid":"03f8d45a-6755-44f8-8342-32c78839f1bc",
    "tx_hash":"eb4daea5edc379b6ab024c0e4ef963feafe6affcfe581d66f7ad3ec6d1828463",
    "signatures":[
        [
            "4f5e0b94f983ab6d98c230e7edf031951c93809af318d0bb6c362714794c38c5939df58e43ac1a45599d777386724354c0ced7bab54d19bac02c233fa9e2050a"
        ]
    ],
    "pubkey":"mock"
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Array of object- data, 返回结果数据
    - Object- tx, 构建的交易信息.
    - String- raw_transaction, 原生交易信息.
    - Integer-status, 交易状态.
    - String- signing_instructions, 签名信息.
    - String- signatures, 签名者信息.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "data":[
            {
                "tx":{
                    "hash":"7de59581513c471ef4cf2cd205de2608d2d7e9dd1d598251d75f8df112bd1ecd",
                    "memo":"mock",
                    "inputs":[
                        {
                            "script":"2097c36cdeb2fb82c971580f03d632ad5c09187d35e0e3e937cf9fd4894c35dd01160014489ac931be795efecb50a9bd2fcd569425f00be4060040e59c30120600a0724e1809202f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f4dfc01567a64f5010000c358797ca153795579a19a6957790400e1f5059653790400e1f505967c00a07c00a09a69c358797c9f91616429010000005879c2547951005b79895a7989597989587989537a894c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac890274787e008901c07ec169515879c2515a79c16952c3597994c251005a79895979895879895779895679895579890274787e008901c07ec16963f0010000005879c2547951005b79895a7989597989587989537a894c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac890274787e008901c07ec16951c3c2515a79c16963fc010000567a567aae7cac747800c0",
                            "address":"smartcontract",
                            "asset":"80c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba3400",
                            "amount":9995000000000,
                            "type":"spend"
                        },
                        {
                            "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                            "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                            "asset":"2f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f",
                            "amount":1000000000,
                            "type":"spend"
                        },
                        {
                            "vote":"50ef22b3a3fca7bc08916187cc9ec2f4005c9c6b1353aa1decbd4be3f3bb0fbe1967589f0d9dec13a388c0412002d2c267bdf3b920864e1ddc50581be5604ce1",
                            "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                            "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":1000000000,
                            "type":"veto"
                        },
                        {
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":1000000000,
                            "type":"crosschain_input"
                        }
                    ],
                    "outputs":[
                        {
                            "utxo_id":"b4645bf3232bbb971fd7378d41bd74134a43cc7c108cce53e7fc045f641b3159",
                            "script":"2097c36cdeb2fb82c971580f03d632ad5c09187d35e0e3e937cf9fd4894c35dd01160014489ac931be795efecb50a9bd2fcd569425f00be4060040e59c30120600a0724e18092080c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba34004c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac747800c0",
                            "address":"smartcontract",
                            "asset":"2f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f",
                            "amount":1000000000,
                            "type":"control"
                        },
                        {
                            "utxo_id":"3beb18d5e24f38c2742fca12e5468a8a36551607288fcc2d0799f09da182d645",
                            "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                            "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                            "asset":"80c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba3400",
                            "amount":1000000000,
                            "type":"control"
                        },
                        {
                            "vote":"50ef22b3a3fca7bc08916187cc9ec2f4005c9c6b1353aa1decbd4be3f3bb0fbe1967589f0d9dec13a388c0412002d2c267bdf3b920864e1ddc50581be5604ce1",
                            "utxo_id":"28d807f13c854fc07639539cf4442a42e32dc33ed9f7becebf061c6af044f153",
                            "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                            "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":960000000,
                            "type":"vote"
                        },
                        {
                            "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                            "address":"bm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":1000000000,
                            "type":"crosschain_output"
                        }
                    ],
                    "fee":40000000,
                    "balances":[
                        {
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":"41250000000"
                        }
                    ]
                },
                "status":1,
                "signature":[
                    {
                        "name":"中本聪",
                        "pubkey":"mock",
                        "rejected":true
                    }
                ],
                "raw_transaction":"070100010160015e5978c52e0508cbf1cd901919277e4dba80fb4440b4771bbaa3b6c483f9264d21ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8094ebdc0301011600143c21d88332683d060ccf905a0f26ce82907ac132220120f3a597b7a1f8b7b210790d5ceef145ae8616d025a6fdb4aec67338bf937af6b9020139ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff6401160014f0928dc5f8878a4289b981d7c66386ff74be7fc300013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffb4dfcfdc03011600143c21d88332683d060ccf905a0f26ce82907ac13200",
                "signing_instructions":[
                    {
                        "derivation_path":[
                            "2c000000",
                            "99000000",
                            "01000000",
                            "00000000",
                            "01000000"
                        ],
                        "sign_data":[
                            "b9fb5dc0e7b7ee0e9c252439ec32fe6b222e0d170aee6b562634dfad3638560e"
                        ]
                    }
                ]
            }
        ]
    }
}
```

**/merchant/submit-payment 【POST】**

Submit a signed transaction to the chain.

**Body JSON:**

- Object(必选):
  - String- guid, 钱包guid.
  - String- raw_transaction, build-payment 返回的数据.
  - Array of array- signatures, 包含签名数据的数组.
  - String- memo, 备注.

```json
{
    "guid":"03f8d45a-6755-44f8-8342-32c78839f1bc",
    "raw_transaction":"070100010160015e26978b848afd77d559980e58db9b60559b716312bd1e24af7e44c6efa5c1605dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffb0a885fd0201011600140ff923ae0944af0cfdd3eb3671db354eaa6b5795220120c44537a8d397ee09315755cd77fd02d66db015f32ba358c3bfafe20d887d041202013cffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80c2d72f011600141e03ad3233693b551be8c86f5db062bdbe0c9f6600013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffc8b292cd02011600140ff923ae0944af0cfdd3eb3671db354eaa6b579500",
    "signatures":[
        [
            "4f5e0b94f983ab6d98c230e7edf031951c93809af318d0bb6c362714794c38c5939df58e43ac1a45599d777386724354c0ced7bab54d19bac02c233fa9e2050a"
        ]
    ],
    "memo":"mock"
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Object- data, 返回结果数据
    - String- transaction_hash, 链上返回的交易哈希.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "data":{
            "transaction_hash":"9287dfe438f2387a02f6812b2f769ed85498ff59376c60b6d79d2506329330ee"
        }
    }
}
```

**/merchant/submit-txproposal 【POST】**

Submit a signed transaction to the chain.

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.
- String- tx_hash, 交易哈希.

```json
{
    "guid":"03f8d45a-6755-44f8-8342-32c78839f1bc",
    "tx_hash":"eb4daea5edc379b6ab024c0e4ef963feafe6affcfe581d66f7ad3ec6d1828463"
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Object- data, 返回结果数据
    - String- transaction_hash, 链上返回的交易哈希.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "data":{
            "transaction_hash":"9287dfe438f2387a02f6812b2f769ed85498ff59376c60b6d79d2506329330ee"
        }
    }
}
```

**/merchant/build-vote 【POST】**

Body JSON:

Object(必选):

- String- guid, 钱包guid.
- String- amount, 投票数.
- String- vote, 投给的公钥.
- Bool- forbid_chain_tx, 是否禁止链式交易，true 禁止，false 非禁止.
- String- memo, 备注.
- String- confirmations, 确认需要的交易确认数.

```json
{
    "guid":"b5fe58d1-f065-414d-82c8-9be117bd6d66",
    "amount":10000000000,
    "confirmations":1,
    "memo":"mock",
    "vote":"50ef22b3a3fca7bc08916187cc9ec2f4005c9c6b1353aa1decbd4be3f3bb0fbe1967589f0d9dec13a388c0412002d2c267bdf3b920864e1ddc50581be5604ce1",
    "forbid_chain_tx": false,
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Array of object- data, 返回结果数据
    - Object- tx, 构建的交易信息.
    - String- raw_transaction, 原生交易信息.
    - Integer-status, 交易状态.
    - String- signing_instructions, 签名信息.
    - String- signatures, 签名者信息.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "data":[
        	{
                "tx":{
                    "inputs":[
                        {
                            "script":"00147a8a16c665568c2f52e33c5fb373a3bd1fb7f2c1",
                            "address":"tm1q029pd3n926xz75hr830mxuarh50m0ukpw8g27u",
                            "asset":"0000000000000000000000000000000000000000000000000000000000000000",
                            "amount":0,
                            "type":"spend"
                        }
                    ],
                    "outputs":[
                        {
                            "script":"00147a8a16c665568c2f52e33c5fb373a3bd1fb7f2c1",
                            "address":"tm1q029pd3n926xz75hr830mxuarh50m0ukpw8g27u",
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":41250000000,
                            "type":"vote",
                            "vote":"50ef22b3a3fca7bc08916187cc9ec2f4005c9c6b1353aa1decbd4be3f3bb0fbe1967589f0d9dec13a388c0412002d2c267bdf3b920864e1ddc50581be5604ce1"
                        }
                    ],
                    "fee":0,
                    "balances":[
                        {
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":"41250000000"
                        }
                    ],
                    "hash":"eb4daea5edc379b6ab024c0e4ef963feafe6affcfe581d66f7ad3ec6d1828463"
                },
                "raw_transaction":"070100010160015e5978c52e0508cbf1cd901919277e4dba80fb4440b4771bbaa3b6c483f9264d21ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8094ebdc0301011600143c21d88332683d060ccf905a0f26ce82907ac132220120f3a597b7a1f8b7b210790d5ceef145ae8616d025a6fdb4aec67338bf937af6b9020139ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff6401160014f0928dc5f8878a4289b981d7c66386ff74be7fc300013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffb4dfcfdc03011600143c21d88332683d060ccf905a0f26ce82907ac13200",
                "signing_instructions":[
                    {
                        "derivation_path":[
                            "2c000000",
                            "99000000",
                            "01000000",
                            "00000000",
                            "01000000"
                        ],
                        "sign_data":[
                            "b9fb5dc0e7b7ee0e9c252439ec32fe6b222e0d170aee6b562634dfad3638560e"
                        ]
                    }
                ]
        	}
        ]
    }
}
```

**/merchant/build-veto 【POST】**

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.
- String- amount, 取消的投票数.
- String- vote, 取消之前投给的公钥.
- Bool- forbid_chain_tx, 是否禁止链式交易，true 禁止，false 非禁止.
- String- memo, 备注.
- String- confirmations, 确认需要的交易确认数.

```json
{
    "guid":"b5fe58d1-f065-414d-82c8-9be117bd6d66",
    "amount":10000000000,
    "confirmations":1,
    "memo":"mock",
    "vote":"50ef22b3a3fca7bc08916187cc9ec2f4005c9c6b1353aa1decbd4be3f3bb0fbe1967589f0d9dec13a388c0412002d2c267bdf3b920864e1ddc50581be5604ce1",
    "forbid_chain_tx": false,
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Array of object- data, 返回结果数据
    - Object- tx, 构建的交易信息.
    - String- raw_transaction, 原生交易信息.
    - Integer-status, 交易状态.
    - String- signing_instructions, 签名信息.
    - String- signatures, 签名者信息.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "data":[
        	{
                "tx":{
                    "inputs":[
                        {
                            "script":"00147a8a16c665568c2f52e33c5fb373a3bd1fb7f2c1",
                            "address":"tm1q029pd3n926xz75hr830mxuarh50m0ukpw8g27u",
                            "asset":"0000000000000000000000000000000000000000000000000000000000000000",
                            "amount":0,
                            "type":"veto",
                            "vote":"50ef22b3a3fca7bc08916187cc9ec2f4005c9c6b1353aa1decbd4be3f3bb0fbe1967589f0d9dec13a388c0412002d2c267bdf3b920864e1ddc50581be5604ce1"
                        }
                    ],
                    "outputs":[
                        {
                            "script":"00147a8a16c665568c2f52e33c5fb373a3bd1fb7f2c1",
                            "address":"tm1q029pd3n926xz75hr830mxuarh50m0ukpw8g27u",
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":41250000000,
                            "type":"control"
                        }
                    ],
                    "fee":0,
                    "balances":[
                        {
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":"41250000000"
                        }
                    ],
                    "hash":"eb4daea5edc379b6ab024c0e4ef963feafe6affcfe581d66f7ad3ec6d1828463"
                },
                "raw_transaction":"070100010160015e5978c52e0508cbf1cd901919277e4dba80fb4440b4771bbaa3b6c483f9264d21ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8094ebdc0301011600143c21d88332683d060ccf905a0f26ce82907ac132220120f3a597b7a1f8b7b210790d5ceef145ae8616d025a6fdb4aec67338bf937af6b9020139ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff6401160014f0928dc5f8878a4289b981d7c66386ff74be7fc300013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffb4dfcfdc03011600143c21d88332683d060ccf905a0f26ce82907ac13200",
                "signing_instructions":[
                    {
                        "derivation_path":[
                            "2c000000",
                            "99000000",
                            "01000000",
                            "00000000",
                            "01000000"
                        ],
                        "sign_data":[
                            "b9fb5dc0e7b7ee0e9c252439ec32fe6b222e0d170aee6b562634dfad3638560e"
                        ]
                    }
                ]
        	}
        ]
    }
}
```

**/merchant/build-crosschain 【POST】**

Vapor->Bytom
**Body JSON:**

- Object(必选):
  - String- guid, 钱包guid.
  - String- asset, 资产类型.
  - Object- recipients, 接收地址和金额组成的键值对.
  - Bool- forbid_chain_tx, 是否禁止链式交易，true 禁止，false 非禁止.
  - String- confirmations, 确认需要的交易确认数.

```json
{
    "guid":"b5fe58d1-f065-414d-82c8-9be117bd6d66",
    "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "confirmations":1,
    "recipients": {
    	"bm1q2lm5c7ajtutcjzhr4cvuge385ygynupe90m7xj": 10000000000
    }
    "forbid_chain_tx": false, //多签传true
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Array of object- data, 返回结果数据
    - Object- tx, 构建的交易信息.
    - String- raw_transaction, 原生交易信息.
    - Integer-status, 交易状态.
    - String- signing_instructions, 签名信息.
    - String- signatures, 签名者信息.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "data":[
        	{
                "tx":{
                    "inputs":[
                        {
                            "script":"00147a8a16c665568c2f52e33c5fb373a3bd1fb7f2c1",
                            "address":"tm1q029pd3n926xz75hr830mxuarh50m0ukpw8g27u",
                            "asset":"0000000000000000000000000000000000000000000000000000000000000000",
                            "amount":0,
                            "type":"spend"
                        }
                    ],
                    "outputs":[
                        {
                            "script":"00147a8a16c665568c2f52e33c5fb373a3bd1fb7f2c1",
                            "address":"bm1q2lm5c7ajtutcjzhr4cvuge385ygynupe90m7xj",
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":41250000000,
                            "type":"crosschain_output"
                        }
                    ],
                    "fee":0,
                    "balances":[
                        {
                            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                            "amount":"41250000000"
                        }
                    ],
                    "hash":"eb4daea5edc379b6ab024c0e4ef963feafe6affcfe581d66f7ad3ec6d1828463"
                },
                "raw_transaction":"070100010160015e5978c52e0508cbf1cd901919277e4dba80fb4440b4771bbaa3b6c483f9264d21ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8094ebdc0301011600143c21d88332683d060ccf905a0f26ce82907ac132220120f3a597b7a1f8b7b210790d5ceef145ae8616d025a6fdb4aec67338bf937af6b9020139ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff6401160014f0928dc5f8878a4289b981d7c66386ff74be7fc300013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffb4dfcfdc03011600143c21d88332683d060ccf905a0f26ce82907ac13200",
                "signing_instructions":[
                    {
                        "derivation_path":[
                            "2c000000",
                            "99000000",
                            "01000000",
                            "00000000",
                            "01000000"
                        ],
                        "sign_data":[
                            "b9fb5dc0e7b7ee0e9c252439ec32fe6b222e0d170aee6b562634dfad3638560e"
                        ]
                    }
                ]
        	}
        ]
    }
}
```

**/merchant/build-transaction 【POST】**

高级build功能

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.
- String- inputs, 构建交易的输入.
- String- outputs, 构建交易的输出.
- String- confirmations, 确认需要的交易确认数.

```json
{
    "guid":"b5fe58d1-f065-414d-82c8-9be117bd6d66",
    "fee":100000000,
    "confirmations":1,
    "inputs":[
        {
            "type":"spend_utxo",
            "output_id":"37921eee9453d2598b6357dfeddd2cf351850f5be917bdac0d1cf208859ece06"
        },
        {
            "type":"spend_wallet",
            "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "amount":10000000000
        }
    ],
    "outputs":[
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
}
```

**Result JSON:**

Object:

- String- raw_transaction, 原生交易信息.
- Integer-fee, 交易费.
- String- signing_instructions, 签名信息.

```json
{
    "raw_transaction":"070100010161015f26b670f600f5a6f7330e2efa0f648d56bdf5ed3f3532cb2346db7803ff12cca7ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe8ccdfd59901000116001413d965b24f5b3ce4d9b6e1621173e57c161a2729220120a833c20ed909dd0154d7d41e6c97424a4c583ef02afc2969730b41c7ba30606d01013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8099c4d5990101160014f0928dc5f8878a4289b981d7c66386ff74be7fc300",
    "signing_instructions":[
        {
            "derivation_path":[

            ],
            "sign_data":[
                "fd82cc65e6821daf8d2fc98bf2641a579c17aa484838c01dc4b88c68eeca1299"
            ],
            "pubkey":"mock"
        },
        {
            "derivation_path":[
                "2c000000",
                "99000000",
                "01000000",
                "00000000",
                "01000000"
            ],
            "sign_data":[
                "fd82cc65e6821daf8d2fc98bf2641a579c17aa484838c01dc4b88c68eeca1299"
            ],
            "pubkey":"a833c20ed909dd0154d7d41e6c97424a4c583ef02afc2969730b41c7ba30606d"
        }
    ],
    "fee":449000
}
```

---

#### Q

**/q/asset 【GET】**

Query the price of an asset on a given blockchain. asset_id is a hexdecimal string.

**Example:**

Object(必选):

- String- id, 资产ID.

```http
/q/asset?id=ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Array of object- data, 返回结果数据
  - String- asset_id, 资产ID.
  - String- alias, 资产别名.
  - Float- usd_price, 1BTM等价的USDT.
  - Float- cny_price, 1BTM等价的CNY.
  - Float- btc_price, 1BTM等价的BTC.

```json
{
    "code":200,
    "msg":"mock",
    "data":{
        "asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "alias":"btm",
        "usd_price":0.1426,
        "cny_price":0.996774,
        "btc_price":0.00002673
    }
}
```

**/q/chain-status 【GET】**

Query the current status of a blockchain.

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Array of object- data, 返回结果数据
    - String- block_height, 当前区块高度.
    - String- block_hash, 当前区块哈希.

```json
{
    "code":200,
    "msg":"mock",
    "data":{
        "block_height":86200,
        "block_hash":"b02efbc6cf4b6326bd514c990250e535e4bbdcdfe4dfa7f862815a1e53d7e477"
    }
}
```

**/q/get-vote-status 【GET】**

Only available in vapor

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Object- data, 返回结果数据
    - Object- consensus_nodes,投票的数据.
      - String- location, 国家地区.
      - Object- introduce, 节点介绍.
      - String- vote_num, 投票数量.
      - Object- declaration, 宣言.
      - String- role, 超级节点类型[正式节点，备选节点，其他].
      - Object- name, 节点名称.
      - String- name_en, 节点英文名称.
      - Object- homepage, 节点主页.
    - String- total_vote_num, 总投票数.

```json
{   
    "code":200,
    "msg":"",
    "result":{
        "data":{
            "consensus_nodes":[
                {
                    "location":"中国",
                    "introduce":"Tokenview是全球首款全币种区块链浏览器，旨在运用全球最新的区块链技术为用户提供一站式区块链信息检索与数据分析服务。Tokenview支持所有主流加密货币区块信息的极速查询，同时提供多样化的数据分析和图表。与此同时。",
                    "logo":"/uploads/1560843168.41532161560842725.83183341560752713.024456Tokenview.jpg",
                    "vote_num":560000000,
                    "declaration":"尊敬的Bytom社区： 谨代表 Tokenview 团队，郑重宣布参加Bystack网络节点竞选。 Tokenview是全球首款全币种区块链浏览器，旨在运用全球最新的区块链技术为用户提供一站式区块链信息检索与数据分析服务。与此同时，Tokenview会不断增加新的币种和功能，致力于成为全球最好用，最强大的区块链浏览器。",
                    "role":2,
                    "pub_key":"74248234dd0f57f505dae45168987b205f8423e882ad44fe5cfa2b055b9308e2c5a09e2abd6635204f312ded4b61ab7aa563ad743523ca7bec4c4ba8fe5a657d",
                    "name":"22Tokenview",
                    "name_en": "22Tokenview",
                    "homepage":"https://tokenview22.com"
                },
                {
                    "location":"",
                    "introduce":"",
                    "logo":"",
                    "vote_num":560000000,
                    "declaration":"",
                    "role":2,
                    "pub_key":"964909579fac3b527ed48cf037142737799ff6f37b38860f8537e9d761b95a1e4ac209a13b766be5f43a930160dad9355ddcca7db965819767629aff571953bd",
                    "name":"",
                    "name_en":"",
                    "homepage":""
                }
            ],
            "total_vote_num":1120000000
        }
    },
}
```

**/q/list-utxos 【POST】**

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.

Optional(可选):

- `Integer`- start, 开始数量，默认0.
- `Integer`- _limit_, 查询数量，默认10.
- Object- filter, 过滤条件.
  - String- asset, 资产ID.
  - String- script, 地址脚本.
- Object- sort, 排序方式.
  - String- by, 排序的键.
  - String- order, 顺序 desc，asc.

```json
{ 
    "filter":	{ "script":"00147a6cfb076a560b9a37c4fd6ca2acc66f85b64a21", 
        "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff" 
    },
    "sort": { "by":"amount", 
        	"order":"desc"
    }
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - String- _links, 下一页的URL
  - Array of object- data, 返回结果数据
    - String- hash, UTXO的ID.
    - String- asset, 资产ID.
    - Integer-amount, 资产金额.
  - `Integer`- start, 开始数量.
  - `Integer`- _limit_, 查询数量.

```json
{ 
	"code":200, 
  "msg":"mock", 
  "result":{ "data":
  						{ "votes":[ 
                	{ "vote":"mock", 
                 		"total":1, 
                 		"name":"mock", 
                 		"desc":"mock" } 
              	], 
              "total_vote_num": 1 
           		}
    			}
      "limit":10,
        "start":0
    }
}
```

