---
layout: post
title: chaincode开发、调试教程以及api介绍
date: 2018-02-23 21:59:00.000000000 +09:00
tags: chaincode 开发调试教程 运维教程 api
---

# Chaincode教程

-  [1. 概要](https://github.com/berryjam/fabric-learning/blob/master/chaincode%E6%95%99%E7%A8%8B.md#1-%E6%A6%82%E8%A6%81)

    - [1.1 什么是Chaincode?](https://github.com/berryjam/fabric-learning/blob/master/chaincode%E6%95%99%E7%A8%8B.md#11-%E4%BB%80%E4%B9%88%E6%98%AFchaincode)
  
    - [1.2 两种角色](https://github.com/berryjam/fabric-learning/blob/master/chaincode%E6%95%99%E7%A8%8B.md#12-%E4%B8%A4%E7%A7%8D%E8%A7%92%E8%89%B2)
  
- [2. chaincode开发者教程](https://github.com/berryjam/fabric-learning/blob/master/chaincode%E6%95%99%E7%A8%8B.md#2-chaincode%E5%BC%80%E5%8F%91%E8%80%85%E6%95%99%E7%A8%8B)

    - [2.1 chaincode API]()
    
    - [2.2 一个简单示例：“资产管理” chaincode]()
    
    - [2.3 安装Hyperledger Fabric示例]()
    
    - [2.4 下载fabric相关docker镜像]()
    
    - [2.5 终端1-启动示例网络]()
    
    - [2.6 终端2-编译&启动chaincode]()
    
    - [2.7 终端3-使用chaincode]()
    
    - [2.8 测试新chaincode]()
    
    - [2.9 chaincode加密]()
    
    - [2.10 管理go语言编写的chaincode外部依赖]()
    
- [3. chaincode运维者教程](https://github.com/berryjam/fabric-learning/blob/master/chaincode%E6%95%99%E7%A8%8B.md#3-chaincode%E8%BF%90%E7%BB%B4%E8%80%85%E6%95%99%E7%A8%8B)

    - [3.1 chaincode生命周期]()
    
    - [3.2 chaincode打包]()
    
    - [3.3 系统chaincode]()

- [4. 参考](https://github.com/berryjam/fabric-learning/blob/master/chaincode%E6%95%99%E7%A8%8B.md#4-%E5%8F%82%E8%80%83)

## 1. 概要

### 1.1 什么是Chaincode？

Chaincode是一个程序，用Go,node.js,java等编程语言编写，并实现了特定的接口（后面会详细介绍，分别为`Init`和`Invoke`）。Chaincode在一个安全的Docker容器中运行，该容器与背书peer进程隔离。Chaincode通过应用程序提交的事务来初始化和管理账本状态。

Chaincode通常处理区块链网络成员商定的业务逻辑，因此可以将其视为“智能合约”。由chaincode创建的状态仅限于该chaincode，不能由另一个chaincode直接访问。然而，在同一个区块链网络中，给定适当的权限，chaincode可以调用另一个chaincode来访问其状态。

### 1.2 两种角色

我们可以从两种不同的角色来认识chaincode。一个是从应用程序开发人员的角度出发，应用开发者会开发一个名为[Chaincode for Developers](https://github.com/berryjam/fabric-learning/blob/master/chaincode%E6%95%99%E7%A8%8B.md#2-chaincode%E5%BC%80%E5%8F%91%E8%80%85%E6%95%99%E7%A8%8B)的区块链应用程序／解决方案；另一个是面向区块链网络运维人员[Chaincode for Operators](https://github.com/berryjam/fabric-learning/blob/master/chaincode%E6%95%99%E7%A8%8B.md#3-chaincode%E8%BF%90%E7%BB%B4%E8%80%85%E6%95%99%E7%A8%8B)，区块链网络运维人员负责管理区块链网络，并利用Hyperledger Fabric API来安装、实例化和升级chaincode，但很可能不会涉及chaincode应用程序的开发。

下面我们将分别从chaincode开发者和运维人员两方面对chaincode做一个较为详细的介绍，最后通过结合源码分析，加深对chaincode的理解。最后希望能帮助chaincode开发者能快速上手chaincode的开发，还有帮助chaincode运维人员能够保证chaincode能正常的运行。

## 2. chaincode开发者教程

### 2.1 chaincode API

每个chaincode程序必须实现`Chaincode接口`：

- [Go](http://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim#ChaincodeStub)

- [node.js](https://fabric-shim.github.io/ChaincodeStub.html)

Chaincode接口被调用以回应接收到的事务。特别是当chaincode接收`instantiate`或`upgrade`事务时，会调用`Init`方法，以便chaincode可以执行任何必要的初始化，包括应用程序状态的初始化。`Invoke`方法是为了响应接收调用事务来处理事务提案。PS：当通过命令行方式peer chaincode invoke XXX 就会调用指定chaincode重写的`Invoke`方法。

chaincode "shim" API的另外一个接口是`ChaincodeStubInterface`:

- [Go](http://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim#ChaincodeStub)

- [node.js](https://fabric-shim.github.io/ChaincodeStub.html)

这个接口用于访问和修改区块账本，并在chaincode之间能够互相调用。

**在本教程中，我们将通过实现一个用于管理简单“资产”的简单chiancode应用来描述如何使用这些API，所有API请参考文末的图1。如`GetState`、`PutState`、`GetHistoryForKey`、`CreateCompositeKey`、`GetStateByPartialCompositeKey`、`DelState`等等，通过这些API基本能完成传统关系型数据的增删改查操作。本教程会在最后分析这些接口具体是什么，怎么用，实现原理是什么。希望能帮助读者更加深入了解chaincode，并能根据自身业务场景选择合适的API，保证区块链有较高的存储速度时，空间效率也不会太低。**

### 2.2 一个简单示例：“资产管理” chaincode

我们的应用程序是一个基本示例chaincode，用于在账本上创建资产（键值对）。

#### 2.2.1 选择代码的位置

如果你之前没用过Go语言编程，你可能需要确保已安装[Go编程语言](http://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html#golang)并且已正确设置好GO的开发环境。

现在，你需要为chaincode应用程序创建一个目录作为`$GOPATH/src/`的子目录。

为了简单起见，我们使用下面的命令：
```
mkdir -p $GOPATH/src/sacc && cd $GOPATH/src/sacc
```

现在，让我们创建即将补充代码的源文件：

```
touch sacc.go
```

#### 2.2.2 基本框架

首先，我们从chaincode的基本框架开始。每个chaincode都一样，都会实现[chaincode接口](http://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim#Chaincode)，即`Init`和`Invoke`函数。因此，让我们添加go import语句以获取chaincode的必要依赖代码（类似于java的import、c语言的#include）。我们将导入chaincode shim包和[peer protobuf package](http://godoc.org/github.com/hyperledger/fabric/protos/peer)。接下来，让我们添加一个结构`SimpleAsset`作为chaincode shim函数的接收方。

```
package main

import (
    "fmt"

    "github.com/hyperledger/fabric/core/chaincode/shim"
    "github.com/hyperledger/fabric/protos/peer"
)

// SimpleAsset implements a simple chaincode to manage an asset
type SimpleAsset struct {
}
```

#### 2.2.3 初始化chaincode

接下来，我们将实现`Init`函数。

```
// Init is called during chaincode instantiation to initialize any data.
func (t *SimpleAsset) Init(stub shim.ChaincodeStubInterface) peer.Response {

}
```

**Note.请注意，chaincode升级也会调用此函数。在编写将升级现有chaincode的chiancode的时候，请确保是当地修改`Init`函数。特别是，如果没有“迁移”或没有任何内容作为升级的一部分进行初始化，请提供一个空洞“Init”方法。**

接下来，我们将使用[ChaincodeStubInterface.GetStringArgs]()函数来获取`Init`调用的参数并检查其有效性。在我们的例子中，我们将获取到一个键值对。

```
// Init is called during chaincode instantiation to initialize any
// data. Note that chaincode upgrade also calls this function to reset
// or to migrate data, so be careful to avoid a scenario where you
// inadvertently clobber your ledger's data!
func (t *SimpleAsset) Init(stub shim.ChaincodeStubInterface) peer.Response {
  // Get the args from the transaction proposal
  args := stub.GetStringArgs()
  if len(args) != 2 {
    return shim.Error("Incorrect arguments. Expecting a key and a value")
  }
}
```

再接下来，现在我们已经确定这个调用成功，我们将把初始状态存储到账本中。要完成这个存储动作，我们将调用函数[ChaincodeStubInterface.PutState](http://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim#ChaincodeStub.PutState)，并将key、value作为参数传入。假设一切顺利，返回一个指示初始化成功的peer.Response对象。

```
// Init is called during chaincode instantiation to initialize any
// data. Note that chaincode upgrade also calls this function to reset
// or to migrate data, so be careful to avoid a scenario where you
// inadvertently clobber your ledger's data!
func (t *SimpleAsset) Init(stub shim.ChaincodeStubInterface) peer.Response {
  // Get the args from the transaction proposal
  args := stub.GetStringArgs()
  if len(args) != 2 {
    return shim.Error("Incorrect arguments. Expecting a key and a value")
  }

  // Set up any variables or assets here by calling stub.PutState()

  // We store the key and the value on the ledger
  err := stub.PutState(args[0], []byte(args[1]))
  if err != nil {
    return shim.Error(fmt.Sprintf("Failed to create asset: %s", args[0]))
  }
  return shim.Success(nil)
}
```

#### 调用 chaincode

首先，我们添加`Invoke`函数的签名。

```
// Invoke is called per transaction on the chaincode. Each transaction is
// either a 'get' or a 'set' on the asset created by Init function. The 'set'
// method may create a new asset by specifying a new key-value pair.
func (t *SimpleAsset) Invoke(stub shim.ChaincodeStubInterface) peer.Response {

}
```

## 3. chaincode运维者教程 

---

## 4. 参考

<div align="center">
<img src="https://github.com/berryjam/fabric-learning/blob/master/markdown_graph/chaincode-class-diagram.jpeg?raw=true">
</div>

<p align="center">
  <b>图 1 chaincode api类图</b><br>
</p>






















