---
id: docs_39
title: BlockCenter Bytom API
sidebar_label: BlockCenter Bytom API
---

**BlockCenter-Bytom API**

BaseURL：[https://api-dev.bycoin.im:8000/api/v2/btm](https://api-dev.bycoin.im:8000/api/v2/btm)

---

#### Account

**/account/create 【POST】**

使用公钥创建钱包，每个钱包有一个唯一标识（GUID）

**Body JSON:**

Object:

- String- pubkey, 公钥.
- String- label, 创建钱包地址的标签.
- String- email, 邮箱地址.

```json
{
    "pubkey": "82b5c28d07a5f7442d564d25b2f95a9bf46c826bff4b27593f37250cd9639a797d1f501b69f895b830138fabc6f91e2b3b3c8df26642a34be4af27886b9134dc",
    "label": "1st address",
    "email": "abc@abc.com"
}
```

**Result JSON:**

Object:

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Object- data, 返回结果数据 《为什么还要嵌套一层》
    - String- guid, 钱包唯一标识.
    - String- address, 钱包的Bytom地址.
    - String- label, 地址的标签.

```json
{
    "code": 200,
    "msg": "",
    "result": {
        "data": {
            "guid": "e270a474-cc9f-4db0-93d5-6e8259193eae",
            "address": "tm1qcdprxudy9p5radtqscg4gl7uwufy593357cgc0",
            "label": "1st address"
        }
    }
}
```

**/account/list-addresses 【POST】**

查询指定钱包下的所有地址和金额相关信息

**Body JSON:**

Object(必选):

- String- guid, 钱包的唯一标识.

Optional(可选):

- Integer- start, 开始数量，默认0.
- Integer- _limit_, 查询数量，默认10.

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
      - String- in_usd, 资产转换成usdt的价值.
      - String- in_cny, 资产转换成cny的价值.
      - String- in_btc, 资产转换成btc的价值.
  - Integer- start, 分页开始数.
  - Integer- limit, 分页结束数.

```json
{
    "code": 200,
    "msg": "mock",
    "result": {
        "_links": {},
        "data": [
            {
                "guid": "e276c676-ad38-4879-9790-87e5a1be1111",
                "address": "tm1qz0vktvj0tv7wfkdku93pzul90stp5fefrkxcgm",
                "label": "mock",
                "balances": [
                    {
                        "asset": "a853110464934ba4cb35f1f98512f9f8700ae24da4bf18eb8b216bb38a3ffd1f",
                        "balance": "42400000001",
                        "total_received": "44400000000",
                        "total_sent": "1999999999",
                        "decimals": 8,
                        "alias": "mock",
                        "in_usd": "0.00",
                        "in_cny": "0.00",
                        "in_btc": "0.000000"
                    },
                    {
                        "asset": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "balance": "3262476859534000",
                        "total_received": "3263535568514000",
                        "total_sent": "1058708980000",
                        "decimals": 8,
                        "alias": "btm",
                        "in_usd": "2424020.31",
                        "in_cny": "16725740.12",
                        "in_btc": "704.042506"
                    }
                ]
            }
        ],
        "limit": 10,
        "start": 0
    }
}
```

**/account/new-address【POST】**

在钱包下创建新地址

**Body JSON:**

Object(必选):

- String- guid, 钱包的唯一标识.

Optional(可选):

- String- label, 地址标签.

```json
{
    "guid": "e270a474-cc9f-4db0-93d5-6e8259193eae",
    "label": "2nd addr"
}
```

**Result JSON:**

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Object- data, 返回结果数据
    - String- guid, 钱包唯一标识.
    - String- address, 钱包的Bytom地址.
    - String- label, 地址的标签.

```javascript
{
    "code": 200,
    "msg": "",
    "result": {
        "data": {
            "guid": "e270a474-cc9f-4db0-93d5-6e8259193eae",
            "address": "tm1qkfz55c6r7sfj3m86wld57qt4t5vxhjpxxuay9q",
            "label": "2nd addr"
        }
    }
}
```

**/account/restore 【POST】**

**Body JSON:**

Object(必选):

- String- pubkey, 钱包的唯一标识.
- String- account_index, 账户索引.(地址是由公钥+账户索引+地址索引生成)

```json
{
    "pubkey": "82b5c28d07a5f7442d564d25b2f95a9bf46c826bff4b27593f37250cd9639a797d1f501b69f895b830138fabc6f91e2b3b3c8df26642a34be4af27886b9134dc",
    "account_index":1
}
```

**Result JSON:**

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Array if string- data, 返回guid数组

```json
{
    "code": 200,
    "msg": "",
    "result": {
        "data": [
            "e270a474-cc9f-4db0-93d5-6e8259193eae"
        ]
    }
}
```

**/account/create-multisig 【POST】**

创建多签账户

**Body JSON:**

Object(必选):

- String- m, 最小签名数.
- String- n, 签名总数

```json
{
    "m":2,    
    "n":3
}
```

**Result JSON:**

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Object- data, 返回结果数据
    - String- access_token, 加入多钱钱包的验证码.
    - String- guid, 多签钱包的唯一标识.
    - String- status, 结果状态，0 等待，1 完成.
    - String- m, 最小签名数.
    - String- n, 签名总数.
    - Array of object- signers, 最小签名数.
      - String- pubkey, 签名者的公钥（唯一）.
      - String- name, 签名者的名字（唯一）.
    - String- addresses, 多签地址.

```json
{
    "code": 200,
    "msg": "",
    "result": {
        "data": {
            "access_token": "087523",
            "guid": "963deedb-a4d3-4656-ae6d-f4bb47737134",
            "m": 2,
            "n": 3,
            "status": 0,
            "signers": [],
            "addresses": null
        }
    }
}
```

**/account/join-multisig 【POST】**

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.
- String- name, 签名者名称（唯一）.
- String- pubkey, 签名者公钥.

```json
{
    "guid":"963deedb-a4d3-4656-ae6d-f4bb47737134",
    "name":"中本聪1",
    "pubkey":"82b5c28d07a5f7442d564d25b2f95a9bf46c826bff4b27593f37250cd9639a797d1f501b69f895b830138fabc6f91e2b3b3c8df26642a34be4af27886b9134dc"
}
```

**Result JSON:**

- Integer- code, 错误码.
- String- msg, 错误码对应的错误信息.
- Object- result, 返回结果
  - Object- data, 返回结果数据
    - String- guid, 多签钱包的唯一标识.
    - String- status, 结果状态，0 等待，1 完成.
    - String- m, 最小签名数.
    - String- n, 签名总数.
    - Array of object- signers, 最小签名数.
      - String- pubkey, 签名者的公钥（唯一）.
      - String- name, 签名者的名字（唯一）.

```json
{
    "code": 200,
    "msg": "",
    "result": {
        "data": {
            "guid": "963deedb-a4d3-4656-ae6d-f4bb47737134",
            "m": 2,
            "n": 3,
            "status": 0,
            "signers": [
                {
                    "pubkey": "82b5c28d07a5f7442d564d25b2f95a9bf46c826bff4b27593f37250cd9639a797d1f501b69f895b830138fabc6f91e2b3b3c8df26642a34be4af27886b9134fc",
                    "name": "中本聪"
                },
                {
                    "pubkey": "82b5c28d07a5f7442d564d25b2f95a9bf46c826bff4b27593f37250cd9639a797d1f501b69f895b830138fabc6f91e2b3b3c8df26642a34be4af27886b9134dc",
                    "name": "中本聪1"
                }
            ]
        }
    }
}
```

**/account/list-wallets 【POST】**

查询公钥创建的钱包

**Body JSON:**

Object(必选):

- String- pubkey, 公钥.

Optional(可选):

- Integer- start, 开始数量，默认0.
- Integer- _limit_, 查询数量，默认10.
- Object- filter, 过滤条件.
  - String- type, 地址类型, pay-to-pubkey (P2PK), pay-to-pubkey-hash (P2PKH), and pay-to-script-hash (P2SH).
  - String- guid, 钱包guid.
  - String- account_index, 账户索引

```json
{
    "pubkey":"82b5c28d07a5f7442d564d25b2f95a9bf46c826bff4b27593f37250cd9639a797d1f501b69f895b830138fabc6f91e2b3b3c8df26642a34be4af27886b9134fc",
    "filter":{
        "guid":"963deedb-a4d3-4656-ae6d-f4bb47737134",
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
    - Array of String- address, 钱包的Bytom地址.
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
                "guid": "963deedb-a4d3-4656-ae6d-f4bb47737134",
                "m": 2,
                "n": 3,
                "status": 1,
                "signers": [
                    {
                        "pubkey": "82b5c28d07a5f7442d564d25b2f95a9bf46c826bff4b27593f37250cd9639a797d1f501b69f895b830138fabc6f91e2b3b3c8df26642a34be4af27886b9134fc",
                        "name": "中本聪"
                    },
                    {
                        "pubkey": "82b5c28d07a5f7442d564d25b2f95a9bf46c826bff4b27593f37250cd9639a797d1f501b69f895b830138fabc6f91e2b3b3c8df26642a34be4af27886b9134dc",
                        "name": "中本聪1"
                    },
                    {
                        "pubkey": "b7f463446a31b3792cd168d52b7a89b3657bca3e25d6854db1488c389ab6fc8d538155c25c1ee6975cc7def19710908c7d9b7463ca34a22058b456b45e498db9",
                        "name": "中本聪2"
                    }
                ],
                "addresses": [
                    "tm1qed62lwjc4jjmvpx7r5sczt7gexvtueu2lhe4qjeekz35zdmjlu2qmcvc89"
                ]
            }
        ],
        "limit": 10,
        "start": 0
    }
}
```

---

#### Merchant

**/merchant/build-payment 【POST】**

构建单笔交易和多签交易

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.
- String- asset, 资产类型.
- Object- recipients, 接收地址和金额组成的键值对.
- Bool- forbid_chain_tx, 是否禁止链式交易，true 禁止，false 非禁止.
- String- memo, 备注.
- String- confirmations, 确认需要的交易确认数.

```javascript
{
    "guid":"90ab0024-0f71-4fcd-b136-3bb282286057",
    "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "recipients": {
        "tm1q9jpad933ddjfm3ryena49f0fajg955ckjnjuhn": 10000000000
    },
    "confirmations":1,
    "memo": "中本聪",
    "forbid_chain_tx": false
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
    "code": 200,
    "msg": "",
    "result": {
        "data": {
            "tx": {
                "hash": "2ead2ce7b6629eded07544632bcc7556f19b9188884a0910d5c960e3c1ac8c9b",
                "status_fail": false,
                "size": 270,
                "submission_timestamp": 0,
                "memo": "",
                "inputs": [
                    {
                        "script": "00148c465ed9cd4b9b9990765b91361ffc0fe0eebe35",
                        "address": "tm1q33r9akwdfwdenyrktwgnv8luplswa0342qkh5c",
                        "asset": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "amount": 100000000000,
                        "type": "spend"
                    }
                ],
                "outputs": [
                    {
                        "utxo_id": "7560c0eee3aefbefb1f450f06bb6429cba78fe689e89a788b27558c4f7b550b3",
                        "script": "00142c83d696316b649dc464ccfb52a5e9ec905a5316",
                        "address": "tm1q9jpad933ddjfm3ryena49f0fajg955ckjnjuhn",
                        "asset": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "amount": 1000000000,
                        "type": "control"
                    },
                    {
                        "utxo_id": "dc0d0b39869450b5606ade06b466706b340ee9cc1d9be8673bc8a8d774aacd7b",
                        "script": "00148c465ed9cd4b9b9990765b91361ffc0fe0eebe35",
                        "address": "tm1q33r9akwdfwdenyrktwgnv8luplswa0342qkh5c",
                        "asset": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "amount": 98999551000,
                        "type": "control"
                    }
                ],
                "fee": 449000,
                "balances": [
                    {
                        "asset": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "amount": "-1000449000"
                    }
                ],
                "types": [
                    "ordinary"
                ]
            },
            "raw_transaction": "0701f18816010161015fc423c1beb9219773b533b2ca9d0cf3f399f3555249bfd7fa3de507441a7d67e4ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80d0dbc3f40200011600148c465ed9cd4b9b9990765b91361ffc0fe0eebe352201204c6af110ebf580e7a0fd9615fcb295018209c7c6c4403c621c7e16bfcf8f06e202013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8094ebdc03011600142c83d696316b649dc464ccfb52a5e9ec905a531600013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff9888d5e6f002011600148c465ed9cd4b9b9990765b91361ffc0fe0eebe3500",
            "signing_instructions": [
                {
                    "derivation_path": [
                        "2c000000",
                        "99000000",
                        "01000000",
                        "00000000",
                        "02000000"
                    ],
                    "sign_data": [
                        "da1dd31fc90756798e8cefb4f0578bc9d7b069a0510390e238ba97416f55bc42"
                    ],
                    "pubkey": "4c6af110ebf580e7a0fd9615fcb295018209c7c6c4403c621c7e16bfcf8f06e2"
                }
            ],
            "fee": 449000
        }
    }
}
```

**/merchant/list-transactions 【POST】**

列出和钱包或者和地址相关的所有交易

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.

Optional(可选):

- Integer- start, 开始数量，默认0.
- Integer- _limit_, 查询数量，默认100.
- Object- filter, 过滤条件.
  - String- asset_id, 资产ID.
  - Object- sort, 排序方式.
    - String- by, 排序的键.
    - String- order, 顺序 desc，asc.

```json
{
    "guid":"b5fe58d1-f065-414d-82c8-9be117bd6d66",
    "filter":{
        "asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
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
      - Integer-fee, 交易费.
  - Integer- start, 开始数量.
  - Integer- _limit_, 查询数量.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "_links":{
            "next":"/api/v1/btm/merchant/list-transactions?limit=100&start=100"
        },
        "data":[
            {
                "hash":"0f44a0af521b7993e72512a2fadc9736aaaf70f67fab16c7ae3f55494a7a3536",
                "status_fail":false,
                "size":81,
                "submission_timestamp":1547012157,
                "block_height":85198,
                "block_timestamp":1545188911,
                "memo":"mock",
                "inputs":[
                    {
                        "script":"003835313938",
                        "address":"coinbase",
                        "asset":"0000000000000000000000000000000000000000000000000000000000000000",
                        "amount":0
                    }
                ],
                "outputs":[
                    {
                        "script":"001413d965b24f5b3ce4d9b6e1621173e57c161a2729",
                        "address":"tm1qz0vktvj0tv7wfkdku93pzul90stp5fefrkxcgm",
                        "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "amount":41750000000
                    }
                ],
                "fee":0,
                "balances":[
                    {
                        "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                        "amount":"41750000000"
                    }
                ]
            }
        ],
        "limit":100,
        "start":0
    }
}
```

**/merchant/submit-payment 【POST】**

Submit a signed transaction to the chain.

signatures 里存储的是解锁交易的部分参数，创建交易的时候已经生成了 raw_transaction，里面包含了部分解锁所需要的参数，例如，如果是普通的转账交易，那么创建的交易里就包含公钥信息，这时只需要在 signatures 里加入签名参数即可。

**Body JSON:**

Object(必选):

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

**/merchant/build-transaction 【POST】**

高级build功能，系统会自动加入手续费交易，所有的交易输入如果没有指定接收者，那么系统默认返回给guid 所对应的地址。对于交易手续费，系统会默认将手续费所对应的 BTM 数额作为交易输入。

**Body JSON**

Object(必选):

- String- guid, 钱包guid.
- String- inputs, 构建交易的输入.
- String- outputs, 构建交易的输出.
- String- confirmations, 确认需要的交易确认数.

```json
{
    "guid":"b5fe58d1-f065-414d-82c8-9be117bd6d66",
    "fee":100000000, // 手续费自定义
    "confirmations":1,
    "inputs":[
        { // 多资产A
            "type":"spend_wallet",
            "asset":"dd2cf351850f5be917bdac0d1cf20837921eee9453d2598b6357dfed859ece06",
            "amount":10000000000
        },
        { // 多资产B
            "type":"spend_wallet",
            "asset":"37921eee9453d2598b6357dfeddd2cf351850f5be917bdac0d1cf208859ece06",
            "amount":20000000000
        }
    ],
    "outputs":[
        {// 多资产A
            "type":"control_address",
            "amount":10000000000,
            "asset":"dd2cf351850f5be917bdac0d1cf20837921eee9453d2598b6357dfed859ece06",
            "address":"bm1q50u3z8empm5ke0g3ngl2t3sqtr6sd7cepd3z68"
        },
        {// 多资产B
            "type":"control_address",
            "amount":20000000000,
            "asset":"37921eee9453d2598b6357dfeddd2cf351850f5be917bdac0d1cf208859ece06",
            "address":"bm1q50u3z8empm5ke0g3ngl2t3sqtr6sd7cepd3z68"
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

**/merchant/list-txproposals 【POST】**

**Body-JSON:**

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
    }
    "sort":{
        "by":"proposal_timestamp",
        "order":"desc"
    }
}
```

**Result-JSON:**

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

Sign transaction.

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
                    ],
                    "hash":"eb4daea5edc379b6ab024c0e4ef963feafe6affcfe581d66f7ad3ec6d1828463"
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

/merchant/submit-txproposal 【POST】

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

**/merchant/get-transaction 【POST】**

Get the transaction by tx_id

**Body JSON:**

Object(必选):

- String- tx_id, 交易哈希.

```javascript
{
    "tx_id":"0f44a0af521b7993e72512a2fadc9736aaaf70f67fab16c7ae3f55494a7a3536"
}
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

```javascript
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
                    "amount":9995000000000
                },
                {
                    "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                    "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                    "asset":"2f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f",
                    "amount":1000000000
                },
                {
                    "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                    "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                    "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount":1000000000
                }
            ],
            "outputs":[
                {
                    "utxo_id":"b4645bf3232bbb971fd7378d41bd74134a43cc7c108cce53e7fc045f641b3159",
                    "script":"2097c36cdeb2fb82c971580f03d632ad5c09187d35e0e3e937cf9fd4894c35dd01160014489ac931be795efecb50a9bd2fcd569425f00be4060040e59c30120600a0724e18092080c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba34004c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac747800c0",
                    "address":"smart contract",
                    "asset":"2f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f",
                    "amount":1000000000
                },
                {
                    "utxo_id":"3beb18d5e24f38c2742fca12e5468a8a36551607288fcc2d0799f09da182d645",
                    "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                    "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                    "asset":"80c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba3400",
                    "amount":1000000000
                },
                {
                    "utxo_id":"983f71645b2f184108e896551cb48260503096aff8d9d3771c8e5d5059596224",
                    "script":"2097c36cdeb2fb82c971580f03d632ad5c09187d35e0e3e937cf9fd4894c35dd01160014489ac931be795efecb50a9bd2fcd569425f00be4060040e59c30120600a0724e1809202f411f8edadc0f6c7a5ff956848550c63dee4b1e2e2ecf63af9070565f4d086f4dfc01567a64f5010000c358797ca153795579a19a6957790400e1f5059653790400e1f505967c00a07c00a09a69c358797c9f91616429010000005879c2547951005b79895a7989597989587989537a894c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac890274787e008901c07ec169515879c2515a79c16952c3597994c251005a79895979895879895779895679895579890274787e008901c07ec16963f0010000005879c2547951005b79895a7989597989587989537a894c9a567a649300000057790400e1f5059653790400e1f505967800a07800a09a5a7956799f9a6955797b957c96c37800a052797ba19a69c3787c9f9161647c0000000059795479515979c1695178c2515b79c16952c3527994c251005b79895a79895979895879895779895679890274787e008901c07ec169638e0000000059795479515979c16951c3c2515b79c169639a000000567a567aae7cac890274787e008901c07ec16951c3c2515a79c16963fc010000567a567aae7cac747800c0",
                    "address":"smart contract",
                    "asset":"80c41b4c0f37e3fbb764ebe7730da26e54b0ab1dab711cf02c70f2756fba3400",
                    "amount":9994000000000
                },
                {
                    "utxo_id":"28d807f13c854fc07639539cf4442a42e32dc33ed9f7becebf061c6af044f153",
                    "script":"0014b5ce3ba56acef9c8d8772587351d3f40fd5379ee",
                    "address":"tm1qkh8rhft2emuu3krhykrn28flgr74x70wclp4zx",
                    "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount":960000000
                }
            ],
            "fee":40000000,
            "balances":"mixed"
        }
    }
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

```javascript
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
    "code": 200,
    "msg": "",
    "result": {
        "data": {
            "block_height": 346417,
            "block_hash": "6dc175bfbbac85e3d50bebb81c2b430abbbb17e5507877c48b560950a6351bff"
        }
    }
}
```

**/q/list-utxos 【POST】**

**Body JSON:**

Object(必选):

- String- guid, 钱包guid.

Optional(可选):

- Integer- start, 开始数量，默认0.
- Integer- _limit_, 查询数量，默认10.
- Object- filter, 过滤条件.
  - String- asset, 资产ID.
  - String- script, 地址脚本.
- Object- sort, 排序方式.
  - String- by, 排序的键.
  - String- order, 顺序 desc，asc.

```json
{
    "filter":{
        "script":"00147a6cfb076a560b9a37c4fd6ca2acc66f85b64a21",
        "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
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
    - String- hash, UTXO的ID.
    - String- asset, 资产ID.
    - Integer-amount, 资产金额.
  - Integer- start, 开始数量.
  - Integer- _limit_, 查询数量.

```json
{
    "code":200,
    "msg":"mock",
    "result":{
        "_links":{

        },
        "data":[
            {
                "hash":"d63652a946a7ea1051d3e863b3f002856905616cfde4bcfcfdbfbbca611b8f0f",
                "asset":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                "amount":4353442344
            }
        ],
        "limit":10,
        "start":0
    }
}
```

