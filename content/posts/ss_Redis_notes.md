---
title: "Redis 筆記"
date: 2023-12-26T14:12:12+08:00
author: "celine"
tags: [""]
categories: ["StudyNote"]
---

######  資料分群

三種依照服務類型儲存資料的方法：

1. namespace：在key的前面加上如`user:`的prefix，所有資料放在一個Redis內

   建議用`:`區分。
   可以用 redis-audit，它會用自動分群，也可以加入部分規則，減少自動分群花的時間。

2. 使用 database：用指令 select 切換用不同的 "database"

   Redis Cluster只能用database 0，因此長遠 scalability 考量，database並不適用。

3. 用不同的 Redis servers

   管理成本較高。設成LRU mode存cache資料，作為第一個Redis Cluster：存persistent data，作為第二個Redis Cluster。如此，可以個別設定適合的 shards、replicas 以及 各別設定適合的shards、replicas 以及 OOM 時的處理方式。

###### 使用Expiration -- 標上有效期限

避免無限量的丟東西到記憶體裡面，以降低太多資料的管理成本

須注意，Redis只有支援在第一層keys加上expiration。如果需要用到`hash`，可是`hash`部分內容需要加expiration的話，比較善用expiration的方法是「將一個大hash攤成多個小hash或者string，才能善用expiration。另外，使用[多個小hash會比一個大hash更省時間，因為Redis有針對小hash的最佳化儲存方式](https://redis.io/docs/management/optimization/memory-optimization/)。

###### 分析占空間的keys

除了分析工具 (redis-audit等其他記憶體分析工具)，也可以用redis-cli查找問題

- 找出常見的keys，看數量分佈是否合理：`redis-cli scan 0 count 100` 

- 取樣找出各個資料類型最大的keys：`redis-cli --bigkeys`

- 查看 KEY serialized 後的大小：`DEBUG OBJECT key`

- 看某個 KEY 多久沒有被存取：`OBJECT IDLETIME key`

  [注意] default setting下，不會回傳正確的值

###### [Latency](https://redis.io/docs/management/optimization/latency/)

常用指令：

- 了解自己機器合理的 operation per second：[benchmark](https://redis.io/docs/management/optimization/benchmarks/)

- 找出 ≥ 10ms的操作：[slowlog](https://redis.io/commands/slowlog/)

  因為 Redis 單線程(只有main thread)，可以用這指令找出比較花時間的操作

- 用 unlink 取代 del 刪除 key

  Redis 用 background thread 處理 unlink 之後的 keys。

  相較於 del 指令，不會卡到 main thread，造成服務停擺的可能

- 如果是在 Redis client lib 的執行緒池 (thread pool) 處理 <u>**Redis 返回值**</u>，需要花額外的CPU時間，將其轉移到別的執行緒操作，以避免卡到 Redis client lib 的執行緒池。

- 開啟 latency monitor，用`LATENCY DOCTOR`檢查後續改善之處
  - [Latency doctor](https://redis.io/commands/latency-doctor/) 回報不同延遲相關議題，並建議可能的改善方式
  - [Latency monitor](https://redis.io/docs/management/optimization/latency-monitor/) 建立延遲門檻，運用`LATENCY`與相關子指令回報資訊
    - `CONFIG SET latency-monitor-threshold 100`  
    - 最多只接受 100 ms 的延遲，如果有block server 耗時 ≥ 100ms 的事件，就記錄log起來
    - `LATENCY`指令：回傳所有事件的延遲sample
      - 最近期的延遲事件 `LATENCY LATEST`
      - 某事件的latency time series `LATENCY HISTORY`
      - 重置單一或多個事件的 latency time series `LATENCY RESET`
      - 將某事件的延遲sample炫染成ASCII-art圖表 `LATENCY GRAPH`
      - 產出易讀的延遲分析報告 `LATENCY DOCTOR`

###### Lua Script

- 可以減少 roundtrip time，還是唯一可以做好原子操作 (atomic) 的方法

- 但是如果沒用好Lua的話，Redis會卡很久，也不能隨意中止 Lua script，使用時要非常小心

- Lua的索引index從1開始

- How to debug?

  可以用 `redis.log()`寫資訊到 Redis server log 或者存到 table 裡面，在 [Lua script 結束之後回傳 table](https://redis.com/blog/5-6-7-methods-for-tracing-and-debugging-redis-lua-scripts/)

###### 同步資料

可以用[redis-shake](https://github.com/tair-opensource/RedisShake) 來同步 redis cluster，將單一節點升級為 redis cluster 時，可減少停機時間

###### Redis Cluster Client Library 的設定

[Lettuce](https://lettuce.io/) - Redis client side, 這是 SpringBoot 2.X 版本之後的預設 Redis library

###### 參考資料

[SpringBoot：RedisTemplate+Lettuce](https://pdai.tech/md/spring/springboot/springboot-x-redis-lettuce.html)
