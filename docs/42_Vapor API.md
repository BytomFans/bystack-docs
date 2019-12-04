---
id: docs_42
title: Vapor API
sidebar_label: Vapor API
---

**API 接口**

Default JSON-RPC endpoints:

| Client | URL |
| :---: | :---: |
| Go | [http://localhost:9889](http://localhost:9889) |


A complete request example via `curl`:

**Go**

The complete request and response are as follows:

```javascript
// curl -X POST url/method -d data
curl -X POST http://localhost:9889/create-key -d '{"alias": "alice", "password": "123456", "language": "en"}'

// response
{
	"status": "success",
	"data": {
		"alias": "alice",
		"xpub": "c4afb96f600dc7da388b77107ceb471f604aadf49e6d1ec745abf9ae797e69a2a1f113e2cb2541166609ba725dea4072e54376ed90bcbdd0200853191a2f560a",
		"file": "/home/ec2-user/vapor_test/keystore/UTC--2019-06-18T11-04-34.512032731Z--66942e00-1466-45ea-b9f1-32ea30000017",
		"mnemonic": "attend build fog oak awful make diesel episode glove mind fire sleep"
	}
}
```

#### API methods

**available with wallet enable**

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

**available Whether or not the wallet is open**


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
- [`get-vote-result`](#get-vote-result)
- [`net-info`](#net-info)
- [`is-mining`](#is-mining)
- [`set-mining`](#set-mining)
- [`gas-rate`](#gas-rate)
- [`verify-message`](#verify-message)
- [`compile`](#compile)
- [`list-peers`](#list-peers)
- [`disconnect-peer`](#disconnect-peer)
- [`connect-peer`](#connect-peer)

---

#### `create-key`

It is to create private key essentially, returns the information of key. The private key is encrypted in the file and not visible to the user.

**Parameters**

`Object`:

- `String` - _alias_, name of the key.
- `String` - _password_, password of the key.
- `String` - _language_, mnemonic language of the key.

Optional:

- `String` - _mnemonic_, mnemonic of the key, create key by specified mnemonic.

**Returns**

`Object`:

- `String` - _alias_, name of the key.
- `String` - _xpub_, root pubkey of the key.
- `String` - _file_, path to the file of key.

Optional:

- `String` - _mnemonic_, mnemonic of the key, exist when the request mnemonic is null.

**Example**

create key by random pattern:

```javascript
// Request
curl -X POST create-key -d '{"alias": "alice", "password": "123456", "language": "en"}'

// Result
{
	"status": "success",
	"data": {
		"alias": "alice",
		"xpub": "c4afb96f600dc7da388b77107ceb471f604aadf49e6d1ec745abf9ae797e69a2a1f113e2cb2541166609ba725dea4072e54376ed90bcbdd0200853191a2f560a",
		"file": "/home/ec2-user/vapor_test/keystore/UTC--2019-06-18T11-04-34.512032731Z--66942e00-1466-45ea-b9f1-32ea30000017",
		"mnemonic": "attend build fog oak awful make diesel episode glove mind fire sleep"
	}
}
```

create key by specified mnemonic:

```javascript
// Request
curl -X POST create-key -d '{"alias":"jack", "password":"123456", "mnemonic":"please observe raw beauty blue sea believe then boat float beyond position", "language":"en"}'

// Result
{
	"status": "success",
	"data": {
		"alias": "jack",
		"xpub": "c7bcb65febd31c6d900bc84c386d95c3d5b047090628d9bf5c51a848945b6986e99ff70388018a7681fa37a240dbd8df39a994c86f9314a61e75feb33563ca72",
		"file": "/home/ec2-user/vapor_test/keystore/UTC--2019-06-18T11-10-02.390062724Z--3e5a16a3-93bf-4c81-aec1-c4279458a605"
	}
}
```

---

#### `list-keys`

Returns the list of all available keys.

**Parameters**

none

**Returns**

- `Array of Object`, keys owned by the client.
  - `Object`:
    - `String` - _alias_, name of the key.
    - `String` - _xpub_, pubkey of the key.
    
**Example**

```javascript
// Request
curl -X POST list-keys

// Result
{
	"status": "success",
	"data": [{
		"alias": "aa",
		"xpub": "60c9998810e077c9045e71e8acd0c639329cdf9dea73333ba105dd6205b88681ea1a3922375b89a734818c7edf3a233496937b258c7aa119e2f810f9126ec4c4",
		"file": "/home/ec2-user/vapor_test/keystore/UTC--2019-06-18T08-09-25.159209524Z--d48c110f-bfa3-4269-adba-1a6fbf325671"
	}, {
		"alias": "alice",
		"xpub": "c4afb96f600dc7da388b77107ceb471f604aadf49e6d1ec745abf9ae797e69a2a1f113e2cb2541166609ba725dea4072e54376ed90bcbdd0200853191a2f560a",
		"file": "/home/ec2-user/vapor_test/keystore/UTC--2019-06-18T11-04-34.512032731Z--66942e00-1466-45ea-b9f1-32ea30000017"
	}, {
		"alias": "jack",
		"xpub": "c7bcb65febd31c6d900bc84c386d95c3d5b047090628d9bf5c51a848945b6986e99ff70388018a7681fa37a240dbd8df39a994c86f9314a61e75feb33563ca72",
		"file": "/home/ec2-user/vapor_test/keystore/UTC--2019-06-18T11-10-02.390062724Z--3e5a16a3-93bf-4c81-aec1-c4279458a605"
	}]
}
```

---

#### `update-key-alias`

Update alias for the existed key.

**Parameters**

`Object`:

- `String` - _xpub_, pubkey of the key.
- `String` - _new_alias_, new alias of the key.

**Returns**

none if the key alias is updated successfully.

**Example**

```javascript
// Request
curl -X POST update-key-alias -d '{"xpub": "a7dae957c2d35b42efe7e6871cf5a75ebd2a0d0e51caffe767db42d3e6d69dbe211d1ca492ecf05908fe6fa625ad61b3253375ea744c9442dd5551613ba50aea", "new_alias": "new_key"}'

// Result
{
	"status": "success"
}
```

---

#### `delete-key`

Delete existed key, please make sure that there is no balance in the related accounts.

**Parameters**

`Object`:

- `String` - _xpub_, pubkey of the key.
- `String` - _password_, password of the key.

**Returns**

none if the key is deleted successfully.

**Example**

```javascript
// Request
curl -X POST delete-key -d '{"xpub": "a7dae957c2d35b42efe7e6871cf5a75ebd2a0d0e51caffe767db42d3e6d69dbe211d1ca492ecf05908fe6fa625ad61b3253375ea744c9442dd5551613ba50aea", "password": "123456"}'

// Result
{
	"status": "success"
}
```

---

#### `check-key-password`

Check key password.

**Parameters**

`Object`:

- `String` - _xpub_, pubkey of the key.
- `String` - _password_, password of the key.

**Returns**

`Object`:

- `Boolean` - _check_result_, result of check key password, true is check successfully, otherwise is false.

**Example**

```javascript
// Request
curl -X POST check-key-password -d '{"xpub": "a7dae957c2d35b42efe7e6871cf5a75ebd2a0d0e51caffe767db42d3e6d69dbe211d1ca492ecf05908fe6fa625ad61b3253375ea744c9442dd5551613ba50aea", "password": "123456"}'

// Result
{
	"status": "success",
	"data": {
		"check_result": false
	}
}
```

---

#### `reset-key-password`

Reset key password.

**Parameters**

`Object`:

- `String` - _xpub_, pubkey of the key.
- `String` - _old_password_, old password of the key.
- `String` - _new_password_, new password of the key.

**Returns**

`Object`:

- `Boolean` - _changed_, result of reset key password, true is reset successfully, otherwise is false.

**Example**

```javascript
// Request
curl -X POST reset-key-password -d '{"xpub": "a7dae957c2d35b42efe7e6871cf5a75ebd2a0d0e51caffe767db42d3e6d69dbe211d1ca492ecf05908fe6fa625ad61b3253375ea744c9442dd5551613ba50aea", "old_password": "123456", "new_password": "654321"}'

// Result
{
	"status": "success",
	"data": {
		"changed": false
	}
}
```

---

#### `create-account`

Create account to manage addresses. single sign account contain only one root_xpubs and quorum; however multi sign account contain many number of root_xpubs and quorum, quorum is the number of verify signature, the range is [1, len(root_xpubs)].

**Parameters**

`Object`:

- `Array of String` - _root_xpubs_, pubkey array.
- `String` - _alias_, name of the account.
- `Integer` - _quorum_, the default value is `1`, threshold of keys that must sign a transaction to spend asset units controlled by the account.

Optional:

- `String` - _access_token_, if optional when creating account locally. However, if you want to create account remotely, it's indispensable.

**Returns**

`Object`:

- `String` - _id_, account id.
- `String` - _alias_, name of account.
- `Integer` - _key_index_, key index of account.
- `Integer` - _quorom_, threshold of keys that must sign a transaction to spend asset units controlled by the account.
- `Array of Object` - _xpubs_, pubkey array.

**Example**

```javascript
// Request
curl -X POST create-account -d '{"root_xpubs":["2d6c07cb1ff7800b0793e300cd62b6ec5c0943d308799427615be451ef09c0304bee5dd492c6b13aaa854d303dc4f1dcb229f9578786e19c52d860803efa3b9a"],"quorum":1,"alias":"alice"}'

// Result
{
	"status": "success",
	"data": {
		"id": "0TT3D52O00A04",
		"alias": "alice",
		"xpubs": ["c4afb96f600dc7da388b77107ceb471f604aadf49e6d1ec745abf9ae797e69a2a1f113e2cb2541166609ba725dea4072e54376ed90bcbdd0200853191a2f560a"],
		"quorum": 1,
		"key_index": 1,
		"derive_rule": 1
	}
}
```

---

#### `list-accounts`

Returns the list of all available accounts.

**Parameters**

Optional:

- `String` - _id_, account id.
- `String` - _alias_, name of account.

**Returns**

- `Array of Object`, account array.
  - `Object`:
    - `String` - _id_, account id.
    - `String` - _alias_, name of account.
    - `Integer` - _key_index_, key index of account.
    - `Integer` - _quorom_, threshold of keys that must sign a transaction to spend asset units controlled by the account.
    - `Array of Object` - _xpubs_, pubkey array.

**Example**

list all the available accounts:

```javascript
// Request
curl -X POST list-accounts -d '{"alias":"alice"}'

// Result
{
	"status": "success",
	"data": [{
		"id": "0TT3D52O00A04",
		"alias": "alice",
		"xpubs": ["c4afb96f600dc7da388b77107ceb471f604aadf49e6d1ec745abf9ae797e69a2a1f113e2cb2541166609ba725dea4072e54376ed90bcbdd0200853191a2f560a"],
		"quorum": 1,
		"key_index": 1,
		"derive_rule": 1
	}]
}
```

---

#### `update-account-alias`

Update alias for the existed account.

**Parameters**

`Object`: account_alias | account_id

- `String` - _new_alias_, new alias of account.

optional:

- `String` - _account_alias_, alias of account.
- `String` - _account_id_, id of account.

**Returns**

none if the account alias is updated successfully.

**Example**

```javascript
// Request
curl -X POST update-account-alias -d '{"account_id": "0TT3D52O00A04", "new_alias": "new_account"}'
or
curl -X POST update-account-alias -d '{"account_alias": "alice", "new_alias": "new_account"}'

// Result
{
	"status": "success"
}
```

---

#### `delete-account`

Delete existed account, please make sure that there is no balance in the related addresses.

**Parameters**

`Object`: account_alias | account_id

optional:

- `String` - _account_alias_, alias of account.
- `String` - _account_id_, id of account.

**Returns**

none if the account is deleted successfully.

**Example**

```javascript
// Request
curl -X POST delete-account -d '{"account_id": "0TT3D52O00A04"}'
or
curl -X POST delete-account -d '{"account_alias": "alice"}'

// Result
{
	"status": "success"
}
```

---

#### `create-account-receiver`

create address and control program, the address and control program is are one to one relationship. in build-transaction API, receiver is address when action type is control_address, and receiver is control program when action type is control_program, they are the same result.

**Parameters**

`Object`: account_alias | account_id

optional:

- `String` - _account_alias_, alias of account.
- `String` - _account_id_, id of account.

**Returns**

`Object`:

- `String` - _address_, address of account.
- `String` - _control_program_, control program of account.

**Example**

//Request

```bash
curl -X POST create-account-receiver -d '{"account_alias": "alice", "account_id": "0TT3N98KG0A06"}'
```

// Result

```json
{
	"status": "success",
	"data": {
		"control_program": "00147a83b9a234debf51fd351673884319c8c62d8ee7",
		"address": "vp1q02pmng35m6l4rlf4zeecssceerrzmrh8gsjcn6"
	}
}
```

---

#### `list-addresses`

Returns the sub list of all available addresses by account.

**Parameters**

- `String`  - _account_alias_, alias of account.
- `String`  - _account_id_, id of account.
- `Integer` - _from_, the start position of first address
- `Integer` - _count_, the number of returned

**Returns**

- `Array of Object`, account address array.
  - `Object`:
    - `String` - _account_alias_, alias of account.
    - `String` - _account_id_, id of account.
    - `String` - _address_, address of account.
    - `Boolean` - _change_, whether the account address is change.

**Example**

list three addresses from first position by account_id or account_alias:

```javascript
// Request
curl -X POST list-addresses -d '{"account_alias": "alice", "account_id": "0TT3N98KG0A06", "from": 0, "count": 3}'

// Result
{
	"status": "success",
	"data": [{
		"account_alias": "alice",
		"account_id": "0TT3N98KG0A06",
		"address": "vp1q02pmng35m6l4rlf4zeecssceerrzmrh8gsjcn6",
		"control_program": "00147a83b9a234debf51fd351673884319c8c62d8ee7",
		"change": false,
		"key_index": 1
	}]
}
```

---

#### `validate-address`

Verify the address is valid, and judge the address is own.

**Parameters**

`Object`:

- `string` - _address_, address of account.

**Returns**

`Object`:

- `Boolean` - _valid_, whether the account address is valid.
- `Boolean` - _is_local_, whether the account address is local.

**Example**

check whether the address is valid or not.

```javascript
// Request
curl -X POST validate-address -d '{"address": "vp1q02pmng35m6l4rlf4zeecssceerrzmrh8gsjcn6"}'

// Result
{
	"status": "success",
	"data": {
		"valid": true,
		"is_local": true
	}
}
```

---

#### `get-mining-address`

Query the current mining address.

**Parameters**

none

**Returns**

`Object`:

- `String` - _mining_address_, the current mining address being used.

**Example**

```javascript
// Request
curl -X POST get-mining-address

// Result
{
	"status": "success",
	"data": {
		"mining_address": "vp1qcplus0ejvtuzk6s4jf29l3jkvx2pu49cgnr9yy"
	}
}
```

---

#### `set-mining-address`

Set the current mining address, no matter whethere the address is a local one. It returns an error message if the address format is incorrect.

**Parameters**

`Object`:

- `String` - _mining_address_, mining address to set.

**Returns**

`Object`:

- `String` - _mining_address_, the new mining address.

**Example**

```javascript
// Request
curl -X POST set-mining-address -d '{"mining_address":"bm1qnhr65jq3q9gf8uymza8vp0ew8tfyh642wddxh6"}'


// Result
{
	"status": "success",
	"data": {
		"mining_address": "vp1qcplus0ejvtuzk6s4jf29l3jkvx2pu49cgnr9yy"
	}
}
```

---

#### `get-coinbase-arbitrary`

Get coinbase arbitrary.

**Parameters**

none

**Returns**

`Object`:

- `String` - _arbitrary_, the abitrary data append to coinbase, in hexdecimal format. (The full coinbase data for a block will be `0x00`&block_height&abitrary.)

**Example**

```javascript
// Request
curl -X POST get-coinbase-arbitrary

// Result
{
	"status": "success",
	"data": {
		"arbitrary": ""
	}
}
```

---

#### `set-coinbase-arbitrary`

Set coinbase arbitrary.

**Parameters**

`Object`:

- `String` - _arbitrary_, the abitrary data to be appended to coinbase, in hexdecimal format.

**Returns**

`Object`:

- `String` - _arbitrary_, the abitrary data being appended to coinbase, in hexdecimal format. (The full coinbase data for a block will be `0x00`&block_height&abitrary.)

**Example**

```javascript
// Request
curl -X POST set-coinbase-arbitrary -d '{"arbitrary":"ff"}'

// Result
{
	"status": "success",
	"data": {
		"arbitrary": "ff"
	}
}
```

---

#### `list-pubkeys`

Returns the list of all available pubkeys by account.

**Parameters**

`Object`: account_alias | account_id

optional:

- `String` - _account_alias_, alias of account.
- `String` - _account_id_, id of account.
- `string` - _public_key_, public key.

**Returns**

`Object`:

- `string` - _root_xpub_, root xpub.
- `Array of Object` -_pubkey_infos_, public key array.
  - `String` - _pubkey_, public key.
  - `Object` - _derivation_path_, derivated path for root xpub.

**Example**

```javascript
// Request
curl -X POST list-pubkeys -d '{"account_id": "0TT3N98KG0A06"}'

// Result
{
	"status": "success",
	"data": {
		"root_xpub": "2d6c07cb1ff7800b0793e300cd62b6ec5c0943d308799427615be451ef09c0304bee5dd492c6b13aaa854d303dc4f1dcb229f9578786e19c52d860803efa3b9a",
		"pubkey_infos": [{
			"pubkey": "2612d7999c73edb80bd8b484f280b9c3a08961aaa8566085d42ed4ba538b73f6",
			"derivation_path": ["2c000000", "99000000", "01000000", "00000000", "01000000"]
		}]
	}
}
```

---

#### `list-balances`

Returns the list of all available account balances.

**Parameters**

Optional:

- `String` - _account_id_, account id.
- `String` - _account_alias_, name of account.

**Returns**

- `Array of Object`, balances owned by the account.
  - `Object`:
    - `String` - _account_id_, account id.
    - `String` - _account_alias_, name of account.
    - `String` - _asset_id_, asset id.
    - `String` - _asset_alias_, name of asset.
    - `Integer` - _amount_, specified asset balance of account.

**Example**

list all the available account balances.

```javascript
// Request
curl -X POST list-balances -d {}

// Result
{
	"status": "success",
	"data": [{
		"account_id": "0TSPP7RNG0A02",
		"account_alias": "aa",
		"asset_alias": "BTM",
		"asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
		"amount": 53280000000,
		"asset_definition": {
			"decimals": 8,
			"description": "Bytom Official Issue",
			"name": "BTM",
			"symbol": "BTM"
		}
	}]
}
```

---

#### `list-unspent-outputs`

Returns the sub list of all available unspent outputs for all accounts in your wallet.

**Parameters**

`Object`:

optional:

- `String` - _id_, id of unspent output.
- `Boolean` - _unconfirmed_, is include unconfirmed utxo
- `Boolean` - _smart_contract_, is contract utxo
- `Integer` - _from_, the start position of first utxo
- `Integer` - _count_, the number of returned
- `String` - _account_id_, account id.
- `String` - _account_alias_, name of account.

**Returns**

- `Array of Object`, unspent output array.
  - `Object`:
    - `String` - _account_id_, account id.
    - `String` - _account_alias_, name of account.
    - `String` - _asset_id_, asset id.
    - `String` - _asset_alias_, name of asset.
    - `Integer` - _amount_, specified asset balance of account.
    - `String` - _address_, address of account.
    - `Boolean` - _change_, whether the account address is change.
    - `String` - _id_, unspent output id.
    - `String` - _program_, program of account.
    - `String` - _control_program_index_, index of program.
    - `String` - _source_id_, source unspent output id.
    - `String` - _source_pos_, position of source unspent output id in block.
    - `String` - _valid_height_, valid height.

**Example**

list all the available unspent outputs:

```javascript
// Request
curl -X POST list-unspent-outputs -d {}

// Result
{
	"status": "success",
	"data": [{
				"account_alias": "aa",
				"id": "fff305631b7f126b5ef2fc280ac6eeab3013cd53386b0cf5efd3259732b69b2d",
				"asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
				"asset_alias": "BTM",
				"amount": 15000000,
				"account_id": "0TSPP7RNG0A02",
				"address": "vp1qcplus0ejvtuzk6s4jf29l3jkvx2pu49cgnr9yy",
				"control_program_index": 1,
				"program": "0014c07fc83f3262f82b6a1592545fc65661941e54b8",
				"source_id": "be6be5cf4f129e100880da62852d70c03af5c2e75aaccc8eb93fc5da8a3aee59",
				"source_pos": 0,
				"valid_height": 19973,
				"change": false,
				"derive_rule": 0
			}, {
				"account_alias": "aa",
				"id": "ffdc7585f24edcf485e3a6f2d5a6fb3b12bc7b0f2d22f0f83ec726212676c0a9",
				"asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
				"asset_alias": "BTM",
				"amount": 15000000,
				"account_id": "0TSPP7RNG0A02",
				"address": "vp1qcplus0ejvtuzk6s4jf29l3jkvx2pu49cgnr9yy",
				"control_program_index": 1,
				"program": "0014c07fc83f3262f82b6a1592545fc65661941e54b8",
				"source_id": "6c87a7d547dbc8b8daa50d6f09553ad6ad6e384ee8de92affc53b7735093bdff",
				"source_pos": 0,
				"valid_height": 7138,
				"change": false,
				"derive_rule": 0
			}
  ]
}
```

list the unspent output matching the given id:

```javascript
// Request
curl -X POST list-unspent-outputs -d '{"id": "58f29f0f85f7bd2a91088bcbe536dee41cd0642dfb1480d3a88589bdbfd642d9"}'

// Result
{
	"status": "success",
	"data": [{
		"account_alias": "aa",
		"id": "fff305631b7f126b5ef2fc280ac6eeab3013cd53386b0cf5efd3259732b69b2d",
		"asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
		"asset_alias": "BTM",
		"amount": 15000000,
		"account_id": "0TSPP7RNG0A02",
		"address": "vp1qcplus0ejvtuzk6s4jf29l3jkvx2pu49cgnr9yy",
		"control_program_index": 1,
		"program": "0014c07fc83f3262f82b6a1592545fc65661941e54b8",
		"source_id": "be6be5cf4f129e100880da62852d70c03af5c2e75aaccc8eb93fc5da8a3aee59",
		"source_pos": 0,
		"valid_height": 19973,
		"change": false,
		"derive_rule": 0
	}]
}
```

---

#### `rescan-wallet`

Trigger to rescan block information into related wallet.

**Parameters**

none

**Returns**

none if restore wallet success.

**Example**

```javascript
// Request
curl -X POST rescan-wallet -d '{}'

// Result
{
	"status": "success"
}
```

---

#### `recovery-wallet`

Recovery wallet and accounts from root XPubs.
All accounts and balances of bip44 multi-account hierarchy for deterministic wallets can be restored via root xpubs.

**Parameters**

`Object`:

- `Object` - _xpubs_, root XPubs.

**Returns**

Status of recovery wallet.

**Example**

```javascript
// Request
curl -X POST recovery-wallet -d '{ "xpubs":["c536a2c11fafd8278e02e9393dcbf5aa420eb51a1761a7e5da7f2b9b37969b52a8f8e2b692e7dcaf79dfa0d1e28c63eb9fda42942f20feaa8a71b383d9a4668c"]}'

// Result
{
    "status": "success"
}
```

---

#### `wallet-info`

Return the information of wallet.

**Parameters**

none

**Returns**

`Object`:

- `Integer` - _best_block_height_, current block height.
- `Integer` - _wallet_height_, current block height for wallet.

**Example**

```javascript
// Request
curl -X POST wallet-info -d '{}'

// Result
{
	"status": "success",
	"data": {
		"best_block_height": 40914,
		"wallet_height": 40914
	}
}
```

---

#### `sign-message`

Sign a message with the key password(decode encrypted private key) of an address.

**Parameters**

`Object`:

- `String` - _address_, address for account.
- `String` - _message_, message(hex) for signature by address xpub.
- `String` - _password_, password of account.

**Returns**

`Object`:

- `String` - _derived_xpub_, derived xpub.
- `String` - _signature_, signature of message.

**Example**

```javascript
// Request
curl -X POST sign-message -d '{"address":"vp1q7ksq32x0ye2k4a0tdwl0ua5dchy64wmgkh6rew", "message":"a121", "password":"123456"}'

// Result
{
  "signature": "f2971c232ae307645a540b7079c7a3b2503eddcc6d3513904bfe5d952550aef25976721bd8c6272cee1330d5938d6b6c982806e1bbaf83df3fd21765b51f0000",
  "derived_xpub": "065e41d327ca717afa767bc5d403854a3b663449aa1b5a470d8d415ab333da4463d4ffe9a92b335c5094d9f4905353e084cd11be17d0c3c198c4ae421b848435"
}
```

---

#### `decode-program`

Decode program.

**Parameters**

`Object`:

- `String` - _program_, program for account.

**Returns**

`Object`:

- `String` - _instructions_, instructions and data for program.

**Example**

```javascript
// Request
curl -X POST decode-program -d '{"program":"0014c07fc83f3262f82b6a1592545fc65661941e54b8"}'

// Result
{
	"status": "success",
	"data": {
		"instructions": "DUP \nHASH160 \nDATA_20 c07fc83f3262f82b6a1592545fc65661941e54b8\nEQUALVERIFY \nTXSIGHASH \nSWAP \nCHECKSIG \n"
	}
}
```

---

#### `get-transaction`

Query the account related transaction by transaction ID.

**Parameters**

`Object`:

- `String` - _tx_id_, transaction id, hash of transaction.

**Returns**

`Object`:

- `String` - _tx_id_, transaction id, hash of the transaction.
- `Integer` - _block_time_, the unix timestamp for when the requst was responsed.
- `String` - _block_hash_, hash of the block where this transaction was in.
- `Integer` - _block_height_, block height where this transaction was in.
- `Integer` - _block_index_, position of the transaction in the block.
- `Integer` - _block_transactions_count_, transactions count where this transaction was in the block.
- `Boolean` - _status_fail_, whether the state of the transaction request has failed.
- `Integer` - _size_, size of transaction.
- `Array of Object` - _inputs_, object of inputs for the transaction.
  - `String` - _type_, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
  - `String` - _asset_id_, asset id.
  - `String` - _asset_alias_, name of asset.
  - `Object` - _asset_definition_, definition of asset(json object).
  - `Integer` - _amount_, amount of asset.
  - `Object` - _issuance_program_, issuance program, it only exist when type is 'issue'.
  - `Object` - _control_program_, control program of account, it only exist when type is 'spend'.
  - `String` - _address_, address of account, it only exist when type is 'spend'.
  - `String` - _spent_output_id_, the front of outputID to be spent in this input, it only exist when type is 'spend'.
  - `String` - _account_id_, account id.
  - `String` - _account_alias_, name of account.
  - `Object` - _arbitrary_, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
  - `String` - _input_id_, hash of input action.
  - `Array of String` - _witness_arguments_, witness arguments.
- `Array of Object` - _outputs_, object of outputs for the transaction.
  - `String` - _type_, the type of output action, available option include: 'retire', 'control'.
  - `String` - _id_, outputid related to utxo.
  - `Integer` - _position_, position of outputs.
  - `String` - _asset_id_, asset id.
  - `String` - _asset_alias_, name of asset.
  - `Object` - _asset_definition_, definition of asset(json object).
  - `Integer` - _amount_, amount of asset.
  - `String` - _account_id_, account id.
  - `String` - _account_alias_, name of account.
  - `Object` - _control_program_, control program of account.
  - `String` - _address_, address of account.

**Example**

```javascript
// Request
curl -X POST get-transaction -d '{"tx_id": "7798bcb0ae40b3e9ba2e88e47f7d7e67e90bb77bc46e3c1d2f42a34b264a6e9c"}'

// Result
{
	"status": "success",
	"data": {
		"tx_id": "7798bcb0ae40b3e9ba2e88e47f7d7e67e90bb77bc46e3c1d2f42a34b264a6e9c",
		"block_time": 1560870000500,
		"block_hash": "30162352944c61c1e3d34437405c78f5313f802bd9ed50da3f1f4d4f8d114a41",
		"block_height": 42324,
		"block_index": 0,
		"block_transactions_count": 1,
		"inputs": [{
			"type": "coinbase",
			"asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
			"asset_definition": null,
			"amount": 0,
			"arbitrary": "003432333234ff",
			"input_id": "d5c1933282a5f506c5d9ddb31799c9ebab0997a2e03f2e7fc1692e3036907e08",
			"witness_arguments": null
		}],
		"outputs": [{
			"type": "control",
			"id": "6cdb5c36ac200247afa024552bb26495ad4dc7eeb7483ed6dd2a8ee30346c61a",
			"position": 0,
			"asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
			"asset_alias": "BTM",
			"asset_definition": {
				"decimals": 8,
				"description": "Bytom Official Issue",
				"name": "BTM",
				"symbol": "BTM"
			},
			"amount": 15000000,
			"account_id": "0TSPP7RNG0A02",
			"account_alias": "aa",
			"control_program": "0014c07fc83f3262f82b6a1592545fc65661941e54b8",
			"address": "vp1qcplus0ejvtuzk6s4jf29l3jkvx2pu49cgnr9yy"
		}],
		"status_fail": false,
		"size": 82
	}
}
```

---

#### `list-transactions`

**list cross-chain transactions**

To list cross-chain transactions, `list-transactions` needs to be called by a vapord node.

If a transaction contains a `cross_chain_in` input or a `cross_chain_out` output, it is recognized as a cross-chain transaction.

**Parameters**

`Object`:

optional:

- `String` - _id_, transaction id, hash of transaction.
- `String` - _account_id_, id of account.
- `Boolean` - _detail_ , flag of detail transactions, default false (only return transaction summary)
- `Boolean` - _unconfirmed_, flag of unconfirmed transactions(query result include all confirmed and unconfirmed transactions), default false.
- `Integer` - _from_, the start position of first transaction
- `Integer` - _count_, the number of returned

**Returns**

`Array of Object`, transaction array.

optional:

- `Object`:(summary transaction)
  - `String` - _tx_id_, transaction id, hash of the transaction.
  - `Integer` - _block_time_, the unix timestamp for when the requst was responsed.
  - `Array of Object` - _inputs_, object of summary inputs for the transaction.
    - `String` - _type_, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - _asset_id_, asset id.
    - `String` - _asset_alias_, name of asset.
    - `Integer` - _amount_, amount of asset.
    - `String` - _account_id_, account id.
    - `String` - _account_alias_, name of account.
    - `Object` - _arbitrary_, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
  - `Array of Object` - _outputs_, object of summary outputs for the transaction.
    - `String` - _type_, the type of output action, available option include: 'retire', 'control'.
    - `String` - _asset_id_, asset id.
    - `String` - _asset_alias_, name of asset.
    - `Integer` - _amount_, amount of asset.
    - `String` - _account_id_, account id.
    - `String` - _account_alias_, name of account.
    - `Object` - _arbitrary_, arbitrary infomation can be set by miner, it only exist when type is input 'coinbase'(this place is empty).
- `Object`:(detail transaction)
  - `String` - _tx_id_, transaction id, hash of the transaction.
  - `Integer` - _block_time_, the unix timestamp for when the requst was responsed.
  - `String` - _block_hash_, hash of the block where this transaction was in.
  - `Integer` - _block_height_, block height where this transaction was in.
  - `Integer` - _block_index_, position of the transaction in the block.
  - `Integer` - _block_transactions_count_, transactions count where this transaction was in the block.
  - `Boolean` - _status_fail_, whether the state of the transaction request has failed.
  - `Integer` - _size_, size of transaction.
  - `Array of Object` - _inputs_, object of inputs for the transaction.
    - `String` - _type_, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - _asset_id_, asset id.
    - `String` - _asset_alias_, name of asset.
    - `Object` - _asset_definition_, definition of asset(json object).
    - `Integer` - _amount_, amount of asset.
    - `Object` - _issuance_program_, issuance program, it only exist when type is 'issue'.
    - `Object` - _control_program_, control program of account, it only exist when type is 'spend'.
    - `String` - _address_, address of account, it only exist when type is 'spend'.
    - `String` - _spent_output_id_, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - _account_id_, account id.
    - `String` - _account_alias_, name of account.
    - `Object` - _arbitrary_, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - _input_id_, hash of input action.
    - `Array of String` - _witness_arguments_, witness arguments.
  - `Array of Object` - _outputs_, object of outputs for the transaction.
    - `String` - _type_, the type of output action, available option include: 'retire', 'control'.
    - `String` - _id_, outputid related to utxo.
    - `Integer` - _position_, position of outputs.
    - `String` - _asset_id_, asset id.
    - `String` - _asset_alias_, name of asset.
    - `Object` - _asset_definition_, definition of asset(json object).
    - `Integer` - _amount_, amount of asset.
    - `String` - _account_id_, account id.
    - `String` - _account_alias_, name of account.
    - `Object` - _control_program_, control program of account.
    - `String` - _address_, address of account.

**Example**

list all the available transactions:

```javascript
// Request
curl -X POST list-transactions -d {}

// Result
{
	"status": "success",
	"data": [{
				"tx_id": "7798bcb0ae40b3e9ba2e88e47f7d7e67e90bb77bc46e3c1d2f42a34b264a6e9c",
				"block_time": 1560870000500,
				"inputs": [{
					"type": "coinbase",
					"asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
					"arbitrary": "003432333234ff"
				}],
				"outputs": [{
					"type": "control",
					"asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
					"asset_alias": "BTM",
					"amount": 15000000,
					"account_id": "0TSPP7RNG0A02",
					"account_alias": "aa"
				}]
			}, {
				"tx_id": "22b097a28e4b3ea25d273d92982d5c1ebb8ff4a73a77b94568e374312f4c0026",
				"block_time": 1560870000000,
				"inputs": [{
					"type": "coinbase",
					"asset_id": "0000000000000000000000000000000000000000000000000000000000000000",
					"arbitrary": "003432333233ff"
				}],
				"outputs": [{
					"type": "control",
					"asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
					"asset_alias": "BTM",
					"amount": 15000000,
					"account_id": "0TSPP7RNG0A02",
					"account_alias": "aa"
				}]
			}
}
```

---

#### `build-transaction`

**mainchain(bytom) to sidechain(vapor)**

To build a mainchain-to-sidechain transaction, `build-transaction` is called by one or more federation members, using `cross_chain_in` action.

**Parameters**

`Object`:

- `String` - _base_transaction_, base data for the transaction, default is null.
- `Arrary of Object` - _actions_:
  - `Object`:
    - `String` - _asset_id_ | _asset_alias_, (type is cross_chain_in, control_program and control_address) alias or ID of asset.
    - `Integer` - _amount_, (type is cross_chain_in, control_program and control_address) the specified asset of the amount sent with this transaction.
    - `String`- _type_, type of transaction, valid types: 'cross_chain_in', 'control_address', 'control_program'.
    - `String` - _address_, (type is control_address) address of receiver, the style of address is P2PKH or P2SH.
    - `String` - _control_program_, (type is control_program) control program of receiver.
    - Integer   - _vm_version_, (type is cross_chain_in) asset vm_version.
    - `String` - _issuance_program_, (type is cross_chain_in) asset issuance_program hexdecimal string.
    - `String` - _raw_definition_byte_, (type is cross_chain_in) asset raw_definition_byte hexdecimal string.
    - `String` - _source_id_, (type is cross_chain_in) mainchain output mux id.
    - `Integer` - _source_pos_, (type is cross_chain_in) mainchain output source position.

**Returns**

- `Object of build-transaction` - _transaction_, builded transaction.

**Example**

```javascript
// Request
curl -X POST build-transaction -d '{
    "base_transaction":null,
    "actions":[
        {
            "asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "amount":20000000,
            "source_id":"d5156f4477fcb694388e6aed7ca390e5bc81bb725ce7461caa241777c1f62236",
            "source_pos":3,
            "type":"cross_chain_in"
        },
        {
            "amount":20000000,
            "asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "address":"vp1qfk5rudmkfny6x5enzxzj0qks3ce9dvgmyg4d2h",
            "type":"control_address"
        }
    ]
}'
```

```javascript
// Result
{
  "status":"success",
  "data":{
    "raw_transaction":"07010001019001008d01d5156f4477fcb694388e6aed7ca390e5bc81bb725ce7461caa241777c1f62236ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80dac409030146ae2071dcacf8495651f205858a3a4b64c4d4bb24f382ff517b558c8e0acaac5819652039579ddd54e667057e175032683c1152578c9b05b5ecbc11b3f500b4263e41885152ad01000001013e003cffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80ade204011600144da83e37764cc9a3533311852782d08e3256b11b00",
    "signing_instructions":[
      {
        "position":0,
        "witness_components":[
          {
            "type":"raw_tx_signature",
            "quorum":1,
            "keys":[
              {
                "xpub":"71dcacf8495651f205858a3a4b64c4d4bb24f382ff517b558c8e0acaac5819650abcd618c10058f867fce07b0455cdb34c273b2bf8fc7ee9e3c70706d5bba8f4",
                "derivation_path":[

                ]
              },
              {
                "xpub":"39579ddd54e667057e175032683c1152578c9b05b5ecbc11b3f500b4263e418868da2376963dbbfb18a0d8aac64ea0d97e7c82061c58e98e22d4d1c7f5f5809d",
                "derivation_path":[

                ]
              }
            ],
            "signatures":null
          }
        ]
      }
    ],
    "fee":10000000,
    "allow_additional_actions":false
  }
}
```

## sidechain(vapor) to mainchain(bytom)

To build a sidechain-to-mainchain transaction, `build-transaction` is called by a vapor user, using `cross_chain_out` action.

**Parameters**

`Object`:

- `String` - _base_transaction_, base data for the transaction, default is null.
- `Integer` - _ttl_, integer of the time to live in milliseconds, it means utxo will be reserved(locked) for builded transaction in this time range, if the transaction will not to be submitted into block, it will be auto unlocked for build transaction again after this ttl time. it will be set to 5 minutes(300 seconds) defaultly when ttl is 0.
- `Integer` - _time_range_, the block height at which this transaction will be allowed to be included in a block. If the block height of the main chain exceeds this value, the transaction will expire and no longer be valid.
- `Arrary of Object` - _actions_:
  - `Object`:
    - `String` - _account_id_ | _account_alias_, (type is spend_account) alias or ID of account.
    - `String` - _asset_id_ | _asset_alias_, (type is spend_account, cross_chain_out) alias or ID of asset.
    - `Integer` - _amount_, (type is spend_account, cross_chain_out) the specified asset of the amount sent with this transaction.
    - `String`- _type_, type of transaction, valid types: 'spend_account', 'spend_account_unspent_output', 'cross_chain_out', 'control_program'.
    - `String` - _address_, (type is cross_chain_out) address of receiver, the style of address is P2PKH or P2SH.
    - `String` - _use_unconfirmed_, (type is spend_account and spend_account_unspent_output) flag of use unconfirmed UTXO, default is false.

**Returns**

- `Object of build-transaction` - _transaction_, builded transaction.

**Example**

```javascript
// Request
curl -X POST build-transaction -d '{
    "base_transaction":null,
    "actions":[
        {
            "amount":9000000,
            "asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "account_id":"0TAOSK9J00A02",
            "type":"spend_account",
            "use_unconfirmed":true
        },
        {
            "amount":4000000,
            "asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "address":"bm1q3yt265592czgh96r0uz63ta8fq40uzu5a8c2h0",
            "type":"cross_chain_out"
        }
    ]
}'
```

```javascript
// Result
{
  "status":"success",
  "data":{
    "raw_transaction":"07010001015f015dcca6be7f2b17d0695ed2ae5497f4711788e355ff1c3a93401c8e035a1f84a7b8ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80ade20400011600144da83e37764cc9a3533311852782d08e3256b11b010002013d003bffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffc0843d011600146e544784f94bc9b6b608b561075c03804319ed4f00013e013cffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8092f401011600148916ad528556048b97437f05a8afa7482afe0b9400",
    "signing_instructions":[
      {
        "position":0,
        "witness_components":[
          {
            "type":"raw_tx_signature",
            "quorum":1,
            "keys":[
              {
                "xpub":"71dcacf8495651f205858a3a4b64c4d4bb24f382ff517b558c8e0acaac5819650abcd618c10058f867fce07b0455cdb34c273b2bf8fc7ee9e3c70706d5bba8f4",
                "derivation_path":[
                  "2c000000",
                  "99000000",
                  "01000000",
                  "00000000",
                  "01000000"
                ]
              }
            ],
            "signatures":null
          },
          {
            "type":"data",
            "value":"5b4c19722110ea7c1b5ace5d63393d401ef0797a605568a88a8fc4cdfcf293b6"
          }
        ]
      }
    ],
    "fee":5000000,
    "allow_additional_actions":false
  }
}
```

---

#### vote and veto

Build vote and veto transaction.

- vote is the user voting for the node
- veto is the user who cancels the vote to the node

**Parameters**

`Object`:

- `String` - _base_transaction_, base data for the transaction, default is null.
- `Integer` - _ttl_, integer of the time to live in milliseconds, it means utxo will be reserved(locked) for builded transaction in this time range, if the transaction will not to be submitted into block, it will be auto unlocked for build transaction again after this ttl time. it will be set to 5 minutes(300 seconds) defaultly when ttl is 0.
- `Integer` - _time_range_, the block height at which this transaction will be allowed to be included in a block. If the block height of the main chain exceeds this value, the transaction will expire and no longer be valid.
- `Arrary of Object` - _actions_:
  - `Object`:
    - `String` - _account_id_ | _account_alias_, (type is spend_account) alias or ID of account.
    - `String` - _asset_id_ | _asset_alias_, (type is spend_account, vote_output, veto, control_program and control_address) alias or ID of asset.
    - `Integer` - _amount_, (type is spend_account, vote_output, veto, control_program and control_address) the specified asset of the amount sent with this transaction.
    - `String`- _type_, type of transaction, valid types: 'spend_account', 'vote_output', 'veto', 'control_address', 'control_program'.
    - `String` - _address_, (type is control_address) address of receiver, the style of address is P2PKH or P2SH.
    - `String` - _control_program_, (type is control_program) control program of receiver.

**Returns**

- `Object of build-transaction` - _transaction_, builded transaction.

**Example**

- vote-  transaction type is vote
```json
// request
curl -s -X POST http://127.0.0.1:9889/build-transaction -d '{"base_transaction":null,"actions":[{"account_id":"0TORKU02G0A02","amount":20000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","type":"spend_account"},{"account_id":"0TORKU02G0A02","amount":10000000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","type":"spend_account"},{"amount":10000000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","address":"sm1qke6d5lqan33kcrl5cdphynnapzc5wpkflp5exc","vote":"af594006a40837d9f028daabb6d589df0b9138daefad5683e5233c2646279217294a8d532e60863bcf196625a35fb8ceeffa3c09610eb92dcfb655a947f13269","type":"vote_output"}],"ttl":0,"time_range": 43432}'

//respone
{
    "status": "success", 
    "data": {
        "raw_transaction": "0701a8d302010160015eace886c5033851f084b00dc2659c30d097d0c9b424a2579a4131ca272a1b15c8ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80e08bb4590001160014b674da7c1d9c636c0ff4c343724e7d08b14706c9010002013f003dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80be978a3401160014588b88307a3792c4e72f5fedf1310af16fe00ef9000180010240af594006a40837d9f028daabb6d589df0b9138daefad5683e5233c2646279217294a8d532e60863bcf196625a35fb8ceeffa3c09610eb92dcfb655a947f132693dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80c8afa02501160014b674da7c1d9c636c0ff4c343724e7d08b14706c900", 
        "signing_instructions": [
            {
                "position": 0, 
                "witness_components": [
                    {
                        "type": "raw_tx_signature", 
                        "quorum": 1, 
                        "keys": [
                            {
                                "xpub": "3bfa74f4db31fbaed0d31f55fa852d72c0d8b954b827a01800762c8a8c4dbf080600ad0b2ee9021bbae503f18748bdbd2cec3a23b3f417774f5703b59fa5dddc", 
                                "derivation_path": [
                                    "2c000000", 
                                    "99000000", 
                                    "01000000", 
                                    "00000000", 
                                    "01000000"
                                ]
                            }
                        ], 
                        "signatures": null
                    }, 
                    {
                        "type": "data", 
                        "value": "92fb2d5a164626651fb7c12fae138a7ea4b213f1811887f0175ca7cb5703e655"
                    }
                ]
            }
        ], 
        "fee": 20000000, 
        "allow_additional_actions": false
    }
}
```

- veto-  transaction type is veto
```json
// request
curl -X POST http://127.0.0.1:9889/build-transaction -d '{"base_transaction":null,"actions":[{"account_id":"0TORKU02G0A02","amount":20000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","type":"spend_account"},{"account_id":"0TORKU02G0A02","amount":10000000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","vote":"af594006a40837d9f028daabb6d589df0b9138daefad5683e5233c2646279217294a8d532e60863bcf196625a35fb8ceeffa3c09610eb92dcfb655a947f13269","type":"veto"},{"amount":10000000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","address":"sm1qke6d5lqan33kcrl5cdphynnapzc5wpkflp5exc","type":"control_address"}],"ttl":0,"time_range": 43432}'

// respone
{
    "status": "success", 
    "data": {
        "raw_transaction": "0701a8d302020160015e08a15aeeda2d57ef275da3d0a56f264884d358e252a71a7589d650497fcd3192ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80be978a340001160014588b88307a3792c4e72f5fedf1310af16fe00ef901000160035e08a15aeeda2d57ef275da3d0a56f264884d358e252a71a7589d650497fcd3192ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80c8afa0250101160014b674da7c1d9c636c0ff4c343724e7d08b14706c9420040af594006a40837d9f028daabb6d589df0b9138daefad5683e5233c2646279217294a8d532e60863bcf196625a35fb8ceeffa3c09610eb92dcfb655a947f1326902013f003dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80e4d28034011600140048871e1f4f743ae75d5bc0693565fe16d1456000013f003dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80c8afa02501160014b674da7c1d9c636c0ff4c343724e7d08b14706c900", 
        "signing_instructions": [
            {
                "position": 0, 
                "witness_components": [
                    {
                        "type": "raw_tx_signature", 
                        "quorum": 1, 
                        "keys": [
                            {
                                "xpub": "3bfa74f4db31fbaed0d31f55fa852d72c0d8b954b827a01800762c8a8c4dbf080600ad0b2ee9021bbae503f18748bdbd2cec3a23b3f417774f5703b59fa5dddc", 
                                "derivation_path": [
                                    "2c000000", 
                                    "99000000", 
                                    "01000000", 
                                    "01000000", 
                                    "01000000"
                                ]
                            }
                        ], 
                        "signatures": null
                    }, 
                    {
                        "type": "data", 
                        "value": "8bf372183cf3fc62fcf80bd10195cd4820c56ecc0578dcd0d22c18b29ffd7fec"
                    }
                ]
            }, 
            {
                "position": 1, 
                "witness_components": [
                    {
                        "type": "raw_tx_signature", 
                        "quorum": 1, 
                        "keys": [
                            {
                                "xpub": "3bfa74f4db31fbaed0d31f55fa852d72c0d8b954b827a01800762c8a8c4dbf080600ad0b2ee9021bbae503f18748bdbd2cec3a23b3f417774f5703b59fa5dddc", 
                                "derivation_path": [
                                    "2c000000", 
                                    "99000000", 
                                    "01000000", 
                                    "00000000", 
                                    "01000000"
                                ]
                            }
                        ], 
                        "signatures": null
                    }, 
                    {
                        "type": "data", 
                        "value": "92fb2d5a164626651fb7c12fae138a7ea4b213f1811887f0175ca7cb5703e655"
                    }
                ]
            }
        ], 
        "fee": 20000000, 
        "allow_additional_actions": false
    }
}
```

#### build-chain-transactions`

Build chain transactions. To solve the problem of excessive utxo causing the transaction to fail, the utxo merge will be performed automatically. Currently, only btm transactions are supported.Warning, this feature requires the mine pool bytomd software to be higher than v1.0.6.

**Parameters**

`Object`:

- `String` - _base_transaction_, base data for the transaction, default is null.
- `Integer` - _ttl_, integer of the time to live in milliseconds, it means utxo will be reserved(locked) for builded transaction in this time range, if the transaction will not to be submitted into block, it will be auto unlocked for build transaction again after this ttl time. it will be set to 5 minutes(300 seconds) defaultly when ttl is 0.
- `Integer` - _time_range_, time stamp(block height)is maximum survival time for the transaction, the transaction will be not submit into block after this time stamp.
- `Arrary of Object` - _actions_:
  - `Object`:
    - `String` - _account_id_ | _account_alias_, (type is spend_account) alias or ID of account.
    - `String` - _asset_id_ | _asset_alias_, (type is spend_account, issue, retire, control_program and control_address) alias or ID of asset.
    - `Integer` - _amount_, (type is spend_account, issue, retire, control_program and control_address) the specified asset of the amount sent with this transaction.
    - `String`- _type_, type of transaction, valid types: 'spend_account', 'issue', 'spend_account_unspent_output', 'control_address', 'control_program', 'retire'.
    - `String` - _address_, (type is control_address) address of receiver, the style of address is P2PKH or P2SH.
    - `String` - _control_program_, (type is control_program) control program of receiver.
    - `String` - _use_unconfirmed_, (type is spend_account and spend_account_unspent_output) flag of use unconfirmed UTXO, default is false.
    - `Arrary of Object` - _arguments_, (type is issue and spend_account_unspent_output) arguments of contract, null when it's not contract.
      - `String`- _type_, type of argument, valid types: 'raw_tx_signature', 'data'.
      - `Object`- _raw_data_, json object of argument content.
        - `String`- _xpub_, (type is raw_tx_signature) root xpub.
        - `String`- _derivation_path_, (type is raw_tx_signature) derived path.
        - `String`- _value_, (type is data) string of binary value.

**Returns**

- `Object of raw_transaction` - _raw_transaction_, builded transactions.
- `Object of signing_instructions` - _signing_instructions_, Information used to sign a transactions.

**Example**

- `spend` - transaction type is spend

```javascript
// Request
curl -X POST localhost:9889/build-chain-transactions -d '{"base_transaction": null,"actions":[{"account_id":"0JCH28A600A02","amount":30000500000000,"asset_id":"ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","type": "spend_account"}, {"amount": 30000490000000,"asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","address": "sm1qx93sge8jkgzclc7pled7uqr596hjm2xe558lkr","type": "control_address"}],"ttl": 1000000,"time_range": 0}'
```

```javascript
// Result
{
	"status": "success",
	"data": [{
		"raw_transaction": "0701000201620160a0d36052ca3d1335120ae48e1ffb2fb6b25588628eff90fa88bef3117dfb4301ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80ddb2c490e906010116001431630464f2b2058fe3c1fe5bee00742eaf2da8d901000161015f72de2064ab999acf22c05b5cf9c7d53164f80038b46b1ce426708514a30a3485ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80d4f4f69901000116001431630464f2b2058fe3c1fe5bee00742eaf2da8d9010001013fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8084c5b6aaea060116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900",
		"signing_instructions": [{
			"position": 0,
			"witness_components": [{
				"type": "raw_tx_signature",
				"quorum": 1,
				"keys": [{
					"xpub": "b4d084e77bcda7fd8a37e31135200b2a6af98d19018674125dc6290dd14176f92523f229d9f1f3514b461f6931ac2073f586a35cd628c90270063725e6e1e983",
					"derivation_path": ["010100000000000000", "0100000000000000"]
				}],
				"signatures": null
			}, {
				"type": "data",
				"value": "a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a8973919"
			}]
		}, {
			"position": 1,
			"witness_components": [{
				"type": "raw_tx_signature",
				"quorum": 1,
				"keys": [{
					"xpub": "b4d084e77bcda7fd8a37e31135200b2a6af98d19018674125dc6290dd14176f92523f229d9f1f3514b461f6931ac2073f586a35cd628c90270063725e6e1e983",
					"derivation_path": ["010100000000000000", "0100000000000000"]
				}],
				"signatures": null
			}, {
				"type": "data",
				"value": "a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a8973919"
			}]
		}],
		"allow_additional_actions": false
	}, {
		"raw_transaction": "0701000101620160571cc5d99a2994ff6b192bc9387838a3651245cb66dad4a6bc5f660310cebfa9ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8084c5b6aaea06000116001431630464f2b2058fe3c1fe5bee00742eaf2da8d9010002013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80faafed99010116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900013fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80ddb2c490e9060116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900",
		"signing_instructions": [{
			"position": 0,
			"witness_components": [{
				"type": "raw_tx_signature",
				"quorum": 1,
				"keys": [{
					"xpub": "b4d084e77bcda7fd8a37e31135200b2a6af98d19018674125dc6290dd14176f92523f229d9f1f3514b461f6931ac2073f586a35cd628c90270063725e6e1e983",
					"derivation_path": ["010100000000000000", "0100000000000000"]
				}],
				"signatures": null
			}, {
				"type": "data",
				"value": "a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a8973919"
			}]
		}],
		"allow_additional_actions": false
	}]
}
```

---

#### `sign-transaction`

Sign transaction.

**Parameters**

`Object`:

- `String` - _password_, signature of the password.
- `Object` - _transaction_, builded transaction.

**Returns**

`Object`:

- `Boolean` - _sign_complete_, returns true if sign succesfully and false otherwise.
- `Object of sign-transaction` - _transaction_, signed transaction.

**Example**

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

---

#### `sign-transactions`

Sign transactions used for batch signing transactions.

**Parameters**

`Object`:

- `String` - _password_, signature of the password.
- `Object` - _transaction_, builded transactions.

**Returns**

`Object`:

- `Boolean` - _sign_complete_, returns true if sign succesfully and false otherwise.
- `Object of sign-transactions` - _transaction_, signed transactions.

**Example**

```javascript
// Request
curl -X POST localhost:9889/sign-transactions -d '{"password":"123456","transactions":[{"raw_transaction":"0701000201620160a0d36052ca3d1335120ae48e1ffb2fb6b25588628eff90fa88bef3117dfb4301ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80ddb2c490e906010116001431630464f2b2058fe3c1fe5bee00742eaf2da8d901000161015f72de2064ab999acf22c05b5cf9c7d53164f80038b46b1ce426708514a30a3485ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80d4f4f69901000116001431630464f2b2058fe3c1fe5bee00742eaf2da8d9010001013fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8084c5b6aaea060116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900","signing_instructions":[{"position":0,"witness_components":[{"type":"raw_tx_signature","quorum":1,"keys":[{"xpub":"b4d084e77bcda7fd8a37e31135200b2a6af98d19018674125dc6290dd14176f92523f229d9f1f3514b461f6931ac2073f586a35cd628c90270063725e6e1e983","derivation_path":["010100000000000000","0100000000000000"]}],"signatures":null},{"type":"data","value":"a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a8973919"}]},{"position":1,"witness_components":[{"type":"raw_tx_signature","quorum":1,"keys":[{"xpub":"b4d084e77bcda7fd8a37e31135200b2a6af98d19018674125dc6290dd14176f92523f229d9f1f3514b461f6931ac2073f586a35cd628c90270063725e6e1e983","derivation_path":["010100000000000000","0100000000000000"]}],"signatures":null},{"type":"data","value":"a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a8973919"}]}],"allow_additional_actions":false},{"raw_transaction":"0701000101620160571cc5d99a2994ff6b192bc9387838a3651245cb66dad4a6bc5f660310cebfa9ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8084c5b6aaea06000116001431630464f2b2058fe3c1fe5bee00742eaf2da8d9010002013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80faafed99010116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900013fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80ddb2c490e9060116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900","signing_instructions":[{"position":0,"witness_components":[{"type":"raw_tx_signature","quorum":1,"keys":[{"xpub":"b4d084e77bcda7fd8a37e31135200b2a6af98d19018674125dc6290dd14176f92523f229d9f1f3514b461f6931ac2073f586a35cd628c90270063725e6e1e983","derivation_path":["010100000000000000","0100000000000000"]}],"signatures":null},{"type":"data","value":"a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a8973919"}]}],"allow_additional_actions":false}]}'
```

```
// Result

{
	"status": "success",
	"data": {
		"transaction": [{
				"raw_transaction": "0701000201620160a0d36052ca3d1335120ae48e1ffb2fb6b25588628eff90fa88bef3117dfb4301ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80ddb2c490e906010116001431630464f2b2058fe3c1fe5bee00742eaf2da8d9630240acb57bc06f7e5de99ef3e630ce34fc74c33d4694301202968092ca50ae7842e3331bfeb0cf7b65f383e27670c4d58aeeeb0b77e5355957ca729298d2b4e2470c20a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a89739190161015f72de2064ab999acf22c05b5cf9c7d53164f80038b46b1ce426708514a30a3485ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80d4f4f69901000116001431630464f2b2058fe3c1fe5bee00742eaf2da8d96302404298424e89e5528f1d0cdd9028489b9d9e3f031ec34a74440cacc7900dc1eac9359c408a4342fc6cef935d2978919df8b23f3912ac4419800d375fac06ddb50620a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a897391901013fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8084c5b6aaea060116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900",
				"signing_instructions": [{
						"position": 0,
						"witness_components": [{
								"type": "raw_tx_signature",
								"quorum": 1,
								"keys": [{
									"xpub": "b4d084e77bcda7fd8a37e31135200b2a6af98d19018674125dc6290dd14176f92523f229d9f1f3514b461f6931ac2073f586a35cd628c90270063725e6e1e983",
									"derivation_path": [
										"010100000000000000",
										"0100000000000000"
									]
								}],
								"signatures": [
									"acb57bc06f7e5de99ef3e630ce34fc74c33d4694301202968092ca50ae7842e3331bfeb0cf7b65f383e27670c4d58aeeeb0b77e5355957ca729298d2b4e2470c"
								]
							},
							{
								"type": "data",
								"value": "a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a8973919"
							}
						]
					},
					{
						"position": 1,
						"witness_components": [{
								"type": "raw_tx_signature",
								"quorum": 1,
								"keys": [{
									"xpub": "b4d084e77bcda7fd8a37e31135200b2a6af98d19018674125dc6290dd14176f92523f229d9f1f3514b461f6931ac2073f586a35cd628c90270063725e6e1e983",
									"derivation_path": [
										"010100000000000000",
										"0100000000000000"
									]
								}],
								"signatures": [
									"4298424e89e5528f1d0cdd9028489b9d9e3f031ec34a74440cacc7900dc1eac9359c408a4342fc6cef935d2978919df8b23f3912ac4419800d375fac06ddb506"
								]
							},
							{
								"type": "data",
								"value": "a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a8973919"
							}
						]
					}
				],
				"allow_additional_actions": false
			},
			{
				"raw_transaction": "0701000101620160571cc5d99a2994ff6b192bc9387838a3651245cb66dad4a6bc5f660310cebfa9ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8084c5b6aaea06000116001431630464f2b2058fe3c1fe5bee00742eaf2da8d96302408c742d77eba6c56a8db8c114e60be6c6263df6120aefd7538376129d04ec71b78b718c2085bba85254b44bf4600ba31d4c5a7869d0be0c46d88bd5eb27490e0820a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a897391902013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80faafed99010116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900013fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80ddb2c490e9060116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900",
				"signing_instructions": [{
					"position": 0,
					"witness_components": [{
							"type": "raw_tx_signature",
							"quorum": 1,
							"keys": [{
								"xpub": "b4d084e77bcda7fd8a37e31135200b2a6af98d19018674125dc6290dd14176f92523f229d9f1f3514b461f6931ac2073f586a35cd628c90270063725e6e1e983",
								"derivation_path": [
									"010100000000000000",
									"0100000000000000"
								]
							}],
							"signatures": [
								"8c742d77eba6c56a8db8c114e60be6c6263df6120aefd7538376129d04ec71b78b718c2085bba85254b44bf4600ba31d4c5a7869d0be0c46d88bd5eb27490e08"
							]
						},
						{
							"type": "data",
							"value": "a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a8973919"
						}
					]
				}],
				"allow_additional_actions": false
			}
		],
		"sign_complete": true
	}
}
```

---

#### `submit-transaction`

Submit transaction.

**Parameters**

`Object`:

- `Object` - _raw_transaction_, raw_transaction of signed transaction.

**Returns**

`Object`:

- `String` - _tx_id_, transaction id, hash of transaction.

**Example**

```javascript
// Request
curl -X POST submit-transaction -d '{"raw_transaction":"07010000010161015ffe8bdb49bbd08f711a54f0fbed4141b74c276de44c831999aac43bdd56f98309ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80c0b1ca94120001160014d61f9b3354771283461e6cb269bec98e7ee975f6630240403d7b35ec72a80816b21822d645adc98af2c1f07d5b379dda4527d8585ec12ef213ada312e3807ae0ccf7206575f313ffe2b405a286309d3172feabe07e7e0620d013706a314a57e84c2b262c2a291e8e2b063785aea9338476a66d41be4de4f202013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80bc82eb931201160014bdf260424fd2f322dbec9ce4ddbaed8618d8e959000139ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff6301160014a3f9111f3b0ee96cbd119a3ea5c60058f506fb1900"}'

// Result
{
  "tx_id": "2c0624a7d251c29d4d1ad14297c69919214e78d995affd57e73fbf84ece316cb"
}
```

---

#### `submit-transactions`

Submit transactions used for batch submit transactions.

**Parameters**

`Object`:

- `Object` - _raw_transactions_, raw_transactions of signed transactions.

**Returns**

`Object`:

- `String` - _tx_id_, transactions id, hash of transactions.

**Example**

```javascript
// Request
curl -X POST localhost:9889/submit-transactions -d '{"raw_transactions":["0701000201620160a0d36052ca3d1335120ae48e1ffb2fb6b25588628eff90fa88bef3117dfb4301ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80ddb2c490e906010116001431630464f2b2058fe3c1fe5bee00742eaf2da8d9630240acb57bc06f7e5de99ef3e630ce34fc74c33d4694301202968092ca50ae7842e3331bfeb0cf7b65f383e27670c4d58aeeeb0b77e5355957ca729298d2b4e2470c20a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a89739190161015f72de2064ab999acf22c05b5cf9c7d53164f80038b46b1ce426708514a30a3485ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80d4f4f69901000116001431630464f2b2058fe3c1fe5bee00742eaf2da8d96302404298424e89e5528f1d0cdd9028489b9d9e3f031ec34a74440cacc7900dc1eac9359c408a4342fc6cef935d2978919df8b23f3912ac4419800d375fac06ddb50620a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a897391901013fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8084c5b6aaea060116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900","0701000101620160571cc5d99a2994ff6b192bc9387838a3651245cb66dad4a6bc5f660310cebfa9ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8084c5b6aaea06000116001431630464f2b2058fe3c1fe5bee00742eaf2da8d96302408c742d77eba6c56a8db8c114e60be6c6263df6120aefd7538376129d04ec71b78b718c2085bba85254b44bf4600ba31d4c5a7869d0be0c46d88bd5eb27490e0820a86ab33efa9d71994270898ad99f198d60889ef617d5eaf25e776929a897391902013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80faafed99010116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900013fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80ddb2c490e9060116001431630464f2b2058fe3c1fe5bee00742eaf2da8d900"]}'
```

```
// Result
{
	"status": "success",
	"data": {
		"tx_id": ["8524bf38701c17c57e2ad7368c0d6c815eb30e92713ff5dd86c1a931cddf2e95", "a0bbfb75c9a00bb2d4801aa95ed0479993a67acfd1cec5b77a8ff86966f52dac"]
	}
}
```

---

#### `estimate-transaction-gas`

Estimate consumed neu(1BTM = 10^8NEU) for the transaction.

**Parameters**

`Object`:

- `Object` - _transaction_template_, builded transaction response.

**Returns**

`Object`:

- `Integer` - _total_neu_, total consumed neu(1BTM = 10^8NEU) for execute transaction, total_neu is rounded up storage_neu + vm_neu.
- `Integer` - _storage_neu_, consumed neu for storage transaction .
- `Integer` - _vm_neu_, consumed neu for execute VM.

**Example**

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

---

#### `create-access-token`

Create access token, it provides basic access authentication for HTTP protocol, returns token contain username and password, they are separated by a colon.

**Parameters**

`Object`:

- `String` - _id_, token ID.

optional:

- `String` - _type_, type of token.

**Returns**

`Object`:

- `String` - _token_, access token, authentication username and password are separated by a colon.
- `String` - _id_, token ID.
- `String` - _type_, type of token.
- `Object` - _created_at_, time to create token.

**Example**

create access token.

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

---

#### `list-access-tokens`

Returns the list of all available access tokens.

**Parameters**

none

**Returns**

- `Array of Object`, access token array.
  - `Object`:
    - `String` - _token_, access token.
    - `String` - _id_, token ID.
    - `String` - _type_, type of token.
    - `Object` - _created_at_, time to create token.

**Example**

list all the available access tokens.

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

---

#### `delete-access-token`

Delete existed access token.

**Parameters**

`Object`:

- `String` - _id_, token ID.

**Returns**

none if the access token is deleted successfully.

**Example**

delete access token.

```javascript
// Request
curl -X POST delete-access-token -d '{"id": "token1"}'

// Result
```

---

#### `check-access-token`

Check access token is valid.

**Parameters**

`Object`:

- `String` - _id_, token ID.
- `String` - _secret_, secret of token, the second part of the colon division for token.

**Returns**

none if the access token is checked valid.

**Example**

check whether the access token is vaild or not.

```javascript
// Request
curl -X POST check-access-token -d '{"id": "token1", "secret": "1fee70f537128a201338bd5f25a3adbf33dad02eae4f4c9ac43f336a069df8f3"}'

// Result
```

---

#### `create-transaction-feed`

Create transaction feed.

**Parameters**

`Object`:

- `String` - _alias_, name of the transaction feed.
- `String` - _filter_, filter of the transaction feed.

**Returns**

none if the transaction feed is created success.

**Example**

```javascript
// Request
curl -X POST create-transaction-feed -d '{"alias": "test1", "filter": "asset_id='84778a666fe453cf73b2e8c783dbc9e04bc4fd7cbb4f50caeaee99cf9967ebed' AND amount_lower_limit = 50 AND amount_upper_limit = 100"}'

// Result
```

---

#### `get-transaction-feed`

Query detail transaction feed by name.

**Parameters**

`Object`:

- `String` - _alias_, name of the transaction feed.

**Returns**

`Object`:

- `String` - _id_, id of the transaction feed.
- `String` - _alias_, name of the transaction feed.
- `String` - _filter_, filter of the transaction feed.
- `Object` - _param_, param of the transaction feed.
  - `String` - _assetid_, asset id.
  - `Integer` - _lowerlimit_, the lower limit of asset amount.
  - `Integer` - _upperlimit_, the upper limit of asset amount.
  - `String` - _transtype_, type of transaction.

**Example**

list the available txfeed by alias:

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

---

#### `list-transaction-feeds`

Returns the list of all available transaction feeds.

**Parameters**

none

**Returns**

- `Array of Object`, the transaction feeds.
  - `Object`:
    - `String` - _id_, id of the transaction feed.
    - `String` - _alias_, name of the transaction feed.
    - `String` - _filter_, filter of the transaction feed.
    - `Object` - _param_, param of the transaction feed.
      - `String` - _assetid_, asset id.
      - `Integer` - _lowerlimit_, the lower limit of asset amount.
      - `Integer` - _upperlimit_, the upper limit of asset amount.
      - `String` - _transtype_, type of transaction.

**Example**

list all the available txfeed:

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

#### `delete-transaction-feed`

Delete transaction feed by name.

**Parameters**

`Object`:

- `String` - _alias_, name of the transaction feed.

**Returns**

none if the transaction feed is deleted success.

**Example**

```javascript
// Request
curl -X POST delete-transaction-feed -d '{"alias": "test1"}'

// Result
```

---

#### `update-transaction-feed`

Update transaction feed.

**Parameters**

`Object`:

- `String` - _alias_, name of the transaction feed.
- `String` - _filter_, filter of the transaction feed.

**Returns**

none if the transaction feed is updated success.

**Example**

deleted when the txfeed exists, and create it with alias and filter:

```javascript
// Request
curl -X POST update-transaction-feed -d '{"alias": "test1", "filter": "asset_id='84778a666fe453cf73b2e8c783dbc9e04bc4fd7cbb4f50caeaee99cf9967ebed' AND amount_lower_limit = 60 AND amount_upper_limit = 80"}'

// Result
```

---

#### `get-unconfirmed-transaction`

Query mempool transaction by transaction ID.

**Parameters**

`Object`:

- `String` - _tx_id_, transaction id, hash of transaction.

**Returns**

`Object`:

- `String` - _id_, transaction id, hash of the transaction.
- `Integer` - _version_, version of transaction.
- `Integer` - _size_, size of transaction.
- `Integer` - _time_range_, the time range of transaction.
- `Boolean` - _status_fail_, whether the state of the request has failed.
- `String` - _mux_id_, the previous transaction mux id(wallet enable can be acquired, this place is empty).
- `Array of Object` - _inputs_, object of inputs for the transaction(input struct please refer to get-transction API description).
- `Array of Object` - _outputs_, object of outputs for the transaction(output struct please refer to get-transction API description).

**Example**

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
      "input_id": "a0c2fa0719bfe1446681537dcf1f8d0f03add093e29d12481eb807e07778d7b3",
      "spent_output_id": "161b44e547a6cc68d732eb64fa38031da98211a99319e088cfe632223f9ac6d8",
      "type": "spend",
      "witness_arguments": [
        "cf0e1b217ab92ade8e81fab10f9f307bb5cc1ad947b5629e3f7a760aba722f5044f2ab59ec92fa4264ff5811de4361abb6eabd7e75ffd28a813a98ceff434c01",
        "6890a19b21c326059eef211cd8414282a79d3b9203f2592064221fd360e778a7"
      ]
    }
  ],
  "mux_id": "842cd07eed050b547377b5b123f14a5ec0d76933d564f030cf4d5d5c15769645",
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

---

#### `list-unconfirmed-transactions`

Returns the total number of mempool transactions and the list of transaction IDs.

**Parameters**

none

**Returns**

`Object`:

- `Integer` - _total_, version of transaction.
- `Array of Object` - _tx_ids_, list of transaction id.

##### Example

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

#### `decode-raw-transaction`

Decode a serialized transaction hex string into a JSON object describing the transaction.

**Parameters**

`Object`:

- `String` - _raw_transaction_, hexstring of raw transaction.

**Returns**

`Object`:

- `String` - _tx_id_, transaction ID.
- `Integer` - _version_, version of transaction.
- `String` - _size_, size of transaction.
- `String` - _time_range_, time range of transaction.
- `String` - _fee_, fee for sending transaction.
- `Array of Object` - _inputs_, object of inputs for the transaction(input struct please refer to get-transction API description).
- `Array of Object` - _outputs_, object of outputs for the transaction

(output struct please refer to get-transction API description).

**Example**

```javascript
// Request
curl -X POST decode-raw-transaction -d '{"raw_transaction": "070100010161015fc8215913a270d3d953ef431626b19a89adf38e2486bb235da732f0afed515299ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8099c4d59901000116001456ac170c7965eeac1cc34928c9f464e3f88c17d8630240b1e99a3590d7db80126b273088937a87ba1e8d2f91021a2fd2c36579f7713926e8c7b46c047a43933b008ff16ecc2eb8ee888b4ca1fe3fdf082824e0b3899b02202fb851c6ed665fcd9ebc259da1461a1e284ac3b27f5e86c84164aa518648222602013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80bbd0ec980101160014c3d320e1dc4fe787e9f13c1464e3ea5aae96a58f00013cffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8084af5f01160014bb93cdb4eca74b068321eeb84ac5d33686281b6500"}'

// Result
{
  "fee": 20000000,
  "inputs": [
    {
      "address": "sm1q26kpwrrevhh2c8xrfy5vnaryu0ugc97csrdy69",
      "amount": 41250000000,
      "asset_definition": {},
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "control_program": "001456ac170c7965eeac1cc34928c9f464e3f88c17d8",
      "input_id": "9963265eb601df48501cc240e1480780e9ed6e0c8f18fd7dd57954068c5dfd02",
      "spent_output_id": "01bb3309666618a1507cb5be845b17dee5eb8028ee7e71b17d74b4dc97085bc8",
      "type": "spend",
      "witness_arguments": [
        "b1e99a3590d7db80126b273088937a87ba1e8d2f91021a2fd2c36579f7713926e8c7b46c047a43933b008ff16ecc2eb8ee888b4ca1fe3fdf082824e0b3899b02",
        "2fb851c6ed665fcd9ebc259da1461a1e284ac3b27f5e86c84164aa5186482226"
      ]
    }
  ],
  "outputs": [
    {
      "address": "sm1qc0fjpcwuflnc06038s2xfcl2t2hfdfv0lxzg7s",
      "amount": 41030000000,
      "asset_definition": {},
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "control_program": "0014c3d320e1dc4fe787e9f13c1464e3ea5aae96a58f",
      "id": "567b34857614d16292220beaca16ce34b939c75023a49cc43fa432fff51ca0dd",
      "position": 0,
      "type": "control"
    },
    {
      "address": "sm1qhwfumd8v5a9sdqepa6uy43wnx6rzsxm9essn4l",
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
  "tx_id": "4c97d7412b04d49acc33762fc748cd0780d8b44086c229c1a6d0f2adfaaac2db",
  "version": 1
}
```

---

#### `get-block-count`

Returns the current block height for blockchain.

**Parameters**

none

**Returns**

`Object`:

- `Integer` - _block_count_, recent block height of the blockchain.

**Example**

```javascript
// Request
curl -X POST get-block-count

// Result
{
    "block_count": 519
}
```

---

#### `get-block-hash`

Returns the current block hash for blockchain.

**Parameters**

none

**Returns**

`Object`:

- `String` - _block_hash_, recent block hash of the blockchain.

**Example**

```javascript
// Request
curl -X POST get-block-hash

// Result
{
  "block_hash": "997bf5cecb4df097991a7a121a7fd3cb5a404fa856e3d6032c791ac07bc7c74c"
}
```

---

#### `get-block`

Returns the detail block by block height or block hash.

**Parameters**

`Object`: block_height | block_hash

optional:

- `String` - _block_hash_, hash of block.
- `Integer` - _block_height_, height of block.

**Returns**

`Object`:

- `String` - _hash_, hash of block.
- `Integer` - _size_, size of block.
- `Integer` - _version_, version of block.
- `Integer` - _height_, height of block.
- `String` - _previous_block_hash_, previous block hash.
- `Integer` - _timestamp_, timestamp of block.
- `Integer` - _nonce_, nonce value.
- `Integer` - _bits_, bits of difficulty.
- `String` - _difficulty_, difficulty value(String type).
- `String` - _transaction_merkle_root_, merkle root of transaction.
- `String` - _transaction_status_hash_, merkle root of transaction status.
- `Array of Object` - _transactions_, transaction object:
  - `String` - _id_, transaction id, hash of the transaction.
  - `Integer` - _version_, version of transaction.
  - `Integer` - _size_, size of transaction.
  - `Integer` - _time_range_, the unix timestamp for when the requst was responsed.
  - `Boolean` - _status_fail_, whether the state of the request has failed.
  - `String` - _mux_id_, the previous transaction mux id(source id of utxo).
  - `Array of Object` - _inputs_, object of inputs for the transaction.
    - `String` - _type_, the type of input action, available option include: 'spend', 'issue', 'coinbase'.
    - `String` - _asset_id_, asset id.
    - `String` - _asset_alias_, name of asset.
    - `Object` - _asset_definition_, definition of asset(json object).
    - `Integer` - _amount_, amount of asset.
    - `Object` - _issuance_program_, issuance program, it only exist when type is 'issue'.
    - `Object` - _control_program_, control program of account, it only exist when type is 'spend'.
    - `String` - _address_, address of account, it only exist when type is 'spend'.
    - `String` - _spent_output_id_, the front of outputID to be spent in this input, it only exist when type is 'spend'.
    - `String` - _account_id_, account id.
    - `String` - _account_alias_, name of account.
    - `Object` - _arbitrary_, arbitrary infomation can be set by miner, it only exist when type is 'coinbase'.
    - `String` - _input_id_, hash of input action.
    - `Array of String` - _witness_arguments_, witness arguments.
  - `Array of Object` - _outputs_, object of outputs for the transaction.
    - `String` - _type_, the type of output action, available option include: 'retire', 'control'.
    - `String` - _id_, outputid related to utxo.
    - `Integer` - _position_, position of outputs.
    - `String` - _asset_id_, asset id.
    - `String` - _asset_alias_, name of asset.
    - `Object` - _asset_definition_, definition of asset(json object).
    - `Integer` - _amount_, amount of asset.
    - `String` - _account_id_, account id.
    - `String` - _account_alias_, name of account.
    - `Object` - _control_program_, control program of account.
    - `String` - _address_, address of account.

**Example**

get specified block information by block_hash or block_height, if both exists, the block result is querying by hash.

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
          "input_id": "6cb8491e4b1cbdc052c2fdb5f2849194d59118b954d5ea5244bbd20e3cff3b80",
          "type": "coinbase",
          "witness_arguments": null
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

#### `get-block-header`

Returns the detail block header by block height or block hash.

**Parameters**

`Object`: block_height | block_hash

optional:

- `String` - _block_hash_, hash of block.
- `Integer` - _block_height_, height of block.

**Returns**

`Object`:

- `Object` - _block_header_, header of block.
- `Integer` - _reward_, reward.

**Example**

```javascript
// Request
curl -X POST get-block-header -d '{"block_height": 43, "block_hash": "886a8e85b275e7d65b569ba510875c0e63dece1a94569914d7624c0dac8002f9"}'

// Result
{
  "block_header": "01019601e87da37e7d73f31d54304c719c9058ec7bc7de7819deda89a7c8834a99bc05b8fbdbe6d60540eba9e5d5cb79fd87b3c0fad32f6772c1e4483f2a070e093a6176d85226d986a8c9c377e5192668bc0a367e4a4764f11e7c725ecced1d7b6a492974fab1b6d5bc00ad918480808080801e",
  "reward": 41250000000
}
```

---

#### `get-vote-result`

Returns the vote result by block height or block hash.

**Parameters**

`Object`: block_height | block_hash

optional:

- `String` - _block_hash_, hash of block.
- `Integer` - _block_height_, height of block.

**Returns**

- `Array of Object`, connected peers.
  - `Object`:
    - `String` - _vote_, the public key of vote.
    - `String` - _vote_number_, the number of vote.

**Example**

```javascript
// Request
curl -X POST get-vote-result -d '{"block_height": 43, "block_hash": "886a8e85b275e7d65b569ba510875c0e63dece1a94569914d7624c0dac8002f9"}'

// Result
[
  {
    "vote": "a0b57b8c26578541a8194ee0aedd3f6f1f037fc3082f61a4d1ac78e86efe0f532a9050242c53fe61580693a5ae78f7241720f4e21043138820c98d35625d33f9",
    "vote_number": 99999999
  },
  {
    "vote": "fe09ce823dee651486b2d737dc03f74962c03106a09897fbfe1fa92a7f28e0ca500d17b1081de128fcf17bf6613addbc3ea6474a027800ea12089d9e42b322be",
    "vote_number": 1111111111111,
  },
  {
    "vote": "431f38d4dda3887998c9ef6c01ec34d2e6c3131bbc1beedfaf5a5e8ab31b68d97c78c16554b4f538b1e9ef6754176e17295cd8d76ba556968bd3bd5d5ec08593",
    "vote_number": 2222222323,
  }
]
```

---

#### `net-info`

Returns the information of current network node.

**Parameters**

none

**Returns**

`Object`:

- `Boolean` - _listening_, whether the node is listening.
- `Boolean` - _syncing_, whether the node is syncing.
- `Boolean` - _mining_, whether the node is mining.
- `Integer` - _peer_count_, current count of connected peers.
- `Integer` - _current_block_, current block height in the node's blockchain.
- `Integer` - _highest_block_, current highest block of the connected peers.
- `String` - _network_id_, network id.
- `Object` - _version_info_, bytomd version information:
  - `String` - _version_, current version of the running `bytomd`.
  - `uint16` - _update_, whether there exists an update.
    - 0: no update;
    - 1: small update;
    - 2: significant update.
  - `String` - _new_version_, the newest version of `bytomd` if there is one.

**Example**

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

---

#### `is-mining`

Returns the mining status.

**Parameters**

none

**Returns**

`Object`:

- `Boolean` - _is_mining_, whether the node is mining.

**Example**

```javascript
// Request
curl -X POST is-mining

// Result
{
  "is_mining": true
}
```

---

#### `set-mining`

Start up node mining.

**Parameters**

`Object`:

- `Boolean` - _is_mining_, whether the node is mining.

**Example**

```javascript
// Request
curl -X POST set-mining -d '{"is_mining": true}'

// Result
```

---

#### `gas-rate`

Quary gas rate.

**Parameters**

none

**Returns**

`Object`:

- `Integer` - _gas_rate_, gas rate.

**Example**

```javascript
// Request
curl -X POST gas-rate -d '{}'

// Result
{
  "gas_rate": 1000
}
```

---

#### `verify-message`

Verify a signed message with derived pubkey of the address.

**Parameters**

`Object`:

- `String` - _address_, address for account.
- `String` - _derived_xpub_, derived xpub.
- `String` - _message_, message for signature by derived_xpub.
- `String` - _signature_, signature for message.

**Returns**

`Object`:

- `Boolean` - _result_, verify result.

**Example**

```javascript
// Request
curl -X POST verify-message -d '{"address":"bm1qx2qgvvjz734ur8x5lpfdtlau74aaa5djs0a5jn", "derived_xpub":"6ff8c3d1321ce39a3c3550f57ba70b67dcbcef821e9b85f6150edb7f2f3f91009e67f3075e6e76ed5f657ee4b1a5f4749b7a8c74c8e7e6a1b0e5918ebd5df4d0", "message":"this is a test message", "signature":"74da3d6572233736e3a439166719244dab57dd0047f8751b1efa2da26eeab251d915c1211dcad77e8b013267b86d96e91ae67ff0be520ef4ec326e911410b609"}'

// Result
{
  "result": true
}
```

---

#### `compile`

Compile equity contract.

**Parameters**

`Object`:

- `String` - _contract_, content of equity contract.

Optional:

- `Array of Object` - _args_, parameters of contract.
  - `Boolean` - _boolean_, boolean parameter.
  - `Integer` - _integer_, integer parameter.
  - `String` - _string_, string parameter.

**Returns**

`Object`:

- `String` - _name_, contract name.
- `String` - _source_, source content of contract.
- `String` - _program_, generated program by compiling contract.
- `Array of Object` - _params_, parameters of contract.
- `String` - _value_, locked value name of contract.
- `Array of Object` - _clause_info_, clauses of contract.
- `String` - _opcodes_, opcodes of contract.
- `String` - _error_, returned error information for compiling contract.

**Example**

```javascript
// Request
{
  "contract": "contract LockWithPublicKey(publicKey: PublicKey) locks valueAmount of valueAsset { clause unlockWithSig(sig: Signature) { verify checkTxSig(publicKey, sig) unlock valueAmount of valueAsset } }",
  "args": [
    {
      "string": "e9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e78"
    }
  ]
}

// Result
{
  "name": "LockWithPublicKey",
  "source": "contract LockWithPublicKey(publicKey: PublicKey) locks valueAmount of valueAsset { clause unlockWithSig(sig: Signature) { verify checkTxSig(publicKey, sig) unlock valueAmount of valueAsset } }",
  "program": "20e9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e787403ae7cac00c0",
  "params": [
    {
      "name": "publicKey",
      "type": "PublicKey"
    }
  ],
  "value": "valueAmount of valueAsset",
  "clause_info": [
    {
      "name": "unlockWithSig",
      "params": [
        {
          "name": "sig",
          "type": "Signature"
        }
      ],
      "values": [
        {
          "name": "",
          "asset": "valueAsset",
          "amount": "valueAmount"
        }
      ]
    }
  ],
  "opcodes": "0xe9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e78 DEPTH 0xae7cac FALSE CHECKPREDICATE",
  "error": ""
}
```

---

#### `list-peers`

Returns the list of connected peers.

**Parameters**

none

**Returns**

- `Array of Object`, connected peers.
  - `Object`:
    - `String` - _peer_id_, peer id.
    - `String` - _remote_addr_, the address(IP and port) of connected peer.
    - `Integer` - _height_, the current height of connected peer.
    - `String` - _ping_, the delay time of connected peer.
    - `String` - _duration_, the connected time.
    - `Integer` - _total_sent_, total data sent in byte.
    - `Integer` - _total_received_, total data received in byte.
    - `Integer` - _average_sent_rate_, average data sent rate in byte.
    - `Integer` - _average_received_rate_, average data received rate in byte.
    - `Integer` - _current_sent_rate_, current data sent rate in byte.
    - `Integer` - _current_received_rate_, current data received rate in byte.

**Example**

```javascript
// Request
curl -X POST list-peers -d '{}'

// Result
[  
   {  
      "peer_id":"3B58D7139B53066F2031FC1F027D2B3423FA4CE01F1FB1CC2DC4003C78413C24",
      "remote_addr":"52.83.251.197:46656",
      "height":84222,
      "ping":"40ms",
      "duration":"17.26s",
      "total_sent":17565,
      "total_received":3642187,
      "average_sent_rate":1018,
      "average_received_rate":211019,
      "current_sent_rate":2420,
      "current_received_rate":157087
   }
]
```

---

#### `disconnect-peer`

Disconnect to specified peer.

**Parameters**

`Object`:

- `String` - _peer_id_, peer id.

**Returns**

none if disconnect peer successfully.

**Example**

```javascript
// Request
curl -X POST disconnect-peer -d '{"peer_id":"29661E8BB9A8149F01C6594E49EA80C6B18BF247946A7E2E01D8235BBBFC3594"}'

// Result
```

---

#### `connect-peer`

Connect to specified peer.

**Parameters**

`Object`:

- `String` - _ip_, peer IP address.
- `Integer` - _port_, peer port.

**Returns**

`Object`:

- `String` - _peer_id_, peer id.
- `String` - _remote_addr_, the address(IP and port) of connected peer.
- `Integer` - _height_, the current height of connected peer.
- `Integer` - _delay_, the delay time of connected peer.

**Example**

```javascript
// Request
curl -X POST connect-peer -d '{"ip":"139.198.177.164", "port":46657}'

// Result
{
  "peer_id": "29661E8BB9A8149F01C6594E49EA80C6B18BF247946A7E2E01D8235BBBFC3594",
  "remote_addr": "139.198.177.164:46657",
  "height": 65941,
  "delay": 0
}
```


#### Websocket

**API Endpoint**

Default JSON-RPC endpoints:

| Client | URL |
| :---: | :---: |
| Go | ws://localhost:9889/websocket-subscribe |


**Method Overview**

| # | Topic | Description | NotificationType | NotificationData |
| :---: | :---: | :---: | :---: | :---: |
| 1 | notify_raw_blocks | Send notifications when a block is connected or disconnected from the best chain. | raw_blocks_connected raw_blocks_disconnected  request_status | raw block |
| 2 | stop_notify_raw_blocks | Cancel registered notifications for whenever a block is connected or disconnected from the main (best) chain. | None | None |
| 3 | notify_new_transactions | Send notifications for all new transactions as they are accepted into the mempool. | new_transaction  request_status | {"transaction": string, "status_fail": bool} |
| 4 | stop_notify_new_transactions | Stop sending either a tx accepted  notification when a new transaction is accepted into the mempool. | None | None |


