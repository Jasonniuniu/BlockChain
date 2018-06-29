## 区块链+小程序

### geth  

##### 安装

```
https://www.ethereum.org/cli
mac 安装
brew tap ethereum/ethereum brew install ethereum
```

### 启动

#### 第一步

创建一个目录作为以太坊的数据存放目录

#### 第二步

创建一个配置文件用来做创世块以及设置网络genesis.json

```
{
  // nonce 和 mixhash 是作为输入，让每个节点都可以通过计算来做
  // proof-of-work，确认这个区块的挖掘者确实做了足够多的计算找到了合法的
  // nonce 和 mixhash
  "nonce": "0x0000000000000042",
  "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  // difficulty 就是制定了本链一开始的挖矿难度，在我们的私有测试节点中，
  // 这个值设得很低，这样就比较容易挖到矿
  "difficulty": "0x400",
  // alloc 可以预分配一些以太币给某些地址，这里我们不做预分配
  "alloc": {},
  // coinbase 就是当成功挖出 genesis 区块后，接收奖金的地址
  "coinbase": "0x0000000000000000000000000000000000000000",
  // timestamp 本区块挖出来的时间戳，全网将依据前后
  // 两个区块的时间戳之差来调整挖矿的难度
  "timestamp": "0x0",
  // parentHash 指向前一个区块的哈希指针，创世纪区块中的 parentHash 接地
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  // extraData 可用于存储任何信息
  "extraData": "0x",
  // gasLimit 规定了每一个区块中能够消耗的最大的 gas 值，也就事实上
  // 限制了区块的大小
  "gasLimit": "0xffffffff",
  // config 用来为这个私有网络确立一系列参数
  "config": {
      // chainId 是本私有链的 ID
      "chainId": 4224,
      // homesteadBlock 指明 Homestead 版本的兼容的区块开始编号
      "homesteadBlock": 0,
      // EIP155 兼容的区块开始编号
      // EIP155 - "Simple Relay Attack Protection"
      "eip155Block": 0,
      // EIP158 兼容的区块开始编号
      "eip158Block": 0
  }
}
```
{
  // proof-of-work，确认这个区块的挖掘者确实做了足够多的计算找到了合法的
  // nonce 和 mixhash 是作为输入，让每个节点都可以通过计算来做
  "nonce": "0x0000000000000042",
  "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "difficulty": "0x400",  // difficulty 就是制定了本链一开始的挖矿难度，在我们的私有测试节点中，这个值设得很低，这样就比较容易挖到矿
  "alloc": {},   // alloc 可以预分配一些以太币给某些地址，这里我们不做预分配
  "coinbase": "0x0000000000000000000000000000000000000000",  // coinbase 就是当成功挖出 genesis 区块后，接收奖金的地址
  "timestamp": "0x0",   // timestamp 本区块挖出来的时间戳，全网将依据前后，两个区块的时间戳之差来调整挖矿的难度
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",  // parentHash 指向前一个区块的哈希指针，创世纪区块中的 parentHash 接地
  "extraData": "0x",   // extraData 可用于存储任何信息
  "gasLimit": "0xffffffff",  // gasLimit 规定了每一个区块中能够消耗的最大的 gas 值，也就事实上限制了区块的大小

  // config 用来为这个私有网络确立一系列参数
  "config": {
      "chainId": 4224,     // chainId 是本私有链的 ID
      "homesteadBlock": 0, // homesteadBlock 指明 Homestead 版本的兼容的区块开始编号
      "eip155Block": 0,    // EIP155 兼容的区块开始编号,EIP155 - "Simple Relay Attack Protection"
      "eip158Block": 0     // EIP158 兼容的区块开始编号
  }
}

#### 第三步

初始化genesis 文件

```
cd ~/chainwork/private 目录
geth --datadir . init genesis.json
```

#### 第四步

启动节点

```
geth --datadir . --networkid 4224 --rpc --rpcport 8545
--port 30303 --rpccorsdomain="*" -rpcapi eth,web3,personal,net console 2> log.txt
```



### Remix 开发介绍

Remix 是一个 IDE (integrated development environment 集成开发环境)，用于智能合约开发，使用的语言是 solidity，是一个基于浏览器的 IDE，也是以太坊官方的 IDE。Remix 在线 IDE： 

[http](http://remix.ethereum.org/)[://remix.ethereum.org](http://remix.ethereum.org/)[/](http://remix.ethereum.org/)

开发智能合约(集成了 solidity 编辑器)

访问已部署的智能合约的状态和属性

代码分析，编译错误检查

调试和测试 DApp



### 在网页当中调用以太坊节点

#### 第一步

引入web3.js 文件

```
<script src="js/web3.js"></script>
```

#### 第二步

```
//跟以太坊节点建立连接
var web3 = new Web3(new Web3.providers.HttpProvider("http://127.0.0.1:8545"));
```

#### 第三步

调用以太坊账户数据

```
获取账号
web3.eth.getAccounts(function(error,result){
     if(!error){
     		console.log(result);
     }
});
//创建账户
web3.personal.newAccount("123456",function(error,result){
    console.log(error);
    console.log(result);
});
//解锁账户                web3.personal.unlockAccount("0xc4c25a268ca6ab65c449c7aefc6eddfc5f9134fb","123456",function(error,result){
      console.log(result);
});
```

### 在微信小程序当中调用以太坊数据

#### 第一步  node.js 服务器端

```
搭建node.js 的基本环境，然后启动 node.js
var Web3=require("web3");
var web3 = new Web3(new Web3.providers.HttpProvider("http://127.0.0.1:8545"));
var abi=[
    {
        "constant": false,
        "inputs": [
            {
                "name": "_message",
                "type": "string"
            }
        ],
        "name": "setMessage",
        "outputs": [],
        "payable": false,
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "constant": true,
        "inputs": [],
        "name": "getMessage",
        "outputs": [
            {
                "name": "",
                "type": "string"
            }
        ],
        "payable": false,
        "stateMutability": "view",
        "type": "function"
    }
];
var address="0xd069af98379fe4326c33f6718e0ce820c0f63a55";
var message = new web3.eth.Contract(abi,address);
router.get("/getMessage",function(req,resp){
		message.methods.getMessage().call(function(error,result){
                resp.send(result);
        });
});
```

#### 第二步 

在小程序端掉用以太坊的数据

```
调用小程序提供的api 去请求中心化的服务器node.js 的数据
wx.request({
      url: 'http://localhost:3000/user/getMessage', //仅为示例，并非真实的接口地址
      success: function (res) {
        console.log(res.data)
        _this.setData({
            message:res.data
        })
      }
})
```



















