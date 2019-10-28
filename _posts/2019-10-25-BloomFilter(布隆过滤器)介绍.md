---
layout: post
title: BloomFilter(布隆过滤器)介绍
date: 2019-10-25 17:37:00.000000000 +09:00
tags: BloomFilter bitmap 布隆过滤器 位图 数据结构
---


# BloomFilter(布隆过滤器)介绍

**Note. 本篇介绍一种存储结构-布隆过滤器。这种存储结构类似于散列表，能够存储和查询元素是否存在，并且存储效率一般要比散列表高很多。应用场景有，比如爬虫应用会应用它来进行URL去重，避免重复爬取相同网页。在此之前，再介绍一种数据结构-位图。本质上，布隆过滤器是一种改进后的位图，存储效率更高。**

- [1.有1000万个整数，范围在1～1亿之间，如何快速判断某个整数是否存在？](https://github.com/berryjam/berryjam.github.io/blob/master/_posts/2019-10-25-BloomFilter(%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8)%E4%BB%8B%E7%BB%8D.md#1-%E6%9C%891000%E4%B8%87%E4%B8%AA%E6%95%B4%E6%95%B0%E8%8C%83%E5%9B%B4%E5%9C%A811%E4%BA%BF%E4%B9%8B%E9%97%B4%E5%A6%82%E4%BD%95%E5%BF%AB%E9%80%9F%E5%88%A4%E6%96%AD%E6%9F%90%E4%B8%AA%E6%95%B4%E6%95%B0%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8)

- [2. 位图](https://github.com/berryjam/berryjam.github.io/blob/master/_posts/2019-10-25-BloomFilter(%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8)%E4%BB%8B%E7%BB%8D.md#2-%E4%BD%8D%E5%9B%BE)

- [3. 布隆过滤器](https://github.com/berryjam/berryjam.github.io/blob/master/_posts/2019-10-25-BloomFilter(%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8)%E4%BB%8B%E7%BB%8D.md#3-%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8)

- [4. 参考资料](https://github.com/berryjam/berryjam.github.io/blob/master/_posts/2019-10-25-BloomFilter(%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8)%E4%BB%8B%E7%BB%8D.md#4-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)


## 1. 有1000万个整数，范围在1～1亿之间，如何快速判断某个整数是否存在？

在介绍位图之前，我们先看一个问题：**假设有1千万个整数，整数范围在1到1亿之间。如何快速查找某个整数是否在这1千万个整数中呢？**

我们可能马上想到用散列表来解决，并且散列表是可以动态扩容的。散列表的每个元素所占用的空间至少为27bit，取整后为4个字节（100,000,000至少需要27位，2^26为6700万，存放不下1亿），那么1千万个整数，最坏的情况下需要4000万字节，大概38MB多。如果整数数量不止1千万个，假设是8千万个，那么需要的内存空间又增长至8倍，304MB多，所占用的内存空间与整数数量呈正比例增长。

## 2. 位图


## 3. 布隆过滤器



## 4. 参考资料

[[1]](https://blog.csdn.net/ce123_zhouwei/article/details/6971544) 详解大端模式和小端模式

[[2]](http://dwarfstd.org/doc/dwarf-2.0.0.pdf) DWARF Debugging Information Format 附录4第97页. uleb128、sleb128算法伪代码

[[3]](https://www.cnblogs.com/liwugang/p/7594093.html) LEB128相关知识
