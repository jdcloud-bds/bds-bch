# bds-bch 
## 介绍
bds-bch 是开源项目区块链数据服务（BDS）的其中一个独立模块 - 提供全节点服务。

*bds-btc* 基于 [Bitcoin-ABC/bitcoin-abc](https://github.com/Bitcoin-ABC/bitcoin-abc) 的 v0.18.1 版本进行二次开发，支持直接将新增的区块数据往消息中间件服务 kafka 发送，方便上游的服务来订阅消费。

## 架构
![架构](./doc/bds-architecture.jpg)

## 环境部署 
### 安装 BCH
#### 环境初始化
[build-unix](./doc/build-unix.md)

#### 运行步骤

1.编译

 ```
   ./autogen.sh
   ./configure
   make
   make install # optional
 ```

2.运行全节点，并支持向 kafka 发送消息

```
    # 日志地址: <数据目录>/debug.log

   /usr/local/bin/bitcoind -kafka -kafkaproxyhost=<kafka 代理的ip地址> -kafkaproxyport=<kafka 代理的访问端口，默认是 8082> -kafkatopic=bch -datadir=<数据目录> -rpcuser=<账号> -rpcpassword=<密码>
```

### 安装 confluent 和 kafka
#### 安装kafka
参见 [kafka 官网](http://kafka.apache.org/quickstart)

修改 config/server.properties 文件

* message.max.bytes=1048576000

#### 安装 confluent
参见 [confluent](https://docs.confluent.io/current/installation/installing_cp/zip-tar.html#prod-kafka-cli-install)

解压缩 confluent 安装包并运行Confluent Rest Proxy

修改 /etc/kafka-rest/kafka-rest.properties 文件

* max.request.size = 1048576000
* buffer.memory = 1048576000
* send.buffer.bytes = 1048576000

### 安装 BDS
参见[BDS](https://github.com/jdcloud-bds/bds)

### 数据库
我们现在支持 SQL server 和 PostgreSQL 两种数据库，您可以选择其中一种作为数据存储方法。

#### SQL Server
购买 [云数据库 SQL Server](https://www.jdcloud.com/cn/products/jcs-for-sql-server)

#### PostgreSQL 
购买 [云数据库 PostgreSQL](https://www.jdcloud.com/cn/products/jcs-for-postgresql)

### 安装 Grafana 
参见 [Grafana 官网](https://grafana.com/)

## 新增功能

1. 增加了向 kafka 发送消息的新功能（每次一个新块被全节点同步时，该块的数据将以定制的数据结构被发送到 kafka 中）
2. sendblock 和 sendbatchblock 是新添加的两个 RPC 接口，用于触发全节点发送特定块的数据。

### 源代码修改历史记录
[bds-btc](./CHANGE_HISTORY.md)

## 贡献
[贡献指南](./CONTRIBUTING.md)

## 开源许可 
[Apache License 2.0](./LICENSE)

## 项目展示
[区块链数据服务](https://bds.jdcloud.com/)

