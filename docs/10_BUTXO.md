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

| 字段 | 类型 | 描述 | 
| --- | --- |--- |
| Type | string | Entry的类型，例如Issuance1、Retirement1等等 |
| Body | struct | 因类型而异 |
| Witness | struct | 因类型而异 |

| 主体字段 | 类型 | 描述 | 
| --- | --- |--- |
| proto.Message | Struct | 类型产生唯一标识可读短字符串 |
| Type | string | Entry的Type |
| writeForHash | Struct | 哈希entry的主体 |

	type Entry interface {
		proto.Message

		// type produces a short human-readable string uniquely identifying
		// the type of this entry.
		typ() string

		// writeForHash writes the entry's body for hashing.
		writeForHash(w io.Writer)
	}

### Entry ID

Entry ID基于其余其 _类型_ 与 _主体_ 。类型编码成原始字节序列（不带长度前缀）。主体编码为连接body结构的所有字段的SHA3-256哈希。

	entryID = SHA3-256("entryid:" || type || ":" || SHA3-256(body))

```golang
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

| 字段 | 类型 | 描述 | 
| --- | --- |--- |
| Type | string | "txheader" |
| Body | struct | 见下方 |
| Witness | struct | 空的结构体 |

| 主体字段 | 类型 | 描述 | 
| --- | --- |--- |
| Version | uint64 | 交易版本号 = 1 |
| SerializedSize | uint64 | 交易总字节数 |
| TimeRange | uint64 | 交易过期时间 |
| ResultIds | *Hash | 指针列表指向输出或指令引退。该列表一定包含至少一项元素。 |

	type TxHeader struct {
		Version        uint64
		SerializedSize uint64
		TimeRange      uint64
		ResultIds      []*Hash
	}

### output1

| 字段 | 类型 | 描述 | 
| --- | --- |--- |
| Type | string | "output1" |
| Body | struct | 见下方 |
| Witness | struct | 空的结构体 |

| 主体字段 | 类型 | 描述 | 
| --- | --- |--- |
| Source | *ValueSource | 单元的Source包含于output之中。  |
| ControlProgram | *Program | 控制其输出的程序 |
| Ordinal | uint64 | 序号 |

	type Output struct {
		Source         *ValueSource
		ControlProgram *Program
		Ordinal        uint64
	}

### issuance1

| 字段 | 类型 | 描述 | 
| --- | --- |--- |
| Type | string | "issuance1" |
| Body | struct | 见下方 |
| Witness | struct | 见下方 |

| 主体字段 | 类型 | 描述 | 
| --- | --- |--- |
| NonceHash | *Hash | 临时的哈希  |
| Value | *AssetAmount | 已发行的Asset ID和数量.  |
| Ordinal | uint64 | 序号 |

| 见证字段 | 类型 | 描述 | 
| --- | --- |--- |
| WitnessDestination | *ValueDestination | 该[Spend](#spend1)中值的Destination（前指针）。直接指向某一`Output`或`Mux`，经过其自己的`Destinations`指向`Output` Entry  |
| WitnessAssetDefinition | *AssetDefinition | 已发行Asset的Asset定义.  |
| WitnessArguments | *byte | control program的参数包含于SpentOutput |

	type Issuance struct {
		NonceHash              *Hash
		Value                  *AssetAmount
		WitnessDestination     *ValueDestination
		WitnessAssetDefinition *AssetDefinition
		WitnessArguments       [][]byte
		Ordinal                uint64
	}

### mux1

| 字段 | 类型 | 描述 | 
| --- | --- |--- |
| Type | string | "mux1" |
| Body | struct | 见下方 |
| Witness | struct | 见下方 |

| 主体字段 | 类型 | 描述 | 
| --- | --- |--- |
| Sources | *ValueSource | 单元的Source包含于Mux之中。  |
| Program | *Program | 控制Mux中值并一定计算为true的程序  |

| 见证字段 | 类型 | 描述 | 
| --- | --- |--- |
| WitnessDestination | *ValueDestination | 该Mux中值的Destination（前指针）。可以直接指向某一`Output`或其他Mux，经过其自己的`Destinations`指向`Output` Entry  |
| WitnessArguments | *byte | program的参数包含于[Nonce](#nonce) |

	type Mux struct {
		Sources             []*ValueSource
		Program             *Program
		WitnessDestinations []*ValueDestination
		WitnessArguments    [][]byte
	}

### Value Source1

| 字段 | 类型 | 描述 | 
| --- | --- |--- |
| Ref | *Hash | 上一个entry引用自该ValueSource。 |
| Value | *AssetAmount | 数量和资产ID包含于引用的entry。 |
| Position | uint64 | 如果该source是[Mux](#mux1) entry，那么`Position`就是输出的索引。如果该source是[Issuance](#issuance1)或[Spend](#spend1) entry，那么`Position`一定是0。 |

	type ValueSource struct {
		Ref      *Hash
		Value    *AssetAmount
		Position uint64
	}

### value Destination1

| 字段 | 类型 | 描述 | 
| --- | --- |--- |
| Ref | *Hash | 下一个entry引用自该ValueDestination。 |
| Value | *AssetAmount | 数量和资产ID包含于引用的entry。 |
| Position | uint64 | 如果该destination是[Mux](#mux1) entry，那么`Position`就是一个mux的标号输入。如果不是，那么`Position`一定是0。 |

	type ValueDestination struct {
		Ref      *Hash
		Value    *AssetAmount
		Position uint64
	}

### Retirement1

| 字段 | 类型 | 描述 | 
| --- | --- |--- |
| Type | string | "retirement1" |
| Body | struct | 见下方 |
| Witness | struct | 空的结构体 |

| 主体字段 | 类型 | 描述 | 
| --- | --- |--- |
| Source | *ValueSource | 退役的单元Source。  |
| Ordinal | uint64 | 序号 |

	type Retirement struct {
		Source  *ValueSource
		Ordinal uint64
	}

### Spend1

| 字段 | 类型 | 描述 | 
| --- | --- |--- |
| Type | string | "mux1" |
| Body | struct | 见下方 |
| Witness | struct | 见下方 |

| 主体字段 | 类型 | 描述 | 
| --- | --- |--- |
|SpentOutputId | *Hash | 已被该spend消耗的Output entry |
|Ordinal | uint64 | 序号 |

| 见证字段 | 类型 | 描述 | 
| --- | --- |--- |
| WitnessDestination | *ValueDestination | 该spend中值的Destination（前指针）。可以直接指向某一`Output`或Mux，经过其自己的`Destinations`指向`Output` Entry  |
| WitnessArguments | *byte | program的参数包含于SpentOutput |

	type Spend struct {
		SpentOutputId      *Hash
		WitnessDestination *ValueDestination
		WitnessArguments   [][]byte
		Ordinal            uint64
	}

### nonce

| 字段 | 类型 | 描述 | 
| --- | --- |--- |
| Type | string | "nonce" |
| Body | struct | 空的结构体 |
| Witness | struct | 空的结构体 |

	Nonce uint64

### timerange

| 字段 | 类型 | 描述 | 
| --- | --- |--- |
| Type | string | "timerange" |
| Body | struct | 空的结构体 |
| Witness | struct | 空的结构体 |

	timeRange uint64
