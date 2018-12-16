# 第二周打卡

## Algorithm

题目

Given an array of integers `nums`, write a method that returns the "pivot" index of this array.

We define the pivot index as the index where the sum of the numbers to the left of the index is equal to the sum of the numbers to the right of the index.

If no such index exists, we should return -1. If there are multiple pivot indexes, you should return the left-most pivot index.

**Example 1:**

```
Input: 
nums = [1, 7, 3, 6, 5, 6]
Output: 3
Explanation: 
The sum of the numbers to the left of index 3 (nums[3] = 6) is equal to the sum of numbers to the right of index 3.
Also, 3 is the first index where this occurs.
```

**Example 2:**

```
Input: 
nums = [1, 2, 3]
Output: -1
Explanation: 
There is no index that satisfies the conditions in the problem statement.
```

**Note:**

The length of `nums` will be in the range `[0, 10000]`.

Each element `nums[i]` will be an integer in the range `[-1000, 1000]`.

### 解题思路

首先把所有可能的答案放到一个数组里，如果是空的话则返回 -1，否则返回第一个。最简单办法就是，移动索引分别计算到左边和到右边的和，然后比较，如果相同就保存这个索引。

```java
class Solution {
    public int pivotIndex(int[] nums) {
        if (nums.length == 0) {
            return -1;
        }
        if (nums.length == 1) {
            return 0;
        }
        ArrayList<Integer> pivotIndexs = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            // i is the index
            int leftSum = 0, rightSum = 0;
            for (int left = 0; left < i; left++) {
                leftSum += nums[left];
            }
            for (int right = nums.length - 1; i < right; right--) {
                rightSum += nums[right];
            }
            if (leftSum == rightSum) {
                pivotIndexs.add(i);
            }
        }
        if (!pivotIndexs.isEmpty()) {
            return pivotIndexs.get(0);
        }
        return -1;
    }
}
```

看了答案后，的确有更简洁的办法，那就是先计算总和，然后遍历数组并判断单当前和两倍等于总和减去当前数时，这个索引便是答案。这样可以降低一定程度的时间复杂度。

## Review

原文：[REST API: Java Spring Boot and MongoDB](https://medium.com/@gtommee97/rest-api-java-spring-boot-and-mongodb-4dffbcabbaf5)

读后感：

- MongoDB是最受欢迎的NoSQL数据库之一。特别适合结合Spring Boot开发REST API，以下几点关键原因
  - JSON格式存储
  - 天然的Spring Boot连接器
- 整个示例工程很快就能搭建起来，关键不用在写一些逻辑进行数据处理，直接返回数据内容（即JSON）。



## Tip

### Linux

- Linux查询挂载磁盘状态

```shell
df -H
```

- 挂载某个磁盘或PV到本地目录

```shell
sudo mount -t filesystem_type ip_addr:/volume /local_path
# examples
sudo mount -t glusterfs 99.12.228.121:/vol_1d29f8bcf83a /home/michael/gfs_prod
```

### Git使用

- git pull / git fetch

与git pull相比git fetch相当于是从远程获取最新版本到本地，但不会自动merge。如果需要有选择的合并git fetch是更好的选择。效果相同时git pull将更为快捷。

## Share

- Fabric chaincode develop

该书册适用于Fabric平台，详细请参考[官方介绍](https://hyperledger-fabric.readthedocs.io/en/latest/chaincode.html)及[开发者手册](https://hyperledger-fabric.readthedocs.io/en/latest/chaincode4ade.html)。

### 智能合约是什么

智能合约（Chaincode，链码）是一个用Go，node.js或Java编写的程序，它实现了一个指定的接口。 Chaincode运行在与背书节点进程隔离的安全Docker容器中。 Chaincode通过应用程序提交的交易来初始化和管理账本状态。

在给定适当的许可的情况下，链代码可以在相同的通道或不同的通道中调用另一个链代码来访问其账本状态。 请注意，如果被调用的链代码与调用链代码位于不同的通道上，则只允许读取查询。 也就是说，不同通道上的被调用链代码只是一个Query，它在后续提交阶段不参与状态验证检查。 

### Chaincode API

每个chaincode必须实现Chaincode接口

- [Go](https://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim#Chaincode)
- [node.js](https://fabric-shim.github.io/ChaincodeInterface.html)
- [Java](https://fabric-chaincode-java.github.io/org/hyperledger/fabric/shim/Chaincode.html)

调用其方法以响应收到的交易。特别是当链代码接收实例化或升级事务时调用Init方法，以便链码可以执行任何必要的初始化，包括应用程序状态的初始化。 调用Invoke方法以响应接收调用交易以处理交易投案。

链码“shim”API中的另一个接口是ChaincodeStubInterface：

- [Go](https://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim#ChaincodeStubInterface)
- [node.js](https://fabric-shim.github.io/ChaincodeStub.html)
- [Java](https://fabric-chaincode-java.github.io/org/hyperledger/fabric/shim/ChaincodeStub.html)

用于访问和修改账本，以及在链代码之间进行调用。

### 开发步骤

#### 初始化链码

```go
// Init is called during chaincode instantiation to initialize any data.
func (t *SimpleAsset) Init(stub shim.ChaincodeStubInterface) peer.Response {

}
```

#### 调用链码

在我们的例子中，我们的应用程序将只有两个函数：set和get，它们允许设置资产的值或检索其当前状态。 我们首先调用ChaincodeStubInterface.GetFunctionAndParameters来为该链代码应用程序函数提取函数名称和参数。

```go
// Invoke is called per transaction on the chaincode. Each transaction is
// either a 'get' or a 'set' on the asset created by Init function. The Set
// method may create a new asset by specifying a new key-value pair.
func (t *SimpleAsset) Invoke(stub shim.ChaincodeStubInterface) peer.Response {
    // Extract the function and args from the transaction proposal
    fn, args := stub.GetFunctionAndParameters()

}
```

接下来，我们将函数名称验证为set或get，并调用这些链代码应用程序函数，通过shim.Success或shim.Error函数返回适当的响应，该函数将响应序列化为gRPC protobuf消息。

然后把实现这两各接口的代码整合成智能合约。

