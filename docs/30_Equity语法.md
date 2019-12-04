---
id: docs_30
title: Equity语法
sidebar_label: Equity语法
---

#### 合约组成

`Equity`合约程序是由一个用`contract`关键字定义的合约结构组成。一个合约的形式为：
`contract ContractName ( parameters ) locks valueAmount of valueAsset { clauses }`

- `ContractName` 合约名，是一个标识符，代表合约的名称，在编写合约时自定义。
- `parameters` 合约参数列表，其类型名必须是合约语言的基本类型
- `valueAmount` 合约锁定的资产数量，即`UTXO`中的`amount`，标识符可以自定义
- `valueAsset` 合约锁定的资产类型，即`UTXO`中的`assetID`，标识符可以自定义
- `clauses` 条款（即函数）列表，一个合约至少包含一个`clause`

#### 条款组成

每个`clause`条款函数描述了一种解锁合约`UTXO`的方法和解锁所需的参数信息。`clause`的结构为：
`clause ClauseName ( parameters ) { statements }`

- `ClauseName` 条款名，是一个标识符，代表条款函数的名称，在编写时自定义。
- `parameters` 条款参数列表
- `statements` 合约语句列表，一个`clause`至少包含一条语句

#### 参数列表

合约和条款的参数需指明变量名和变量类型。参数定义的格式是：

- `name : TypeName`

参数列表的格式为：

- `name1 : TypeName1 , name2 : TypeName2 , …`

为简洁起见，可以像这样合并相同类型的相邻参数：

- `name1 , name2 , … : TypeName`

所以这两种合约的声明是等价的：

- `contract LockWithMultiSig(key1: PublicKey, key2: PublicKey, key3: PublicKey)`
- `contract LockWithMultiSig(key1, key2, key3: PublicKey)`

可用的变量类型有：

- `Integer` `Amount` `Boolean` `String` `Hash` `Asset` `PublicKey` `Signature` `Program`

在数据类型中将会详细介绍这些变量类型。

#### 语句组成

`statement`合约语句，除了`verify`、`lock`和`unlock`基本语句类型之外，目前还新增加了`define`、`assign`和`if-else`扩展语句类型的支持，语句的格式如下：

- `verify`验证条件语句，用来验证表达式的结果是否为真，模式如下:`verify expression`  其中`expression`的结果必须是`bool`类型，`expression`表达式的结果必须为`true`时表示验证成功。示例如下：
- `verify above(blockNumber)` 检测当前块的区块高度是否高于 `blockNumber`。
- `verify checkTxSig(key, sig)` 检测给定的签名是否与预先设定的公钥相匹配。
- `verify newBid > currentBid` 检测`newBid`是否大于`currentBid`。  
- `lock` 锁定合约资产语句，模式如下：
  `lock valueAmount of valueAsset with program`
  其中`valueAmount`表示资产数量，`valueAsset`表示资产类型，而`program`表示接收对象且必须为`Program`类型。  

- `unlock` 解锁合约资产语句，模式如下：
  `unlock valueAmount of valueAsset`
 
其中`valueAmount`表示资产数量，`valueAsset`表示资产类型，`unlock`语句表示解锁的资产可以指定给任意接收对象。  

- `define`自定义变量语句，模式如下：`define identifier : TypeName = expression`或`define identifier : TypeName`  其中`identifier`表示用户定义了数据类型为`TypeName`的变量，如果自定义的变量没有赋值，则该变量必须`assign`语句中赋值。示例如下：
  - `define value : Integer = amount` 定义了整型的变量`value`，并且将`amount`赋值给该变量。
  - `define value : Integer = amount + shift` 定义了整型的变量`value`，并且将表达式`amount + shift`的结果赋值给该变量。
  - `define value : Integer` 定义了整型的变量`value`，并没有赋值，该变量需要在`assign`语句中赋值，否则会报错“变量未赋值”。  
- `assign`自定义变量赋值语句，模式如下`assign identifier = expression`  其中`identifier`必须为`define`语句中用户自定义的变量，禁止修改`contract`和`clause`中的变量。示例如下：
  - `assign value = amount` 将`amount`赋值给变量`value`，并且变量`amount`和`value`的数据类型必须相同。  
- `if-else` 条件判断语句，模式如下：
  `if expression { statements }` 或 `if expression { statements } else { statements }`
  其中`expression`为`if-else`语句的条件判断表达式，并且该表达式的结果必须是`bool`类型，当结果为`true`时执行`if`下面的`statements`语句块，否则执行`else`下面的`statements`语句块。

#### 数据类型

Equity语言支持的数据类型如下：

- `Boolean` 布尔类型，值为`true`或`false`.
- `Integer` 整数类型，取值范围为`[-2^63, 2^63-1]`.
- `Amount` 无符号整数类型，取值范围为`[0, 2^63-1]`.
- `Asset` 资产类型，32个字节长度的资产ID.
- `Hash` 哈希类型，32个字节长度的`hash`值.
- `PublicKey` 公钥类型，32个字节长度的`publickey`.
- `Signature` 签名类型，该类型需要根据`publickey`对应的主公钥`root_xpub`和`derivation_path`来构造，且只能用于`clause`的参数列表中.
- `Program` 程序类型，接收`program`，跟地址是一一对应.
- `String` 字符串类型，16进制字符串.

#### 表达式

`Equity`表达式可用于上述语句中的`expression`中，支持的表达式类别如下：

- 一元表达式
  - `- expr` : 对数学表达式取负值
  - `~ expr` : 对字节串做按位翻转  
- 条件表达式，下面的表达式都必须为数字类型操作数(即`Integer`或`Amount`类型)，并且返回一个`Boolean`型的结果：
  - `expr1 > expr2` : 检测`expr1`是否大于`expr2`
  - `expr1 < expr2` : 检测`expr1`是否小于`expr2`
  - `expr1 >= expr2` : 检测`expr1`是否大于或等于`expr2`
  - `expr1 <= expr2` : 检测`expr1`是否小于或等于`expr2`
  - `expr1 == expr2` : 检测`expr1`是否等于`expr2`
  - `expr1 != expr2` : 检测`expr1`是否不等于`expr2`  
- 按位操作表达式，下面的表达式为字节类型，且返回值也是字节类型：
  - `expr1 ^ expr2` : 得到两操作数按位异或(XOR)的结果
  - `expr1 | expr2` : 得到两操作数按位或(OR)的结果
  - `expr1 & expr2` : 得到两操作数按位与(AND)的结果  
- 数值表达式，下面的表达式都是数值型操作数(`Integer`或`Amount`)，并且返回数值型的结果：
  - `expr1 + expr2` : 两操作数相加
  - `expr1 - expr2` : 两操作数相减，`expr1`减去`expr2`
  - `expr1 * expr2` : 两操作数相乘
  - `expr1 / expr2` : 两操作数相除，`expr1`除以`expr2`
  - `expr1 % expr2` : 操作数取余，即`expr1`对`expr2`取余
  - `expr1 << expr2` : 将`expr1`按位左移`expr2`位
  - `expr1 >> expr2` : 将`expr1`按位右移`expr2`位  
- 其他的表达式类型：
  - `( expr )` : 表示`expr`本身
  - `expr ( arguments )` : 表示函数调用，传入的参数列表`arguments`是用逗号分隔的，具体可查阅下面的内置函数
  - 单独出现的标识符表示变量本身
  - `[exprs]` : 表示一个列表，其中`exprs`是以逗号分隔的表达式列表(列表参数目前仅用于`checkTxMultiSig`中)
  - 以 `-` 开头的一串数字序列表示整型数据列表
  - 单引号 `'...'` 之间的字节序列表示一个字符串
  - 前缀 `0x` 后跟 `2n` 个十六进制数字，其长度为`n`

#### 内置函数

`Equity`提供了一下内置函数，相关函数如下：

- `abs(n)` 返回数值`n`的绝对值.
- `min(x, y)` 返回两个数值`x`和`y`中最小的一个.
- `max(x, y)` 返回两个数值`x`和`y`中最大的一个.
- `size(s)` 返回任意类型的字节大小`size`.
- `concat(s1, s2)` 返回连接两个字符串`s1`和`s2`生成新的字符串.
- `concatpush(s1, s2)` 将两个字符串类型的虚拟机执行操作码`s1`和`s2`连接起来(即将`s2`拼接在`s1`的后面），然后将他们`push`到栈中. 该操作函数主要用于嵌套合约中.
- `below(height)` 判断当前区块高度是否低于参数`height`，如果是则返回`true`，否则返回`false`.
- `above(height)` 判断当前区块高度是否高于参数`height`，如果是则返回`true`，否则返回`false`.
- `sha3(s)` 返回字节类型字符串参数`s`的`SHA3-256`的哈希运算结果.
- `sha256(s)` 返回字节类型字符串参数`s`的`SHA-256`的哈希运算结果.
- `checkTxSig(key, sig)` 根据一个`PublicKey`和一个`Signature`验证交易的签名是否正确.
- `checkTxMultiSig([key1, key2, ...], [sig1, sig2, ...])` 根据多个`PublicKey`和多个`Signature`验证交易的多重签名是否正确.

