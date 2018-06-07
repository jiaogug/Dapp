# Dapp（unbox 搭建）
### 整体流程，终于搞懂了，amazing~

    1.创建 Dapp 项目
    2.在项目中的 contracts 文件夹中编写合约
    3.在以太坊网络上部署合约，要交钱的，但是可以免费获取测试环境的以太币...
    4.本地编译合约，形成 .json 的 abi
    5.在前端调用
    sixsixsix~饿美zing~斯钙伊~
### 创建Dapp项目

>#### 安装 Node 

自行下载安装，node -v 查看版本信息，显示则安装成功

>#### 安装以太坊客户端，即上面说的 Dapp

由于整个合约代码的执行需要一个虚拟的环境，所以需要安装一个是新以太坊虚拟机的节点，教程选择 TestRPC，现在已经改名字叫 ganache-cli:

`sudo npm install ganache-cli`

老版: `sudo npm install ethereumjs-testrpc truffle`

此项目用的老版

>#### 创建 Dapp 项目

创建空文件夹,yan后：

`truffle unbox react-box`

静等5分钟...

>#### 项目结构

`contracts`:编写智能合约的文件夹，所有智能合约都放置这这个里面

`migration`:部署合约配置的文件夹

`src`:React 的前端调用代码

`test`:只能合约测试用例文件夹

>#### 编写智能合约

在`contract`文件夹下创建见 Voting.sol 文件，这个就是将来要部署到以太坊官网上的合约，所以代码要精简，以为是要钱的，下面是个例子，拷贝以下代码：

    pragma solidity ^0.4.4;

    contract Voting {

      // liyuechun -> 10
      // xietingfeng -> 5
      // liudehua -> 20
      mapping (bytes32 => uint8) public votesReceived;

      // 存储候选人名字的数组
      bytes32[] public candidateList;

      // 构造函数 初始化候选人名单
      function Voting(bytes32[] candidateNames) {

        candidateList = candidateNames;
      }

      // 查询某个候选人的总票数
      function totalVotesFor(bytes32 candidate)  constant returns (uint8) {
        require(validCandidate(candidate) == true);
        // 或者
        // assert(validCandidate(candidate) == true);
        return votesReceived[candidate];
      }

      // 为某个候选人投票
      function voteForCandidate(bytes32 candidate) {
        assert(validCandidate(candidate) == true);
        votesReceived[candidate] += 1;
      }

      // 检索投票的姓名是不是候选人的名字
      function validCandidate(bytes32 candidate) constant returns (bool) {
        for(uint i = 0; i < candidateList.length; i++) {
          if (candidateList[i] == candidate) {
            return true;
          }
        }
        return false;
      }
    }

>#### 通过 remix + metamask 将写好的合约部署到 Kovan Test Net

在Google浏览器中安装 `MetaMask` 插件，小狐狸，注册账号

打开 [remix-IDE](http://remix.ethereum.org/)将合约拷贝带里面

>##### 注意

        1.MetaMask钱包一定是等于状态（？）,并且一定要有一定的以太币支付给旷工
        
        2.确保Environment是Injected Web3，如果切换不过来，关掉浏览器重新启动
        
        3.在create函数中输入一个数组，数组里面的内容为候选人名单
        
        4.点击create按钮，会弹出MetaMask界面让你确认，确认提交，过一会儿，合约就部署成功
      
        5.可以测试给某个候选人投票，查询某个候选人的票数
        
由于我的以太币还没有搞下来，so...以上

如何搞到 Testnet 以太币，[查看链接](https://blog.csdn.net/fidelhl/article/details/52573274)

我的测试币搞下来了，但是，在官方的编辑器上总是编译不了，太坑了，也没有好的解决方案，妈卖批

nnd,这个官方的编辑器炸了，靠腰，改用国内的编辑器，也可以正常部署，[点击](http://remix2.ju3ban.net)

##### 接着
在 Compile 之后，点击 Run 配置网络环境，选择`injected Web3`，意思是发布到公链，`Account` 是部署合约的钱包地址，也就是 狐狸的地址，（点击 ... 然后 点击 `Copy Address to clipboard`就会获得钱包地址），注意钱包里必须有钱！！！其他配置走默认。

完了就支付给矿工代币，等待一会儿会儿，显示 Success 之后，就完成部署了。回到编辑器的 Run 页面，会看到部署成功之后产生的地址信息，以及暴露的方法，注意，以后读取数据是免费的，如果是写入数据，还是需要代币！！！
    
