---
title: "初學 Kafka"
date: 2024-01-03T15:52:54+08:00
author: "celine"
tags: [""]
categories: ["StudyNote"]
---

### Kafka Broker

Kafka broker (9092) is the first server that users interact with, it listens to some TCP connection. There are two pieces of abstractions --- producer and consumer. 消費者與生產者的抽象概念

 Topic: 資料邏輯分層 logical partitioning of data

```
               Kafka broker(9092)
                   [Topic A]
Producer <--->     [Topic B]      <---> Consumer 
```

假設現在 Kafka broker 裡面有個 Users Topic，而生產者發佈 `John` 進去 Users topic，當生產者持續發佈東西進 Users Topic，這個 Topic 就會越來越肥大

消費者在從 Users Topic 拉訊息 (pull information) 下來，

當資料表記有百萬筆資料，會採用sharding 將資料分成不同 database，而Kafka借用了這種技術，將Users Topic 分成了 Partition 1 (A - M) 與 Partition 2 (N-Z)。 

因此 Producer 要再發佈 "Nader" 進 Users Topic 時，會推進 partition 2 (publish "Nader" to users topic on partition 2, current position 4)

You don't use Kafka to do ad-hoc queries, this is not a relational database, and we work with indexes and partitions. You use it for fast writing and distributed of events that happens.

### Queue vs Pub Sub

- Queue: Message published once, consumed once

  RabbitMQ started with queue. (oh... but people want to do pub sub...)

- Pub Sub: Message published once, consumed many times

- Kafka asked: How can we do both? Kafka build a system with those two in mind.

  Answer: Consumer Group

#### Consumer Group

Consumer groups are invented to do essentially parallel processing and partitions. 平行處理與資料分片

Consumer group can remove the awareness from the consumer of a partition. Another benefit is it can kind of run and consume parallel data, like consume parallel information from multiple partitions.

You can have one consumer consuming two partitions or three or four, but one partition better be consumed with one consumer. 一個消費者可以同時從一至多個partitions 拉資料，但一個partition最好只有一個consumer跟他拿資料，而 Consumer Group 就可以保證這一點。

1. If you want to act like a queue, put all your consumers in one group.
2. If you want to act like a pub-sub system, put each consumer in a unique group. 

#### Kafka Pros

- Append only commit log.  紀錄檔都在最後面加入

  It always goes in the end, fast, easy to find.

- Performance is amazing.  效能極佳

  Seeking to a position is extremely fast.

- Distributed with zookeeper

- Long polling
  You make a request and you wait for it, there's no empty misses.

- Event driven architecture, Pub-sub and queue

- Scaling

  scale like there's no tomorrow

- Parallel processing

  One-topic, multiple partitions, you can read them in parallel.

#### Kafka Cons

- Zookeeper

  replace ZooKeeper with a self-managed metadata quorum

  People complaining about zookeeper, especially at scale.

- Producer explicit partition can lead to problems.

- Complex to install, cofigure and manage



##### 參考資料

[卡夫卡 ithelp](https://ithelp.ithome.com.tw/users/20140255/ironman/4026)

















