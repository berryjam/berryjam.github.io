---
layout: post
title: LEB128(Little-Endian Base 128)格式介绍
date: 2019-09-01 23:11:00.000000000 +09:00
tags: LEB128 变长编码 大小端
---


# LEB128(Little-Endian Base 128)格式介绍

**Note. 本篇介绍Andorid系统在Dex文件采用LEB128变长编码格式，相对固定长度的编码格式，leb128编码存储利用率比较高，能让Dex文件尽可能的小。对于存储空间比较紧缺的移动设备，这非常有用。其中LEB128可以分为无符号(ULEB128)、有符号整数编码(SLEB128)，其中还包括一种特殊的无符号整数编码(ULEB128p1 unsigned LEB128 plus 1)。下面将分别具体介绍，并给出相关的编码、解码代码，在区块链领域内的应用场景有智能合约编码，希望能带来启发。**

- [1. 小端表示法](https://github.com/berryjam/berryjam.github.io/blob/master/_posts/2019-09-01-LEB128(Little-Endian%20Base%20128)%E6%A0%BC%E5%BC%8F%E4%BB%8B%E7%BB%8D.md#1-%E5%B0%8F%E7%AB%AF%E8%A1%A8%E7%A4%BA%E6%B3%95)

- [2. ULEB128(unsigned LEB128，无符号整数编码)](https://github.com/berryjam/berryjam.github.io/blob/master/_posts/2019-09-01-LEB128(Little-Endian%20Base%20128)%E6%A0%BC%E5%BC%8F%E4%BB%8B%E7%BB%8D.md#2-uleb128unsigned-leb128%E6%97%A0%E7%AC%A6%E5%8F%B7%E6%95%B4%E6%95%B0%E7%BC%96%E7%A0%81)

- [3. SLEB128(signed LEB128，有符号整数编码)](https://github.com/berryjam/berryjam.github.io/blob/master/_posts/2019-09-01-LEB128(Little-Endian%20Base%20128)%E6%A0%BC%E5%BC%8F%E4%BB%8B%E7%BB%8D.md#3-sleb128signed-leb128%E6%9C%89%E7%AC%A6%E5%8F%B7%E6%95%B4%E6%95%B0%E7%BC%96%E7%A0%81)

- [4. ULEB128p1(unsigned LEB128 plus 1，特殊无符号整数编码)](https://github.com/berryjam/berryjam.github.io/blob/master/_posts/2019-09-01-LEB128(Little-Endian%20Base%20128)%E6%A0%BC%E5%BC%8F%E4%BB%8B%E7%BB%8D.md#4-uleb128p1unsigned-leb128-plus-1%E7%89%B9%E6%AE%8A%E6%97%A0%E7%AC%A6%E5%8F%B7%E6%95%B4%E6%95%B0%E7%BC%96%E7%A0%81)

- [5, 参考资料](https://github.com/berryjam/berryjam.github.io/blob/master/_posts/2019-09-01-LEB128(Little-Endian%20Base%20128)%E6%A0%BC%E5%BC%8F%E4%BB%8B%E7%BB%8D.md#5-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)





## 1. 小端表示法



## 2. ULEB128(unsigned LEB128，无符号整数编码)


## 3. SLEB128(signed LEB128，有符号整数编码)


## 4. ULEB128p1(unsigned LEB128 plus 1，特殊无符号整数编码)


## 5. 参考资料
