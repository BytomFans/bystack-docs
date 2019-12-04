---
id: docs_51
title: Bytom浏览器 API
sidebar_label: Bytom浏览器 API
---

**API 接口**

默认 JSON-RPC 接口:

| Client | URL |
| --- | --- |
| Base URL | [https://blockmeta.com/api/v2](https://blockmeta.com/api/v2) |


**一个完整的请求和返回示例如下：**

```
// curl -X GET url/method
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/blocks?page=1&limit=2'

// response
{
  "blocks": [
    {
      "hash": "b6cad6622267fd1f8f11d913866f1b0de63090456b034d9b85eab47b1c9400e3",
      "size": 416,
      "version": 1,
      "height": 289641,
      "previous_block_hash": "01b76975aaf9e8f35c2000c9ebc1799fc459122220769aebed1d2a45de4ea05b",
      "timestamp": 1566262005,
      "nonce": 979573656718097200,
      "bits": 2017612633063140000,
      "difficulty": "62228534746",
      "transaction_merkle_root": "eda3e458a9c38ed27e78f52443a4925c2baf43eb74c55f1fb2d5c0f8165ccc17",
      "transaction_status_hash": "c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc",
      "hash_rate": 1414284880,
      "miner_address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
      "transaction_count": 1,
      "chain_status": "mainnet",
      "cross_chain": false,
      "miner_name": "antpool"
    },
    {
      "hash": "01b76975aaf9e8f35c2000c9ebc1799fc459122220769aebed1d2a45de4ea05b",
      "size": 416,
      "version": 1,
      "height": 289640,
      "previous_block_hash": "40fe178f081a614dbc0099bb277c21b8ca7cf9491d9687ed10e7b7da3587fa5d",
      "timestamp": 1566261961,
      "nonce": 700133858850492800,
      "bits": 2017612633063140000,
      "difficulty": "62228534746",
      "transaction_merkle_root": "6688192c2b808fb1ddbd767e95c67581a38b7ee41ec5eaec26fda3c982d86c36",
      "transaction_status_hash": "c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc",
      "hash_rate": 1196702591,
      "miner_address": "bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
      "transaction_count": 1,
      "chain_status": "mainnet",
      "cross_chain": false,
      "miner_name": "f2pool"
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 2,
    "total": 289642
  }
}
```

**API 方法**

- 区块相关
  - [`blocks`](#57ec8edb)
  - [`block/{height}`](#bbb30935)
  - [`block/{hash}`](#bbb30935)
- 交易相关
  - [`transactions`](#bbb30935)
  - [`transaction/{transaction_id}`](#bbb30935)
  - [`unconfirmed-transactions`](#271c43b8)
  - [`unconfirmed-transactions/{transaction_id}`](#b53aeb55)
- 地址相关
  - [`address/{address}`](#ab14488c)
  - [`address/{adress}/asset/{asset_id}`](#ab14488c)
  - [`address-assets`](#a103284a)
  - [`address-assets-info`](#ddc0deaa)
  - [`latest-block`](#ed856e9d)
- 资产相关
  - [`asset/{asset_id}`](#3dd6f042)
  - [`asset-totalcoins/{asset_id}`](#ee6813b3)
  - [`assets`](#7ed2da57)
  - [`rank/{asset_id}`](#5dae61b7)
- 日统计数据相关
  - [`daily/kline/{pair}`](#d5478846)
  - [`daily/miner`](#9efe5284)
  - [`daily/total`](#3bbe49c3)
- 历史统计相关
  - [`stat/diffculty`](#e07d99ab)
  - [`stat/miner`](#d4eb4c36)
  - [`stat/hash-rate`](#aedcec63)
  - [`stat/total`](#087f8bc9)
  - [`stat/utxo`](#efab9195)
  - [`stat/address`](#a9b8e423)
- 其他
  - [`kline/{pair}`](#282a9f0d)
  - [`nodes`](#282a9f0d)
  - [`circulation-totalcoins`](#a581ca46)

---

##### `blocks`

获取当前最后一个区块

**参数**

可选:

- `Integer`- _page_, 页码.
- `Integer`- _limit_, 每页的数量.

**返回**

`Object`:

- `Array of Object` -blocks, block info list.
  - `String`- _hash_, hash of block.
  - `Integer`- _size_, size of block.
  - `Integer`- _version_, version of block.
  - `Integer`- _height_, height of block.
  - `String`- _previous_block_hash_, previous block hash.
  - `Integer`- _timestamp_, timestamp of block.
  - `Integer`- _nonce_, nonce value.
  - `Integer`- _bits_, bits of difficulty.
  - `String`- _difficulty_, difficulty value(String type).
  - `String`- _transaction_merkle_root_, merkle root of transaction.
  - `String`- _transaction_status_hash_, merkle root of transaction status.
  - `Integer`- _hash_rate_, the hash rate of block.
  - `Integer`- _miner_address_, the address of miner.
  - `Integer`- _transaction_count_, the count of transaction in the block.
  - `String`- _chain_status_, mainchain or orphan.
  - `String`- _cross_chain_, the flag of this block include cross chain transaction.
  - `String`- _miner_name_, the name of miner.
- `Array of Object` -pagination, pagination info.
  - `Integer`- _current_, current number of page.
  - `Integer`- _limit_, number of data per page.
  - `Integer`- _total_, the number of total blocks.

**例子：**

获取第一页的2个区块信息:

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/blocks?page=1&limit=2'
// Result
{
  "blocks": [
    {
      "hash": "b6cad6622267fd1f8f11d913866f1b0de63090456b034d9b85eab47b1c9400e3",
      "size": 416,
      "version": 1,
      "height": 289641,
      "previous_block_hash": "01b76975aaf9e8f35c2000c9ebc1799fc459122220769aebed1d2a45de4ea05b",
      "timestamp": 1566262005,
      "nonce": 979573656718097200,
      "bits": 2017612633063140000,
      "difficulty": "62228534746",
      "transaction_merkle_root": "eda3e458a9c38ed27e78f52443a4925c2baf43eb74c55f1fb2d5c0f8165ccc17",
      "transaction_status_hash": "c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc",
      "hash_rate": 1414284880,
      "miner_address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
      "transaction_count": 1,
      "chain_status": "mainnet",
      "cross_chain": false,
      "miner_name": "antpool"
    },
    {
      "hash": "01b76975aaf9e8f35c2000c9ebc1799fc459122220769aebed1d2a45de4ea05b",
      "size": 416,
      "version": 1,
      "height": 289640,
      "previous_block_hash": "40fe178f081a614dbc0099bb277c21b8ca7cf9491d9687ed10e7b7da3587fa5d",
      "timestamp": 1566261961,
      "nonce": 700133858850492800,
      "bits": 2017612633063140000,
      "difficulty": "62228534746",
      "transaction_merkle_root": "6688192c2b808fb1ddbd767e95c67581a38b7ee41ec5eaec26fda3c982d86c36",
      "transaction_status_hash": "c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc",
      "hash_rate": 1196702591,
      "miner_address": "bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
      "transaction_count": 1,
      "chain_status": "mainnet",
      "cross_chain": false,
      "miner_name": "f2pool"
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 2,
    "total": 289642
  }
}
```

---

##### `blocks/{height}`

根据区块高度获取区块信息

**参数**

可选:

- `String`- _height_, block height.

**返回**

可选:

- `String`- _hash_, hash of block.
- `Integer`- _size_, size of block.
- `Integer`- _version_, version of block.
- `Integer`- _height_, height of block.
- `String`- _previous_block_hash_, previous block hash.
- `Integer`- _timestamp_, timestamp of block.
- `Integer`- _nonce_, nonce value.
- `Integer`- _bits_, bits of difficulty.
- `String`- _difficulty_, difficulty value(String type).
- `String`- _transaction_merkle_root_, merkle root of transaction.
- `String`- _transaction_status_hash_, merkle root of transaction status.
- `Integer`- _hash_rate_, the hash rate of block.
- `Integer`- _miner_address_, the address of miner.
- `Integer`- _transaction_count_, the count of transaction in the block.
- `String`- _chain_status_, mainchain or orphan.
- `String`- _cross_chain_, the flag of this block include cross chain transaction.
- `String`- _miner_name_, the name of miner.
- `Array of Object` - transactions, transaction object:
  - `String` - id, transaction id, hash of the transaction.
  - `Integer` - version, version of transaction.
  - `Integer` - size, size of transaction.
  - `Integer` - time_range, the unix timestamp for when the requst was responsed.
  - `Boolean` - status_fail, whether the state of the request has failed.
  - `String` - mux_id, the previous transaction mux id(source id of utxo).
  - `Integer` - height, block height.
  - `Integer` - chain_status, mainnet or orphan.
  - `Integer` - coinbase, the flag of coinbase transaction.
  - `Boolean` - cross_chain, the flag of cross chain transaction.
  - `Integer` - fee, transaction fee.
  - `Integer` - transaction_amount, the amount of transaction.
  - `Integer` - confirmations, the number comfirmed.
  - `Array of Object` - inputs, object of inputs for the transaction.
    - `String` - type, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `Object` - issuance_program, issuance program, it only exist when type is 'issue'.
    - `Object` - control_program, control program of account, it only exist when type is 'spend'.
    - `String` - address, address of account, it only exist when type is 'spend'.
    - `String` - spent_output_id, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - arbitrary, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - input_id, hash of input action.
    - `Array of String` - witness_arguments, witness arguments.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.
  - `Array of Object` - outputs, object of outputs for the transaction.
    - `String` - type, the type of output action, available option include: 'retire', 'control'.
    - `String` - id, outputid related to utxo.
    - `Integer` - position, position of outputs.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - control_program, control program of account.
    - `String` - address, address of account.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.

**例子：**

获取区块高度为123的区块信息:

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/block/123'
// Result
{
  "hash": "531e1ebdd6f38924bfdd86331bc1286b1626da0d0b5b6f6da233a2d054d7f041",
  "size": 396,
  "version": 1,
  "height": 123,
  "previous_block_hash": "91a1fce360040bb9943f8374980032692ba967e1fa24e318b6a1fb3f648b0a58",
  "timestamp": 1524556238,
  "nonce": 8946685314549,
  "bits": 2161727821137910500,
  "difficulty": "15154807",
  "transaction_merkle_root": "4031f270c360fd98187db674b94dc0a9aea371e69701b8451ef9238666d4b022",
  "transaction_status_hash": "c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc",
  "hash_rate": 688854,
  "miner_address": "bm1qrp2fmpx675e5f5e9vwpscl8e08wpn4wqhrv0zt",
  "transaction_count": 1,
  "chain_status": "mainnet",
  "miner_name": null,
  "transactions": [
    {
      "id": "a3f370af6df14f07861ae0a0219f52b9db606dc04624fc190ebbee7142bf2177",
      "version": 1,
      "size": 76,
      "time_range": 0,
      "status_fail": false,
      "mux_id": "38296f40609dcf661e3706b444147daa934b669c8806a38c5a47341d070e7f64",
      "height": 123,
      "timestamp": 1524556238,
      "chain_status": "mainnet",
      "coinbase": 1,
      "fee": 0,
      "transaction_amount": 41250000000,
      "confirmations": 289546,
      "cross_chain": null,
      "details": [
        {
          "type": "coinbase",
          "asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
          "amount": 0,
          "arbitrary": "7b",
          "input_id": "8a2df61847e8b8a00922f0ac95a14d0f8f9a16a20bc17f11c890365bae819baa",
          "transaction_id": "a3f370af6df14f07861ae0a0219f52b9db606dc04624fc190ebbee7142bf2177",
          "status_fail": false,
          "io": 0
        },
        {
          "type": "control",
          "id": "8dfa1bc333aa4c47332b3b941641e87341d3d79b102095b4a26d88c7b3ed8087",
          "position": 0,
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 41250000000,
          "control_program": "001418549d84daf53344d32563830c7cf979dc19d5c0",
          "address": "bm1qrp2fmpx675e5f5e9vwpscl8e08wpn4wqhrv0zt",
          "transaction_id": "a3f370af6df14f07861ae0a0219f52b9db606dc04624fc190ebbee7142bf2177",
          "status_fail": false,
          "io": 1,
          "decode_program": [
            "DUP",
            "HASH160",
            "DATA_20 18549d84daf53344d32563830c7cf979dc19d5c0",
            "EQUALVERIFY",
            "TXSIGHASH",
            "SWAP",
            "CHECKSIG"
          ],
          "asset_name": "BTM",
          "asset_decimals": "8"
        }
      ]
    }
  ]
}
```

---

##### `blocks/{hash}`

通过区块hash获取区块信息

**参数**

可选:

- `String`- _hash_, block hash.

**返回**

`Object`:

- `String`- _hash_, hash of block.
- `Integer`- _size_, size of block.
- `Integer`- _version_, version of block.
- `Integer`- _height_, height of block.
- `String`- _previous_block_hash_, previous block hash.
- `Integer`- _timestamp_, timestamp of block.
- `Integer`- _nonce_, nonce value.
- `Integer`- _bits_, bits of difficulty.
- `String`- _difficulty_, difficulty value(String type).
- `String`- _transaction_merkle_root_, merkle root of transaction.
- `String`- _transaction_status_hash_, merkle root of transaction status.
- `Integer`- _hash_rate_, the hash rate of block.
- `Integer`- _miner_address_, the address of miner.
- `Integer`- _transaction_count_, the count of transaction in the block.
- `String`- _chain_status_, mainchain or orphan.
- `String`- _cross_chain_, the flag of this block include cross chain transaction.
- `String`- _miner_name_, the name of miner.
- `Array of Object` - transactions, transaction object:
  - `String` - id, transaction id, hash of the transaction.
  - `Integer` - version, version of transaction.
  - `Integer` - size, size of transaction.
  - `Integer` - time_range, the unix timestamp for when the requst was responsed.
  - `Boolean` - status_fail, whether the state of the request has failed.
  - `String` - mux_id, the previous transaction mux id(source id of utxo).
  - `Integer` - height, block height.
  - `Integer` - chain_status, mainnet or orphan.
  - `Integer` - coinbase, the flag of coinbase transaction.
  - `Boolean` - cross_chain, the flag of cross chain transaction.
  - `Integer` - fee, transaction fee.
  - `Integer` - transaction_amount, the amount of transaction.
  - `Integer` - confirmations, the number comfirmed.
  - `Array of Object` - inputs, object of inputs for the transaction.
    - `String` - type, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `Object` - issuance_program, issuance program, it only exist when type is 'issue'.
    - `Object` - control_program, control program of account, it only exist when type is 'spend'.
    - `String` - address, address of account, it only exist when type is 'spend'.
    - `String` - spent_output_id, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - arbitrary, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - input_id, hash of input action.
    - `Array of String` - witness_arguments, witness arguments.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.
  - `Array of Object` - outputs, object of outputs for the transaction.
    - `String` - type, the type of output action, available option include: 'retire', 'control'.
    - `String` - id, outputid related to utxo.
    - `Integer` - position, position of outputs.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - control_program, control program of account.
    - `String` - address, address of account.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.

**例子：**

获取区块hash是 531e1ebdd6f38924bfdd86331bc1286b1626da0d0b5b6f6da233a2d054d7f041的区块信息:

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/block/531e1ebdd6f38924bfdd86331bc1286b1626da0d0b5b6f6da233a2d054d7f041'

// Result
{
  "hash": "531e1ebdd6f38924bfdd86331bc1286b1626da0d0b5b6f6da233a2d054d7f041",
  "size": 396,
  "version": 1,
  "height": 123,
  "previous_block_hash": "91a1fce360040bb9943f8374980032692ba967e1fa24e318b6a1fb3f648b0a58",
  "timestamp": 1524556238,
  "nonce": 8946685314549,
  "bits": 2161727821137910500,
  "difficulty": "15154807",
  "transaction_merkle_root": "4031f270c360fd98187db674b94dc0a9aea371e69701b8451ef9238666d4b022",
  "transaction_status_hash": "c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc",
  "hash_rate": 688854,
  "miner_address": "bm1qrp2fmpx675e5f5e9vwpscl8e08wpn4wqhrv0zt",
  "transaction_count": 1,
  "chain_status": "mainnet",
  "miner_name": null,
  "transactions": [
    {
      "id": "a3f370af6df14f07861ae0a0219f52b9db606dc04624fc190ebbee7142bf2177",
      "version": 1,
      "size": 76,
      "time_range": 0,
      "status_fail": false,
      "mux_id": "38296f40609dcf661e3706b444147daa934b669c8806a38c5a47341d070e7f64",
      "height": 123,
      "timestamp": 1524556238,
      "chain_status": "mainnet",
      "coinbase": 1,
      "fee": 0,
      "transaction_amount": 41250000000,
      "confirmations": 289546,
      "cross_chain": null,
      "details": [
        {
          "type": "coinbase",
          "asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
          "amount": 0,
          "arbitrary": "7b",
          "input_id": "8a2df61847e8b8a00922f0ac95a14d0f8f9a16a20bc17f11c890365bae819baa",
          "transaction_id": "a3f370af6df14f07861ae0a0219f52b9db606dc04624fc190ebbee7142bf2177",
          "status_fail": false,
          "io": 0
        },
        {
          "type": "control",
          "id": "8dfa1bc333aa4c47332b3b941641e87341d3d79b102095b4a26d88c7b3ed8087",
          "position": 0,
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 41250000000,
          "control_program": "001418549d84daf53344d32563830c7cf979dc19d5c0",
          "address": "bm1qrp2fmpx675e5f5e9vwpscl8e08wpn4wqhrv0zt",
          "transaction_id": "a3f370af6df14f07861ae0a0219f52b9db606dc04624fc190ebbee7142bf2177",
          "status_fail": false,
          "io": 1,
          "decode_program": [
            "DUP",
            "HASH160",
            "DATA_20 18549d84daf53344d32563830c7cf979dc19d5c0",
            "EQUALVERIFY",
            "TXSIGHASH",
            "SWAP",
            "CHECKSIG"
          ],
          "asset_name": "BTM",
          "asset_decimals": "8"
        }
      ]
    }
  ]
}
```

---

##### `transactions`

获取当前区块的最后一笔交易

**参数**

可选:

- `Integer`- _page_, page nunber of data.
- `Integer`- _limit_, number of data per page.

**返回：**

对象:

- `Array of Object` - transactions, transaction object:
  - `String` - id, transaction id, hash of the transaction.
  - `Integer` - version, version of transaction.
  - `Integer` - size, size of transaction.
  - `Integer` - time_range, the unix timestamp for when the requst was responsed.
  - `Boolean` - status_fail, whether the state of the request has failed.
  - `String` - mux_id, the previous transaction mux id(source id of utxo).
  - `Integer` - height, block height.
  - `Integer` - chain_status, mainnet or orphan.
  - `Integer` - coinbase, the flag of coinbase transaction.
  - `Boolean` - cross_chain, the flag of cross chain transaction.
  - `Integer` - fee, transaction fee.
  - `Integer` - transaction_amount, the amount of transaction.
  - `Integer` - confirmations, the number comfirmed.
  - `Array of Object` - inputs, object of inputs for the transaction.
    - `String` - type, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `Object` - issuance_program, issuance program, it only exist when type is 'issue'.
    - `Object` - control_program, control program of account, it only exist when type is 'spend'.
    - `String` - address, address of account, it only exist when type is 'spend'.
    - `String` - spent_output_id, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - arbitrary, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - input_id, hash of input action.
    - `Array of String` - witness_arguments, witness arguments.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.
  - `Array of Object` - outputs, object of outputs for the transaction.
    - `String` - type, the type of output action, available option include: 'retire', 'control'.
    - `String` - id, outputid related to utxo.
    - `Integer` - position, position of outputs.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - control_program, control program of account.
    - `String` - address, address of account.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.
- `Array of Object` -pagination, pagination info.
  - `Integer`- _current_, current number of page.
  - `Integer`- _limit_, number of data per page.
  - `Integer`- _total_, the number of total blocks.

**例子：**

获取当前第一页的一笔交易信息:

```json
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/transactions?page=1&limit=1'
// Result
{
  "transactions": [
    {
      "id": "50180503aedb6df8ea5ccd1590c5f6f6e0025911d2a0c929701d358bf28ce59a",
      "version": 1,
      "size": 82,
      "time_range": 0,
      "status_fail": false,
      "mux_id": "89aa0833dcacddc43e72626dbd0068e1abd295cd841706b00a3a0da4eeda6b0f",
      "height": 289677,
      "timestamp": 1566267155,
      "chain_status": "mainnet",
      "coinbase": 1,
      "cross_chain": 0,
      "fee": 0,
      "transaction_amount": 41270898000,
      "confirmations": 1,
      "details": [
        {
          "type": "coinbase",
          "asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
          "amount": 0,
          "arbitrary": "00323839363737",
          "input_id": "24ce1989f2002dc3445ea450476749e770f6484b2705c7b1f2e30c66aa75589c",
          "transaction_id": "50180503aedb6df8ea5ccd1590c5f6f6e0025911d2a0c929701d358bf28ce59a",
          "status_fail": false,
          "io": 0
        },
        {
          "type": "control",
          "id": "a1eb80699f1a08b67f16e74a0f87276c28b84d718b91db3ade5ba2c8a5dcff63",
          "position": 0,
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 41270898000,
          "control_program": "0014c190f77b1e7adadae97e69a6a7d7762649c6e04f",
          "address": "bm1qcxg0w7c70tdd46t7dxn204mkyeyudcz063s49e",
          "transaction_id": "50180503aedb6df8ea5ccd1590c5f6f6e0025911d2a0c929701d358bf28ce59a",
          "status_fail": false,
          "io": 1,
          "decode_program": [
            "DUP",
            "HASH160",
            "DATA_20 c190f77b1e7adadae97e69a6a7d7762649c6e04f",
            "EQUALVERIFY",
            "TXSIGHASH",
            "SWAP",
            "CHECKSIG"
          ],
          "asset_name": "BTM",
          "asset_decimals": "8"
        }
      ]
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 1,
    "total": 635284
  }
}
```

---

##### `transactions/{transaction_id}`

根据交易id获取交易详情

**参数**

对象:

- `String`- _transaction_id_, tranasction id.

**返回**

对象:

- `Array of Object` - transactions, transaction object:
  - `String` - id, transaction id, hash of the transaction.
  - `Integer` - version, version of transaction.
  - `Integer` - size, size of transaction.
  - `Integer` - time_range, the unix timestamp for when the requst was responsed.
  - `Boolean` - status_fail, whether the state of the request has failed.
  - `String` - mux_id, the previous transaction mux id(source id of utxo).
  - `Integer` - height, block height.
  - `Integer` - chain_status, mainnet or orphan.
  - `Integer` - coinbase, the flag of coinbase transaction.
  - `Boolean` - cross_chain, the flag of cross chain transaction.
  - `Integer` - fee, transaction fee.
  - `Integer` - transaction_amount, the amount of transaction.
  - `Integer` - confirmations, the number comfirmed.
  - `Array of Object` - inputs, object of inputs for the transaction.
    - `String` - type, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `Object` - issuance_program, issuance program, it only exist when type is 'issue'.
    - `Object` - control_program, control program of account, it only exist when type is 'spend'.
    - `String` - address, address of account, it only exist when type is 'spend'.
    - `String` - spent_output_id, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - arbitrary, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - input_id, hash of input action.
    - `Array of String` - witness_arguments, witness arguments.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.
  - `Array of Object` - outputs, object of outputs for the transaction.
    - `String` - type, the type of output action, available option include: 'retire', 'control'.
    - `String` - id, outputid related to utxo.
    - `Integer` - position, position of outputs.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - control_program, control program of account.
    - `String` - address, address of account.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.
- `Array of Object` -pagination, pagination info.
  - `Integer`- _current_, current number of page.
  - `Integer`- _limit_, number of data per page.
  - `Integer`- _total_, the number of total blocks.

**例子：**

获取交易id是b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7的交易详情：

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/transaction/b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7'

// Result
{
  "id": "b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7",
  "version": 1,
  "size": 531,
  "time_range": 306954,
  "status_fail": false,
  "mux_id": "c2f96eac6c97fb0729024d3c050010bf981903d2a9ee54edb85508fef6a801b2",
  "height": 289676,
  "timestamp": 1566267013,
  "chain_status": "mainnet",
  "coinbase": 0,
  "cross_chain": 0,
  "fee": 898000,
  "transaction_amount": 83700000000,
  "confirmations": 5,
  "details": [
    {
      "type": "spend",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 83700000000,
      "control_program": "0014a49a20e654f4813ce9154a4624a13743c56b4797",
      "address": "bm1q5jdzpej57jqne6g4ffrzfgfhg0zkk3uh7hgnuc",
      "spent_output_id": "7149736715cfacdd9c108d0cc052e5bf88bef900afe48efcf0aee7a7209e7bee",
      "input_id": "2de66fe9f3e292c633121792aef284f7074a17f798f290d7ddee30da9ee4e739",
      "witness_arguments": [
        "dd38366d1f6cca4a6d51fc6d4902303aabf1f0ce3512be20c31d1cc5f0dff60971adaab2262a60feb90315fd9a3afb9811c2324ab33512868fcd3fa3a1b8da0d",
        "ef6220d45792bf9057a56ce5ffc21ee55c96c1cba7267c0edff14f227f3fe1f7"
      ],
      "transaction_id": "b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7",
      "status_fail": false,
      "io": 0,
      "decode_program": [
        "DUP",
        "HASH160",
        "DATA_20 a49a20e654f4813ce9154a4624a13743c56b4797",
        "EQUALVERIFY",
        "TXSIGHASH",
        "SWAP",
        "CHECKSIG"
      ],
      "asset_name": "BTM",
      "asset_decimals": "8"
    },
    {
      "type": "spend",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 61168913,
      "control_program": "0014a49a20e654f4813ce9154a4624a13743c56b4797",
      "address": "bm1q5jdzpej57jqne6g4ffrzfgfhg0zkk3uh7hgnuc",
      "spent_output_id": "b098d65d1260913ff21dceae7806af02d66b50141b779bf085c6130486cce4ef",
      "input_id": "169442accea85946040d19df2cb25c939b36dafafb8acbf33a817cb643f73b36",
      "witness_arguments": [
        "8c53335eff0afee679a888846cf3f45fb2ebfb2c3b1f2b116ea74e14b6b83bbb4d2f0a4a72bb2661b787ebf1247433bff56ebc5fcc70b7aaeb4a25df7f9ffd0d",
        "ef6220d45792bf9057a56ce5ffc21ee55c96c1cba7267c0edff14f227f3fe1f7"
      ],
      "transaction_id": "b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7",
      "status_fail": false,
      "io": 0,
      "decode_program": [
        "DUP",
        "HASH160",
        "DATA_20 a49a20e654f4813ce9154a4624a13743c56b4797",
        "EQUALVERIFY",
        "TXSIGHASH",
        "SWAP",
        "CHECKSIG"
      ],
      "asset_name": "BTM",
      "asset_decimals": "8"
    },
    {
      "type": "control",
      "id": "09a345b86d94e40750b444ddc754c46ceda1c482d4211f706c2da818a2900d77",
      "position": 0,
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 83700000000,
      "control_program": "001474e7a09a2729d93c30010b0be906588ca9f7de2e",
      "address": "bm1qwnn6px3898vncvqppv97jpjc3j5l0h3wpzadya",
      "transaction_id": "b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7",
      "status_fail": false,
      "io": 1,
      "decode_program": [
        "DUP",
        "HASH160",
        "DATA_20 74e7a09a2729d93c30010b0be906588ca9f7de2e",
        "EQUALVERIFY",
        "TXSIGHASH",
        "SWAP",
        "CHECKSIG"
      ],
      "asset_name": "BTM",
      "asset_decimals": "8"
    },
    {
      "type": "control",
      "id": "a5baa6606c2550a390cc541c2effcf9dcf81695bc6069b53bb2149be8f91077a",
      "position": 1,
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 60270913,
      "control_program": "0014a49a20e654f4813ce9154a4624a13743c56b4797",
      "address": "bm1q5jdzpej57jqne6g4ffrzfgfhg0zkk3uh7hgnuc",
      "transaction_id": "b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7",
      "status_fail": false,
      "io": 1,
      "decode_program": [
        "DUP",
        "HASH160",
        "DATA_20 a49a20e654f4813ce9154a4624a13743c56b4797",
        "EQUALVERIFY",
        "TXSIGHASH",
        "SWAP",
        "CHECKSIG"
      ],
      "asset_name": "BTM",
      "asset_decimals": "8"
    }
  ]
}
```

---

##### `unconfirmed-transactions`

从Bytom节点交易池中获取未确认的交易

**参数**

无

**返回**

对象:

- `Array of Object` - transactions, transaction object:
  - `String` - id, transaction id, hash of the transaction.
  - `Integer` - version, version of transaction.
  - `Integer` - size, size of transaction.
  - `Integer` - time_range, the unix timestamp for when the requst was responsed.
  - `Boolean` - status_fail, whether the state of the request has failed.
  - `String` - mux_id, the previous transaction mux id(source id of utxo).
  - `Integer` - fee, transaction fee.
  - `Integer` - transaction_amount, the amount of transaction.

**例子：**

获取未确认的交易

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/unconfirmed-transactions'


// Result
[
  {
    "id": "721429208d16a7ee9e9363557ed5972c99be43435b36e1138cb7819ebe474a76",
    "version": 1,
    "size": 529,
    "time_range": 306964,
    "status_fail": false,
    "mux_id": "0fc996022dbbbdb78e81eb66c5c605800f13194cbce25acbc69c6d5e2e172e33",
    "fee": 898000,
    "transaction_amount": 16600000000
  },
  {
    "id": "34be489aac90c7e6431556028c13f4a5ac611061e138d25903452c444421b7e6",
    "version": 1,
    "size": 333,
    "time_range": 0,
    "status_fail": false,
    "mux_id": "0a7abb83232779b50d6cf95f11733e0f286dcebfbc41d37fa446853b1f31eb4f",
    "fee": 1400000,
    "transaction_amount": 108335100000
  },
  {
    "id": "247c68920b2a6725de32a6a1de2dc1cf0bcab898840b83b20f5bcd6578244158",
    "version": 1,
    "size": 3717,
    "time_range": 0,
    "status_fail": false,
    "mux_id": "dcb87baa014d42f0454e7410705badf6486b1041d23a1707af655d2c6d163c7c",
    "fee": 6152400,
    "transaction_amount": 742493847600
  }
]
```

---

##### `unconfirmed-transaction/{transaction_id}`

根据交易id获取bytom节点交易池中未确认的交易.

**参数**

可选:

- `String`- _transaction_id_, tranasction id.

**返回：**

可选:

- `String` - id, transaction id, hash of the transaction.
- `Integer` - version, version of transaction.
- `Integer` - size, size of transaction.
- `Integer` - time_range, the unix timestamp for when the requst was responsed.
- `Boolean` - status_fail, whether the state of the request has failed.
- `String` - mux_id, the previous transaction mux id(source id of utxo).
- `Integer` - chain_status, mainnet or orphan.
- `Integer` - fee, transaction fee.
- `Array of Object` - inputs, object of inputs for the transaction.
  - `String` - type, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
  - `String` - asset_id, asset id.
  - `String` - asset_alias, name of asset.
  - `Object` - asset_definition, definition of asset(json object).
  - `Integer` - amount, amount of asset.
  - `Object` - issuance_program, issuance program, it only exist when type is 'issue'.
  - `Object` - control_program, control program of account, it only exist when type is 'spend'.
  - `String` - address, address of account, it only exist when type is 'spend'.
  - `String` - spent_output_id, the front of outputID to be spent in this input, it only exist when type is 'spend'.
  - `String` - account_id, account id.
  - `String` - account_alias, name of account.
  - `Object` - arbitrary, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
  - `String` - input_id, hash of input action.
  - `Array of String` - witness_arguments, witness arguments.
  - `String` - asset_name, asset name.
  - `String` - asset_decimals, decimal of asset.
- `Array of Object` - outputs, object of outputs for the transaction.
  - `String` - type, the type of output action, available option include: 'retire', 'control'.
  - `String` - id, outputid related to utxo.
  - `Integer` - position, position of outputs.
  - `String` - asset_id, asset id.
  - `String` - asset_alias, name of asset.
  - `Object` - asset_definition, definition of asset(json object).
  - `Integer` - amount, amount of asset.
  - `String` - account_id, account id.
  - `String` - account_alias, name of account.
  - `Object` - control_program, control program of account.
  - `String` - address, address of account.
  - `String` - asset_name, asset name.
  - `String` - asset_decimals, decimal of asset.

**例子：**

获取交易ID为bfec8511aa9771a8ae4eec6e3eb3113446998e850ecf03bf8fc88190bc7cdb62的未确认交易

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/unconfirmed-transaction/bfec8511aa9771a8ae4eec6e3eb3113446998e850ecf03bf8fc88190bc7cdb62'

// Result
{
  "id": "bfec8511aa9771a8ae4eec6e3eb3113446998e850ecf03bf8fc88190bc7cdb62",
  "version": 1,
  "size": 3782,
  "time_range": 0,
  "status_fail": false,
  "mux_id": "c3ad50dd24aab5188d2faf8e8c176bc554ef53a5263e9b1e51a5553d1dd9e764",
  "fee": 6165400,
  "details": [
    {
      "type": "spend",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 41250000000,
      "control_program": "00148916ad528556048b97437f05a8afa7482afe0b94",
      "address": "bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
      "spent_output_id": "d7a2aea381f393b5246705aef8dddaa7ff2b517e9fdb32c4c20ee6c97426653c",
      "input_id": "97a58e42c7076f23561777ba44540add8537af97a7053c6b1bc21860726ba3c5",
      "witness_arguments": [
        "06eede387d51bbce6ad207bad2f4e93d7c570623f3080a6383eea94460642d185e873963d8993b98661a6d821bbeba618a6a7ef3683e1fbfb0d85ad848806201",
        "997c6d3617aa3743b7530c522667bb2aa871f58d0e37ccf3ad538649f29f4dee"
      ],
      "io": 0,
      "decode_program": [
        "DUP",
        "HASH160",
        "DATA_20 8916ad528556048b97437f05a8afa7482afe0b94",
        "EQUALVERIFY",
        "TXSIGHASH",
        "SWAP",
        "CHECKSIG"
      ],
      "asset_name": "BTM",
      "asset_decimals": "8"
    },
    {
      "type": "spend",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 41250000000,
      "control_program": "00148916ad528556048b97437f05a8afa7482afe0b94",
      "address": "bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
      "spent_output_id": "d58596966ba56a44c3476ddfa53717aa6070203b704845cef46356926393f0dd",
      "input_id": "36ef095ff825432a9f0f9826bb6aba3f888f34504a7d8b33487acec5367f08cb",
      "witness_arguments": [
        "5c4a6ef7ca037da5632a3a98e3c2c7bf8b51c1e8a319fbbfdd16d92ff2709b397dc2e8bf0f8c341f6d393675f7c1f6e93f8d12c719ae69533cc19ad3c9853306",
        "997c6d3617aa3743b7530c522667bb2aa871f58d0e37ccf3ad538649f29f4dee"
      ],
      "io": 0,
      "decode_program": [
        "DUP",
        "HASH160",
        "DATA_20 8916ad528556048b97437f05a8afa7482afe0b94",
        "EQUALVERIFY",
        "TXSIGHASH",
        "SWAP",
        "CHECKSIG"
      ],
      "asset_name": "BTM",
      "asset_decimals": "8"
    },
    {
      "type": "control",
      "id": "478ab8c3336326077ec1f1fe05ceea01756e30a2810cefab149da375606bfbfb",
      "position": 1,
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 261574122064,
      "control_program": "00149d1e0f7ef3cee1ba1499f0e89bc845c715cecf62",
      "address": "bm1qn50q7lhnemsm59ye7r5fhjz9cu2uanmz38qa5t",
      "io": 1,
      "decode_program": [
        "DUP",
        "HASH160",
        "DATA_20 9d1e0f7ef3cee1ba1499f0e89bc845c715cecf62",
        "EQUALVERIFY",
        "TXSIGHASH",
        "SWAP",
        "CHECKSIG"
      ],
      "asset_name": "BTM",
      "asset_decimals": "8"
    },
    {
      "type": "control",
      "id": "6bbb86989a20db1856cabf157314c34fe860e94aeefbc9c54654ecc5206135d2",
      "position": 2,
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 158752756500,
      "control_program": "00143f02e465970771270d808770d95c3218162865eb",
      "address": "bm1q8upwgevhqacjwrvqsacdjhpjrqtzse0t5y0mla",
      "io": 1,
      "decode_program": [
        "DUP",
        "HASH160",
        "DATA_20 3f02e465970771270d808770d95c3218162865eb",
        "EQUALVERIFY",
        "TXSIGHASH",
        "SWAP",
        "CHECKSIG"
      ],
      "asset_name": "BTM",
      "asset_decimals": "8"
    }
  ]
}
```

---

##### `address/{address}`   or  `address/{address}/asset/{asset_id}`

根据地址获取账户资产，接收资产数量，发送资产数量和最近交易信息。

**参数**

对象:

- `String`- _address_, address
- `String`- _asset_id_, asset id.

可选:

- `Integer`- _page_, page nunber of data.
- `Integer`- _limit_, number of data per page.

**返回：**

`Object`:

- `String` - address, address.
- `String` - asset_id, asset id.
- `String` - asset_name, asset name.
- `Integer` - balance, address balance.
- `Integer` - receive, address reveived asset total amount.
- `Integer` - sent, address sent asset total amount.
- `Integer` - join_timestamp, first time create address.
- `String` - last_transaction_id, latest transaction id.
- `Integer` - last_transaction_amount, latest transaction amount.
- `Integer` - last_transaction_timestamp, latest transaction timestamp.
- `Integer` - transaction_count, the address's transaction count.
- `Array of Object` - transactions, transaction object:
  - `String` - id, transaction id, hash of the transaction.
  - `Integer` - version, version of transaction.
  - `Integer` - size, size of transaction.
  - `Integer` - time_range, the unix timestamp for when the requst was responsed.
  - `Boolean` - status_fail, whether the state of the request has failed.
  - `String` - mux_id, the previous transaction mux id(source id of utxo).
  - `Integer` - height, block height.
  - `Integer` - chain_status, mainnet or orphan.
  - `Integer` - coinbase, the flag of coinbase transaction.
  - `Boolean` - cross_chain, the flag of cross chain transaction.
  - `Integer` - fee, transaction fee.
  - `Integer` - transaction_amount, the amount of transaction.
  - `Integer` - confirmations, the number comfirmed.
  - `Array of Object` - inputs, object of inputs for the transaction.
    - `String` - type, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `Object` - issuance_program, issuance program, it only exist when type is 'issue'.
    - `Object` - control_program, control program of account, it only exist when type is 'spend'.
    - `String` - address, address of account, it only exist when type is 'spend'.
    - `String` - spent_output_id, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - arbitrary, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - input_id, hash of input action.
    - `Array of String` - witness_arguments, witness arguments.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.
  - `Array of Object` - outputs, object of outputs for the transaction.
    - `String` - type, the type of output action, available option include: 'retire', 'control'.
    - `String` - id, outputid related to utxo.
    - `Integer` - position, position of outputs.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - control_program, control program of account.
    - `String` - address, address of account.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.
- `Array of Object` -pagination, pagination info.
  - `Integer`- _current_, current number of page.
  - `Integer`- _limit_, number of data per page.
  - `Integer`- _total_, the number of total blocks.

**例子：**

获取第一页地址为bm1q050g0urjwgvpr7eu3e3y8s0xrlntzd5kneapal的1条信息。

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/address/bm1q050g0urjwgvpr7eu3e3y8s0xrlntzd5kneapal?page=1&limit=1'

// Result
{
  "address": "bm1q050g0urjwgvpr7eu3e3y8s0xrlntzd5kneapal",
  "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "asset_name": "BTM",
  "balance": 64624051,
  "receive": 261723846,
  "sent": 197099795,
  "join_timestamp": 1562079240,
  "last_transaction_id": "c0be54ec93a89b13b96fa5b88a0c649c246da2c30fadc668a13ccee2981a2180",
  "last_transaction_amount": -11237872,
  "last_transaction_timestamp": 1566212686,
  "transaction_count": 5,
  "transactions": [
    {
      "id": "c0be54ec93a89b13b96fa5b88a0c649c246da2c30fadc668a13ccee2981a2180",
      "version": 1,
      "size": 328,
      "time_range": 0,
      "status_fail": false,
      "mux_id": "9b8d0656c7b12603f2af2634297fce411a762880e9dd226f8f10ff50f5d8e0c9",
      "height": 289288,
      "timestamp": 1566212686,
      "chain_status": "mainnet",
      "coinbase": 0,
      "cross_chain": 0,
      "fee": 1237872,
      "transaction_amount": 10000000,
      "confirmations": 403,
      "details": [
        {
          "type": "spend",
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 66099795,
          "control_program": "00147d1e87f072721811fb3c8e6243c1e61fe6b13696",
          "address": "bm1q050g0urjwgvpr7eu3e3y8s0xrlntzd5kneapal",
          "spent_output_id": "08ce803b65f55061d0f31e040e786260c8686a1e8a0ea9c0f768a212031250cf",
          "input_id": "dd95f639161938044f27df419840aaf117e404664ec3fee39f735d7e8fdb4787",
          "witness_arguments": [
            "755513c9e7632f2d3fd474a231971bce7060ea3f74ece97ff56aaeba3a4dbff842b4da5149550c1ea9ac6b8573836abbc470e071bae590b1feb6f8207aa89806",
            "1e744f10240a65f59882be59ed875296189b2a54b1e6d83eb42e7a031f1d72aa"
          ],
          "transaction_id": "c0be54ec93a89b13b96fa5b88a0c649c246da2c30fadc668a13ccee2981a2180",
          "status_fail": false,
          "io": 0,
          "decode_program": [
            "DUP",
            "HASH160",
            "DATA_20 7d1e87f072721811fb3c8e6243c1e61fe6b13696",
            "EQUALVERIFY",
            "TXSIGHASH",
            "SWAP",
            "CHECKSIG"
          ],
          "asset_name": "BTM",
          "asset_decimals": "8"
        },
        {
          "type": "control",
          "id": "0125803de363683b2ccd7a76ebb7572948a596326ef333362158a5c83638993b",
          "position": 0,
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 10000000,
          "control_program": "00147e3caa577adacaa2988b026929f4a068814eef40",
          "address": "bm1q0c7254m6mt929xytqf5jna9qdzq5am6qhc2rmt",
          "transaction_id": "c0be54ec93a89b13b96fa5b88a0c649c246da2c30fadc668a13ccee2981a2180",
          "status_fail": false,
          "io": 1,
          "decode_program": [
            "DUP",
            "HASH160",
            "DATA_20 7e3caa577adacaa2988b026929f4a068814eef40",
            "EQUALVERIFY",
            "TXSIGHASH",
            "SWAP",
            "CHECKSIG"
          ],
          "asset_name": "BTM",
          "asset_decimals": "8"
        },
        {
          "type": "control",
          "id": "be1d47c079ca907451e02ebec3b83523ac6930c530ea2646ba9e4c55db14a460",
          "position": 1,
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 54861923,
          "control_program": "00147d1e87f072721811fb3c8e6243c1e61fe6b13696",
          "address": "bm1q050g0urjwgvpr7eu3e3y8s0xrlntzd5kneapal",
          "transaction_id": "c0be54ec93a89b13b96fa5b88a0c649c246da2c30fadc668a13ccee2981a2180",
          "status_fail": false,
          "io": 1,
          "decode_program": [
            "DUP",
            "HASH160",
            "DATA_20 7d1e87f072721811fb3c8e6243c1e61fe6b13696",
            "EQUALVERIFY",
            "TXSIGHASH",
            "SWAP",
            "CHECKSIG"
          ],
          "asset_name": "BTM",
          "asset_decimals": "8"
        }
      ]
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 1,
    "total": 5
  }
}
```

---

##### `address-assets`

获取地址的所有账户资产，接收资产数量，发送资产数量和最近的交易信息。

**参数**

对象:

- `String`- _address_, address

**返回**

对象:

- `Array of Object` - transactions, transaction object:
  - `String` - _id, transaction id, hash of the transaction.
  - `Integer` - total_amoount, asset total amount.
  - `String` - name, asset name.
  - `String` - decimals, decimal of asset.
  - `String` - symbol, size of transaction.
  - `String` - description, asset description.
  - `Integer` - address_count, the asset has the number of address.
  - `Integer` - balance, address own the asset's amount.
  - `Integer` - issue_timestamp, issuse timestamp.
  - `Integer` - receive, receive this asset amount.
  - `Integer` - sent, sent this asset amount.
  - `Integer` - transaction_amount, the amount of transaction.

**例子：**

获取地址为bm1q050g0urjwgvpr7eu3e3y8s0xrlntzd5kneapal的资产信息：

```
// Request
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{"address": "bm1q050g0urjwgvpr7eu3e3y8s0xrlntzd5kneapal"}' 'https://blockmeta.com/api/v2/address-assets'


// Result
  [
  {
    "_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "total_amount": 209999999906186430,
    "decimals": "8",
    "description": "Bytom Official Issue",
    "name": "BTM",
    "symbol": "BTM",
    "address_count": 293376,
    "issue_timestamp": 1524499200,
    "balance": 64624051,
    "receive": 261723846,
    "sent": 197099795,
    "transaction_amount": 5
  }
]
```

---

##### `address-assets-info`

通过资产id或者地址获取账户资产，接收资产数量，发送资产数量和接收的交易。

**参数**

`Object`:

- `String`- _address_, address
- `String`- _asset_id_, asset id

**返回**

`对象`:

- `Array of Object` - transactions, transaction object:
  - `Integer` - balance, address own the asset's amount.
  - `Integer` - receive, receive this asset amount.
  - `Integer` - sent, sent this asset amount.
  - `Integer` - transaction_amount, the amount of transaction.

**例子**

获取地址为bm1q050g0urjwgvpr7eu3e3y8s0xrlntzd5kneapal的资产信息：

```
// Request
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{"address": "bm1q050g0urjwgvpr7eu3e3y8s0xrlntzd5kneapal","asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"}' 'https://blockmeta.com/api/v2/address-assets-info'

// Result
{
  "balance": 64624051, 
  "receive": 261723846, 
  "sent": 197099795, 
  "transaction_amount": 5
}
```

---

##### `latest-block`

获取地址挖矿的最后一个区块

**参数**

可选:

- `String`- _address_, miner address.

**返回**

`可选`:

- `String`- _hash_, hash of block.
- `Integer`- _size_, size of block.
- `Integer`- _version_, version of block.
- `Integer`- _height_, height of block.
- `String`- _previous_block_hash_, previous block hash.
- `Integer`- _timestamp_, timestamp of block.
- `Integer`- _nonce_, nonce value.
- `Integer`- _bits_, bits of difficulty.
- `String`- _difficulty_, difficulty value(String type).
- `String`- _transaction_merkle_root_, merkle root of transaction.
- `String`- _transaction_status_hash_, merkle root of transaction status.
- `Integer`- _hash_rate_, the hash rate of block.
- `Integer`- _miner_address_, the address of miner.
- `Integer`- _transaction_count_, the count of transaction in the block.
- `String`- _chain_status_, mainchain or orphan.
- `String`- _cross_chain_, the flag of this block include cross chain transaction.
- `String`- _miner_name_, the name of miner.
- `Array of Object` - transactions, transaction object:
  - `String` - id, transaction id, hash of the transaction.
  - `Integer` - version, version of transaction.
  - `Integer` - size, size of transaction.
  - `Integer` - time_range, the unix timestamp for when the requst was responsed.
  - `Boolean` - status_fail, whether the state of the request has failed.
  - `String` - mux_id, the previous transaction mux id(source id of utxo).
  - `Integer` - height, block height.
  - `Integer` - chain_status, mainnet or orphan.
  - `Integer` - coinbase, the flag of coinbase transaction.
  - `Boolean` - cross_chain, the flag of cross chain transaction.
  - `Integer` - fee, transaction fee.
  - `Integer` - transaction_amount, the amount of transaction.
  - `Integer` - confirmations, the number comfirmed.
  - `Array of Object` - inputs, object of inputs for the transaction.
    - `String` - type, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `Object` - issuance_program, issuance program, it only exist when type is 'issue'.
    - `Object` - control_program, control program of account, it only exist when type is 'spend'.
    - `String` - address, address of account, it only exist when type is 'spend'.
    - `String` - spent_output_id, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - arbitrary, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - input_id, hash of input action.
    - `Array of String` - witness_arguments, witness arguments.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.
  - `Array of Object` - outputs, object of outputs for the transaction.
    - `String` - type, the type of output action, available option include: 'retire', 'control'.
    - `String` - id, outputid related to utxo.
    - `Integer` - position, position of outputs.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - control_program, control program of account.
    - `String` - address, address of account.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.

**例子**

获取地址是bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0挖矿的最新出块：

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/latest-block?address=bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0'

// Result
{
    "hash": "3ff69fdcfe18fa1a7e75e7dd9356c51905ff9ff85a2e8b198ed7748a6fd5b741",
    "size": 2700,
    "version": 1,
    "height": 289676,
    "previous_block_hash": "ef66e7e1c7472c077b3ad0c465922f0731b4deb938975052cb591fd457a72e61",
    "timestamp": 1566267013,
    "nonce": 961065577589284010,
    "bits": 2017612633063140159,
    "difficulty": "62228534746",
    "transaction_merkle_root": "60e9251bc24e945231de2cafaf6fbb456eb3cf541623c4e4f381a832f130327a",
    "transaction_status_hash": "a4489d66751139d2d3f120b2dacf4a8c52e6cadd7de603dc8ef1c66c350cba74",
    "hash_rate": 426222840,
    "miner_address": "bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
    "transaction_count": 3,
    "chain_status": "mainnet",
    "cross_chain": false,
    "miner_name": "f2pool",
    "transactions": [
        {
            "id": "37a906fb0459831556a646dc18e75a98f052c47947f42512265ed41e72697dfb",
            "version": 1,
            "size": 82,
            "time_range": 0,
            "status_fail": false,
            "mux_id": "581f4e1ce9cd5d579f1c38112e0bdeca6e4dc28d25894cc14576d2a613b93d29",
            "height": 289676,
            "timestamp": 1566267013,
            "chain_status": "mainnet",
            "coinbase": 1,
            "cross_chain": 0,
            "fee": 0,
            "transaction_amount": 41251898000,
            "confirmations": 1,
            "details": [
                {
                    "type": "coinbase",
                    "asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
                    "amount": 0,
                    "arbitrary": "00323839363736",
                    "input_id": "b7b6618cb3fae688326ccaad45ca30f6ff4bf765f2bdfe25bfc0a943481c930a",
                    "transaction_id": "37a906fb0459831556a646dc18e75a98f052c47947f42512265ed41e72697dfb",
                    "status_fail": false,
                    "io": 0
                },
                {
                    "type": "control",
                    "id": "48dcca5ee9c02c1b74eba2593ce436ecc5850ac90681edd9ba74e77ae9f18aa3",
                    "position": 0,
                    "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount": 41251898000,
                    "control_program": "00148916ad528556048b97437f05a8afa7482afe0b94",
                    "address": "bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
                    "transaction_id": "37a906fb0459831556a646dc18e75a98f052c47947f42512265ed41e72697dfb",
                    "status_fail": false,
                    "io": 1,
                    "decode_program": [
                        "DUP",
                        "HASH160",
                        "DATA_20 8916ad528556048b97437f05a8afa7482afe0b94",
                        "EQUALVERIFY",
                        "TXSIGHASH",
                        "SWAP",
                        "CHECKSIG"
                    ],
                    "asset_name": "BTM",
                    "asset_decimals": "8"
                }
            ]
        },
        {
            "id": "b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7",
            "version": 1,
            "size": 531,
            "time_range": 306954,
            "status_fail": false,
            "mux_id": "c2f96eac6c97fb0729024d3c050010bf981903d2a9ee54edb85508fef6a801b2",
            "height": 289676,
            "timestamp": 1566267013,
            "chain_status": "mainnet",
            "coinbase": 0,
            "cross_chain": 0,
            "fee": 898000,
            "transaction_amount": 83700000000,
            "confirmations": 1,
            "details": [
                {
                    "type": "spend",
                    "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount": 83700000000,
                    "control_program": "0014a49a20e654f4813ce9154a4624a13743c56b4797",
                    "address": "bm1q5jdzpej57jqne6g4ffrzfgfhg0zkk3uh7hgnuc",
                    "spent_output_id": "7149736715cfacdd9c108d0cc052e5bf88bef900afe48efcf0aee7a7209e7bee",
                    "input_id": "2de66fe9f3e292c633121792aef284f7074a17f798f290d7ddee30da9ee4e739",
                    "witness_arguments": [
                        "dd38366d1f6cca4a6d51fc6d4902303aabf1f0ce3512be20c31d1cc5f0dff60971adaab2262a60feb90315fd9a3afb9811c2324ab33512868fcd3fa3a1b8da0d",
                        "ef6220d45792bf9057a56ce5ffc21ee55c96c1cba7267c0edff14f227f3fe1f7"
                    ],
                    "transaction_id": "b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7",
                    "status_fail": false,
                    "io": 0,
                    "decode_program": [
                        "DUP",
                        "HASH160",
                        "DATA_20 a49a20e654f4813ce9154a4624a13743c56b4797",
                        "EQUALVERIFY",
                        "TXSIGHASH",
                        "SWAP",
                        "CHECKSIG"
                    ],
                    "asset_name": "BTM",
                    "asset_decimals": "8"
                },
                {
                    "type": "spend",
                    "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount": 61168913,
                    "control_program": "0014a49a20e654f4813ce9154a4624a13743c56b4797",
                    "address": "bm1q5jdzpej57jqne6g4ffrzfgfhg0zkk3uh7hgnuc",
                    "spent_output_id": "b098d65d1260913ff21dceae7806af02d66b50141b779bf085c6130486cce4ef",
                    "input_id": "169442accea85946040d19df2cb25c939b36dafafb8acbf33a817cb643f73b36",
                    "witness_arguments": [
                        "8c53335eff0afee679a888846cf3f45fb2ebfb2c3b1f2b116ea74e14b6b83bbb4d2f0a4a72bb2661b787ebf1247433bff56ebc5fcc70b7aaeb4a25df7f9ffd0d",
                        "ef6220d45792bf9057a56ce5ffc21ee55c96c1cba7267c0edff14f227f3fe1f7"
                    ],
                    "transaction_id": "b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7",
                    "status_fail": false,
                    "io": 0,
                    "decode_program": [
                        "DUP",
                        "HASH160",
                        "DATA_20 a49a20e654f4813ce9154a4624a13743c56b4797",
                        "EQUALVERIFY",
                        "TXSIGHASH",
                        "SWAP",
                        "CHECKSIG"
                    ],
                    "asset_name": "BTM",
                    "asset_decimals": "8"
                },
                {
                    "type": "control",
                    "id": "09a345b86d94e40750b444ddc754c46ceda1c482d4211f706c2da818a2900d77",
                    "position": 0,
                    "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount": 83700000000,
                    "control_program": "001474e7a09a2729d93c30010b0be906588ca9f7de2e",
                    "address": "bm1qwnn6px3898vncvqppv97jpjc3j5l0h3wpzadya",
                    "transaction_id": "b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7",
                    "status_fail": false,
                    "io": 1,
                    "decode_program": [
                        "DUP",
                        "HASH160",
                        "DATA_20 74e7a09a2729d93c30010b0be906588ca9f7de2e",
                        "EQUALVERIFY",
                        "TXSIGHASH",
                        "SWAP",
                        "CHECKSIG"
                    ],
                    "asset_name": "BTM",
                    "asset_decimals": "8"
                },
                {
                    "type": "control",
                    "id": "a5baa6606c2550a390cc541c2effcf9dcf81695bc6069b53bb2149be8f91077a",
                    "position": 1,
                    "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount": 60270913,
                    "control_program": "0014a49a20e654f4813ce9154a4624a13743c56b4797",
                    "address": "bm1q5jdzpej57jqne6g4ffrzfgfhg0zkk3uh7hgnuc",
                    "transaction_id": "b81cd381f29fc5f6b0b930329fb9c036c6873e6abd9ceb0f61111e76b6f1e7b7",
                    "status_fail": false,
                    "io": 1,
                    "decode_program": [
                        "DUP",
                        "HASH160",
                        "DATA_20 a49a20e654f4813ce9154a4624a13743c56b4797",
                        "EQUALVERIFY",
                        "TXSIGHASH",
                        "SWAP",
                        "CHECKSIG"
                    ],
                    "asset_name": "BTM",
                    "asset_decimals": "8"
                }
            ]
        },
        {
            "id": "f0b93c68aad982238adc3d8861a8164289de019c51ea66b3046ea7f829719ddc",
            "version": 1,
            "size": 611,
            "time_range": 0,
            "status_fail": false,
            "mux_id": "36227e0126a348eda43b225161a8cb71f751a83417a52c7b978fbca1cf90cbbe",
            "height": 289676,
            "timestamp": 1566267013,
            "chain_status": "mainnet",
            "coinbase": 0,
            "cross_chain": 0,
            "fee": 1000000,
            "transaction_amount": 7230100000,
            "confirmations": 1,
            "details": [
                {
                    "type": "spend",
                    "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount": 7231100000,
                    "control_program": "001482d32c1da9c2708646009955bd8ab5546aefa8b5",
                    "address": "bm1qstfjc8dfcfcgv3sqn92mmz44234wl294203hjg",
                    "spent_output_id": "498a99ea6c9c6f20761768b04be77527221cea464e5e070c4243f632b548a640",
                    "input_id": "f8cb5c5ad4864661efe1a6256afa29ebb58748f7a487281411b234a00875870e",
                    "witness_arguments": [
                        "700521a2d0fa8220d625d2e87dd052c5c5defcfdb71770215538011d9231fcbfc92dac0d947e224390e0e1d90f8c132c26ab25e16fc3040f625b4c2046d5fe0a",
                        "5e632df83c0ed3e8bcb62df70400faf652d57e154e9c2f9b24be6e4ab5fd4635"
                    ],
                    "transaction_id": "f0b93c68aad982238adc3d8861a8164289de019c51ea66b3046ea7f829719ddc",
                    "status_fail": false,
                    "io": 0,
                    "decode_program": [
                        "DUP",
                        "HASH160",
                        "DATA_20 82d32c1da9c2708646009955bd8ab5546aefa8b5",
                        "EQUALVERIFY",
                        "TXSIGHASH",
                        "SWAP",
                        "CHECKSIG"
                    ],
                    "asset_name": "BTM",
                    "asset_decimals": "8"
                },
                {
                    "type": "spend",
                    "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
                    "amount": 47798700000000,
                    "control_program": "0014dee5f96a151ed4094793194ea7e89e9ae53aa5b2",
                    "address": "bm1qmmjlj6s4rm2qj3unr98206y7ntjn4fdjvzrcy9",
                    "spent_output_id": "012840466f8613cad614fa013678a696e2364dab77a54e8bb5b6cdafdfc0fbc3",
                    "input_id": "c3c78875d95a5a0553ef5bc5605207f5ae30cbbe009ee86a317715bd303b2edb",
                    "witness_arguments": [
                        "9916bd1993a768788fcde9ff8e3cbf014ca90be55c42d64cd4c181f3830eac0e086357a09dadffb908e71cd9a35ac6c82e2e023727760d45d99633aa4530fc00",
                        "0f6d37072d82e3b850e10993f38c9b600cc0875273005ea258407ddf003d472f"
                    ],
                    "transaction_id": "f0b93c68aad982238adc3d8861a8164289de019c51ea66b3046ea7f829719ddc",
                    "status_fail": false,
                    "io": 0,
                    "decode_program": [
                        "DUP",
                        "HASH160",
                        "DATA_20 dee5f96a151ed4094793194ea7e89e9ae53aa5b2",
                        "EQUALVERIFY",
                        "TXSIGHASH",
                        "SWAP",
                        "CHECKSIG"
                    ],
                    "asset_name": "BQB",
                    "asset_decimals": "8"
                },
                {
                    "type": "control",
                    "id": "5b3809429088321b715ff4de7d78fbe4f5fb1c6cf3ddcb4d8934169507b7be27",
                    "position": 0,
                    "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                    "amount": 7230100000,
                    "control_program": "0014c753bba259e6cd5d2d8af846ee32848fa4f1877b",
                    "address": "bm1qcafmhgjeumx46tv2lprwuv5y37j0rpmmjl0y3a",
                    "transaction_id": "f0b93c68aad982238adc3d8861a8164289de019c51ea66b3046ea7f829719ddc",
                    "status_fail": false,
                    "io": 1,
                    "decode_program": [
                        "DUP",
                        "HASH160",
                        "DATA_20 c753bba259e6cd5d2d8af846ee32848fa4f1877b",
                        "EQUALVERIFY",
                        "TXSIGHASH",
                        "SWAP",
                        "CHECKSIG"
                    ],
                    "asset_name": "BTM",
                    "asset_decimals": "8"
                },
                {
                    "type": "control",
                    "id": "b02f680069c7e2833fc7bdb7236bcee0ebee38e06130a936f8ed1067e3be5ef0",
                    "position": 1,
                    "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
                    "amount": 47798600000000,
                    "control_program": "001488921459c8f577ebe89af394b7e742e1d8dcc659",
                    "address": "bm1q3zfpgkwg74m7h6y67w2t0e6zu8vde3jef6amly",
                    "transaction_id": "f0b93c68aad982238adc3d8861a8164289de019c51ea66b3046ea7f829719ddc",
                    "status_fail": false,
                    "io": 1,
                    "decode_program": [
                        "DUP",
                        "HASH160",
                        "DATA_20 88921459c8f577ebe89af394b7e742e1d8dcc659",
                        "EQUALVERIFY",
                        "TXSIGHASH",
                        "SWAP",
                        "CHECKSIG"
                    ],
                    "asset_name": "BQB",
                    "asset_decimals": "8"
                },
                {
                    "type": "retire",
                    "id": "97ac9b0808b93e38a98a5a2ba6db53a7e500ef24dc5c12497914ce1c4e807bc1",
                    "position": 2,
                    "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
                    "amount": 100000000,
                    "control_program": "6a235c2ba7b391fa999e93a39c67cf586581f90c3b1dc09ab2e404870e2af4955b6f1d2b55",
                    "transaction_id": "f0b93c68aad982238adc3d8861a8164289de019c51ea66b3046ea7f829719ddc",
                    "status_fail": false,
                    "io": 1,
                    "decode_program": [
                        "FAIL",
                        "DATA_35 5c2ba7b391fa999e93a39c67cf586581f90c3b1dc09ab2e404870e2af4955b6f1d2b55"
                    ],
                    "asset_name": "BQB",
                    "asset_decimals": "8"
                }
            ]
        }
    ]
}
```

---

##### `asset/{asset_id}`

通过资产id获取资产详情

**参数**

`可选`:

- `String`- _asset_id_, asset id.

`可选`:

- `Integer`- _page_, page nunber of data.
- `Integer`- _limit_, number of data per page.

**返回**

`可选`:

- `Integer` - total_amount, issue total amount.
- `String` - decimals, decimals.
- `String` - description, asset description.
- `String` - name, asset name.
- `String` - symbol, asset symbol.
- `Integer` - address_count, asset's address count.
- `String` - asset_id, uuid of asset.
- `Integer` - issuse_timestamp, latest transaction amount.
- `Array of Object` - transactions, transaction object:
  - `String` - id, transaction id, hash of the transaction.
  - `Integer` - version, version of transaction.
  - `Integer` - size, size of transaction.
  - `Integer` - time_range, the unix timestamp for when the requst was responsed.
  - `Boolean` - status_fail, whether the state of the request has failed.
  - `String` - mux_id, the previous transaction mux id(source id of utxo).
  - `Integer` - height, block height.
  - `Integer` - chain_status, mainnet or orphan.
  - `Integer` - coinbase, the flag of coinbase transaction.
  - `Boolean` - cross_chain, the flag of cross chain transaction.
  - `Integer` - fee, transaction fee.
  - `Integer` - transaction_amount, the amount of transaction.
  - `Integer` - confirmations, the number comfirmed.
  - `Array of Object` - inputs, object of inputs for the transaction.
    - `String` - type, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `Object` - issuance_program, issuance program, it only exist when type is 'issue'.
    - `Object` - control_program, control program of account, it only exist when type is 'spend'.
    - `String` - address, address of account, it only exist when type is 'spend'.
    - `String` - spent_output_id, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - arbitrary, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - input_id, hash of input action.
    - `Array of String` - witness_arguments, witness arguments.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.
  - `Array of Object` - outputs, object of outputs for the transaction.
    - `String` - type, the type of output action, available option include: 'retire', 'control'.
    - `String` - id, outputid related to utxo.
    - `Integer` - position, position of outputs.
    - `String` - asset_id, asset id.
    - `String` - asset_alias, name of asset.
    - `Object` - asset_definition, definition of asset(json object).
    - `Integer` - amount, amount of asset.
    - `String` - account_id, account id.
    - `String` - account_alias, name of account.
    - `Object` - control_program, control program of account.
    - `String` - address, address of account.
    - `String` - asset_name, asset name.
    - `String` - asset_decimals, decimal of asset.
- `Array of Object` -pagination, pagination info.
  - `Integer`- _current_, current number of page.
  - `Integer`- _limit_, number of data per page.
  - `Integer`- _total_, the number of total blocks.

**例子：**

获取资产id是ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff的信息。

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/asset/ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff?page=1&limit=1'

// Result
{
  "total_amount": 210000000000000000,
  "decimals": "8",
  "description": "Bytom Official Issue",
  "name": "BTM",
  "symbol": "BTM",
  "address_count": 293378,
  "issue_timestamp": 1524499200,
  "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "transactions": [
    {
      "id": "5b04e342b7943c819ea86d8a8b966c7b0bf437a187276f337022c21a824856d5",
      "version": 1,
      "size": 82,
      "time_range": 0,
      "status_fail": false,
      "mux_id": "cc1715e52d2572789410fa13abad4f169b2683bfb76190e1eff025aba6d4611c",
      "height": 289698,
      "timestamp": 1566270535,
      "chain_status": "mainnet",
      "coinbase": 1,
      "cross_chain": 0,
      "fee": 0,
      "transaction_amount": 41256397400,
      "confirmations": 1,
      "details": [
        {
          "type": "coinbase",
          "asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
          "amount": 0,
          "arbitrary": "00323839363938",
          "input_id": "6243e9b668f601cdd010207ef66428b40c9b9047f55368b11ea8a70cbe7f3fd6",
          "transaction_id": "5b04e342b7943c819ea86d8a8b966c7b0bf437a187276f337022c21a824856d5",
          "status_fail": false,
          "io": 0
        },
        {
          "type": "control",
          "id": "1fd8fd51f1dc1971cae5267af91e5935fad98eb9ca2457031c91319029c39a56",
          "position": 0,
          "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
          "amount": 41256397400,
          "control_program": "001479c730753af7e89bd97a7fc158cdfe30331d7447",
          "address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
          "transaction_id": "5b04e342b7943c819ea86d8a8b966c7b0bf437a187276f337022c21a824856d5",
          "status_fail": false,
          "io": 1,
          "decode_program": [
            "DUP",
            "HASH160",
            "DATA_20 79c730753af7e89bd97a7fc158cdfe30331d7447",
            "EQUALVERIFY",
            "TXSIGHASH",
            "SWAP",
            "CHECKSIG"
          ],
          "asset_name": "BTM",
          "asset_decimals": "8"
        }
      ]
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 1,
    "total": 635929
  }
}
```

---

##### `asset-totalcoins/{asset_id}`

通过资产id获取资产发行总量

**参数**

`可选`:

- `String`- _asset_id_, asset id.
**返回**

`可选`:

- `Integer` - total_amount, issue total amount.

**例子**

根据资产id获取资产发行总量：

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/asset-totalcoins/ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff'

// Result
2100000000
```

---

##### `assets`

在比原链上获取issued资产和BTM

**参数**

`可选`:

- `Integer`- _page_, page nunber of data.
- `Integer`- _limit_, number of data per page.

**返回**

`可选`:

- `Array of Object` - assets, asset object:
  - `Object` - issuance_program, issuance program, it only exist when type is 'issue'.
  - `Array of String` - decode_program, decode arguments.
  - `Integer` - total_amount, total amount.
  - `String` - name, asset name.
  - `String` - decimals, decimal of asset.
  - `Object` - description, issue description'.
  - `String` - symbol, symbol.
  - `Integer` - address_count, address count.
  - `Integer` - issue_timestamp, issue timestamp.
  - `String` - reissue, is reissue.
  - `Integer` - quorum, is quorum.
  - `Integer` - is_bap2, protocal of bap2.
  - `String` - asset_id, asset id.
  - `Integer` - confirmations, the number comfirmed.
- `Array of Object` -pagination, pagination info.
  - `Integer`- _current_, current number of page.
  - `Integer`- _limit_, number of data per page.
  - `Integer`- _total_, the number of total blocks.

**例子：**

在比原链网络上获取issued资产：

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/assets?page=2&limit=1'

// Result
{
  "assets": [
    {
      "issuance_program": "ae200f245fb4ea2de35e5509000faf74ae5b9067934d36ed660366518bc5d98da2265151ad",
      "decode_program": [
        "TXSIGHASH",
        "DATA_32 0f245fb4ea2de35e5509000faf74ae5b9067934d36ed660366518bc5d98da226",
        "1 01",
        "1 01",
        "CHECKMULTISIG"
      ],
      "total_amount": 1000000000000000,
      "decimals": "8",
      "description": {},
      "name": "515",
      "symbol": "GRIN",
      "address_count": 71,
      "issue_timestamp": 1557907356,
      "reissue": "true",
      "quorum": 1,
      "is_bap2": 1,
      "asset_id": "07ec279f395c6b8dbf0a6af456f09734d3e294f7376e2f8d38dbf40df2e9f82f"
    }
  ],
  "pagination": {
    "current": 2,
    "limit": 1,
    "total": 137
  }
}
```

---

##### `rank/{asset_id}`

获取地址下资产的排名。

**参数**

`可选`:

- `String`- _asset_id_, asset id.

`可选`:

- `Integer`- _page_, page nunber of data.
- `Integer`- _limit_, number of data per page.

**返回**

`可选`:

- `Array of Object` - address, address object:
  - `String` - address, address.
  - `String` - asset_id, asset id.
  - `String` - asset_name, asset name.
  - `Integer` - balance, address balance.
  - `Integer` - receive, address reveived asset total amount.
  - `Integer` - sent, address sent asset total amount.
  - `Integer` - join_timestamp, first time create address.
  - `String` - last_transaction_id, latest transaction id.
  - `Integer` - last_transaction_amount, latest transaction amount.
  - `Integer` - last_transaction_timestamp, latest transaction timestamp.
  - `Integer` - transaction_count, the address's transaction count.
  - `Float` - percent, percent of address in total:
- `Array of Object` -pagination, pagination info.
  - `Integer`- _current_, current number of page.
  - `Integer`- _limit_, number of data per page.
  - `Integer`- _total_, the number of total blocks.

**例子**

获取资产id是ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff的所有地址资产排名：

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/rank/ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff?page=1&limit=1'
// Result
{
  "addresses": [
    {
      "address": "bm1qtmt60f9jamarpyvw2eplhmsuzrkfcmxp37s94fzvg9lypgnvsg7qt2q492",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "asset_name": "BTM",
      "balance": 21000000000000000,
      "receive": 105000000280000000,
      "sent": 84000000280000000,
      "join_timestamp": 1525680122,
      "last_transaction_id": "3df9d16ca089e39fb3d76d6f8210a264d5a35cd6898ac42a57e12df2202f5d95",
      "last_transaction_amount": -21000000000000000,
      "last_transaction_timestamp": 1563774584,
      "percent": 0.10000000004467312
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 1,
    "total": 293408
  }
}
```

---

##### `daily/kline/{pair}`

获取btm日常k线的收盘价，主要的交易对有btm_btc, btm_eth, btm_usd, btm_cny.

**参数**

`可选`:

- `String`- _pair_, btm exchange pair, Pair include btm_btc,btm_eth,btm_usd,btm_cny.

`可选`:

- `Integer`- _from_, start timestamp of statistic.
- `Integer`- _to_, end timestamp of statistic.

**返回**

`可选`:

- `Array` :
  - `Array` - [price,timetamp].

**例子**

获取今天btm_cny的价格:

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/daily/kline/btm_cny'

// Result
[
  [
    0.6497919999999999,
    1566259200
  ]
]
```

---

##### `daily/miner`

获取比原链网络挖矿每日统计数据

**参数**

`可选`:

- `Integer`- _from_, start timestamp of statistic.
- `Integer`- _to_, end timestamp of statistic.

**返回**

`可选`:

- `Array` :
  - `Array of Object`
    - `String` - address, address.
    - `Integer` - fee, transaction fee.
    - `Integer` - mined_block_count, mined block count.
    - `String` - name, miner name.
    - `Integer` - timestamp, mined timestamp.
    - `Integer` - profit, profit.
    - `Float` - percent, percent of total miner:

**例子**

获取比原链网络今日挖矿数据：

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/daily/miner'

// Result
[
  [
    {
      "address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
      "fee": 677435204,
      "mined_block_count": 169,
      "percent": 0.5331230283911672,
      "profit": 6971927435204,
      "timestamp": 1566230400,
      "name": "antpool"
    },
    {
      "address": "bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
      "fee": 131713600,
      "mined_block_count": 93,
      "percent": 0.29337539432176657,
      "profit": 3836381713600,
      "timestamp": 1566230400,
      "name": "f2pool"
    },
    {
      "address": "bm1qrwhwspf4mva328xtaeed9fjmgj2u8mqywv887z",
      "fee": 9100000,
      "mined_block_count": 12,
      "percent": 0.03785488958990536,
      "profit": 495009100000,
      "timestamp": 1566230400,
      "name": "beepool"
    },
    {
      "address": "bm1qlr5e6ep34tdr6566q9d6zp60d449338nwuhkdw",
      "fee": 113224591,
      "mined_block_count": 37,
      "percent": 0.1167192429022082,
      "profit": 1526363224591,
      "timestamp": 1566230400,
      "name": "matpool"
    },
    {
      "address": "bm1qcxg0w7c70tdd46t7dxn204mkyeyudcz063s49e",
      "fee": 21347000,
      "mined_block_count": 6,
      "percent": 0.01892744479495268,
      "profit": 247521347000,
      "timestamp": 1566230400,
      "name": "uupool"
    }
  ]
]
```

---

##### `daily/total`

获取比原网络基本数据每日统计

**参数**

`可选`:

- `Integer`- _from_, start timestamp of statistic.
- `Integer`- _to_, end timestamp of statistic.
 
**返回**

`可选`:

- `Array` :
  - `Array of Object`
    - `String` - date, date time.
    - `Integer` - confirmed_block_count, confirmed block count.
    - `Integer` - issue_count, new issue count.
    - `Integer` - mined_btm_amount, mined BTM amount.
    - `Integer` - new_address_count, new address count.
    - `Integer` - new_asset_count, new asset count.
    - `Integer` - orphan_block_count, orphan block count.
    - `Integer` - retire_count, retire transaction count.
    - `Integer` - transaction_amount, transaction amount(BTMZ).
    - `Integer` - transaction_count, transaction count.
    - `Integer` - transaction_fee, transaction fee.
    - `Float` - transaction_gas, transaction gas.
    - `Integer` - average_block_time, average block time.

**例子**

获取今日比原网络基本数据统计:

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/daily/total'

// Result
[
  {
    "confirmed_block_count": 324,
    "issue_count": 10,
    "mined_btm_amount": 13365000000000,
    "new_address_count": 244,
    "new_asset_count": 6,
    "orphan_block_count": 0,
    "retire_count": 97,
    "transaction_amount": 715175145397613,
    "transaction_count": 566,
    "transaction_fee": 963320395,
    "transaction_gas": 4816601.975,
    "date": "2019-08-20",
    "average_block_time": 2.675925925925926
  }
]
```

---

##### `stat/diffculty`

获取比原链网络挖矿难度统计

**参数**

`可选`:

- `Integer`- _from_, start timestamp of statistic.
- `Integer`- _to_, end timestamp of statistic.

**返回**

`可选`:

- `Array` :
  - `Array of Object`
    - `String` - change_time, detail time of diffculty change.
    - `Integer` - diffculty, diffculty.
    - `Float` - change_rate, change rate.

**例子**

获取比原链网络区块从1565740800到1566259200的挖矿难度统计:

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/stat/difficulty?from=1565740800&to=1566259200'

// Result
[
  {
    "change_time": "2019-08-14 16:10:33",
    "difficulty": 58652409817,
    "change_rate": 0.13959267530303915
  },
  {
    "change_time": "2019-08-17 23:18:15",
    "difficulty": 62228534746,
    "change_rate": 0.060971491881710964
  }
]
```

---

##### `stat/miner`

获取比原链网络挖矿总统计

**参数**

`可选`:

- `Integer`- _from_, start timestamp of statistic.
- `Integer`- _to_, end timestamp of statistic.

**返回**

`可选`:

- `Array` :
  - `Array of Object`
    - `String` - address, miner address.
    - `String` - name, miner name.
    - `Integer` - mined_block_count, mined block count.
    - `Integer` - profit, miner profit.
    - `Integer` - fee, transaction fee.
    - `Float` - percent, percent in total.
    - `Float` - hash_rate, hash rate.

**例子**

获取比原链网络区块高度从1566040800到1566259200的总的挖矿统计:

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/stat/miner?from=1566040800&to=1566259200'

// Result
[
  {
    "address": "bm1q08rnqaf67l5fhkt60lq43n07xqe36az8gwlfqx",
    "name": "antpool",
    "mined_block_count": 733,
    "profit": 30238815737766,
    "fee": 2565737766,
    "percent": 0.5111576011157601,
    "hash_rate": 207941890.83206874
  },
  {
    "address": "bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
    "name": "f2pool",
    "mined_block_count": 482,
    "profit": 19884079462580,
    "fee": 1579462580,
    "percent": 0.33612273361227335,
    "hash_rate": 136736686.74086922
  },
  {
    "address": "bm1qlr5e6ep34tdr6566q9d6zp60d449338nwuhkdw",
    "name": "matpool",
    "mined_block_count": 152,
    "profit": 6270409681800,
    "fee": 409681800,
    "percent": 0.10599721059972106,
    "hash_rate": 43120282.95562681
  },
  {
    "address": "bm1qrwhwspf4mva328xtaeed9fjmgj2u8mqywv887z",
    "name": "beepool",
    "mined_block_count": 49,
    "profit": 2021700060068,
    "fee": 450060068,
    "percent": 0.03417015341701534,
    "hash_rate": 13900617.531748114
  },
  {
    "address": "bm1qcxg0w7c70tdd46t7dxn204mkyeyudcz063s49e",
    "name": "uupool",
    "mined_block_count": 17,
    "profit": 701332125672,
    "fee": 82125672,
    "percent": 0.011854951185495118,
    "hash_rate": 4822663.225300366
  },
  {
    "address": "bm1qgdaaft9h7lt59sjfvxc3dyajjca6un2nfmc4cv",
    "name": "viabtc",
    "mined_block_count": 1,
    "profit": 41250000000,
    "fee": 0,
    "percent": 0.000697350069735007,
    "hash_rate": 283686.0720764921
  }
]
```

---

##### `stat/hash-rate`

统计比原链网络hash率

**参数**

`可选`:

- `Integer`- _from_, start timestamp of statistic.
- `Integer`- _to_, end timestamp of statistic.

**返回**

`可选`:

- `Array` :
  - `Array` - [timetamp,hash_rate].

**例子**

获取比原链区块高度从1566040800到1566259200的hash率：

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/stat/hash-rate?from=1566040800&to=1566259200'

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

##### `stat/total`

总统计Bytom网络基本数据

**参数**

`可选`:

- `Integer`- _from_, start timestamp of statistic.
- `Integer`- _to_, end timestamp of statistic.

**返回**

`可选`:

- `Integer` - _confirmed_block_count_, confirmed block count.
- `Integer` - _orphan_block_count_, orphan block count.
- `Integer` - _transaction_count_, transaction count.
- `Integer` - _transaction_amount_, transaction amount(BTM).
- `Integer` - _transaction_fee_,  transaction fee.
- `Float` - _transaction_gas_, transaction gas.
- `Integer` - _new_asset_count_, new asset count.
- `Integer` - _mined_btm_amount_, mined btm amount.
- `Integer` - _issue_count_, issue count.
- `Integer` - _retire_count_, retire count.
- `Integer` - _mining_supply_, mining supply.
- `Integer` - _circulating_supply_, circulating supply.
- `String` - _node_count_, node count.
- `String` - _market_capitalization_, market capitalization.

**例子**

总统计比原链网络区块高度从1566040800到1566259200的基本数据：

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/stat/total?from=1566040800&to=1566259200'

// Result
{
  "confirmed_block_count": 1434,
  "orphan_block_count": 1,
  "transaction_count": 2513,
  "transaction_amount": 3116086300234232,
  "transaction_fee": 5087067886,
  "transaction_gas": 25435339.43,
  "new_address_count": 1025,
  "new_asset_count": 8,
  "mined_btm_amount": 59152500000000,
  "issue_count": 15,
  "retire_count": 383,
  "mining_supply": 11952806250000000,
  "circulating_supply": 152652806250000000,
  "node_count": "51",
  "market_capitalization": "71"
}
```

---

##### `stat/utxo`

统计比原链网络的utxo

**参数**

`可选`:

- `Integer`- _from_, start timestamp of statistic.
- `Integer`- _to_, end timestamp of statistic.

**返回**

`可选`:

- `Array` :
  - `String` - _asset_id_, asset uuid.
  - `Integer` - _utxo_count_, utxo count.

**例子**

统计比原链网络从区块高度1566040800到1566259200的utxo:

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/stat/utxo?from=1566040800&to=1566259200'
// Result
[
    {
        "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "utxo_count": 5144
    },
    {
        "asset_id": "bd18639abbffa3e184d4e0add8cbc2ce1e9eb3f35d3d32a1e19018aa94441d2a",
        "utxo_count": 364
    },
    {
        "asset_id": "80013f81a66cb99977879e31639bb4fe4b12b4c7050fe518585d3f7f159d26a9",
        "utxo_count": 16
    },
    {
        "asset_id": "61f01005b8ae38976b73f362cd9e54409899fff6a388818f0fb0f01ab5953af3",
        "utxo_count": 27
    },
    {
        "asset_id": "8f18ff992ee217f88e232fe781ef756b464681e24a3d69533c45a9f611731f35",
        "utxo_count": 30
    },
    {
        "asset_id": "31434830dd7af31d7bb2aed3942cbc15f5ad78c438c11ff52caef10a05bef40c",
        "utxo_count": 26
    },
    {
        "asset_id": "ae524eebd3dd3b1c8c5678d73a8485dc27ffc9ad7ec0c2b1efa42ff5b444cb4f",
        "utxo_count": 2
    },
    {
        "asset_id": "98f21b09d06f03c4099ff17b76c7e8317ef9c465056dc7e16f72da2b481ec4f0",
        "utxo_count": 2
    },
    {
        "asset_id": "ebfd16af3da16917f5e9e54de262c1121e45aa3a6749170ad28a2a58c6cfbfa1",
        "utxo_count": 1
    },
    {
        "asset_id": "83b731179649b050f86c051acfa4032c4265cec0dc3074f1cf1c632d70fd15e3",
        "utxo_count": 1
    },
    {
        "asset_id": "c1d06db9dece76429dab31d4f23a6791a504707ce76e89abce0729f68157f469",
        "utxo_count": 1
    },
    {
        "asset_id": "afb31db5b1366b6cdd97c8b8dabb30c8387886dd49f705f8071749cae322474c",
        "utxo_count": 1
    },
    {
        "asset_id": "336022c9b5370b483900fa68364a5ad17a9dcfca2999150d379819d976af3ad8",
        "utxo_count": 1
    },
    {
        "asset_id": "5ffe03b9829b9608a0db51a13b4eced3ecd66f58e6b3833cb09fea6cbeac7900",
        "utxo_count": 3
    },
    {
        "asset_id": "c79ea0b951828626a37ca3d939af42c236ee7e0d8a90515ff9cfb430e0d13f79",
        "utxo_count": 1
    },
    {
        "asset_id": "79c0d078060d5440c5477686796e7aae073c50002469571f7e2ba150a502cbb0",
        "utxo_count": 1
    },
    {
        "asset_id": "8bee65dc952f9673479b5953ae293cf7848e3724cb633782e3e4431f60fc42dc",
        "utxo_count": 1
    },
    {
        "asset_id": "0a2d8bf55d0edcdfc8a7712cbb880cb4114b93f64b7c6e9c0d47f6a4282b7220",
        "utxo_count": 2
    }
]
```

---

##### `stat/address`

统计比原链网络的地址信息

**参数**

`可选`:

- `Integer`- _from_, start timestamp of statistic.
- `Integer`- _to_, end timestamp of statistic.

**返回**

`可选`:

- `Array` :
  - `Float` - _timestamp_, date timetamp.
  - `Integer` - _address_total_count_, address total count.
  - `Integer` - _new_address_count_, new address count on date.

**例子**

统计比原链网络区块高度从1566040800到1566259200的地址信息：

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/stat/address?from=1566040800&to=1566259200'
// Result
[
    {
        "timestamp": 1566057600,
        "address_total_count": 318714,
        "new_address_count": 409
    },
    {
        "timestamp": 1566144000,
        "address_total_count": 319138,
        "new_address_count": 424
    }
]
```

---

##### `kline/{pair}`

获取btm最近48小时的价格k线，交易对包括 btm_btc, btm_eth, btm_usd, btm_cny

**参数**

`可选`:

- `String`- _pair_, Pair include btm_btc,btm_eth,btm_usd,btm_cny.

**返回**

`可选`:

- `Array` [_item_:price]:

**例子**

获取价格K线

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/kline/btm_cny'

// Result
[
  0.61476,
  0.60771,
  0.62322,
  0.623925,
  0.629565,
  0.62463,
  0.62745,
  0.61617,
  0.618285,
  0.61899,
  0.621105,
  0.61476,
  0.616875,
  0.6204,
  0.618285,
  0.61335,
  0.614055,
  0.61758,
  0.62322,
  0.6345,
  0.64155,
  0.62886,
  0.623925,
  0.62886,
  0.635205,
  0.6337949999999999,
  0.66552,
  0.670455,
  0.670455,
  0.6718649999999999,
  0.676095,
  0.6768,
  0.66975,
  0.653535,
  0.6683399999999999,
  0.66693,
  0.6683399999999999,
  0.663405,
  0.65706,
  0.656355,
  0.6789149999999999,
  0.660585,
  0.6768,
  0.667635,
  0.6612899999999999,
  0.6612899999999999,
  0.64719,
  0.643665
]
```

---

##### `nodes`

获取比原链所有节点所在的国家，包括cn, sg, jp, es, de, us, kr, ca, ru, uk

**参数**

`可选`:

- `String`- _country_, country include cn,sg,jp,es,de,us,kr,ca,ru,uk.
- `Integer`- _page_, page number of data.
- `Integer`- _limit_, number of data per page.

**返回**

`可选`:
- `Array of Object` - address, address object:
  - `String` - address, host:port.
  - `String` - status, network status.
  - `Integer` - height, block height.
  - `String` - status_time, datetime.
  - `Integer` - rtt, Round Trip Time.
  - `String` - network, mainnet testnet.
  - `String` - version, bytom version.
  - `Boolean` - id_seed, seed node or not.
  - `Object` - coordinate, coordinate:
    - `Float` - longitude, longitude
    - `Float` - latitude, latitude.
  - `Integer` - country, country name.
  - `Integer` - symbol, country symbol.
  - `Float` - name, node name.
- `Array of Object` -pagination, pagination info.
  - `Integer`- _current_, current number of page.
  - `Integer`- _limit_, number of data per page.
  - `Integer`- _total_, the number of total blocks.

**例子**

获取节点信息：

```
// Request
curl -X GET --header 'Accept: application/json' 'https://blockmeta.com/api/v2/nodes?page=1&limit=2'

// Result
{
  "nodes": [
    {
      "address": "193.112.67.165:46657",
      "status": "active",
      "height": 289770,
      "status_time": "2019-08-20T07:05:51Z",
      "rtt": 61990659,
      "network": "mainnet",
      "version": "1.0.8+56443ac4",
      "is_seed": false,
      "coordinate": {
        "longitude": 116.3883,
        "latitude": 39.9289
      },
      "country": "China",
      "symbol": "cn",
      "name": "EONE"
    },
    {
      "address": "52.221.206.150:46657",
      "status": "active",
      "height": 289770,
      "status_time": "2019-08-20T07:05:52Z",
      "rtt": 109075953,
      "network": "mainnet",
      "version": "1.0.9",
      "is_seed": false,
      "coordinate": {
        "longitude": 103.8558,
        "latitude": 1.2931
      },
      "country": "Singapore",
      "symbol": "sg",
      "name": "比原摇摇乐"
    }
  ],
  "pagination": {
    "current": 1,
    "limit": 2,
    "total": 51
  }
}
```

---

##### `circulation-totalcoins`

获取btm的流通总量

**参数**

None

**返回**

`Float`

**例子**

获取btm的流通总量

```
// Request
https://blockmeta.com/api/v2/circulation-totalcoins

// Result
1526533425.0
```


