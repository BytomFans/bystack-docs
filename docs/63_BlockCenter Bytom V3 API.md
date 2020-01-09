---
id: docs_63
title: BlockCenter Bytom API  V3
sidebar_label: BlockCenter Bytom API  V3
---


**默认 JSON-RPC 接口:**

| Client | URL |
| --- | --- |
| Base URL | [https://blockmeta.com/api/v3](https://blockmeta.com/api/v3) |

**一个完整的请求和返回示例如下：**

```json
// curl -X GET url/method
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/blocks?page=1&limit=2'

// response
{
    "blocks": [
        {
            "hash": "46c5c128269cc6001d59b61fb6a15c080ebb74a5395ea6b8c98a497a37d501c3",
            "previous_block_hash": "ad7f81fb93f47a5a14c9bea91d033af3fa77e85f844ad7ef0b5fe818df2d5feb",
            "transaction_status_hash": "6978a65b4ee5b6f4914fe5c05000459a803ecf59132604e5d334d64249c5e50a",
            "transaction_merkle_root": "5755cad062140b233d3bab55c8caaca5bc4fa0f59cf1b58aa9c0673780875bfa",
            "size": 7850,
            "version": 1,
            "height": 352099,
            "timestamp": 1575599788,
            "miner_address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
            "miner_name": "antpool",
            "chain_status": "mainnet",
            "nonce": 966909481856598328,
            "bits": 2017612633063013959,
            "hash_rate": 2182503155,
            "difficulty": "69840100991",
            "transaction_count": 2
        },
        {
            "hash": "ad7f81fb93f47a5a14c9bea91d033af3fa77e85f844ad7ef0b5fe818df2d5feb",
            "previous_block_hash": "0d57c73a75b7261c229acd969ee2eec10fc0d72432963f04536c5ab3ea439f7d",
            "transaction_status_hash": "a4489d66751139d2d3f120b2dacf4a8c52e6cadd7de603dc8ef1c66c350cba74",
            "transaction_merkle_root": "92ae06588fcc2006d23a2bfd16dd25d8a8c03a59ad7b23f17c4b9ddc45765f7e",
            "size": 2400,
            "version": 1,
            "height": 352098,
            "timestamp": 1575599756,
            "miner_address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
            "miner_name": "antpool",
            "chain_status": "mainnet",
            "nonce": 955392097488305462,
            "bits": 2017612633063013959,
            "hash_rate": 200114902,
            "difficulty": "69840100991",
            "transaction_count": 3
        }
    ],
    "pagination": {
        "current": 1,
        "limit": 2,
        "total": 352100
    }
}
```

### API 方法

- 区块相关
  - [`blocks`](#blocks)
  - [`block/height/{height}`](#block/height/{height})
  - [`block/hash/{hash}`](#block/hash/{hash})
  - [`latest-block`](#latest-block)
- 交易相关
  - [`transactions`](#transactions)
  - [`transaction/{transaction_id}`](#transaction/{transaction_id})
  - [`unconfirmed-transactions`](#unconfirmed-transactions)
  - [`unconfirmed-transactions/{transaction_id}`](#unconfirmed-transactions/{transaction_id})
- 地址资产相关
  - [`address/{address_id}/asset`](#address/{address_id}/asset)
  - [`asset/{asset_id}/address/address/{address_id}/transaction`](#asset/{asset_id}/address/address/{address_id}/transaction)
  - [`asset/{asset_id}/transaction`](#asset/{asset_id}/transaction)
  - [`asset/{asset_id}`](#asset/{asset_id})
  - [`list-asset`](#list-asset)
  - [`asset/{asset_id}/addresses`](#asset/{asset_id}/addresses)
- 日统计数据相关
  - [`daily/kline/{pair}`](#daily/kline/{pair})
  - [`daily/miner`](#daily/miner)
  - [`daily/total`](#daily/total)
  - [`kline/{pair}`](#kline/{pair})
- 历史统计相关
  - [`stat/diffculty`](#stat/diffculty)
  - [`stat/miner`](#stat/miner)
  - [`stat/hash-rate`](#stat/hash-rate)
  - [`stat/total`](#stat/total)
- 其他
  - [`kline/{pair}`](#kline/{pair})
  - [`nodes`](#nodes)

---

### Detail

#### `blocks`

获取当前最后一个区块

##### 参数

**可选**:

- `Integer`- *page*, 页码.
- `Integer`- *limit*, 每页的数量.

##### 返回

`Object`:

- `Array of Object` -*blocks*, block info list.
  - `String`- *hash*, hash of block.
  - `String`- *previous_block_hash*, previous block hash.
  - `String`- *transaction_merkle_root*, merkle root of transaction.
  - `String`- *transaction_status_hash*, merkle root of transaction status.
  - `Integer`- *size*, size of block.
  - `Integer`- *version*, version of block.
  - `Integer`- *heigh*, height of block.
  - `Integer`- *timestamp*, timestamp of block.
  - `Integer`- *nonce*, nonce value.
  - `Integer`- *bits*, bits of difficulty.
  - `String`- *difficulty*, difficulty value(String type).
  - `Integer`- *hash_rate*, the hash rate of block.
  - `Integer`- *miner_address*, the address of miner.
  - `Integer`- *transaction_count*, the count of transaction in the block.
  - `String`- *chain_status*, mainchain or orphan.
  - `String`- *cross_chain*, the flag of this block include cross chain transaction.
  - `String`- *miner_name*, the name of miner.
  - `String`- *anchor_id*, the anchor transaction id of partner.
  - `String`- *partner*, the name of partner.
- `Array of Object` -*pagination*, pagination info.
  - `Integer`- *current*, current number of page.
  - `Integer`- *limit*, number of data per page.
  - `Integer`- *total*, the number of total.

##### 例子

获取第一页的2个区块信息:

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/blocks?page=1&limit=2'
// Result
{
  "blocks": [
    {
      "hash": "4064ef8e909b40a41d3052ea677d50c0367a1f163c45b4a61ad83831998f9099",
      "previous_block_hash": "46c5c128269cc6001d59b61fb6a15c080ebb74a5395ea6b8c98a497a37d501c3",
      "transaction_status_hash": "c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc",
      "transaction_merkle_root": "f86c6eb178263999e1fbbc983aa1eae687d8767c20c8cde4e42e2ffd57c0b25e",
      "size": 416,
      "version": 1,
      "height": 352100,
      "timestamp": 1575599833,
      "miner_address": "bm1qlr5e6ep34tdr6566q9d6zp60d449338nwuhkdw",
      "miner_name": "matpool",
      "chain_status": "mainnet",
      "nonce": 803787027616180200,
      "bits": 2017612633063014000,
      "hash_rate": 1552002244,
      "difficulty": "69840100991",
      "transaction_count": 1
    },
    {
      "hash": "46c5c128269cc6001d59b61fb6a15c080ebb74a5395ea6b8c98a497a37d501c3",
      "previous_block_hash": "ad7f81fb93f47a5a14c9bea91d033af3fa77e85f844ad7ef0b5fe818df2d5feb",
      "transaction_status_hash": "6978a65b4ee5b6f4914fe5c05000459a803ecf59132604e5d334d64249c5e50a",
      "transaction_merkle_root": "5755cad062140b233d3bab55c8caaca5bc4fa0f59cf1b58aa9c0673780875bfa",
      "size": 7850,
      "version": 1,
      "height": 352099,
      "timestamp": 1575599788,
      "miner_address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
      "miner_name": "antpool",
      "chain_status": "mainnet",
      "nonce": 966909481856598300,
      "bits": 2017612633063014000,
      "hash_rate": 2182503155,
      "difficulty": "69840100991",
      "transaction_count": 2
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 2,
    "total": 352101
  }
}
```

---

#### `blocks/height/{height}`

根据区块高度获取区块信息

##### 参数

**可选**:

- `String`- height, block height.

##### 返回

**可选**:

- `String`- *hash*, hash of block.
- `Integer`- *size*, size of block.
- `Integer`- *version*, version of block.
- `Integer`- *height*, height of block.
- `String`- *previous_block_hash*, previous block hash.
- `Integer`- *timestamp*, timestamp of block.
- `Integer`- *nonce*, nonce value.
- `Integer`- *bits*, bits of difficulty.
- `String`- *difficulty*, difficulty value(String type).
- `String`- *transaction_merkle_root*, merkle root of transaction.
- `String`- *transaction_status_hash*, merkle root of transaction status.
- `Integer`- *hash_rate*, the hash rate of block.
- `Integer`- *miner_address*, the address of miner.
- `Integer`- *transaction_count*, the count of transaction in the block.
- `String`- *chain_status*, mainchain or orphan.
- `String`- *cross_chain*, the flag of this block include cross chain transaction.
- `String`- *miner_name*, the name of miner.
- `String`- *anchor_id*, the anchor transaction id of partner.
- `String`- *partner*, the name of partner.
- `Array of Object` - *transactions*, transaction object:
  - `String` - *id*, transaction id, hash of the transaction.
  - `Integer` - *version*, version of transaction.
  - `Integer` - *size*, size of transaction.
  - `Integer` - *timestamp*, the unix timestamp for when the requst was responsed.
  - `Integer` - *time_range*, the unix timestamp for when the requst was responsed.
  - `Boolean` - *status_fail*, whether the state of the request has failed.
  - `String` - *mux_id*, the previous transaction mux id(source id of utxo).
  - `Integer` - *block_height*, block height.
  - `Integer` - *chain_status*, mainnet or orphan.
  - `Integer` - *coinbase*, the flag of coinbase transaction.
  - `Boolean` - *cross_chain*, the flag of cross chain transaction.
  - `Integer` - *trx_fee*, transaction fee.
  - `Integer` - *trx_amount*, the amount of transaction.
  - `Integer` - *confirmations*, the number comfirmed.
  - `Array of Object` - *inputs*, object of inputs for the transaction.
    - `String` - *txtype*, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - *asset_id*, asset id.
    - `String` - *asset_alias*, name of asset.
    - `Object` - *asset_definition*, definition of asset(json object).
    - `Integer` - *amount*, amount of asset.
    - `Object` - *issuance_program*, issuance program, it only exist when type is 'issue'.
    - `Object` - *control_program*, control program of account, it only exist when type is 'spend'.
    - `String` - *address*, address of account, it only exist when type is 'spend'.
    - `String` - *spent_output_id*, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - *account_id*, account id.
    - `String` - *account_alias*, name of account.
    - `Object` - *arbitrary*, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - *input_id*, hash of input action.
    - `Array of String` - *witness_arguments*, witness arguments.
    - `String` - *asset_name*, asset name.
    - `String` - *asset_decimals*, decimal of asset.
  - `Array of Object` - *outputs*, object of outputs for the transaction.
    - `String` - *txtype*, the type of output action, available option include: 'retire', 'control'.
    - `String` - *id*, outputid related to utxo.
    - `Integer` - *position*, position of outputs.
    - `String` - *asset_id*, asset id.
    - `String` - *asset_alias*, name of asset.
    - `Object` - *asset_definition*, definition of asset(json object).
    - `Integer` - *amount*, amount of asset.
    - `String` - *account_id*, account id.
    - `String` - *account_alias*, name of account.
    - `Object` - *control_program*, control program of account.
    - `String` - *address*, address of account.
    - `String` - *asset_name*, asset name.
    - `String` - *asset_decimals*, decimal of asset.

##### 例子

获取区块高度为123的区块信息:

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/block/123'
// Result
{
  "hash": "531e1ebdd6f38924bfdd86331bc1286b1626da0d0b5b6f6da233a2d054d7f041",
  "previous_block_hash": "91a1fce360040bb9943f8374980032692ba967e1fa24e318b6a1fb3f648b0a58",
  "transaction_status_hash": "c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc",
  "transaction_merkle_root": "4031f270c360fd98187db674b94dc0a9aea371e69701b8451ef9238666d4b022",
  "size": 396,
  "version": 1,
  "height": 123,
  "timestamp": 1524556238,
  "miner_address": "bm1qrp2fmpx675e5f5e9vwpscl8e08wpn4wqhrv0zt",
  "miner_name": "bm1qrp2fm...",
  "chain_status": "mainnet",
  "nonce": 8946685314549,
  "bits": 2161727821137910500,
  "hash_rate": 688854,
  "difficulty": "15154807",
  "transaction_count": 1,
  "transactions": [
    {
      "id": "a3f370af6df14f07861ae0a0219f52b9db606dc04624fc190ebbee7142bf2177",
      "timestamp": 1524556238,
      "block_height": 123,
      "trx_amount": 41250000000,
      "trx_fee": 0,
      "status_fail": false,
      "coinbase": true,
      "size": 76,
      "chain_status": "mainnet",
      "time_range": 0,
      "index_id": 124,
      "mux_id": "38296f40609dcf661e3706b444147daa934b669c8806a38c5a47341d070e7f64",
      "inputs": [
        {
          "txtype": "coinbase",
          "asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
          "amount": 0,
          "spent_output_id": "<nil>",
          "arbitrary": "7b",
          "input_id": "8a2df61847e8b8a00922f0ac95a14d0f8f9a16a20bc17f11c890365bae819baa",
          "witness_arguments": null,
          "asset_name": "",
          "asset_definition": "{}",
          "asset_decimals": 0
        }
      ],
      "outputs": [
        {
          "txtype": "control",
          "id": "8dfa1bc333aa4c47332b3b941641e87341d3d79b102095b4a26d88c7b3ed8087",
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 41250000000,
          "control_program": "001418549d84daf53344d32563830c7cf979dc19d5c0",
          "address": "bm1qrp2fmpx675e5f5e9vwpscl8e08wpn4wqhrv0zt",
          "asset_name": "BTM",
          "asset_definition": "{}",
          "position": 0,
          "asset_decimals": 8
        }
      ]
    }
  ]
}
```

---

#### `blocks/hash/{hash}`

通过区块hash获取区块信息

##### 参数

**可选**:

- `String`- *hash*, block hash.

**返回**

`Object`:


- `String`- *hash*, hash of block.
- `Integer`- *size*, size of block.
- `Integer`- *version*, version of block.
- `Integer`- *height*, height of block.
- `String`- *previous_block_hash*, previous block hash.
- `Integer`- *timestamp*, timestamp of block.
- `Integer`- *nonce*, nonce value.
- `Integer`- *bits*, bits of difficulty.
- `String`- *difficulty*, difficulty value(String type).
- `String`- *transaction_merkle_root*, merkle root of transaction.
- `String`- *transaction_status_hash*, merkle root of transaction status.
- `Integer`- *hash_rate*, the hash rate of block.
- `Integer`- *miner_address*, the address of miner.
- `Integer`- *transaction_count*, the count of transaction in the block.
- `String`- *chain_status*, mainchain or orphan.
- `String`- *cross_chain*, the flag of this block include cross chain transaction.
- `String`- *miner_name*, the name of miner.
- `String`- *anchor_id*, the anchor transaction id of partner.
- `String`- *partner*, the name of partner.
- `Array of Object` - *transactions*, transaction object:
  - `String` - *id*, transaction id, hash of the transaction.
  - `Integer` - *version*, version of transaction.
  - `Integer` - *size*, size of transaction.
  - `Integer` - *timestamp*, the unix timestamp for when the requst was responsed.
  - `Integer` - *time_range*, the unix timestamp for when the requst was responsed.
  - `Boolean` - *status_fail*, whether the state of the request has failed.
  - `String` - *mux_id*, the previous transaction mux id(source id of utxo).
  - `Integer` - *block_height*, block height.
  - `Integer` - *chain_status*, mainnet or orphan.
  - `Integer` - *coinbase*, the flag of coinbase transaction.
  - `Boolean` - *cross_chain*, the flag of cross chain transaction.
  - `Integer` - *trx_fee*, transaction fee.
  - `Integer` - *trx_amount*, the amount of transaction.
  - `Integer` - *confirmations*, the number comfirmed.
  - `Array of Object` - *inputs*, object of inputs for the transaction.
    - `String` - *txtype*, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - *asset_id*, asset id.
    - `String` - *asset_alias*, name of asset.
    - `Object` - *asset_definition*, definition of asset(json object).
    - `Integer` - *amount*, amount of asset.
    - `Object` - *issuance_program*, issuance program, it only exist when type is 'issue'.
    - `Object` - *control_program*, control program of account, it only exist when type is 'spend'.
    - `String` - *address*, address of account, it only exist when type is 'spend'.
    - `String` - *spent_output_id*, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - *account_id*, account id.
    - `String` - *account_alias*, name of account.
    - `Object` - *arbitrary*, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - *input_id*, hash of input action.
    - `Array of String` - *witness_arguments*, witness arguments.
    - `String` - *asset_name*, asset name.
    - `String` - *asset_decimals*, decimal of asset.
  - `Array of Object` - *outputs*, object of outputs for the transaction.
    - `String` - *txtype*, the type of output action, available option include: 'retire', 'control'.
    - `String` - *id*, outputid related to utxo.
    - `Integer` - *position*, position of outputs.
    - `String` - *asset_id*, asset id.
    - `String` - *asset_alias*, name of asset.
    - `Object` - *asset_definition*, definition of asset(json object).
    - `Integer` - *amount*, amount of asset.
    - `String` - *account_id*, account id.
    - `String` - *account_alias*, name of account.
    - `Object` - *control_program*, control program of account.
    - `String` - *address*, address of account.
    - `String` - *asset_name*, asset name.
    - `String` - *asset_decimals*, decimal of asset.

##### 例子：
获取区块hash是 531e1ebdd6f38924bfdd86331bc1286b1626da0d0b5b6f6da233a2d054d7f041的区块信息:

```json
// Request
curl -X GET "https://blockmeta.com/api/v3/block/hash/531e1ebdd6f38924bfdd86331bc1286b1626da0d0b5b6f6da233a2d054d7f041" -H "accept: application/json"

// Result
{
  "hash": "531e1ebdd6f38924bfdd86331bc1286b1626da0d0b5b6f6da233a2d054d7f041",
  "previous_block_hash": "91a1fce360040bb9943f8374980032692ba967e1fa24e318b6a1fb3f648b0a58",
  "transaction_status_hash": "c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc",
  "transaction_merkle_root": "4031f270c360fd98187db674b94dc0a9aea371e69701b8451ef9238666d4b022",
  "size": 396,
  "version": 1,
  "height": 123,
  "timestamp": 1524556238,
  "miner_address": "bm1qrp2fmpx675e5f5e9vwpscl8e08wpn4wqhrv0zt",
  "miner_name": "bm1qrp2fm...",
  "chain_status": "mainnet",
  "nonce": 8946685314549,
  "bits": 2161727821137910500,
  "hash_rate": 688854,
  "difficulty": "15154807",
  "transaction_count": 1,
  "transactions": [
    {
      "id": "a3f370af6df14f07861ae0a0219f52b9db606dc04624fc190ebbee7142bf2177",
      "timestamp": 1524556238,
      "block_height": 123,
      "trx_amount": 41250000000,
      "trx_fee": 0,
      "status_fail": false,
      "coinbase": true,
      "size": 76,
      "chain_status": "mainnet",
      "time_range": 0,
      "index_id": 124,
      "mux_id": "38296f40609dcf661e3706b444147daa934b669c8806a38c5a47341d070e7f64",
      "inputs": [
        {
          "txtype": "coinbase",
          "asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
          "amount": 0,
          "spent_output_id": "<nil>",
          "arbitrary": "7b",
          "input_id": "8a2df61847e8b8a00922f0ac95a14d0f8f9a16a20bc17f11c890365bae819baa",
          "witness_arguments": null,
          "asset_name": "",
          "asset_definition": "{}",
          "asset_decimals": 0
        }
      ],
      "outputs": [
        {
          "txtype": "control",
          "id": "8dfa1bc333aa4c47332b3b941641e87341d3d79b102095b4a26d88c7b3ed8087",
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 41250000000,
          "control_program": "001418549d84daf53344d32563830c7cf979dc19d5c0",
          "address": "bm1qrp2fmpx675e5f5e9vwpscl8e08wpn4wqhrv0zt",
          "asset_name": "BTM",
          "asset_definition": "{}",
          "position": 0,
          "asset_decimals": 8
        }
      ]
    }
  ]
}
```

---

#### `transactions`

获取当前区块的最后一笔交易

##### 参数

**可选**:

- `Integer`- *page*, page nunber of data.
- `Integer`- *limit*, number of data per page.

##### 返回

**Object**:

- `Array of Object` - *transactions*, transaction object:
  - `String` - *id*, transaction id, hash of the transaction.
  - `Integer` - *version*, version of transaction.
  - `Integer` - *size*, size of transaction.
  - `Integer` - *timestamp*, the unix timestamp for when the requst was responsed.
  - `Integer` - *time_range*, the unix timestamp for when the requst was responsed.
  - `Boolean` - *status_fail*, whether the state of the request has failed.
  - `String` - *mux_id*, the previous transaction mux id(source id of utxo).
  - `Integer` - *block_height*, block height.
  - `Integer` - *chain_status*, mainnet or orphan.
  - `Integer` - *coinbase*, the flag of coinbase transaction.
  - `Boolean` - *cross_chain*, the flag of cross chain transaction.
  - `Integer` - *trx_fee*, transaction fee.
  - `Integer` - *trx_amount*, the amount of transaction.
  - `Integer` - *confirmations*, the number comfirmed.
  - `Array of Object` - *inputs*, object of inputs for the transaction.
    - `String` - *txtype*, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - *asset_id*, asset id.
    - `String` - *asset_alias*, name of asset.
    - `Object` - *asset_definition*, definition of asset(json object).
    - `Integer` - *amount*, amount of asset.
    - `Object` - *issuance_program*, issuance program, it only exist when type is 'issue'.
    - `Object` - *control_program*, control program of account, it only exist when type is 'spend'.
    - `String` - *address*, address of account, it only exist when type is 'spend'.
    - `String` - *spent_output_id*, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - *account_id*, account id.
    - `String` - *account_alias*, name of account.
    - `Object` - *arbitrary*, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - *input_id*, hash of input action.
    - `Array of String` - *witness_arguments*, witness arguments.
    - `String` - *asset_name*, asset name.
    - `String` - *asset_decimals*, decimal of asset.
  - `Array of Object` - *outputs*, object of outputs for the transaction.
    - `String` - *txtype*, the type of output action, available option include: 'retire', 'control'.
    - `String` - *id*, outputid related to utxo.
    - `Integer` - *position*, position of outputs.
    - `String` - *asset_id*, asset id.
    - `String` - *asset_alias*, name of asset.
    - `Object` - *asset_definition*, definition of asset(json object).
    - `Integer` - *amount*, amount of asset.
    - `String` - *account_id*, account id.
    - `String` - *account_alias*, name of account.
    - `Object` - *control_program*, control program of account.
    - `String` - *address*, address of account.
    - `String` - *asset_name*, asset name.
    - `String` - *asset_decimals*, decimal of asset.
- `Array of Object` -*pagination*, pagination info.
  - `Integer`- *current*, current number of page.
  - `Integer`- *limit*, number of data per page.
  - `Integer`- *total*, the number of total.

##### 例子

获取当前第一页的一笔交易信息:

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/transactions?page=1&limit=2'
// Result
{
  "transactions": [
    {
      "id": "766bf68c5717d3a58fe6096e81d18549adba77179b91458083b14f537e02e0df",
      "timestamp": 1575624156,
      "block_height": 352248,
      "trx_amount": 7497000000,
      "trx_fee": 1000000,
      "status_fail": false,
      "coinbase": false,
      "size": 611,
      "chain_status": "mainnet",
      "time_range": 0,
      "index_id": 754646,
      "mux_id": "c8ca464ada81eeff2ebbb0b0e0644bb9f73500438f69cf8bb1bccaba23287e47",
      "inputs": [
        {
          "txtype": "spend",
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 7498000000,
          "control_program": "0014fabbb8deb24fcf4f11a8a5a4ce59e1bd73e85c8b",
          "address": "bm1ql2am3h4jfl857ydg5kjvuk0ph4e7shytmscnqm",
          "spent_output_id": "c66bbe9fee9e9b0e9f251838699ceb906a29eec2eff82ca281602511a63d59d3",
          "input_id": "a2bc701a4d409b0d109a352be1f3f643747d605f8d460b9ebf10b7434d159620",
          "witness_arguments": [
            "98d0fed72a88637cf1d664c2c42bd8bc8d71840dbfa21535a240b2b349678c5e7e92c859ed797fc3b7884ab2b1acb514b990b123dfab44fc1c76588bd4d6e509",
            "40f9d65e2d3688c7b051f915b94e9c6b6b76481494927dfb2d1311fe86abebba"
          ],
          "asset_name": "BTM",
          "asset_definition": "{}",
          "asset_decimals": 8
        },
        {
          "txtype": "spend",
          "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
          "amount": 46446400000000,
          "control_program": "00148b98a1c27a1675b87fb32e0e57fb7e323770f189",
          "address": "bm1q3wv2rsn6ze6mslan9c8907m7xgmhpuvf063m3n",
          "spent_output_id": "0e89b87f6def9d652b1ee8cedcefe0fdebd6dbcd99d989018f2ec0e799caecdb",
          "input_id": "9b6faa99ab8d821f9229c0b3f4112da9a3390d3d7e82074ee32bf8754d559694",
          "witness_arguments": [
            "bc0a1030637a8a2b5bfc486353ad80d9bc254a6d9bf5b9bf8a244d718f8e37d8fed8cc7ca8cb78a43255bc04bda7a7e332efd2595d9dc64713e242e64301eb00",
            "1f7290e168c5c30cf87eb2cefd03b04abfa8f297b103f4c010972bdd952df6d7"
          ],
          "asset_name": "BQB",
          "asset_definition": "{}",
          "asset_decimals": 8
        }
      ],
      "outputs": [
        {
          "txtype": "control",
          "id": "10e3618b29e0bb34eb2e774a9877deacc8585610660f5e23aebb479ce4427659",
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 7497000000,
          "control_program": "00141672adef4cbb0d9ff9665b2efbcc6641b2e1389c",
          "address": "bm1qzee2mm6vhvxel7txtvh0hnrxgxewzwyu7msqqu",
          "asset_name": "BTM",
          "asset_definition": "{}",
          "position": 0,
          "asset_decimals": 8
        },
        {
          "txtype": "control",
          "id": "03774c875fde718bf52a60b1a2171c7d079ac629b6a969032837ada7d8efb7af",
          "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
          "amount": 46446300000000,
          "control_program": "0014145c67bc4a87a7fcecb20c2f99e371ab9ca0efe6",
          "address": "bm1qz3wx00z2s7nlem9jpshencm34ww2pmlxsaq64g",
          "asset_name": "BQB",
          "asset_definition": "{}",
          "position": 1,
          "asset_decimals": 8
        },
        {
          "txtype": "retire",
          "id": "9a01c1b612627edeaba09cba416f8b53e8e41f3766e892b90a9b0ea903d9c096",
          "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
          "amount": 100000000,
          "control_program": "6a238028edc5d0d0791cca31038562fcbe3ea522a55a88af75b93b0e2b3e03fe307a2fcf90",
          "asset_name": "BQB",
          "asset_definition": "{}",
          "position": 2,
          "asset_decimals": 8
        }
      ],
      "confirmations": 1
    },
    {
      "id": "b498160ad1bee5043923ff14f767d6adc63d98da300ddd3775e415317c5f2f8b",
      "timestamp": 1575624156,
      "block_height": 352248,
      "trx_amount": 41251000000,
      "trx_fee": 0,
      "status_fail": false,
      "coinbase": true,
      "size": 82,
      "chain_status": "mainnet",
      "time_range": 0,
      "index_id": 754645,
      "mux_id": "5a471253f41e684e34d4d8ab8baa7ac261ec88b52cfb275719bd11462dab9e0a",
      "inputs": [
        {
          "txtype": "coinbase",
          "asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
          "amount": 0,
          "spent_output_id": "<nil>",
          "arbitrary": "00333532323438",
          "input_id": "7cf69003f8cfc9968adf31a886bc79dfa8bf11386ae85d7b10f8b57d61527851",
          "witness_arguments": null,
          "asset_name": "",
          "asset_definition": "{}",
          "asset_decimals": 0
        }
      ],
      "outputs": [
        {
          "txtype": "control",
          "id": "0da22c93aa7735ccdd1503ce6b313da212db7b212abc53503c9cc68d3548a7be",
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 41251000000,
          "control_program": "001479c730753af7e89bd97a7fc158cdfe30331d7447",
          "address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
          "asset_name": "BTM",
          "asset_definition": "{}",
          "position": 0,
          "asset_decimals": 8
        }
      ],
      "confirmations": 1
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 2,
    "total": 754646
  }
}
```

---

#### `transaction/{hash}`

根据交易id获取交易详情

##### 参数

**必选**:

- `String`- *hash*, tranasction id.

##### 返回

**`Object`**:

- `String` - *id*, transaction id, hash of the transaction.
- `Integer` - *version*, version of transaction.
- `Integer` - *size*, size of transaction.
- `Integer` - *timestamp*, the unix timestamp for when the requst was responsed.
- `Integer` - *time_range*, the unix timestamp for when the requst was responsed.
- `Boolean` - *status_fail*, whether the state of the request has failed.
- `String` - *mux_id*, the previous transaction mux id(source id of utxo).
- `Integer` - *block_height*, block height.
- `Integer` - *chain_status*, mainnet or orphan.
- `Integer` - *coinbase*, the flag of coinbase transaction.
- `Boolean` - *cross_chain*, the flag of cross chain transaction.
- `Integer` - *trx_fee*, transaction fee.
- `Integer` - *trx_amount*, the amount of transaction.
- `Integer` - *confirmations*, the number comfirmed.
- `Array of Object` - *inputs*, object of inputs for the transaction.
  - `String` - *txtype*, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
  - `String` - *asset_id*, asset id.
  - `String` - *asset_alias*, name of asset.
  - `Object` - *asset_definition*, definition of asset(json object).
  - `Integer` - *amount*, amount of asset.
  - `Object` - *issuance_program*, issuance program, it only exist when type is 'issue'.
  - `Object` - *control_program*, control program of account, it only exist when type is 'spend'.
  - `String` - *address*, address of account, it only exist when type is 'spend'.
  - `String` - *spent_output_id*, the front of outputID to be spent in this input, it only exist when type is 'spend'.
  - `String` - *account_id*, account id.
  - `String` - *account_alias*, name of account.
  - `Object` - *arbitrary*, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
  - `String` - *input_id*, hash of input action.
  - `Array of String` - *witness_arguments*, witness arguments.
  - `String` - *asset_name*, asset name.
  - `String` - *asset_decimals*, decimal of asset.
- `Array of Object` - *outputs*, object of outputs for the transaction.
  - `String` - *txtype*, the type of output action, available option include: 'retire', 'control'.
  - `String` - *id*, outputid related to utxo.
  - `Integer` - *position*, position of outputs.
  - `String` - *asset_id*, asset id.
  - `String` - *asset_alias*, name of asset.
  - `Object` - *asset_definition*, definition of asset(json object).
  - `Integer` - *amount*, amount of asset.
  - `String` - *account_id*, account id.
  - `String` - *account_alias*, name of account.
  - `Object` - *control_program*, control program of account.
  - `String` - *address*, address of account.
  - `String` - *asset_name*, asset name.
  - `String` - *asset_decimals*, decimal of asset.

##### 例子

获取交易id是766bf68c5717d3a58fe6096e81d18549adba77179b91458083b14f537e02e0df的交易详情：

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/transaction/766bf68c5717d3a58fe6096e81d18549adba77179b91458083b14f537e02e0df'

// Result
{
  "id": "766bf68c5717d3a58fe6096e81d18549adba77179b91458083b14f537e02e0df",
  "timestamp": 1575624156,
  "block_height": 352248,
  "trx_amount": 7497000000,
  "trx_fee": 1000000,
  "status_fail": false,
  "coinbase": false,
  "size": 611,
  "chain_status": "mainnet",
  "time_range": 0,
  "index_id": 754646,
  "mux_id": "c8ca464ada81eeff2ebbb0b0e0644bb9f73500438f69cf8bb1bccaba23287e47",
  "inputs": [
    {
      "txtype": "spend",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 7498000000,
      "control_program": "0014fabbb8deb24fcf4f11a8a5a4ce59e1bd73e85c8b",
      "address": "bm1ql2am3h4jfl857ydg5kjvuk0ph4e7shytmscnqm",
      "spent_output_id": "c66bbe9fee9e9b0e9f251838699ceb906a29eec2eff82ca281602511a63d59d3",
      "input_id": "a2bc701a4d409b0d109a352be1f3f643747d605f8d460b9ebf10b7434d159620",
      "witness_arguments": [
        "98d0fed72a88637cf1d664c2c42bd8bc8d71840dbfa21535a240b2b349678c5e7e92c859ed797fc3b7884ab2b1acb514b990b123dfab44fc1c76588bd4d6e509",
        "40f9d65e2d3688c7b051f915b94e9c6b6b76481494927dfb2d1311fe86abebba"
      ],
      "asset_name": "BTM",
      "asset_definition": "{}",
      "asset_decimals": 8
    },
    {
      "txtype": "spend",
      "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
      "amount": 46446400000000,
      "control_program": "00148b98a1c27a1675b87fb32e0e57fb7e323770f189",
      "address": "bm1q3wv2rsn6ze6mslan9c8907m7xgmhpuvf063m3n",
      "spent_output_id": "0e89b87f6def9d652b1ee8cedcefe0fdebd6dbcd99d989018f2ec0e799caecdb",
      "input_id": "9b6faa99ab8d821f9229c0b3f4112da9a3390d3d7e82074ee32bf8754d559694",
      "witness_arguments": [
        "bc0a1030637a8a2b5bfc486353ad80d9bc254a6d9bf5b9bf8a244d718f8e37d8fed8cc7ca8cb78a43255bc04bda7a7e332efd2595d9dc64713e242e64301eb00",
        "1f7290e168c5c30cf87eb2cefd03b04abfa8f297b103f4c010972bdd952df6d7"
      ],
      "asset_name": "BQB",
      "asset_definition": "{}",
      "asset_decimals": 8
    }
  ],
  "outputs": [
    {
      "txtype": "control",
      "id": "10e3618b29e0bb34eb2e774a9877deacc8585610660f5e23aebb479ce4427659",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 7497000000,
      "control_program": "00141672adef4cbb0d9ff9665b2efbcc6641b2e1389c",
      "address": "bm1qzee2mm6vhvxel7txtvh0hnrxgxewzwyu7msqqu",
      "asset_name": "BTM",
      "asset_definition": "{}",
      "position": 0,
      "asset_decimals": 8
    },
    {
      "txtype": "control",
      "id": "03774c875fde718bf52a60b1a2171c7d079ac629b6a969032837ada7d8efb7af",
      "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
      "amount": 46446300000000,
      "control_program": "0014145c67bc4a87a7fcecb20c2f99e371ab9ca0efe6",
      "address": "bm1qz3wx00z2s7nlem9jpshencm34ww2pmlxsaq64g",
      "asset_name": "BQB",
      "asset_definition": "{}",
      "position": 1,
      "asset_decimals": 8
    },
    {
      "txtype": "retire",
      "id": "9a01c1b612627edeaba09cba416f8b53e8e41f3766e892b90a9b0ea903d9c096",
      "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
      "amount": 100000000,
      "control_program": "6a238028edc5d0d0791cca31038562fcbe3ea522a55a88af75b93b0e2b3e03fe307a2fcf90",
      "asset_name": "BQB",
      "asset_definition": "{}",
      "position": 2,
      "asset_decimals": 8
    }
  ],
  "confirmations": 2
}
```

---

#### `unconfirmed-transactions`

从Bytom节点交易池中获取未确认的交易

##### 参数

**无**

##### 返回

**`对象`**:

- `Array of Object`
  - `String` - *id*, transaction id, hash of the transaction.
  - `Integer` - *version*, version of transaction.
  - `Integer` - *size*, size of transaction.
  - `Integer` - *time_range*, the unix timestamp for when the requst was responsed.
  - `Boolean` - *status_fail*, whether the state of the request has failed.
  - `String` - *mux_id*, the previous transaction mux id(source id of utxo).
  - `Integer` - *fee*, transaction fee.
  - `Integer` - *transaction_amount*, the amount of transaction.

##### 例子：

获取未确认的交易

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/unconfirmed-transactions'


// Result
[
  {
    "id": "1825f6901cce38b446a3713ffa80d9eb3cf24d72d32591138dcb344e8ec83c4d",
    "version": 1,
    "size": 611,
    "time_range": 0,
    "status_fail": false,
    "mux_id": "6a4d3ee2245cf1552248640861d2d7e3b024fe47be79603468b873a33eb8b19c",
    "fee": 1000000,
    "transaction_amount": 7496000000,
  }
]
```

---

#### `unconfirmed-transaction/{hash}`

根据交易id获取bytom节点交易池中未确认的交易.

##### 参数

**对象**:

- `String`- *hash*, tranasction id.

##### 返回

**`Object`**:

- `String` - *id*, transaction id, hash of the transaction.
- `Integer` - *version*, version of transaction.
- `Integer` - *size*, size of transaction.
- `Integer` - *timestamp*, the unix timestamp for when the requst was responsed.
- `Integer` - *time_range*, the unix timestamp for when the requst was responsed.
- `Boolean` - *status_fail*, whether the state of the request has failed.
- `String` - *mux_id*, the previous transaction mux id(source id of utxo).
- `Integer` - *block_height*, block height.
- `Integer` - *chain_status*, mainnet or orphan.
- `Integer` - *coinbase*, the flag of coinbase transaction.
- `Boolean` - *cross_chain*, the flag of cross chain transaction.
- `Integer` - *trx_fee*, transaction fee.
- `Integer` - *trx_amount*, the amount of transaction.
- `Integer` - *confirmations*, the number comfirmed.
- `Array of Object` - *inputs*, object of inputs for the transaction.
  - `String` - *txtype*, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
  - `String` - *asset_id*, asset id.
  - `String` - *asset_alias*, name of asset.
  - `Object` - *asset_definition*, definition of asset(json object).
  - `Integer` - *amount*, amount of asset.
  - `Object` - *issuance_program*, issuance program, it only exist when type is 'issue'.
  - `Object` - *control_program*, control program of account, it only exist when type is 'spend'.
  - `String` - *address*, address of account, it only exist when type is 'spend'.
  - `String` - *spent_output_id*, the front of outputID to be spent in this input, it only exist when type is 'spend'.
  - `String` - *account_id*, account id.
  - `String` - *account_alias*, name of account.
  - `Object` - *arbitrary*, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
  - `String` - *input_id*, hash of input action.
  - `Array of String` - *witness_arguments*, witness arguments.
  - `String` - *asset_name*, asset name.
  - `String` - *asset_decimals*, decimal of asset.
- `Array of Object` - *outputs*, object of outputs for the transaction.
  - `String` - *txtype*, the type of output action, available option include: 'retire', 'control'.
  - `String` - *id*, outputid related to utxo.
  - `Integer` - *position*, position of outputs.
  - `String` - *asset_id*, asset id.
  - `String` - *asset_alias*, name of asset.
  - `Object` - *asset_definition*, definition of asset(json object).
  - `Integer` - *amount*, amount of asset.
  - `String` - *account_id*, account id.
  - `String` - *account_alias*, name of account.
  - `Object` - *control_program*, control program of account.
  - `String` - *address*, address of account.
  - `String` - *asset_name*, asset name.
  - `String` - *asset_decimals*, decimal of asset.

##### 例子

获取交易ID为1825f6901cce38b446a3713ffa80d9eb3cf24d72d32591138dcb344e8ec83c4d的未确认交易

```JSON
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/unconfirmed-transaction/1825f6901cce38b446a3713ffa80d9eb3cf24d72d32591138dcb344e8ec83c4d'

// Result
{
  "id": "1825f6901cce38b446a3713ffa80d9eb3cf24d72d32591138dcb344e8ec83c4d",
  "version": 1,
  "timestamp": 0,
  "trx_amount": 7496000000,
  "trx_fee": 1000000,
  "status_fail": false,
  "size": 611,
  "time_range": 0,
  "mux_id": "6a4d3ee2245cf1552248640861d2d7e3b024fe47be79603468b873a33eb8b19c",
  "inputs": [
    {
      "type": "spend",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 7497000000,
      "control_program": "00141672adef4cbb0d9ff9665b2efbcc6641b2e1389c",
      "address": "bm1qzee2mm6vhvxel7txtvh0hnrxgxewzwyu7msqqu",
      "spent_output_id": "10e3618b29e0bb34eb2e774a9877deacc8585610660f5e23aebb479ce4427659",
      "input_id": "8f8eb1b5b66d389321c6f17d565b94d21bdd68762b71a119026aa4457ed3ac3e",
      "witness_arguments": [
        "b8191d6ab68de7a5373e8501b385e06177e85809f6e585d7e0e6cd011769174730b5e2e9ed2f504d10d88eaf970fdee443352fab83d44641ea269b2448c3ec00",
        "42f86529ead4ac40e17f9606513e25db88738b885e54c0438a9f99eb888f85a5"
      ],
      "asset_name": "BTM",
      "asset_decimals": 8
    },
    {
      "type": "spend",
      "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
      "amount": 46446300000000,
      "control_program": "0014145c67bc4a87a7fcecb20c2f99e371ab9ca0efe6",
      "address": "bm1qz3wx00z2s7nlem9jpshencm34ww2pmlxsaq64g",
      "spent_output_id": "03774c875fde718bf52a60b1a2171c7d079ac629b6a969032837ada7d8efb7af",
      "input_id": "9e4dbdff4559727a80f43abb1c12e0b7fc3199176b8d88947f749fab3e619fa3",
      "witness_arguments": [
        "6c60d963361ed5ef2021df13e20570a82fd6ab66e4fb40c53879b6c8e3fbfc7a49ecec2097b5435cdc8edd6c6b5df61aaeef3fa67c115d72ce77eb3756988201",
        "c8ff9152d2b5733fc45142e74079232e287db30b0cd61158178de3b48c01e2c5"
      ],
      "asset_name": "BQB",
      "asset_decimals": 8
    }
  ],
  "outputs": [
    {
      "type": "control",
      "id": "7b349e17b061ce19effb5efb4fddad5ef703df5e03a1e23759daf94c4e26626e",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 7496000000,
      "control_program": "001427488a44aced7c90e1b49b6eb4cc0e4cb88976ae",
      "address": "bm1qyayg539va47fpcd5ndhtfnqwfjugja4wp95c76",
      "asset_name": "BTM",
      "position": 0,
      "asset_decimals": 8
    },
    {
      "type": "control",
      "id": "3be543f5f315865b1902c762decdf40f45f6a444fd9d843054d3ea312150941c",
      "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
      "amount": 46446200000000,
      "control_program": "00144be00f62973235a3c3a4be82975bc76407b42b47",
      "address": "bm1qf0sq7c5hxg668sayh6pfwk78vsrmg268zcj042",
      "asset_name": "BQB",
      "position": 1,
      "asset_decimals": 8
    },
    {
      "type": "retire",
      "id": "bf0e9274734b1e544e8d6c07f75ebd45569b38013a1968d639c29d80f5a83225",
      "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
      "amount": 100000000,
      "control_program": "6a232a8d95cacd59c574422010176bbf31eaecd3b6fa9edac56de762a2146646043f2fd00a",
      "asset_name": "BQB",
      "position": 2,
      "asset_decimals": 8
    }
  ]
}
```

---

#### `address/{address_id}/asset`

获取地址的所有账户资产，接收资产数量，发送资产数量和最近的交易信息。

##### 参数

**`必选`**:

- `String`- *address*, address

##### 返回

`Object`:

- `String` - *address_id*, address.
- `String` - *asset_id*, asset id.
- `String` - *asset_name*, asset name.
- `String` - *balance*, address balance.
- `String` - *receive*, address reveived asset total amount.
- `String` - *sent*, address sent asset total amount.
- `Integer` - *join_timestamp*, first time create address.
- `Integer` - *decimals*, the decimals of asset.
- `Integer` - *s_timestamp*, latest transaction timestamp.
- `Integer` - *tx_count*, the address's transaction count.

##### 例子
获取地址为bm1qtmt60f9jamarpyvw2eplhmsuzrkfcmxp37s94fzvg9lypgnvsg7qt2q492的所有资产信息。

```json
// Request
curl -X GET "https://blockmeta.com/api/v3/address/bm1qtmt60f9jamarpyvw2eplhmsuzrkfcmxp37s94fzvg9lypgnvsg7qt2q492/asset" -H "accept: application/json"

// Result
[
  {
    "address_id": "bm1qtmt60f9jamarpyvw2eplhmsuzrkfcmxp37s94fzvg9lypgnvsg7qt2q492",
    "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "asset_name": "BTM",
    "balance": "21000000000000000",
    "receive": "105000000280000000",
    "sent": "84000000280000000",
    "join_timestamp": 1525680122,
    "s_timestamp": 1563774584,
    "decimals": 8,
    "tx_count": 8
  },
  {
    "address_id": "bm1qtmt60f9jamarpyvw2eplhmsuzrkfcmxp37s94fzvg9lypgnvsg7qt2q492",
    "asset_id": "419449812fe7fcbbd51ad72344b648beae7b00a84ebd7e4c951a2fcde9d41f39",
    "asset_name": "Asset",
    "balance": "800000000",
    "receive": "800000000",
    "sent": "0",
    "join_timestamp": 1532616063,
    "s_timestamp": 1532616063,
    "decimals": 8,
    "tx_count": 1
  },
  {
    "address_id": "bm1qtmt60f9jamarpyvw2eplhmsuzrkfcmxp37s94fzvg9lypgnvsg7qt2q492",
    "asset_id": "61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3",
    "asset_name": "DCA",
    "balance": "1000000000",
    "receive": "1000000000",
    "sent": "0",
    "join_timestamp": 1565836134,
    "s_timestamp": 1565836134,
    "decimals": 8,
    "tx_count": 1
  }
]
```

---

#### `asset/{asset_id}/address/address/{address_id}/transaction`

获取指定地址指定资产相关的交易

##### 参数

**`必选`**:

- `String`- *asset_id*, asset
- `String`- *address_id*, address

**可选**:

- `Integer`- *page*, page nunber of data.
- `Integer`- *limit*, number of data per page.

##### 返回

`Object`:

- `Array of Object` - *transactions*, transaction object:
  - `String` - *id*, transaction id, hash of the transaction.
  - `Integer` - *version*, version of transaction.
  - `Integer` - *size*, size of transaction.
  - `Integer` - *timestamp*, the unix timestamp for when the requst was responsed.
  - `Integer` - *time_range*, the unix timestamp for when the requst was responsed.
  - `Boolean` - *status_fail*, whether the state of the request has failed.
  - `String` - *mux_id*, the previous transaction mux id(source id of utxo).
  - `Integer` - *block_height*, block height.
  - `Integer` - *chain_status*, mainnet or orphan.
  - `Integer` - *coinbase*, the flag of coinbase transaction.
  - `Boolean` - *cross_chain*, the flag of cross chain transaction.
  - `Integer` - *trx_fee*, transaction fee.
  - `Integer` - *trx_amount*, the amount of transaction.
  - `Integer` - *confirmations*, the number comfirmed.
  - `Array of Object` - *inputs*, object of inputs for the transaction.
    - `String` - *txtype*, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - *asset_id*, asset id.
    - `String` - *asset_alias*, name of asset.
    - `Object` - *asset_definition*, definition of asset(json object).
    - `Integer` - *amount*, amount of asset.
    - `Object` - *issuance_program*, issuance program, it only exist when type is 'issue'.
    - `Object` - *control_program*, control program of account, it only exist when type is 'spend'.
    - `String` - *address*, address of account, it only exist when type is 'spend'.
    - `String` - *spent_output_id*, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - *account_id*, account id.
    - `String` - *account_alias*, name of account.
    - `Object` - *arbitrary*, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - *input_id*, hash of input action.
    - `Array of String` - *witness_arguments*, witness arguments.
    - `String` - *asset_name*, asset name.
    - `String` - *asset_decimals*, decimal of asset.
  - `Array of Object` - *outputs*, object of outputs for the transaction.
    - `String` - *txtype*, the type of output action, available option include: 'retire', 'control'.
    - `String` - *id*, outputid related to utxo.
    - `Integer` - *position*, position of outputs.
    - `String` - *asset_id*, asset id.
    - `String` - *asset_alias*, name of asset.
    - `Object` - *asset_definition*, definition of asset(json object).
    - `Integer` - *amount*, amount of asset.
    - `String` - *account_id*, account id.
    - `String` - *account_alias*, name of account.
    - `Object` - *control_program*, control program of account.
    - `String` - *address*, address of account.
    - `String` - *asset_name*, asset name.
    - `String` - *asset_decimals*, decimal of asset.
- `Array of Object` -*pagination*, pagination info.
  - `Integer`- *current*, current number of page.
  - `Integer`- *limit*, number of data per page.
  - `Integer`- *total*, the number of total.
  
##### 例子：

获取指定资产为61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3，指定地址为bm1qtmt60f9jamarpyvw2eplhmsuzrkfcmxp37s94fzvg9lypgnvsg7qt2q492的交易信息：

```json
// Request
curl -X GET "https://blockmeta.com/api/v3/asset/61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3/address/bm1qtmt60f9jamarpyvw2eplhmsuzrkfcmxp37s94fzvg9lypgnvsg7qt2q492/transaction?page=1&limit=1" -H "accept: application/json"

// Result
{
  "transactions": [
    {
      "id": "a6477e5df933a1dd465560f8fc09d92c0710bbe77cf20deb6d60f9226876fc51",
      "timestamp": 1565836134,
      "block_height": 286744,
      "trx_amount": 0,
      "trx_fee": 898000,
      "status_fail": false,
      "coinbase": false,
      "size": 607,
      "chain_status": "mainnet",
      "time_range": 304022,
      "index_id": 629592,
      "mux_id": "0911050d2d886ea7af8de92cf2ae9308505a689258468bcf834a0868d6532a95",
      "inputs": [
        {
          "txtype": "spend",
          "asset_id": "61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3",
          "amount": 47100000000,
          "control_program": "0014baa5455f5345fda568f4b68b7f634911eeedc8be",
          "address": "bm1qh2j52h6ngh762685k69h7c6fz8hwmj97n2xg9s",
          "spent_output_id": "4d5dd703d4e996821b02c48bbbbc69c77ba3cb5a3d5f83357fd3c90be06d8932",
          "input_id": "b9049a3149430f4c61766d9e6101c7440bfcf5e8ebb27167856f39a083627eab",
          "witness_arguments": [
            "ccb6ad019f67c2952479d0a2dbfed87652ca3f048cd6c2a466bdf82379ea46aee154a252a40fe9ad72166590c428ed6f80dd423a1a6dcb7e090b2090db173a06",
            "22e9714a49fc6715b5d93cd04131991e225d128ae662e94062e6adba8a034f1c"
          ],
          "asset_name": "DCA",
          "asset_definition": "{}",
          "asset_decimals": 8
        },
        {
          "txtype": "spend",
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 207047998,
          "control_program": "0014baa5455f5345fda568f4b68b7f634911eeedc8be",
          "address": "bm1qh2j52h6ngh762685k69h7c6fz8hwmj97n2xg9s",
          "spent_output_id": "2d93598fbcee63a6a9ce6da5e6d983439dd62450a654e353b44ea67d781081b6",
          "input_id": "ec291159f92e4f6172df00c2f3fa5f447550bd6f44bdc235600447aad643299e",
          "witness_arguments": [
            "e668782526b289577f85a0097ec9da5a7ae970266251d7f4f10fff1f4356ecb993e1179fb44437ad7391dc98a15b446bd9497f79f6ab82dc79c1875123337305",
            "22e9714a49fc6715b5d93cd04131991e225d128ae662e94062e6adba8a034f1c"
          ],
          "asset_name": "BTM",
          "asset_definition": "{}",
          "asset_decimals": 8
        }
      ],
      "outputs": [
        {
          "txtype": "control",
          "id": "d9e46efc69daf2790392589fdf76786ccb99303712e96798c7c21897ded551e4",
          "asset_id": "61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3",
          "amount": 1000000000,
          "control_program": "00205ed7a7a4b2eefa30918e5643fbee1c10ec9c6cc18fa05aa44c417e40a26c823c",
          "address": "bm1qtmt60f9jamarpyvw2eplhmsuzrkfcmxp37s94fzvg9lypgnvsg7qt2q492",
          "asset_name": "DCA",
          "asset_definition": "{}",
          "position": 0,
          "asset_decimals": 8
        },
        {
          "txtype": "control",
          "id": "48803d3cf86b3492a3314ee1bd26da235a8e104dd06955037e6bd4266512e091",
          "asset_id": "61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3",
          "amount": 46100000000,
          "control_program": "0014baa5455f5345fda568f4b68b7f634911eeedc8be",
          "address": "bm1qh2j52h6ngh762685k69h7c6fz8hwmj97n2xg9s",
          "asset_name": "DCA",
          "asset_definition": "{}",
          "position": 1,
          "asset_decimals": 8
        },
        {
          "txtype": "control",
          "id": "afa03cb52b29852ed0a41103eb849bc8f8630075351923c92178f649c77edfb4",
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 206149998,
          "control_program": "0014baa5455f5345fda568f4b68b7f634911eeedc8be",
          "address": "bm1qh2j52h6ngh762685k69h7c6fz8hwmj97n2xg9s",
          "asset_name": "BTM",
          "asset_definition": "{}",
          "position": 2,
          "asset_decimals": 8
        }
      ]
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 1,
    "total": 1
  }
}
```

---

#### `asset/{asset_id}/transaction`

获取资产相关的交易

##### 参数

**`必选`**:

- `String`- *asset_id*, asset

**可选**:

- `Integer`- *page*, page nunber of data.
- `Integer`- *limit*, number of data per page.

##### 返回

`Object`:

- `String` - *total_amount*, issue total amount.
- `Integer` - *decimals*, decimals.
- `String` - *description*, asset description.
- `String` - *name*, asset name.
- `String` - *symbol*, asset symbol.
- `String` - *asset_id*, uuid of asset.
- `String` - *logo*, asset's logo.
- `String` - *reissue*, reissue.
- `Integer` - *issuse_timestamp*, latest transaction amount.
- `Integer` - *quorum*, asset's sign number.
- `Bool` - *is_bap2*, Bap2 protocol.
- `String` - *turnover*, asset's turnover amount.
- `String` - *decode_program*, the result of decode issue program.
- `String` - *issuance_program*, program of issue.
- `Array of Object` - *transactions*, transaction object:
  - `String` - *id*, transaction id, hash of the transaction.
  - `Integer` - *version*, version of transaction.
  - `Integer` - *size*, size of transaction.
  - `Integer` - *timestamp*, the unix timestamp for when the requst was responsed.
  - `Integer` - *time_range*, the unix timestamp for when the requst was responsed.
  - `Boolean` - *status_fail*, whether the state of the request has failed.
  - `String` - *mux_id*, the previous transaction mux id(source id of utxo).
  - `Integer` - *block_height*, block height.
  - `Integer` - *chain_status*, mainnet or orphan.
  - `Integer` - *coinbase*, the flag of coinbase transaction.
  - `Boolean` - *cross_chain*, the flag of cross chain transaction.
  - `Integer` - *trx_fee*, transaction fee.
  - `Integer` - *trx_amount*, the amount of transaction.
  - `Integer` - *confirmations*, the number comfirmed.
  - `Array of Object` - *inputs*, object of inputs for the transaction.
    - `String` - *txtype*, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - *asset_id*, asset id.
    - `String` - *asset_alias*, name of asset.
    - `Object` - *asset_definition*, definition of asset(json object).
    - `Integer` - *amount*, amount of asset.
    - `Object` - *issuance_program*, issuance program, it only exist when type is 'issue'.
    - `Object` - *control_program*, control program of account, it only exist when type is 'spend'.
    - `String` - *address*, address of account, it only exist when type is 'spend'.
    - `String` - *spent_output_id*, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - *account_id*, account id.
    - `String` - *account_alias*, name of account.
    - `Object` - *arbitrary*, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - *input_id*, hash of input action.
    - `Array of String` - *witness_arguments*, witness arguments.
    - `String` - *asset_name*, asset name.
    - `String` - *asset_decimals*, decimal of asset.
  - `Array of Object` - *outputs*, object of outputs for the transaction.
    - `String` - *txtype*, the type of output action, available option include: 'retire', 'control'.
    - `String` - *id*, outputid related to utxo.
    - `Integer` - *position*, position of outputs.
    - `String` - *asset_id*, asset id.
    - `String` - *asset_alias*, name of asset.
    - `Object` - *asset_definition*, definition of asset(json object).
    - `Integer` - *amount*, amount of asset.
    - `String` - *account_id*, account id.
    - `String` - *account_alias*, name of account.
    - `Object` - *control_program*, control program of account.
    - `String` - *address*, address of account.
    - `String` - *asset_name*, asset name.
    - `String` - *asset_decimals*, decimal of asset.
- `Array of Object` -*pagination*, pagination info.
  - `Integer`- *current*, current number of page.
  - `Integer`- *limit*, number of data per page.
  - `Integer`- *total*, the number of total.

##### 例子：

获取地址为bm1q050g0urjwgvpr7eu3e3y8s0xrlntzd5kneapal的资产信息：

```json
// Request
curl -X GET "https://blockmeta.com/api/v3/asset/ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff/transaction?page=1&limit=1" -H "accept: application/json"

// Result
{
  "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "issuance_program": "",
  "decode_program": null,
  "description": "Bytom Official Issue",
  "total_amount": "210000000000000000",
  "turnover": "209999999845024364",
  "name": "BTM",
  "symbol": "BTM",
  "issue_timestamp": 1524499200,
  "reissue": "",
  "logo": "",
  "decimals": 8,
  "quorum": 0,
  "is_bap2": false,
  "address_count": 321307,
  "transactions": [
    {
      "id": "4904aec0b396b154c1dc270d4a3406f899626391c88c0d76fdea04730c34ab2f",
      "timestamp": 1575857208,
      "block_height": 353855,
      "trx_amount": 41250000000,
      "trx_fee": 0,
      "status_fail": false,
      "coinbase": true,
      "size": 82,
      "chain_status": "mainnet",
      "time_range": 0,
      "index_id": 757258,
      "mux_id": "280e8514a0d233f22a67dcdfec38f2909e0af33c6d48bfe08fd26332f38aab1b",
      "inputs": [
        {
          "txtype": "coinbase",
          "asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
          "amount": 0,
          "spent_output_id": "<nil>",
          "arbitrary": "00333533383535",
          "input_id": "35cd86fb0a570dbf4fbff914f102574eb96d135f2ca8b1e71978945d3011360e",
          "witness_arguments": null,
          "asset_name": "",
          "asset_definition": "{}",
          "asset_decimals": 0
        }
      ],
      "outputs": [
        {
          "txtype": "control",
          "id": "e48cdfb44cfad0e3c94b6dbfe4c4f5f99df55dddf58663ea51fb5f5dcdf6b70f",
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 41250000000,
          "control_program": "0014f8e99d6431aada3d535a015ba1074f6d6a58c4f3",
          "address": "bm1qlr5e6ep34tdr6566q9d6zp60d449338nwuhkdw",
          "asset_name": "BTM",
          "asset_definition": "{}",
          "position": 0,
          "asset_decimals": 8
        }
      ]
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 1,
    "total": 757258
  }
}
```

---

#### `asset/{asset_id}`

通过资产id或者地址获取账户资产，接收资产数量，发送资产数量和接收的交易。

##### 参数

`Object`:

- `String`- *asset_id*, asset id
  
##### 返回

`Object`:

- `String` - *total_amount*, issue total amount.
- `Integer` - *decimals*, decimals.
- `String` - *description*, asset description.
- `String` - *name*, asset name.
- `String` - *symbol*, asset symbol.
- `String` - *asset_id*, uuid of asset.
- `String` - *logo*, asset's logo.
- `String` - *reissue*, reissue.
- `Integer` - *issuse_timestamp*, latest transaction amount.
- `Integer` - *quorum*, asset's sign number.
- `Bool` - *is_bap2*, Bap2 protocol.
- `String` - *turnover*, asset's turnover amount.
- `String` - *decode_program*, the result of decode issue program.
- `String` - *issuance_program*, program of issue.

##### 例子

获取地址为61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3的资产信息：

```json
// Request
curl -X GET "https://blockmeta.com/api/v3/asset/61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3" -H "accept: application/json"

// Result
{
  "asset_id": "61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3",
  "issuance_program": "03423604cda069ae20ba090ba17eee6c73a8261b6672599d8af87b14ad51a6d741489c32d6b54340e45151ad",
  "decode_program": [
    "DATA_3 423604",
    "BLOCKHEIGHT ",
    "GREATERTHAN ",
    "VERIFY ",
    "TXSIGHASH ",
    "DATA_32 ba090ba17eee6c73a8261b6672599d8af87b14ad51a6d741489c32d6b54340e4",
    "1 01",
    "1 01",
    "CHECKMULTISIG "
  ],
  "description": "",
  "total_amount": "1000000000000000",
  "turnover": "1000000000000000",
  "name": "Decentralized  Community Asset",
  "symbol": "DCA",
  "issue_timestamp": 1564385961,
  "reissue": "false",
  "logo": "https://bycoin.oss-cn-shanghai.aliyuncs.com/bystack/asset/dca.png",
  "decimals": 8,
  "quorum": 1,
  "is_bap2": false
}
```

---

#### `list-asset`

在比原链上获取issued资产和BTM

##### 参数

**无**:

##### 返回

**Object**:

- `Array of Object` - *assets*, asset object:
  - `Object` - *issuance_program*, issuance program, it only exist when type is 'issue'.
  - `Array of String` - *decode_program*, decode arguments.
  - `Integer` - *total_amount*, total amount.
  - `String` - *name*, asset name.
  - `String` - *decimals*, decimal of asset.
  - `Object` - *description*, issue description'.
  - `String` - *symbol*, symbol.
  - `Integer` - *address_count*, address count.
  - `Integer` - *issue_timestamp*, issue timestamp.
  - `String` - *reissue*, is reissue.
  - `String` - *logo*, asset logo.
  - `Integer` - *quorum*, number of quorum.
  - `Integer` - *is_bap2*, protocal of bap2.
  - `String` - *asset_id*, asset id.
  - `String` - *turnover*, turnover of asset.
  - `Integer` - *confirmations*, the number comfirmed.

##### 例子

在比原链网络上获取issued资产：

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/assets?page=2&limit=1'

// Result
[
    {
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "issuance_program": "",
        "decode_program": null,
        "description": "Bytom Official Issue",
        "total_amount": "210000000000000000",
        "turnover": "209999999846677374",
        "name": "BTM",
        "symbol": "BTM",
        "issue_timestamp": 1524499200,
        "reissue": "",
        "logo": "",
        "decimals": 8,
        "quorum": 0,
        "is_bap2": false,
        "address_count": 320765
    },
    {
        "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
        "issuance_program": "ae2065b4a4dcb5308e7114ecb67c2c48b1c2ac9d03519bf21093cb2b0241a34603f15151ad",
        "decode_program": [
            "TXSIGHASH ",
            "DATA_32 65b4a4dcb5308e7114ecb67c2c48b1c2ac9d03519bf21093cb2b0241a34603f1",
            "1 01",
            "1 01",
            "CHECKMULTISIG "
        ],
        "description": "",
        "total_amount": "2100000000000000",
        "turnover": "2096248700000000",
        "name": "BQB",
        "symbol": "BQB",
        "issue_timestamp": 1546072313,
        "reissue": "",
        "logo": "",
        "decimals": 8,
        "quorum": 0,
        "is_bap2": false,
        "address_count": 38142
    },
    {
        "asset_id": "61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3",
        "issuance_program": "03423604cda069ae20ba090ba17eee6c73a8261b6672599d8af87b14ad51a6d741489c32d6b54340e45151ad",
        "decode_program": [
            "DATA_3 423604",
            "BLOCKHEIGHT ",
            "GREATERTHAN ",
            "VERIFY ",
            "TXSIGHASH ",
            "DATA_32 ba090ba17eee6c73a8261b6672599d8af87b14ad51a6d741489c32d6b54340e4",
            "1 01",
            "1 01",
            "CHECKMULTISIG "
        ],
        "description": "",
        "total_amount": "1000000000000000",
        "turnover": "1000000000000000",
        "name": "Decentralized  Community Asset",
        "symbol": "DCA",
        "issue_timestamp": 1564385961,
        "reissue": "false",
        "logo": "https://bycoin.oss-cn-shanghai.aliyuncs.com/bystack/asset/dca.png",
        "decimals": 8,
        "quorum": 1,
        "is_bap2": false,
        "address_count": 3574
    },
    {
        "asset_id": "375d9174951417fca654f4ec78660786bdfdb59795781b61dad9b61b1c174e1a",
        "issuance_program": "03a16d04cda069ae207c38e244b6e0274a9e70d4addd5b259841045198780208c28c31d4131ea81ea95151ad",
        "decode_program": [
            "DATA_3 a16d04",
            "BLOCKHEIGHT ",
            "GREATERTHAN ",
            "VERIFY ",
            "TXSIGHASH ",
            "DATA_32 7c38e244b6e0274a9e70d4addd5b259841045198780208c28c31d4131ea81ea9",
            "1 01",
            "1 01",
            "CHECKMULTISIG "
        ],
        "description": "",
        "total_amount": "40000000000000000",
        "turnover": "39999999821000000",
        "name": "Bing  home token",
        "symbol": "BHT",
        "issue_timestamp": 1566352092,
        "reissue": "false",
        "logo": "https://bycoin.oss-cn-shanghai.aliyuncs.com/bystack/asset/bht.png",
        "decimals": 8,
        "quorum": 1,
        "is_bap2": true,
        "address_count": 1211
    },
]
```

---

#### `asset/{asset_id}/addresses`

获取资产下地址的排名。

##### 参数

**可选**:

- `String`- *asset_id*, asset id.

**可选**:

- `Integer`- *page*, page nunber of data.
- `Integer`- *limit*, number of data per page.

##### 返回

**`可选`**:

- `Array of Object` - *addresses*, address object:
  - `String` - *address_id*, address.
  - `String` - *asset_id*, asset id.
  - `String` - *asset_name*, asset name.
  - `String` - *balance*, address balance.
  - `String` - *receive*, address reveived asset total amount.
  - `String` - *sent*, address sent asset total amount.
  - `Integer` - *join_timestamp*, first time create address.
  - `Integer` - *s_timestamp*, latest transaction timestamp.
  - `Integer` - *decimals*, the asset's decimals.
- `Array of Object` -*pagination*, pagination info.
  - `Integer`- *current*, current number of page.
  - `Integer`- *limit*, number of data per page.
  - `Integer`- *total*, the number of total.

##### 例子

获取资产id是61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3的所有地址资产排名：

```json
// Request
curl -X GET "https://blockmeta.com/api/v3/asset/61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3/addresses?page=1&limit=3" -H "accept: application/json"

// Result
{
  "addresses": [
    {
      "address_id": "bm1qlr6n07hcykllthlq3er78zsnexxsm432lm8hxd",
      "asset_id": "61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3",
      "asset_name": "DCA",
      "balance": "28001325000000",
      "receive": "28001325000000",
      "sent": "0",
      "join_timestamp": 1565321323,
      "s_timestamp": 1567600432,
      "decimals": 8
    },
    {
      "address_id": "bm1q0g7cpwfl5wu28dew0aav4qyrky03xmscukxarm",
      "asset_id": "61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3",
      "asset_name": "DCA",
      "balance": "27679475000000",
      "receive": "27679475000000",
      "sent": "0",
      "join_timestamp": 1568179361,
      "s_timestamp": 1568179361,
      "decimals": 8
    },
    {
      "address_id": "bm1qlqvsfdxfgetdrxs0y2vaekvyp7twta42fwgze9",
      "asset_id": "61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3",
      "asset_name": "DCA",
      "balance": "18461875000000",
      "receive": "18461875000000",
      "sent": "0",
      "join_timestamp": 1565321323,
      "s_timestamp": 1567685431,
      "decimals": 8
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 3,
    "total": 3574
  }
}
```

---

#### `daily/kline/{pair}`

获取btm日常k线的收盘价，主要的交易对有btm_btc, btm_eth, btm_usd, btm_cny.

##### 参数

**`可选`**:

- `String`- *pair*, btm exchange pair, Pair include btm_btc,btm_eth,btm_usd,btm_cny.

**可选**:

- `Integer`- *from*, start timestamp of statistic.
- `Integer`- *to*, end timestamp of statistic.

##### 返回

**`Object`**:

- `Array` :
  - `Array` - [price,date]

##### 例子

获取48小时btm_cny的价格:

```json
// Request
curl -X GET "https://blockmeta.com/api/v3/daily/kline/btm_btc" -H "accept: application/json"

// Result
[
  [
    0.00001485,
    1558656000
  ],
  [
    0.00001498,
    1558742400
  ],
  [
    0.00001464,
    1558828800
  ],
  [
    0.00001473,
    1558915200
  ],
  [
    0.00001453,
    1559001600
  ],
]
```

---

#### `daily/miner`

获取比原链网络挖矿每日统计数据

##### 参数

**可选**:

- `Integer`- *from*, start timestamp of statistic.
- `Integer`- *to*, end timestamp of statistic.

##### 返回

**`可选`**:

- `Array` :
  - `Array of Object`
    - `String` - *address*, address.
    - `Integer` - *fee*, transaction fee.
    - `Integer` - *mined_block_count*, mined block count.
    - `String` - *name*, miner name.
    - `Integer` - *start_timestamp*, mined start timestamp.
    - `Integer` - *end_timestamp*, mined end timestamp.
    - `Integer` - *profit*, profit.
    - `Float` - *percent*, percent of total miner:

##### 例子

获取比原链网络今日挖矿数据：

```json
// Request
curl -X GET "https://blockmeta.com/api/v3/daily/miner" -H "accept: application/json"

// Result
[
  [
    {
      "address": "bm1qcxg0w7c70tdd46t7dxn204mkyeyudcz063s49e",
      "start_timestamp": 1567180800,
      "end_timestamp": 1567267199,
      "fee": 14470000,
      "mined_block_count": 5,
      "percent": 0.008532423208191127,
      "profit": 206264470000,
      "name": "uupool"
    },
    {
      "address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
      "start_timestamp": 1567180800,
      "end_timestamp": 1567267199,
      "fee": 1188228797,
      "mined_block_count": 303,
      "percent": 0.5170648464163823,
      "profit": 12499938228797,
      "name": "antpool"
    },
    {
      "address": "bm1qlr5e6ep34tdr6566q9d6zp60d449338nwuhkdw",
      "start_timestamp": 1567180800,
      "end_timestamp": 1567267199,
      "fee": 354302200,
      "mined_block_count": 80,
      "percent": 0.13651877133105803,
      "profit": 3300354302200,
      "name": "matpool"
    },
    {
      "address": "bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
      "start_timestamp": 1567180800,
      "end_timestamp": 1567267199,
      "fee": 662329595,
      "mined_block_count": 174,
      "percent": 0.29692832764505117,
      "profit": 7178162329595,
      "name": "f2pool"
    },
    {
      "address": "bm1qrwhwspf4mva328xtaeed9fjmgj2u8mqywv887z",
      "start_timestamp": 1567180800,
      "end_timestamp": 1567267199,
      "fee": 114134004,
      "mined_block_count": 24,
      "percent": 0.040955631399317405,
      "profit": 990114134004,
      "name": "beepool"
    }
  ],
  [
    {
      "address": "bm1qcxg0w7c70tdd46t7dxn204mkyeyudcz063s49e",
      "start_timestamp": 1567267200,
      "end_timestamp": 1567353599,
      "fee": 21500000,
      "mined_block_count": 9,
      "percent": 0.014802631578947368,
      "profit": 371271500000,
      "name": "uupool"
    },
    {
      "address": "bm1qj7h0lvlvmel3m8nje8c96n7jzu038xgrfqp7a4",
      "start_timestamp": 1567267200,
      "end_timestamp": 1567353599,
      "fee": 0,
      "mined_block_count": 1,
      "percent": 0.001644736842105263,
      "profit": 41250000000,
      "name": "bm1qj7h0..."
    },
    {
      "address": "bm1qrwhwspf4mva328xtaeed9fjmgj2u8mqywv887z",
      "start_timestamp": 1567267200,
      "end_timestamp": 1567353599,
      "fee": 11286000,
      "mined_block_count": 18,
      "percent": 0.029605263157894735,
      "profit": 742511286000,
      "name": "beepool"
    },
    {
      "address": "bm1qlr5e6ep34tdr6566q9d6zp60d449338nwuhkdw",
      "start_timestamp": 1567267200,
      "end_timestamp": 1567353599,
      "fee": 223142600,
      "mined_block_count": 100,
      "percent": 0.16447368421052633,
      "profit": 4125223142600,
      "name": "matpool"
    },
    {
      "address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
      "start_timestamp": 1567267200,
      "end_timestamp": 1567353599,
      "fee": 817950197,
      "mined_block_count": 313,
      "percent": 0.5148026315789473,
      "profit": 12912067950197,
      "name": "antpool"
    },
    {
      "address": "bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
      "start_timestamp": 1567267200,
      "end_timestamp": 1567353599,
      "fee": 504517200,
      "mined_block_count": 167,
      "percent": 0.2746710526315789,
      "profit": 6889254517200,
      "name": "f2pool"
    }
  ],
]
```

---

#### `daily/total`

获取比原网络基本数据每日统计

##### 参数

**可选**:

- `Integer`- *from*, start timestamp of statistic.
- `Integer`- *to*, end timestamp of statistic.

##### 返回

**`Object`**:

- `Array` :
  - `Array of Object`
    - `Integer` - *start_timestamp*, mined start timestamp.
    - `Integer` - *end_timestamp*, mined end timestamp.
    - `Integer` - *confirmed_block_count*, confirmed block count.
    - `Integer` - *issue_count*, new issue count.
    - `Integer` - *mined_btm_amount*, mined BTM amount.
    - `Integer` - *new_address_count*, new address count.
    - `Integer` - *new_asset_count*, new asset count.
    - `Integer` - *orphan_block_count*, orphan block count.
    - `Integer` - *retire_count*, retire transaction count.
    - `Integer` - *transaction_amount*, transaction amount(BTMZ).
    - `Integer` - *transaction_count*, transaction count.
    - `Integer` - *transaction_fee*, transaction fee.
    - `Float` - *transaction_gas*, transaction gas.
    - `Integer` - *average_block_time*, average block time.

##### 例子

获取今日比原网络基本数据统计:

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/daily/total'

// Result
[
  {
    "start_timestamp": 1575734400,
    "end_timestamp": 1575820799,
    "confirmed_block_count": 585,
    "issue_count": 0,
    "mined_btm_amount": 24131250000000,
    "new_address_count": 341,
    "new_asset_count": 0,
    "orphan_block_count": 0,
    "retire_count": 146,
    "transaction_amount": 561460475827734,
    "transaction_count": 1004,
    "transaction_fee": 2130905820,
    "transaction_gas": 10654529,
    "average_block_time": 2.4615384615384617
  },
  {
    "start_timestamp": 1575475200,
    "end_timestamp": 1575561599,
    "confirmed_block_count": 561,
    "issue_count": 0,
    "mined_btm_amount": 23141250000000,
    "new_address_count": 378,
    "new_asset_count": 0,
    "orphan_block_count": 2,
    "retire_count": 150,
    "transaction_amount": 1340257197596519,
    "transaction_count": 988,
    "transaction_fee": 2263809599,
    "transaction_gas": 11319047,
    "average_block_time": 2.5668449197860963
  },
]
```

---

#### `stat/diffculty`

获取比原链网络挖矿难度统计

##### 参数

**可选**:

- `Integer`- *from*, start timestamp of statistic.
- `Integer`- *to*, end timestamp of statistic.

##### 返回

**`可选`**:

- `Array` :
  - `Array of Object`
    - `String` - *change_time*, detail time of diffculty change.
    - `Integer` - *diffculty*, diffculty.
    - `Float` - *change_rate*, change rate.

##### 例子

获取比原链网络区块从1565740800到1566259200的挖矿难度统计:

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/stat/difficulty'

// Result
[
   {
    "change_time": 1567181217,
    "difficulty": 66210054449,
    "change_rate": -0.05010851607842358
  },
  {
    "change_time": 1567243213,
    "difficulty": 66947989577,
    "change_rate": 0.011145363557560786
  },
]
```

---

#### `stat/miner/day`

获取比原链网络挖矿24小时统计

##### 参数

**无**:

##### 返回

**`Object`**:

- `Array` :
  - `Array of Object`
    - `String` - *address*, miner address.
    - `String` - *name*, miner name.
    - `Integer` - *mined_block_count*, mined block count.
    - `Integer` - *profit*, miner profit.
    - `Integer` - *fee*, transaction fee.
    - `Float` - *percent*, percent in total.
    - `Float` - *hash_rate*, hash rate.

##### 例子

获取比原链网络24小时挖矿统计:

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/stat/miner/day'

// Result
[
  {
    "address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
    "fee": 1010985276,
    "mined_block_count": 323,
    "percent": 0.5483870967741935,
    "profit": 13324760985276,
    "name": "antpool",
    "hash_rate": 260532752.63059652
  },
  {
    "address": "bm1qrwhwspf4mva328xtaeed9fjmgj2u8mqywv887z",
    "fee": 36645000,
    "mined_block_count": 20,
    "percent": 0.03395585738539898,
    "profit": 825036645000,
    "name": "beepool",
    "hash_rate": 16132058.986414647
  },
  {
    "address": "bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
    "fee": 609482197,
    "mined_block_count": 127,
    "percent": 0.21561969439728354,
    "profit": 5239359482197,
    "name": "f2pool",
    "hash_rate": 102438574.56373301
  },
  {
    "address": "bm1qcxg0w7c70tdd46t7dxn204mkyeyudcz063s49e",
    "fee": 2745000,
    "mined_block_count": 3,
    "percent": 0.0050933786078098476,
    "profit": 123752745000,
    "name": "uupool",
    "hash_rate": 2419808.8479621974
  },
  {
    "address": "bm1qlr5e6ep34tdr6566q9d6zp60d449338nwuhkdw",
    "fee": 2122787205,
    "mined_block_count": 116,
    "percent": 0.1969439728353141,
    "profit": 4787122787205,
    "name": "matpool",
    "hash_rate": 93565942.12120496
  }
]
```

---

#### `stat/hash-rate`

统计比原链网络hash率

##### 参数

**可选**:

- `Integer`- *from*, start timestamp of statistic.
- `Integer`- *to*, end timestamp of statistic.

##### 返回

**`可选`**:

- `Array` :
  - `Array` - [timetamp,hash_rate].

##### 例子

获取比原链区块时间从1566040800到1566259200的hash率：

```json
// Request
curl -X GET "https://blockmeta.com/api/v3/stat/hash-rate?from=1566040800&to=1566259200" -H "accept: application/json"

// Result
[
    [
    1566057600,
    397711751.0443408
  ],
  [
    1566144000,
    418943795.39227307
  ]
]
```

---

#### `stat/total`

总统计Bytom网络基本数据
##### 参数

**可选**:

- `Integer`- *from*, start timestamp of statistic.
- `Integer`- *to*, end timestamp of statistic.

##### 返回

**`Object`:**

- `Integer` - *confirmed_block_count*, confirmed block count.
- `Integer` - *orphan_block_count*, orphan block count.
- `Integer` - *transaction_count*, transaction count.
- `Integer` - *transaction_amount*, transaction amount(BTM).
- `Integer` - *transaction_fee*,  transaction fee.
- `Float` - *transaction_gas*, transaction gas.
- `Integer` - *new_asset_count*, new asset count.
- `Integer` - *mined_btm_amount*, mined btm amount.
- `Integer` - *issue_count*, issue count.
- `Integer` - *retire_count*, retire count.
- `Integer` - *start_timestamp*, mined start timestamp.
- `Integer` - *end_timestamp*, mined end timestamp.

##### 例子

总统计比原链网络区块高度从1566040800到1566259200的基本数据：

```json
// Request
curl -X GET "https://blockmeta.com/api/v3/stat/total?from=1566040800&to=1566259200" -H "accept: application/json"

// Result
{
  "start_timestamp": 1566057600,
  "end_timestamp": 1566230399,
  "confirmed_block_count": 1134,
  "issue_count": 10,
  "mined_btm_amount": 46777500000000,
  "new_address_count": 833,
  "new_asset_count": 6,
  "orphan_block_count": 0,
  "retire_count": 307,
  "transaction_amount": 2570728393393808,
  "transaction_count": 2056,
  "transaction_fee": 4619497086,
  "transaction_gas": 23097484
}
```

---

#### `kline/{pair}`

获取btm最近48小时的价格k线，交易对包括 btm_btc, btm_eth, btm_usd, btm_cny

##### 参数

**必选**

- `String`- *pair*, Pair include btm_btc,btm_eth,btm_usd,btm_cny.

##### 返回

**`Object`**:

- `Array` :
  - `Array of Object`
    - `Float64` - *close*, close price.
    - `Integer` - *date*, close date.

##### 例子

获取48小时btm_cny的价格:

```json
// Request
curl -X GET "https://blockmeta.com/api/v3/kline/btm_cny" -H "accept: application/json"

// Result
[
   {
    "close": 0.563004,
    "date": 1575687600
  }...
]

```

---

#### `nodes`

获取比原链所有节点所在的国家，包括cn, sg, jp, es, de, us, kr, ca, ru, uk

##### 参数

**可选:**

- `String`- *country*, country include cn,sg,jp,es,de,us,kr,ca,ru,uk.
- `Integer`- *page*, page number of data.
- `Integer`- *limit*, number of data per page.

##### Returns

**`Object`:**

- `Array of Object` - *nodes*, nodes object:
  - `String` - *address*, host:port.
  - `String` - *status*, network status.
  - `Integer` - *height*, block height.
  - `String` - *status_time*, datetime.
  - `Integer` - *rtt*, Round Trip Time.
  - `String` - *network*, mainnet testnet.
  - `String` - *version*, bytom version.
  - `Boolean` - *id_seed*, seed node or not.
  - `Object` - *coordinate*, coordinate:
    - `Float` - *longitude*, longitude
    - `Float` - *latitude*, latitude.
  - `Integer` - *country*, country name.
  - `Integer` - *symbol*, country symbol.
  - `String` - *name*, node name.
- `Array of Object` -*pagination*, pagination info.
  - `Integer`- *current*, current number of page.
  - `Integer`- *limit*, number of data per page.
  - `Integer`- *total*, the number of total.

##### 例子

获取节点信息：

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v3/nodes?page=1&limit=2'

// Result
{
  "pagination": {
    "current": 1,
    "limit": 2,
    "total": 49
  },
  "nodes": [
    {
      "address": "193.112.67.165:46657",
      "status": "active",
      "version": "1.0.8+56443ac4",
      "status_time": "2019-12-09T03:00:11Z",
      "network": "mainnet",
      "country": "China",
      "symbol": "cn,",
      "total": "",
      "volume": "",
      "name": "EONE",
      "height": 353871,
      "rtt": 81474984,
      "coordinate": {
        "longitude": 116.3883,
        "latitude": 39.9289
      },
      "is_seed": false
    },
    {
      "address": "118.25.5.22:46657",
      "status": "active",
      "version": "1.0.8+8db7fe73",
      "status_time": "2019-12-09T03:00:10Z",
      "network": "mainnet",
      "country": "China",
      "symbol": "cn,",
      "total": "",
      "volume": "",
      "name": "邵贤军",
      "height": 353871,
      "rtt": 98061529,
      "coordinate": {
        "longitude": 116.3883,
        "latitude": 39.9289
      },
      "is_seed": false
    }
  ]
}
```
