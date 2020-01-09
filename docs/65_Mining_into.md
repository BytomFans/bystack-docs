---
id: docs_65
title: 矿池对接
sidebar_label: 矿池对接
---

### 矿机配置

[https://gist.github.com/HAOYUatHZ/a47400bde4a138825faef415387b532c](https://gist.github.com/HAOYUatHZ/a47400bde4a138825faef415387b532c)


### 固件升级

[https://service.bitmain.com.cn/support](https://service.bitmain.com.cn/support)

* 两个都要刷,先后顺序没关系
* _update_1000.tar.gz_ 升级时间较长，升级期间请勿断电


### 配置节点

* 测试时可以考虑切换到 testnet 分支降低难度使cpu挖矿也能出块，`./bytomd init --chain_id testnet` 或 `./bytomd init --chain_id solonet`
* `init`/`node` 初始化/启动时可以加上 `-r "your/directory"` 指定数据目录，若目录不存在则会自动新建该目录


### 流程

1、初始化节点先建个账户、地址，不然就挖到空地址

2、矿地址支持自定义，包括 非本地钱包地址

3、[API doc](https://github.com/Bytom/bytom/wiki/API-Reference)

4、矿池向节点 [getwork](https://github.com/Bytom/bytom/wiki/API-Reference#get-work)

`get-work` 得到的 `block_header` 是动态压缩变长的需要进行解析

* 使用 _golang_ 的话可以利用 `"github.com/bytom/protocol/bc/types"` 中 `block_header.go` 中的函数 `UnmarshalText`
* 使用别的语言的话参考 `"github.com/bytom/protocol/bc/types"` 中 `block.go` 中的函数 `UnmarshalText`, `readFrom`, `ReadVarintXXX`. `ReadVarintXXX` 需要参考 [go函数 `binary.ReadUvarint`](https://go.googlesource.com/go/+/go1.4.3/src/encoding/binary/varint.go)

5、解析完后进行下发

* 通信格式参考 [https://github.com/Bytom/B3-Mimic/blob/master/docs/STRATUM-BTM.md](https://github.com/Bytom/B3-Mimic/blob/master/docs/STRATUM-BTM.md) - 收到任务有 `login` 和 矿池主动下发, 没走 `getjob`, 只走 `login` 和 池主动下发 - 这俩都是用 `submit` 提交
* 逻辑参考 [https://github.com/Bytom/B3-Mimic/blob/master/main.go](https://github.com/Bytom/B3-Mimic/blob/master/main.go) - `Version`, `Height`, `Timestamp`, `Bits` 要转小端 - 关于 `target` + _btc.com_antpool_ 的代码 ~, 并说 `target` 用以对 `bits` 对应的 `difficulty` 放松难度，用来使矿机在单位时间内能够有提交，然后矿池再验证~

```
var Diff1 = StringToBig("0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF")

    func GetTargetHex(diff int64) string {
        padded := make([]byte, 32)
        diffBuff := new(big.Int).Div(Diff1, big.NewInt(diff)).Bytes()
        copy(padded[32-len(diffBuff):], diffBuff)
        buff := padded[0:4]
        targetHex := hex.EncodeToString(Reverse(buff))
        return targetHex
     }
```

* 矿池下发的targethex是拿 标准难度（`0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF`） / 一个难度值得出的
* 这个值叫做矿池难度 一般会动态调整 保证矿机提交 share 的频率是稳定的 比如1分钟提交三次 提交得快了就会增加这个值 慢了就会降低这个值
* target 是 16 进制的难度，1, 1024, …..等等，和前导 0 的个数有关，动态调整用来保证矿机每分钟至少提交三次，用来计算矿机算力以及防止矿机算力作弊 `ffff3f00` 对应 1024，`c5a70000` 对应 100001

6、提交完之后矿池需要做验证

* header_hash 使用 golang 的话可以利用 `"github.com/bytom/protocol/bc/types"` 中 `types.BlockHeader{}` 的 `Hash()` 使用别的语言的话参考 [https://github.com/Bytom/B3-Mimic/blob/master/docs/blhr_hash_V3.go](https://github.com/Bytom/B3-Mimic/blob/master/docs/blhr_hash_V3.go)
* 然后就要开始用 tensority 算 hash 结果 很遗憾现在 go 版本、cpp_openblas 版本、cpp_simd 版本都达不到理想的验证效果, 如果想做一个可用的矿池目前有必要上 gpu, 可以考虑 n 卡 1050，或者阿里云服务器 P4

> cpp 的 tensority 逻辑在[这里](https://github.com/Bytom/CppTensority)，并指出了如何针对 gpu 进行优化的建议，这样矩阵乘法能够跑进 2.5 ms, 整个 tensority 大概 6 ms


* init matlist 有开销，seed 其实 256 个区块才改变一次, 遇到新的 seed 每次 gpu tensority 可能需要 100 ms，但做了 cache 的话 init matlist 可以忽略，可以认为每次 tensority 只需要不超过 6 ms
* 用 golang 可以 cgo 调用 c 代码，参考 [https://github.com/Bytom/bytom/blob/dev-ts-simd/mining/tensority/algorithm.go](https://github.com/Bytom/bytom/blob/dev-ts-simd/mining/tensority/algorithm.go)
* 改好 gpu 版本后可以参照这个进行调用

7、验证通过后使用 [submit-work](https://github.com/Bytom/bytom/wiki/API-Reference#submit-work) 接口进行提交

提交的结果 也是 BlockHeader type 的

* 使用 _golang_ 的话可以利用 `"github.com/bytom/protocol/bc/types"` 中 `block_header.go` 中的函数 `MmarshalText`
* 使用别的语言的话参考 `"github.com/bytom/protocol/bc/types"` 中 `block.go` 中的函数 `MarshalText`, `WriteTo`, `WriteVarintXXX`. `WriteVarintXXX` 需要参考 [go函数 `binary.PutUvarint`](https://go.googlesource.com/go/+/go1.4.3/src/encoding/binary/varint.go)

8、retarget

见上面，动态调整使矿机每分钟提交三次

9、收益计算

略

### 批量转账

* 主网地址 bm 开头，长度普通地址42，多签62 工具 [https://github.com/Bytom/bytom/tree/master/tools/sendbulktx](https://github.com/Bytom/bytom/tree/master/tools/sendbulktx)
* 每次发币都会生成新的找零地址
* bytom input有21个的限制

