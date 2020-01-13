---
id: docs_10
title: BUTXO
sidebar_label: BUTXO
---

## BUTXO模型

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

`ControlProgram` 的结构：参见 [智能合约](https://bytomfans.github.io/bystack-docs/docs/docs_30) 。

## MUX结构

比原链在UTXO的基础上加入了MUX结构，从而能够在一笔交易中支持多输入和多输出。Mux是一个资产混合器结构的哈希值。Mux结构可以理解为一个交易池，将一笔交易中的输入放入到MUX中，然后分配成不同的输入。MUX结构将所有的资产根据不同的种类汇总起来，并根据不同的输出进行分配，在MUX中一个资产类型对应一个或者多个输入，同时也可以对应一个或者多个不同输出。MUX结构最重要的好处就是将原本多对多的关系，简化为一次多对一和一次一对多的关系，从而简化多资产的验证逻辑。

	type Mux struct {
		Sources             []*ValueSource
		Program             *Program
		WitnessDestinations []*ValueDestination
		WitnessArguments    [][]byte
	}

### BUTXO ID

BUTXO ID就是`OutputID`,可花费的utxo，其实就是找到接收地址或接收`program`是用户自己的`unspend_output`。

## Entry

Entry是由区块链的每个可寻址单元执行的接口： 交易部件如spends、issuances、outputs和retirements等等，外加区块头。

区块链 _有向无环图_ 中的Entry:区块头引用transaction header（默克尔树），而transaction header又引用来自mux、issuances和spend的output。

### 数据结构

每个Entry有以下通用结构：

	type Entry interface {
		proto.Message

		// type produces a short human-readable string uniquely identifying
		// the type of this entry.
		typ() string

		// writeForHash writes the entry's body for hashing.
		writeForHash(w io.Writer)
	}

### Entry ID

```bash
func EntryID(e Entry) (hash Hash) {
	if e == nil {
		return hash
	}

	// Nil pointer; not the same as nil interface above. (See
	// https://golang.org/doc/faq#nil_error.)
	if v := reflect.ValueOf(e); v.Kind() == reflect.Ptr && v.IsNil() {
		return hash
	}

	hasher := sha3pool.Get256()
	defer sha3pool.Put256(hasher)

	hasher.Write([]byte("entryid:"))
	hasher.Write([]byte(e.typ()))
	hasher.Write([]byte{':'})

	bh := sha3pool.Get256()
	defer sha3pool.Put256(bh)

	e.writeForHash(bh)

	var innerHash [32]byte
	bh.Read(innerHash[:])

	hasher.Write(innerHash[:])

	hash.ReadFrom(hasher)
	return hash
}
```

### tx_header

	type TxHeader struct {
		Version        uint64
		SerializedSize uint64
		TimeRange      uint64
		ResultIds      []*Hash
	}


### output1

	type Output struct {
		Source         *ValueSource
		ControlProgram *Program
		Ordinal        uint64
	}

### issuance1

	type Issuance struct {
		NonceHash              *Hash
		Value                  *AssetAmount
		WitnessDestination     *ValueDestination
		WitnessAssetDefinition *AssetDefinition
		WitnessArguments       [][]byte
		Ordinal                uint64
	}

### mux1

	type Mux struct {
		Sources             []*ValueSource
		Program             *Program
		WitnessDestinations []*ValueDestination
		WitnessArguments    [][]byte
	}

### Value Source1

	type ValueSource struct {
		Ref      *Hash
		Value    *AssetAmount
		Position uint64
	}

### value Destination1

	type ValueDestination struct {
		Ref      *Hash
		Value    *AssetAmount
		Position uint64
	}

### Retirement1

	type Retirement struct {
		Source  *ValueSource
		Ordinal uint64
	}

### Spend1

	type Spend struct {
		SpentOutputId      *Hash
		WitnessDestination *ValueDestination
		WitnessArguments   [][]byte
		Ordinal            uint64
	}

### nonce

	Nonce uint64

### timerange

	timeRange uint64
