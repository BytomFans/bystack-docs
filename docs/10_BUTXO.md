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
		Sources             []*ValueSource      `protobuf:"bytes,1,rep,name=sources" json:"sources,omitempty"`
		Program             *Program            `protobuf:"bytes,2,opt,name=program" json:"program,omitempty"`
		WitnessDestinations []*ValueDestination `protobuf:"bytes,3,rep,name=witness_destinations,json=witnessDestinations" json:"witness_destinations,omitempty"`
		WitnessArguments    [][]byte            `protobuf:"bytes,4,rep,name=witness_arguments,json=witnessArguments,proto3" json:"witness_arguments,omitempty"`
	}

### BUTXO ID

BUTXO ID就是`OutputID`,可花费的utxo，其实就是找到接收地址或接收`program`是用户自己的`unspend_output`。

## Entry

### 数据结构

	// Entry is the interface implemented by each addressable unit in a
	// blockchain: transaction components such as spends, issuances,
	// outputs, and retirements (among others), plus blockheaders.
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
		Version        uint64  `protobuf:"varint,1,opt,name=version" json:"version,omitempty"`
		SerializedSize uint64  `protobuf:"varint,2,opt,name=serialized_size,json=serializedSize" json:"serialized_size,omitempty"`
		TimeRange      uint64  `protobuf:"varint,3,opt,name=time_range,json=timeRange" json:"time_range,omitempty"`
		ResultIds      []*Hash `protobuf:"bytes,4,rep,name=result_ids,json=resultIds" json:"result_ids,omitempty"`
	}

### output1

	type Output struct {
		Source         *ValueSource `protobuf:"bytes,1,opt,name=source" json:"source,omitempty"`
		ControlProgram *Program     `protobuf:"bytes,2,opt,name=control_program,json=controlProgram" json:"control_program,omitempty"`
		Ordinal        uint64       `protobuf:"varint,3,opt,name=ordinal" json:"ordinal,omitempty"`
	}

### issuance1

	type Issuance struct {
		NonceHash              *Hash             `protobuf:"bytes,1,opt,name=nonce_hash,json=nonceHash" json:"nonce_hash,omitempty"`
		Value                  *AssetAmount      `protobuf:"bytes,2,opt,name=value" json:"value,omitempty"`
		WitnessDestination     *ValueDestination `protobuf:"bytes,3,opt,name=witness_destination,json=witnessDestination" json:"witness_destination,omitempty"`
		WitnessAssetDefinition *AssetDefinition  `protobuf:"bytes,4,opt,name=witness_asset_definition,json=witnessAssetDefinition" json:"witness_asset_definition,omitempty"`
		WitnessArguments       [][]byte          `protobuf:"bytes,5,rep,name=witness_arguments,json=witnessArguments,proto3" json:"witness_arguments,omitempty"`
		Ordinal                uint64            `protobuf:"varint,6,opt,name=ordinal" json:"ordinal,omitempty"`
	}

### mux1

	type Mux struct {
		Sources             []*ValueSource      `protobuf:"bytes,1,rep,name=sources" json:"sources,omitempty"`
		Program             *Program            `protobuf:"bytes,2,opt,name=program" json:"program,omitempty"`
		WitnessDestinations []*ValueDestination `protobuf:"bytes,3,rep,name=witness_destinations,json=witnessDestinations" json:"witness_destinations,omitempty"`
		WitnessArguments    [][]byte            `protobuf:"bytes,4,rep,name=witness_arguments,json=witnessArguments,proto3" json:"witness_arguments,omitempty"`
	}

### Value Source1

	type ValueSource struct {
		Ref      *Hash        `protobuf:"bytes,1,opt,name=ref" json:"ref,omitempty"`
		Value    *AssetAmount `protobuf:"bytes,2,opt,name=value" json:"value,omitempty"`
		Position uint64       `protobuf:"varint,3,opt,name=position" json:"position,omitempty"`
	}

### value Destination1

	type ValueDestination struct {
		Ref      *Hash        `protobuf:"bytes,1,opt,name=ref" json:"ref,omitempty"`
		Value    *AssetAmount `protobuf:"bytes,2,opt,name=value" json:"value,omitempty"`
		Position uint64       `protobuf:"varint,3,opt,name=position" json:"position,omitempty"`
	}

### Retirement1

	type Retirement struct {
		Source  *ValueSource `protobuf:"bytes,1,opt,name=source" json:"source,omitempty"`
		Ordinal uint64       `protobuf:"varint,2,opt,name=ordinal" json:"ordinal,omitempty"`
	}

### Spend1

	type Spend struct {
		SpentOutputId      *Hash             `protobuf:"bytes,1,opt,name=spent_output_id,json=spentOutputId" json:"spent_output_id,omitempty"`
		WitnessDestination *ValueDestination `protobuf:"bytes,2,opt,name=witness_destination,json=witnessDestination" json:"witness_destination,omitempty"`
		WitnessArguments   [][]byte          `protobuf:"bytes,3,rep,name=witness_arguments,json=witnessArguments,proto3" json:"witness_arguments,omitempty"`
		Ordinal            uint64            `protobuf:"varint,4,opt,name=ordinal" json:"ordinal,omitempty"`
	}

### nonce

	Nonce uint64

### timerange

	timeRange uint64
