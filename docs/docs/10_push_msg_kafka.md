## 概述

该功能的目的：将区块链运行过程中产生的信息发送至 Kafka，以对外部调用者提供更加详细的信息。

## 设计方案

在app.toml中配置 `brokers` 地址，以及希望推送消息的模块名称`topics`(可以将该模块名称作为 `topic`)；当存在多个broker 或 `topic`时，多个`broker`或`topic` 以`,`分割；当希望数据向配置的brokers推送时，设置`feature-toggle`属性为true.
示例如下:

```
minimum-gas-prices = "20.0cet"
brokers = "kafka:127.0.0.1:8999,127.0.0.1:7862"
subscribe-modules = "market,bankx,asset"
feature-toggle = true
```

系统启动时，通过检测上述配置的信息，链接相应的brokers，将订阅的模块的信息输出给brokers；

*	注意：该方案，需要每个模块提前指定一个 `字符串标识`。如：market 模块，使用`market` 字符串作为标识，当程序启动后，在cetd.toml文件中检测到该标识存在时，会将该模块加入订阅者集合中，在dex运行中，会将该模块的信息推送出来.

推送至Kafka的消息 都为 `Json编码`后的数据。

### feature-toggle

当希望将各个订阅模块的信息向配置的brokers进行推送时，设置该属性为true; 当该属性为false时，不会将任何模块的数据进行推送.

### brokers 类型

brokers支持三种模式的推送：向文件推送，向标准输出推送，向kafka推送。
在 app.toml中，配置相应的推送模式，如下所示

```
// brokers = "file:/usr/local/hello/data.md"
// brokers = "os:stdout"
// brokers = "kafka:127.0.0.1:8999,127.0.0.1:7862"
```

### topics

在dex节点中，每个模块会分配一个topic，当前dex节点支持的模块如下，

```
market          : 订阅市场行情、订单成交数据，
bankx           : 订阅账户转账数据
asset           : 订阅资产发行数据
```



## kafka消息推送流程

1. 下载kafka; http://kafka.apache.org/downloads
2. 启动服务
	* zookeeper-server-start  /usr/local/etc/kafka/zookeeper.properties &
	* kafka-server-start  /usr/local/etc/kafka/server.properties &
3. 配置节点的 `app.toml`  配置文件；写入kafka的brokers地址，以及需要订阅的模块
4. 启动节点 `./cetd start`；发布Dex运行过程中产生的信息
5. 启动 消费者节点 `sarama_test`; 接收kafka推送的信息
	* 项目地址: https://github.com/ludete/kafka_test






