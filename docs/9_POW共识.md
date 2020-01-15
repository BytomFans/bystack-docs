---
id: docs_9
title: POW共识
sidebar_label: POW共识
---

**AI-PoW**

Tensority算法利用种子和区块头哈希作为输入值并产生相应的工作量。

**算法流程**

Tensority算法总体分为以下几个步骤：

1. 生成seed
1. 生成cache
1. 构造矩阵列表
1. 矩阵乘操作
1. 工作量生成
1. 工作量验证

**生成seed**

种子seed是生成cache的输入值。

种子每256个块更新一次。第0～256块（此处有257个块）使用第0块的区块头哈希，第257～512块使用第256块的区块头哈希，第513～768块使用第512块的哈希，以此类推。

**生成cache**

缓存cache由seed生成，seed和cache是相互对应的关系，因此cache的产生周期也与seed相同。cache的作用就是填充256个矩阵组成的列表，因为每个矩阵行列均为256，每个元素为长度为一个字节的整型数据int8，所以cache的大小为256X256X256X1B=16777216B=16MB。生成cache算法使用的是Scrypt算法，Scrypt算法在计算过程中会产生相应的伪随机数集合，以此作为cache值。由于Scrypt算法的特点，需要输入一个128字节的seed作为输入值。因此，首先需要将2.1得到的32字节的seed值进行扩展，得到一个128字节的扩展种子extseed。之后将extseed进行循环128次Scrypt中的smix过程，将128次过程中产生的随机数集合连接起来组成cache。具体算法如Algorithm calcSeedCache.

```
Algorithm: 根据种子生成缓存（calcSeedCache）
------------------------------------------------
Input: seed
Output: cache
------------------------------------------------
1   Initialize extround = 3, scryptround = 128;
2   extseed = seed;
3   tmphash = seed;
4   for i = 1; i <= extround; i++ do
5       tmphash = SHA256(tmphash);
6       extseed = append(extseed, tmphash);
7   end
8   cache = null;
9   tmpv = null;
10  for j = 1; j <= scryptround; j++ do
11      tmpv = Scrypt.smix(extseed, tmpv);
12      cache = append(cache, tmpv);
13  end
14  return cache
------------------------------------------------
```

**构造矩阵列表**

16MB的cache分成128组，每一组中有32X1024个元素，每个元素值类型为uint32。每一组中，将32个元素视为一个单元，由此可以得到一个规格为32X1024X128的缓存矩阵列表tmpmatrix，每个元素为uint32。重组矩阵列表recomposedmatrix规格也与tmpmatrix相同。tmpmatrix中奇数索引的元素值与recomposedmatrix中的1至512号元素相互对应。类似，tmpmatrix中偶数索引的元素值与recomposedmatrix中的513至1024号元素相互对应。之后将之转化为规格为256X256X256的矩阵列表cachematrix，每个矩阵元素为一个字节的int8。具体算法如Algorithm constructCacheMatrix。

```
Algorithm: 构造缓存矩阵（constructCacheMatrix）
------------------------------------------------------------------------------
Input: cache
Output: cachematrix
------------------------------------------------------------------------------
1   Initialize dim1 = 32; dim2 = 1024; dim3 = 128; dim = 256;
2   tmpmatrix = Matrix(cache, dim1, dim2, dim3);
3   recomposedmatrix = NewMatrix(dim1, dim2, dim3);
4   cachematrix = NewMatrix(dim, dim, dim);
5   recomposedmatrix[:][1:dim2/2][:] = tmpmatrix[:][all odd index][:];
6   recomposedmatrix[:][dim2/2+1:dim2][:] = tmpmatrix[:][all even index][:];
7   cachematrix = Float64(Matrix(Int8Array(recomposedmatrix), dim, dim, dim)));
8   return cachematrix;
------------------------------------------------------------------------------
```

**矩阵乘操作**

矩阵乘之前需要生成矩阵乘的索引，将区块头哈希值分成4组，之后将每一组进行哈希运算，得到32字节的哈希值。每个字节的值正好取值为0至255，对应着256个矩阵。矩阵相乘的过程中将每个矩阵元素由int8类型转化为int32类型。相乘的中间结果是int32类型，这个时候可以将int32数值的低位两个字节相加取模256，得到一个新的值，这就完成了32位转8位的操作。两个矩阵乘相乘遵从线性代数的运算法则，左矩阵与右矩阵的转置矩阵相乘。具体算法如Algorithm constructHashMatrix。

```
Algorithm: constructHashMatrix
------------------------------------------------------------------------------
Input: cachematrix, headerhash
Output: hashmatrix
------------------------------------------------------------------------------
1   Initialize drawround = 4; mulround = 2; dim = 256;
2   hashmatrix = Matrix(dim, dim);
3   drawmatrix = Matrix(headerhash, drawround, sizeof(headerhash)/drawround);
4   for i = 1; i <= drawround; i++ do
5       ma = I;
6       mc = Matrix(dim, dim);
7       sequence = SHA256(drawmatrix[i]);
8       for j = 1; j <= mulround; j++ do
9           for k = 1; k <= sizeof(sequence); k++ do
10              index = uint8(sequence[k])+1;
11              mb = srcmatrix[index][:][:];
12              mc = ma * mb.T();
13              for element ∈ mc do
14                  element = Float64(Compress32to8(Int32(element)));
15              end
16              ma = mc
17          end
18      end
19      for row = 1; row <= dim; row++ do
20          for col = 1; col <= dim; col++ do
21              i32vhashmatrix = Int32(hashmatrix[row][col]);
22              i32vma = Int32(ma[row][col]);
23              i8v = Int8(i32vhashmatrix+i32vma);
24              hashmatrix[row][col] = Float64(i8v);
25          end
26      end
27  end
28  return hashmatrix;
------------------------------------------------------------------------------
```

**工作量生成**

为了提高计算效率，生成工作量时使用FNV算法简化矩阵。因为FNV函数的特性，需要将四个元素合并组成一个uint32类型的数字，组成新的256X64的矩阵mat32，然后进行FNV简化操作。之后将简化的结果进行哈希运算。具体算法如 Algorithm _Binary Forwarded FNV_.

```
Algorithm: Binary Forwarded FNV
---------------------------------------------------------------
Input: mat32
Output: hash
---------------------------------------------------------------
1   Initialize dim1 = 256; dim2 = 64;
2   for k = dim1; k > 1; k = k/2 do
3       for i = 1; i <= k; i++ do
4           for j = 1; j <= dim2; j++ do
5               mat32[i][j] = FNV(mat32[i][j], mat32[i+k][j]);
6           end
7       end
8   end
9   hash = SHA256(ToByteArray(mat32[0][:]);
10  return hash;
---------------------------------------------------------------
```

**工作量验证**

将2.5得到的哈希值与区块要求的难度值相比较，小于规定值则说明挖矿成功，否则更换随机数重新生成区块头，开始新的计算。

**随机性分析**

可以参考[Tensority算法白皮书](https://github.com/Bytom/bytom/wiki/download/tensority-v1.2.pdf)。
