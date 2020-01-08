---
layout: post
title: "🍹 Kafka消息流数据实时处理"
date: 2019-11-15 00:00:00
categories: [React]
tags: [React、Kafka、Node.js]
comments: true
---

本 demo 展示了高速实时数据流经由消息队列 Kafka，并使用 websocket 推送，实时在页面进行可视化。后端使用 Node.js（主要使用 kafka-node 包进行数据生产与消费），前端页面使用了 React框架进行实时可视化。主要技术栈包括：Kafka、Socket.io、Node、 ReactJS、FusionCharts、ECharts.

<!--more-->

### 消息队列 Kafka

Kafka 是最初由 Linkedin 公司开发，是一个分布式、支持分区的（partition）、多副本的（replica），基于 zookeeper 协调的分布式消息系统，它的最大的特性就是可以实时的处理大量数据以满足各种需求场景。
本 demo 是在 windows 系统，单机环境中进行，运行前需要安装好 zookeeper 和 kafka，并配置好。在 windows10 环境下，主要使用的命令有：

- **zookeeper**

  - 安装：注意在下载时，是下载带 bin 的包
  - 配置环境变量：`ZOOKEEPER_HOME`，path 系统变量中添加 `%ZOOKEEPER_HOME%\bin`
  - 启动：

  ```
  zkserver
  ```

- **kafka**
  - 启动 kafka：D:\SOFTWARE\Kafka\kafka_2.12-2.3.0 目录下打开 Powershell 窗口（shift+鼠标右键），输入：
  ```
  .\bin\windows\kafka-server-start.bat .\config\server.properties
  ```
  - 查看所有 topic
  ```
  .\bin\windows\kafka-topics.bat --zookeeper localhost:2181 --list
  ```
  - 创建 topic
  ```
  .\bin\windows\kafka-topics.bat --zookeeper localhost:2181 --create --replication-factor 1 --partitions 1 --topic test1
  ```
  - 查看指定的 topic
  ```
  .\bin\windows\kafka-topics.bat --zookeeper localhost:2181 --describe --topic test1
  ```
  - 删除 topic
  ```
  .\bin\windows\kafka-topics.bat --zookeeper localhost:2181 --delete --topic test1
  ```
  - 生产者
  ```
  .\bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic test1
  ```
  - 消费者
  ```
  .\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test --from-beginning
  ```

### producer 发送消息流

```javascript
var kafka = require("kafka-node");
const kclient = new kafka.Client("localhost:2181", "my-client-id", {
  sessionTimeout: 300,
  spinDelay: 100,
  retries: 2
});

const KafkaService = {
  sendRecord: ({ data }, callback = () => {}) => {
    const event = {
      timestamp: new Date(),
      data: data
    };

    const buffer = new Buffer.from(JSON.stringify(event));
    const record = [
      {
        topic: "streaming-data",
        messages: buffer,
        attributes: 1
      }
    ];

    producer.send(record, callback);
  }
};

setInterval(() => {
  // collect data and publish to kafka topic at regular interval
  KafkaService.sendRecord({ data: Math.random() });
}, 2000);
```

### consumer 消费数据

```javascript
var kafka = require("kafka-node");
const io = require("socket.io")();
const kclient = new kafka.Client("127.0.0.1:2181");

io.on("connection", socket => {
  socket.on("subscribeToTopic", unit_key => {
    console.log("client is subscribing to timer with unit key ", unit_key);
  });

  const topics = [
    {
      topic: "streaming-data"
    }
  ];

  const options = {
    autoCommit: true,
    autoCommitIntervalMs: 5000,
    fetchMaxWaitMs: 1000,
    fetchMaxBytes: 1024 * 1024,
    encoding: "buffer",
    fromOffset: true
  };

  const consumer = new kafka.HighLevelConsumer(kclient, topics, options);

  consumer.on("message", function(message) {
    var buf = new Buffer(message.value, "binary");
    var decodedMessage = JSON.parse(buf.toString());
    io.sockets.emit("broadcast", decodedMessage);
  });

  consumer.on("error", function(err) {
    console.log("error", err);
  });

  process.on("SIGINT", function() {
    consumer.close(true, function() {
      process.exit();
    });
  });
});

const port = 8000;
io.listen(port);
console.log("listening on port ", port);
```

### 实时可视化

```javascript
import openSocket from "socket.io-client";
const socket = openSocket("http://localhost:8000");
function subscribeToKafkaSocket(cb, unit_key) {
  socket.on("broadcast", data => cb(null, data));
}

export { subscribeToKafkaSocket };
```

### 效果图
<img src="/image/posts/20200108.gif" style="display:block;margin:0 auto;"> 
