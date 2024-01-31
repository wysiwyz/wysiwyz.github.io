---
title: "Elasticsearch 從頭開始學習(ELK & Elastic Stack)"
date: 2024-01-22T17:00:05+08:00
author: "celine"
tags: ["ElasticSearch", "Kibana"]
categories: ["StudyNote"]
---

> 2023-07-11 的筆記太簡略粗糙，重新整理

## Elastic Search 到底是什麼

- ElasticSearch 是一個開源分析工具 (Opensource analytics) 和全文搜尋引擎 (full-text search engine)
- 通常用於啟用application的搜索功能，有以下特性
  - 如果產品有評價等級，而開發想要提高高評價產品的關聯性
  - 開發要允許用戶過濾結果，例如按照價格範圍、品牌、尺寸、顏色，並依照關聯性排序
- ElasticSearch除了全文搜尋之外，還可以查詢結構化數據（e.g. numbers, aggregated data）並且使用ElasticSearch作為分析平台
  - 可以編寫query of aggregated data，並使用結果製作圓餅圖、折線圖etc.
- 例子：
  - Analyze application logs and system metrics: 存儲來自app的log檔以及各種服務器系統指標，分析並設置告警 (e.g. error and CPU memory usage)
  - Application Performance Management (APM) : 應用程式性能管理
  - Send events to ElasticSearch: 發送事件至ElasticSearch (e.g. 將實體店銷售業績存到ElasticSearch )
  - Excellent at analyzing lots of data: 分析大量數據 
    - 使用機器學習根據歷史資料，來預測未來銷售額)
    - Anomality Detection 偵測異常

- Data is stored as documents, which is just a unit of information 

  *FIELD_NAMES* : *FIELD_VALUES*

  ``````
  {
      "firstName": "Angela",
      "lastName": "Joji",
      "interests": ["ElasticSearch", "Logstash", "Kibana", "ElasticStack"]
  }
  ``````

- 數據存成文件(documents)，再透過RESTful API查詢資料

- 發送到ElasticSearch的查詢也是用JSON編寫的

- ElasticSearch是用Java編寫，並建構於Apache Lucene

- 因為使用起來很方便而且擴充性很告，ElasticSearch的普及率很高


## Overview of the Elastic Stack

- 由Elasticsearch BV開發維護
- Elastic Stack 以 Elastic Search 為核心，加上 X-Pack、KIBANA、BEATS、LOGSTASH

### Kibana 

- An analytics and visualization platform 分析和可視化 平台
- 可以想成是ElasticSearch的儀表板，可以在其中建立資料視覺化，像是圓餅圖、折線圖
- Data Detection & Prediction的配置也是在Kibana做更改
- Kibana也提供了一個介面來管理ElasticSearch的某些部份，例如AuthZ & AuthN
- Kibana: 存在ElasticSearch裡面資料的web interface

### Logstash

- 傳統上，logstash用來處理來自app的日誌log，並將它發送到ElasticSearch
- 但現在Logstash演變成更通用的工具，作為數據處理管道  data processing pipeline
- Logstash收到的數據會以事件的形式被處理
  - log file entries
  - e-commerce orders
  - customers
  - chat messages
- 以上事件會由Logstash處理，接著送往其它目的地(例如⬇️)
  - ElasticSearch
  - Kafka queue
  - Email message
  - HTTP endpoints
- Logstash 管道由三個部分/三個階段組成：  inputs, filters, outputs
- 每個階段都能使用插件(plug-ins)


#### Plugins
- input plugins
  - Logstash接收事件的方式，可以是一個 document
  - logstash 會從給定的文件讀取事件
  - 也可以是透過HTTP發送事件到Logstash，從RDS讀取資料，或者監聽Kafka貯列
- filter plugins
  - Logstash處理事件的方式
  - Parse Data: 可以解析CSV、XML或JSON   
  - Data Enrichment: 例如查詢一個ip位址，並解析其地理位置
  
- Output plugins
  - 將處理過後的事件發送到哪裡，目的地正式名稱為STASHES
  

> 簡言之，Logstash從一到多個管道接收事件，處理事件並且發送事件到一至多個STASHES 

- 一個Logstash instance中，可以運行多個 pipelines
- Logstash可以水平擴展

#### Logstash 配置

- 類似JSON格式的markup format，但又不僅是markup language，因為它還可以添加條件語句，讓管道動態化

- 以下範例配置文本：

  - 解析字串（將非結構化的數據結構化）：使用Grok pattern

    > Grok Pattern類似正則表達式
    >
    > 用途：to match pieces of text and store them within fields

  ```Treetop
  input {
    file {
      patch => "/path/to/apache_access.log"
    }
  }
  
  filter {
    if [request] in ["/robots.txt", "/favicon.ico"] {
      drop {}
    }
  }
  
  output {
    file {
      patch => "%{type}_%{+yyyy_MM_dd}.log"
    }
  }
  ```

  ### X-Pack

  給ElasticSearch 和 Kibana 添加功能

  1. Security: X-Pack給Kibana和ElasticSearch添加了AuthN 與 AuthZ功能
     - Kibana可以整合(集成integrate) 其它AuthN providers，例如LDAP、Active Directory
     - 另外也可以在配置檔加入users & roles，並配置允許特定角色/用戶訪問的內容
  2. Monitoring 
     - X-Pack可以協助監控Elastic Stack (ElasticSearch, Logstash, Kibana) 的性能
     - 例: CPU & memory usage CPU和內存的使用情況, disk space磁碟空間 等有效指標
  3. Alerting警報
     - 例: 當web server的CPU利用率超過90%，或者application error達到門檻
     - 透過電子郵件,  或Slack等其它方式告知
  4. Reporting
     - 將Kibana視覺化的數據導出成PDF文件
     - 可以接收需求後才輸出報告，也可以定期輸出報告並發送至e-mail
     - 另外也可以指定觸發輸出報告的條件（類似alerting)
  5. Machine Learning
     - 功能性(functionality)由X-Pack提供，接口(interface)由Kibana提供
     - 可以做異常檢測、預測未來數據
  6. Graph
     - 指的是資料之間的關係
     - 例: 在商品明細頁面顯示與相關商品們，或者Spotify的下一首推薦歌單
     - 需要區分popularity與relevance，不然可能產生誤導
       - 喜歡聽LinkinPark的樂迷可能也偶爾會聽古典樂
     - 提供一個公開API，可以用來將Graph整合進應用程式
     - 提供一個plugins，可以用來可視化Kibana裡面資料之間的關係
  7. ElasticSearch SQL
     - 使用SQL查詢SQL
     - 使用Query DSL撰寫查詢（flexible but verbose at times)
     - 透過HTTP或JDBC driver發送SQL

### Beats

- Collections of data receiver
- Light-weight agent 輕量級代理, 在Server中，將數據發送到Logstash或者ElasticSearch
  - Beats: data shipper, 收集不同類型的數據

- 常見beats
  1. FileBeat
     - 收集log檔案
     - 包含常見log種類的模組 (例如: nginx, MySQL)
  2. MetricBeat
     - 收集系統與服務的度量指標, 例如: memory and CPU usage
     - 包含常見log種類的模組 (例如: nginx, MySQL)
  3. PacketBeat:
     - Collects network data 收集網路數據
     - 例如: HTTP requests or database transaction HTTP請求或資料庫交易
  4. WinLogBeat: Collects Windows Events Logs 
  5. AuditBit: Collects audit data from Linux 收集Linux審計數據
  6. HeartBeat: monitor service uptime 監控服務上線時間

### 小結

- X-Pack: Add features to the elastic stack
- Logstash and Beats: In charge of data ingestion
- ElasticSearch: Search, analyze and store data
- Kibana: Visualize your data

---

## 常見架構

現在有個電商網站，資料存在資料庫，當client請求資料時是從資料庫拿，再透過後端應用程式傳回前端瀏覽器，但現在想要將ElasticSearch整合進現有架構，怎麼做？

通常建議使用client libraries，同時要保持資料庫與ElasticSearch同步更新，數據在database與ElasticSearch會重複。

如果資料庫已經有10萬筆商品，要怎麼同步到ElasticSearch？

- 如果資料沒有那麼多，可以寫script讓它一口氣完成輸入 
- 如果資料更多就需要paginating或scroll
- 資料輸入後，就需要應用程式負責維護兩邊的資料同步更新



隨著業務成長，要確保目前的架構足以應付未來增長（處理流量高峰，了解何時應該增加一台web server)  ，可以使用metricbeat監控系統級別指標（system level metrics），例如CPU使用率和內存memory使用率。所以在Web Server啟動MetricBeat，但數據如何結束於ElasticSearch？

- Beats支持向Logstash以及ElasticSearch發送數據，後者相對簡單
- 配置Metricbeat，使它能將資料送進 ElasticSearch 的 ingest node
- 指示Metricbeat，在Kibana中配置儀表板
- Kibana的配置則存在ElasticSearch裡面



配置更多的東西：Web Server 的 access logs, error logs。雖然可以利用Google Analytics，但access log更可以提供一些有用訊息，像是

- web server處理每個請求花了多長時間
- 監控響應時間 (respone time) 並聚合每個端點的請求
- 識別bad deployments（例如：太長的響應時間、或4xx/5xx的HTTP狀態碼）
- 上述這些都可以透過Filebeat達成，Filebeat可以
  - 解析日誌(parse logs)
  - 處理多行事件(multi-line events)，例如stack trace, etc.



更高級的事件處理，像是data enrichment數據豐富化，可以在web application中執行這類操作，但可能會導致一些缺點：

1. 首先業務邏輯程式碼會充滿雜亂無章的事件處理，增加服務器響應時間（除非用異步執行） 
2. 去中心化的事件處理：事件處理會發生在很多地方， 可以透過Logstash實現集中化，將事件處理放在同一個地方完成
   - WebServer發送事件，透過HTTP到Logstash，Logstash依照指示的方式處理事件，接著發送到ElasticSearch
   - 那來自MetricBeat和FileBeat的data要怎麼處理？可以透過Logstash發送到elasticsearch（最理想狀況），或直接送到ElasticSearch
   - ElasticSearch only handles query from web-app, not modifying data

---

## 安裝方式

1. 安裝Kibana - 它有Console方便開發，可以輕易的發送查詢到ElasticSearch
2. 如果使用 [Elastic Cloud](https://www.elastic.co/cloud/cloud-trial-overview) 就都不用安裝了😅他是有14天期限的試用版
   - 試用期是從你建立第一個cluster開始算，而不是建立帳戶開始算

### Elastic Cloud

- 集群(cluster)：elastic cloud 把他稱為 deployment (less technical term for  cluster)

- 創建部署 create deployment 

  1. 命名, 選擇cloud platform, 選擇template

     ![Naming](https://i.imgur.com/kMGTeno.jpg)

     ![Choose_Platform](https://i.imgur.com/sSQ4Gii.jpg)

     ![Save_Credentials](https://i.imgur.com/nmKK5Hw.png)

  2. 部署完成後，點擊Actions➡️Manage就可以看到相關的基本信息

     - health status
     - actions we can perform
     - memory and disk usage
     - Cloud ID: 當你要從ElasticStack相關產品 (Beats or Logstash) 發送數據到ElasticSearch時，會需要用到這個ID

### 在MacOS上安裝ElasticSearch

- ElasticSearch是一堆jar檔 (java archive)
- ElasticSearch附帶一個版本的OpenJDK
  - 如果已經有安裝，則會使用JAVA_HOME環境變數指向的JVM
  - 還沒安裝的話就會使用 bundled JDK，不用額外安裝
- [下載連結](https://www.elastic.co/downloads/elasticsearch)

---

# Beginner's Crash Course

## Part 1/6 Kibana & ElasticSearch

##### What is sharing?

Index is just a virtual thing that keeps track of where documents are stored.

When you create an index, one shard is created by default, and the shard is assigned to a node. Remember, shard is where documents are stored, and number of documents a shark could hold depends on the capacity of the node.

Let's say you want to index 600_000 documents(60萬) in your produce index, but the node where the shard is assigned to could only hold 200_000 documents, which is not gonna work, but we do have two more nodes which could hold 200_000 documents each.

因此，一個cluster 裡面有三個node，每個node各有一個produce index的shard，但是produce index裡面的資料還會一直增加，這時候 sharding就派上用場了。

##### The beauty of sharding 

You could add more shards and nodes as the need rises, so you could horizontally scale and adapt to increasing data, but that's not the only superpower that comes with sharding.

Shard is where you store documents, and it's also where you run search queries. Let's talk about a scenario where the client is sending a search request for gingery read bean tang-yuan. In this scenario, you have one chart in a node that could hold the entire produce index.

##### Sharding speedings up your search

You could run a search on all 10 of these shards at the same time in parallel

所以如果有一個Produce Index 具有500_000資料，全部都放在`P0`一個shard裡面，那麽要完成搜尋需要耗費10秒鐘（sequentially）。

但如果有Produce Index有10個shards，分別佈在10個nodes上面，而一個shard存有50_000筆文件，在這裏要完成搜尋只要花1秒鐘。

##### What are replica shards?

Let's say everything is going well with our online grocery shop, but all of a sudden one of our nodes go down and we lose our data. To avoid this, we could make copies of the original shards and store it's copies across different nodes. 

Node 1&2: contain original shards (`P0`, `P1`), also known as primary shards.

Node 3&2: make copies of original shards and store these in different nodes(`R0`, `R1`), also known as replica shards.

除了能夠避免某個節點有問題，還可以增進搜尋效能

##### Replica shards can improve the performance of your search

2000 search queries per second ➡️ 8000 search queries per second

Because the replicas are identical copies of the primary shards, now these replica shards can pick up the slack and your cluster could better handle increased demands on search.

Note: 

- 如果有ElasticSearch密碼，啟動腳本`bin/elasticsearch`之後的curl cmd如下

  `curl -k -u elastic:{password_for_the_elastic_user} https://localhost:9200`（[參](https://discuss.elastic.co/t/curl-x-get-http-localhost-9200-curl-52-empty-reply-from-server/329211)）

  - 只有`curl http://localhost:9200`只會收到Empty reply from Server
  - password_for_the_elastic_user: 這是首次啟動 `bin/elasticsearch` script會產生的密碼，也可以用`bin/elasticsearch-reset-password -u elastic`重新產生一組密碼

- 在$ES_HOME(即解壓縮後的elastic tar.gz檔案) 路徑開啟終端機，輸入 `code .` 打開 vsCode

- 當你啟動了一個node，預設會自動形成一個cluster，最好給他們有意義的命名，以利爾後在多個clusters/多個nodes識別不同的任務/負責項目

  - 修改 elastic search.yml 檔案以下兩行，存檔之後要停止 elasticsearch script & kibana scrip，再重新啟動

  ![cluster_node](https://i.imgur.com/JJGsyYk.jpg)

- GET 可以查看cluster健康、nodes狀態

- PUT 可以建立一筆索引，如果response JSON 的 "acknowledged" 為 true，就表示建立成功

  ![PUT_FAVORITE_CURRY](https://i.imgur.com/T7e1qpT.jpg)

- INDEX as verb 在某筆索引裡面儲放/加入documents，可以用 POST 或 PUT

  - POST: 請ElasticSearch自動幫你建立一個 document ID

    ```
    POST {索引的名稱}/_doc
    {
        "field": "value"
    }
    ```

    ![SyntaxExample](https://i.imgur.com/KS1VhHj.jpg)

  - PUT: 則適用於要給該文件指定一筆ID號碼，通常是使用 natural identifier 給資料編索引時會使用

    ```
    PUT {索引的名稱}/_doc/{這份文件要分配到的指定ID}
    {
        "field": "value"
    }
    ```

    ![PUT_fav_curry](https://i.imgur.com/ehieOWZ.jpg)

- READ 讀取document 

  ```
  GET {索引名稱}/_doc/{要查找的DocumentID}
  ```

  ![Get](https://i.imgur.com/0Y54vkD.jpg)

- 如果用既有的 ID put 一筆文件，會發現原本的Joshua被Jupytor覆寫了

  `result:updated` 以及 `_version:2`

  ![Step1](https://i.imgur.com/6hQCuSc.jpg)

  ![step2](https://i.imgur.com/WZwvlkT.jpg)

- 如果要避免文件被覆蓋，可以使用 `_create_entpoint` ，這樣如果指定的是已經存在的ID，就會拋出錯誤，以免不小心覆蓋掉既有的doc

  ```
  PUT {索引名稱}/_create/{指定的DocId}
  ```

  ![Error409](https://i.imgur.com/b9YfqK2.jpg)

- Update 更新文件

  - 注意這裡的JSON 一定要帶 field `doc`，欄位不用全部都給

  ```
  POST {索引名稱}/_update/{待更新的Doc的Id}
  {
      "doc": {
          "field1": "value",
          "field2": "value"
      }
  }
  ```

  ![Update_only partial fields](https://i.imgur.com/PDKgGKG.jpg)

- Delete 刪除文件

  ```
  DELETE {索引名稱}/_doc/{指定要刪除的DocId}
  ```

  ![Delete Demo](https://i.imgur.com/wrLIgT5.jpg)

#####  Q and A

1. RDBMS與ElasticSearch比較
   - ElasticSearch不是資料庫，它是搜尋分析引擎
   - 沒有schema，建構於Lucene，擅於全文搜索
   - 可以微調搜尋結果的關聯性（precision, ranking, recall)
2. `node`相當於SQL Server的table，而`cluster`相當於SQL Server的servers，這種說法對嗎？
   - 蘋果不能跟橘子相比
3. Nodes會用cache找被之前 搜尋的數據嗎？
   - Yes, node use cache at the level of ElasticSearch and operating system, so that you could retrieve information faster.
4. 如何在一個node裡面做sharding分片化？
   - 不建議使用，因為如果這個node有問題，那data就永遠消失了，而且把資料sharding在不同的nodes才有助於平行化搜尋資料
5. 使用ElasticSearch的缺點
   - 不支持joins、transactions
6. 其它問題，[discuss_elastic_co](https://discuss.elastic.co/) 應該都找得到解答
7. `:9200`(ElasticSearch) 以及 `:5601`(Kibana)，有支援Authentication/ tokens 嗎？
   - 有～～～官方有相關guide，請搜尋以下關鍵字
     - secure-cluster, encrypting-communications, setting-up-authentication

## Part 2/6 搜尋的關聯性

- 如何量測關聯性？Precision + Recall

- 當你傳送search query至ElasticSearch，它會回傳它認為有關聯性的documents

  ![image-20240128115200212](https://i.imgur.com/MviYIa2.png)

  - green dots: true positives
    - relevant documents that are returned to the user
  - red dots: false positives
    - irrelevant documents that are returned to the user
  - gray dots: true negatives
    -  irrelevant documents that are NOT returned to the user
  - magenta dots: false negatives
    - relevant documents that were not returned to the user
  - Precision公式
  
  $$
  Precision = \frac{True\:positives}{True\:positives + \textcolor{orange}{False\:positives}}
  $$
  
  - Recall公式
  
  $$
  Recall = \frac{True\:positives}{True\:positives + \textcolor{magenta}{False\:negatives}}
  $$
  
  - Precision 與 Recall 是負相關的
    - Precision: 回傳的值都必須完美匹配於搜尋條件，即使回傳的document數量較少
    - Recall: 要取得比較多的回傳值，即使回傳的documents不一定是完美匹配
  
  - Precision and recall determine which documents are included in the search results.
  - Precision and recall do not determine which of the returned documents are more relevant compared to the others.
  
- 回傳文件集之間的關聯性排序：Ranking

  - Ranking refers to ordering of the results (from  most relevant result at the top, to least relevant at the bottom)


- Score又是什麼
  - 代表該文件對此查詢條件相關性多高的值
  - A score is computed for each document that is a `hit`
  - `hit`:傳送至user的搜尋結果
- 計算document score的因子
  - Term Frequency (TF)
  - Inverse Document Frequency (IDF)

- Term Frequency (TF) determines how many times each search term appears in a document

  - If search terms are found in high frequency in a document, the document is considered more relevant to this search query.
  - 假設一個Search Query 是 `How to train dragons`，就會有四個Search Terms。ElasticSearch會查看整份document，計算Search Terms在documents出現的次數

- Inverse Document Frequency diminishes the weight of terms that occur very frequently in the document set and increases the weight of terms that occur rarely!

  - 搜尋結果 Hits 可能會得到很多`How to train puppy`, `How to train machine`, `How to train students`, ... etc. 之類的結果，但這些都跟 `How to train dragons`沒有關係

- Find datasets to work with:

  https://www.kaggle.com/datasets

### Syntax

1. Retrieve all document from an index

   - Syntax

     ```
     GET {enter_name_of_the_index_here}/_search
     ```

   - Example

     ```
     GET anime_tv_shows/_search
     ```

   - 如果dataset非常大，elasticsearch會限制total value為10,000筆

   - 如何知道total value是否為實際值？要看total relation

     - `"relation": "gte"` greater than or equal to 10,000
     - `"relation": "eq"` the value is equal to the total number of hits

     ![image-20240128162436889](https://i.imgur.com/vax8q8T.png)

     ![image-20240128164036840](https://i.imgur.com/mJI1JAP.png)

     - `"hits"`: [] 預設提供十筆sample search results

2. Get total hits

   - Syntax

     ```
     GET {enter_name_of_the_index_here}/_search
     {
         "track_total_hits": true
     }
     ```

     ![image-20240128163222326](https://i.imgur.com/F2HqONx.png)

     ![image-20240128164110733](https://i.imgur.com/w2TMTzk.png)

3. Search for data within specific time range

   - ElasticSearch搜尋有兩種主要方式：query & aggregation
     - Query: 取得符合criteria的documents
     - Aggregation: 

   - Syntax ... Query

     ```
     GET {enter_name_of_the_intex_here}/_search
     {
       "query": {
         "Specify the type of query here": {
           "Enter name of the field here": {
             "gte": "Enter lowest value of the range here",
             "lte": "Enter highest value of the range here"
           }
         }
       }
     }
     ```

   - Example ... Query

     ![image-20240128165927972](https://i.imgur.com/B8mW4TS.png)

   - Syntax ... Aggregations

     ```
     GET {enter_name_of_the_index_here}/_search
     {
       "aggs": {
         "name your aggregation here": {
           "specify aggregation type here": {
             "field": "name the field you want to aggregate",
             "size": "state how many buckets you want to retrieve"
           }
         }
       }
     }
     ```

     

   - Example ... Aggregations

     1. 收合`"hits": [...]` 之後就能看到聚合搜尋結果

        ```
        GET news_headlines/_search
        {
          "aggs": {
            "by_category": {
              "terms": {
                "field": "category",
                "size": 100
              }
            }
          }
        }
        ```

        ![image-20240128172158732](https://i.imgur.com/AIbcw1y.png)

        `aggs` or `aggregations`

        ```
        GET news_headlines/_search
        {
          "aggregations": {
            "by_category": {
              "terms": {
                "field": "category",
                "size": 100
              }
            }
            }
          }
        }
        
        ```
     
        ![image-20240129103745319](https://i.imgur.com/sSLZaMB.png)
     
     2. 找出entertainment類別中，最熱門的topics
     
        ```
        GET news_headlines/_search
        {
          "query": {
            "match": { "category": "ENTERTAINMENT" }
          },
          "aggregations": {
            "popular_in_entertainment": {
              "significant_text": { "field": "headline" }
            }
          }
        }
        ```
        ![image-20240128172755896](https://i.imgur.com/jvYYGQq.png)

4. Precision and Recall

   - Increasing Recall

     - Syntax

       ```
       GET {enter_name_of_the_index_here}/_search
       {
         "query": {
           "match": {
             "Specify the field you want to search": {
               "query": "Enter search terms"
             }
           }
         }
       }
       ```

     - Example

       - Note: 以下這四個 search terms 預設是使用`OR`邏輯，所以如果一份document只包含其中一個 search term，也會被算在hit清單中
       - OR logic具有 high recall 特性：關聯性較低的hits增加

       ```
       GET news_headlines/_search
       {
         "query": {
           "match": {
             "headline": {
               "query": "Khloe Kardashian Kendall Jenner"
             }
           }
         }
       }
       ```

       ![image-20240128181334483](https://i.imgur.com/v1wOHI2.png)

   - Increasing Precision

     - Syntax

       ```
       GET {enter_name_of_index_here}/_search
       {
         "query": {
           "match": {
             "Specify the field you want to search": {
               "query": "Enter search terms",
               "operator": "and"
             }
           }
         }
       }
       ```

     - Example

       ```
       GET news_headlines/_search
       {
         "query": {
           "match": {
             "headline": {
               "query": "Khloe Kardashian Kendall Jenner",
               "operator": "and"
             }
           }
         }
       }
       ```

       ![And_Operator](https://i.imgur.com/Ra8qKjP.png)

   - minimum_should_match

     - Specifying the minimum number of terms a document should have to be included in the search results
     - Giving you more control over fine tuning precision and recall off your search

     Syntax:

     ```
     GET {enter_name_of_index_here}/_search
     {
       "query": {
         "match": {
           "Specify the field you want to search": {
             "query": "Enter search terms",
             "minimum_should_match": Enter a number here
           }
         }
       }
     }
     ```

5. Combine several different queries to sort the results:

   - Syntax of **Bool query**

     ```
     GET {enter_name_of_index_here}/_search
     {
       "query": {
         "must": [
           {}
         ],
         "must_not": [
           {}
         ],
         "should": [
           {}
         ],
         "filter": [
           {}
         ]
       }
     }
     ```

6. 刪除已經加入的 index dataset

   1. 首先要 `DELETE {enter_name_of_index_here}`，會回傳true

   2. 接著驗證，輸入`GET {enter_name_of_index_here}/_search`，如果上個步驟有刪除成功，應該會看到 "index_not_found_exception"

   3. 刪除先前存在的 index pattern

      `≡` ➡️ HOME ➡️ Stack Management ➡️ Index Management

   4. 另外也要刪除 existed data view，不然在上傳時會拋出`DuplicateDataViewError`
   
      ![image-20240129174657679](https://i.imgur.com/O3cF8lB.png)

## Part 3/6  全文檢索

1. 匹配片語搜尋 - `match_phrase query`

   Syntax

   ```
   GET Enter_the_name_of_the_index_here/_search
   {
     "query": {
       "match_phrase": {
         "Specify the field u want to search over": "Enter the phrase u are searching for"
       }
     }
   }
   ```

   Example

   ![image-20240129103013654](https://i.imgur.com/KtbxfZU.png)

   - "match"  shape of you   ➡️  超過一萬筆，只要包含 shape 或 of 或 you 皆列入
   - "match" shape of you + "operator": "and" ➡️ 3筆 + 兩筆"shape of you**r** ..."
   - "match_phrase": 3筆

2. 多個匹配條件

   - user可能會查詢由Michelle Obama撰寫的文章、或者關於Michelle Obama的文章，可以透過從多個 fields 查詢來完成
   - 這類query會再每個field搜尋符合條件的document，給每個field計算score

   Syntax

   ```
   GET {enter_name_of_index_here}/_search
   {
     "query": {
       "multi_match": {
         "query": "Enter search terms here",
         "fields": [
           "List the field u want to search over",
           "List the field u want to search over",
           "List the field u want to search over"
         ]
       }
     }
   }
   ```

   Example

   ```
   GET news_headlines/_search
   {
     "query": {
       "multi_match": {
         "query": "Michelle Obama",
         "fields": [
           "headline",
           "short_description",
           "authors"
         ]
       }
     }
   }
   ```

   ![image-20240129112821717](https://i.imgur.com/ITHsrrv.png)

3. 多個匹配條件_給特定field加權

   Syntax: 

   - `^number` 使用 carat 符號加強權重，增進精確度

   ```
   GET {enter_name_of_index_here}/_search
   {
     "query": {
       "multi_match": {
         "query": "Enter search terms here",
         "fields": [
           "List the field u want to search over^2",
           "List the field u want to search over",
           "List the field u want to search over"
         ]
       }
     }
   }
   ```

   Example:

   - 可以看到搜尋結果比數還是一樣，但ranking改變了

   ![image-20240129113252327](https://i.imgur.com/JK4pUHT.png)

4. 多個匹配條件 - 查詢片語並改善其精準度

   Syntax

   ```
   GET {enter_name_of_index_here}/_search
   {
     "query": {
       "multi_match": {
         "query": "Enter search phrase",
         "fields": [
           "List the field u want to boost^2",
           "List the field u want to search over",
           "List the field u want to search over"
         ],
         "type": "phrase"
       }
     }
   }
   ```

   Example

   ```
   GET news_headlines/_search
   {
     "query": {
       "multi_match": {
         "query": "party planning",
         "fields": [
           "headline^2",
           "short_description"
         ],
         "type": "phrase"
       }
     }
   }
   ```

   ![image-20240129113956270](https://i.imgur.com/NRVzKZE.png)

5. 合併 Queries

   - user可能會詢問多面向問題 (multi-faceted question)，需要multiple queries
   - 例：查詢關於Michelle Obama政治方面的、2016年以前發佈的文章

   **Bool query** 

   - SYNTAX

     must: 應徵者**必備**條件  (e.g. 有兩年金融科技相關工作經驗)
     
     must not: 符合此欄所述條件的應徵者會被篩除 (e.g. 非資訊相關科系)
     
     should: 應徵者**加分**條件 (e.g. 對高併發、微服務、分佈式架構有明確理解)
     
     filter: **是否**符合某特定條件 (e.g. 住在與公司地址同縣市區域)
     
     ```
     GET {name_of_index}/_search
     {
       "query": {
         "bool": {
           "must": [
             {One or more queries can be specified here...}
           ],
           "must_not": [
             {A document must NOT match any of the quereies ...}
           ],
           "should": [
             {A document does not have to match any queries ...}
           ],
           "filter": [
             {These filters(queries) place documents in element}
           ]
         }
       }
     }
     ```
   
   - Example
   
     - Must + Must Not
   
       ![image-20240129120518694](https://i.imgur.com/WpaAwJQ.png)
   
     - Must + Should
   
       二月是BlackHistoryMonth，user更想看到black voices相關的documents。這樣的配置會給符合 should 條件的 documents 比較高的 score，document 顯示上的排序也比較靠前。
   
       ![image-20240129120634990](https://i.imgur.com/UIvEF10.png)
       
     - Must + Filter
     
       將documents二分法，例如符合某段時間內的為true，其餘為false。filter 子句的搜尋結果只包含 yes category 的 documents。
     
       ![image-20240129121331309](https://i.imgur.com/9Hxr8nD.png)


## Part 4/6 Aggregations

1. Metric aggregations
2. Bucket aggregations
3. Combined aggregations

將你的資料概括成metrics, statistics, 或其它 analytics

#### Aggregations Request

- Syntax

  ```
  GET {enter_name_of_index_here}/_search
  {
    "aggs": {
      "Name your aggregations here": {
        "Specify the aggregation type here": {
          "field": "Name the field u want to aggregate on"
        }
      }
    }
  }
  ```

- Metrics Aggregation: 根據資料集計算出數值，運用`sum`, `min`, `max`, `avg`, unique count(`cardinality`) 等等

  Syntax --- SUM

  ```
  GET {enter_name_of_index_here}/_search
  {
    "aggs": {
      "Name your aggregations here": {
        "sum": {
          "field": "name the field you want to aggregate on"
        }
      }
    }
  }
  ```

  Example --- 查看 `aggregations`

  ![image-20240129153518448](https://i.imgur.com/DrSIWYI.png)

- Metrics Aggregation --- SUM + 使用size參數

  Syntax 

  - `"size": 0` 告訴ELS不用取得前10大hits結果了 (參上圖右側 line 16 ~ line 157)

  ```
  GET ecommerce_data/_search
  {
    "size": 0,
    "aggs": {
      "sum_unit_price": {
        "sum": {
          "field": "UnitPrice"
        }
      }
    }
  }
  ```

  ![image-20240129154045784](https://i.imgur.com/834SeCa.png)

- Metrics Aggregation：計算最低unit price `min`

  Syntax

  ```
  GET {enter_name_of_index_here}/_search
  {
    "size": 0,
    "aggs": {
      "Name your aggregations here": {
        "min": {
          "field": "Name the field u want to aggregate on"
        }
      }
    }
  }
  ```

  Example

  ```
  GET ecommerce_data/_search
  {
    "size": 0,
    "aggs": {
      "lowest_unit_price": {
        "min": {
          "field": "UnitPrice"
        }
      }
    }
  }
  ```

  ![image-20240129154428170](https://i.imgur.com/Vr5ORAz.png)

- Metrics Aggregation：計算最高unit price `max`

  直接上 Example

  ```
  GET ecommerce_data/_search
  {
    "size": 0,
    "aggs": {
      "highest_unit_price": {
        "max": {
          "field": "UnitPrice"
        }
      }
    }
  }
  ```

  ![image-20240129154613476](https://i.imgur.com/bptBN8N.png)

- 一次得到上面所列的metrics aggs --- **Stats Aggregation**

  Syntax

  ```
  GET {enter_name_of_index_here}/_search
  {
    "size": 0,
    "aggs": {
      "Name your aggregations here": {
        "stats": {
          "field": "Name the field u want to aggregate on"
        }
      }
    }
  }
  ```

  Example

  ```
  GET ecommerce_data/_search
  {
    "size": 0,
    "aggs": {
      "all_stats_unit_price": {
        "stats": {
          "field": "UnitPrice"
        }
      }
    }
  }
  ```

  ![image-20240129154925916](https://i.imgur.com/KNzZIFV.png)

  - `count`: aggregation執行的資料筆數
  - 最低單價、最高單價、平均單價、單價加總

- **Cardinality Aggregations**

  - 應用場景：you want the number of unique customers who bough from our app

  - the cardinality aggregation computes the count of unique values for a given field

    用途：用來計算某一個欄位的唯一值數量

  - Syntax

    ```
    GET {enter_name_of_index_here}/_search
    {
      "size": 0,
      "aggs": {
        "name ur aggregations here": {
          "cardinality": {
            "field": "name the field u want to aggregate on"
          }
        }
      }
    }
    ```

  - Example

    ```
    GET ecommerce_data/_search
    {
      "aggs": {
        "number_unique_country": {
          "cardinality": {
            "field": "Country"
          }
        }
      }
    }
    ```

    ![image-20240129160641480](https://i.imgur.com/D75I861.png)

- 限制aggregation的作用域

  - Syntax (整合query與aggregation)

    ```
    GET {enter_name_of_index_here}/_search
    {
      "size": 0,
      "query": {
        "Enter match or match_phrase here": {
          "Enter the name of the field": "Enter the value you are limiting"
        }
      },
      "aggregations": {
        "Name ur aggregations here": {
          "Specify aggregations type here": {
            "field": "Name the field u want to aggregate here"
          }
        }
      }
    }
    ```

  - Example

    ![image-20240129161534706](https://i.imgur.com/MwbtyK1.png)

#### Bucket Aggregation

上述的例子是 aggregate on ONE subject of documents (即只對文件一個子集做聚合數據處理)，如果要對文件的多個子集做處理，就使用 bucket aggregation

例如說 Transactions index 可能就會依照月份份成不同的 buckets，還有其他不同`bucket aggs`

1. Date Histogram Aggregation
2. Histogram Aggregation
3. Range Aggregation
4. Terms Aggregation

##### Date Histogram Aggregation

- 根據日期區間做分組

- 有兩種定義日期區間的方式

  1. `Fixed_interval`：區間值是固定的常數

     ```
     GET {enter_name_of_index_here}/_search
     {
       "size": 0,
       "aggs": {
         "Name ur aggregations here": {
           "date_histogram": {
             "field": "Name the field u want to aggregate on here",
             "fixed_interval": "Specify the interval here"
           }
         }
       }
     }
     ```

     Example

     ![image-20240129175237148](https://i.imgur.com/II3xdia.png)

  2. `Calendar_interval`：

     - 區間會有些微變化，像是日光節約時間、大小月、leap seconds(閏秒)

     - Syntax
  
       ```
       GET {enter_name_of_index_here}/_search
       {
         "size": 0,
         "aggs": {
           "name ur aggregations here": {
             "date_histogram": {
               "field": "Name the field u want to aggregate on here",
               "calendar_interval": "Specify the interval here"
             }
           }
         }
       }
       ```
       
       Example
       
       ![image-20240129175142547](https://i.imgur.com/WlQBgIU.png)

- Bucket sorting for data histogram aggregation

  - 日期buckets預設是以升冪方式排序，加入`order`參數可以修改排序

  - 直接上Example

    ```
    GET ecommerce_data/_search
    {
      "aggs": {
        "name ur aggregations here": {
          "date_histogram": {
            "field": "@timestamp",
            "calendar_interval": "1M",
            "order": {
              "_key": "desc"
            }
          }
        }
      }
    }
    ```

    ![image-20240129175634473](https://i.imgur.com/rubRPRN.png)

  

##### Histogram Aggregation
- 可以根據任何數值區間（numerical interval）分成多個buckets

- Syntax

  ```
  GET {enter_name_of_index_here}/_search
  {
    "size": 0,
    "aggs": {
      "name your aggregrations here": {
        "histogram": {
          "field": "name the field u want to aggregate on here",
          "interval": Specify the interval here
        }
      }
    }
  }
  ```

- Example

  ```
  GET ecommerce_data/_search
  {
    "size": 0,
    "aggs": {
      "price_interval_descending": {
        "histogram": {
          "field": "UnitPrice",
          "interval": 10
        }
      }
    }
  }
  ```

  ![image-20240129180547945](https://i.imgur.com/XanOYs9.png)

  ![image-20240129180349104](https://i.imgur.com/xHxpYLf.png)

##### Range Aggregation

- 讓你能夠定義不同intervals of varying sizes (可供客製化)

- 例如將單價分組成 (\$50以下)(\$50~\$200區間)(\$200以上)

- Syntax

- ```
  GET {enter_name_of_index_here}/_search
  {
    "size": 0,
    "aggs": {
      "name your aggregrations here": {
        "range": {
          "field": "name the field u want to aggregate on here",
          "ranges": [
            {
              "to": x
            }, 
            {
              "from": x, 
              "to": y
            },
            {
              "from": x
            }
          ]
        }
      }
    }
  }
  ```

- Example

  ```
  GET ecommerce_data/_search
  {
    "aggs": {
      "transactions_per_custom_price_ranges": {
        "range": {
          "field": "UnitPrice",
          "ranges": [
            {
              "to": 50
            }, 
            {
              "from": 50, 
              "to": 200
            },
            {
              "from": 200
            }
          ]
        }
      }
    }
  }
  ```

  ![image-20240130102154666](https://i.imgur.com/IDM6fu2.png)

##### Terms Aggregation

- creates a new bucket for every unique term it encounters for the specified field

- 通常用來找document中，搜尋頻率最高的terms

- 例如：要識別交易金額最高的前五筆客戶

- Syntax

  ```
  GET {enter_name_of_index_here}/_search
  {
    "aggs": {
      "name your aggregrations here": {
        "terms": {
          "field": "name the field u want to aggregate on here",
          "size": State how many top results u want returned
        }
      }
    }
  }
  ```

- Example --- Descending

  ```
  GET ecommerce_data/_search
  {
    "aggs": {
      "top_5_countries": {
        "terms": {
          "field": "Country",
          "size": 5
        }
      }
    }
  }
  ```

  ![image-20240130103132239](https://i.imgur.com/CGJstzy.png)

- Example --- Ascending

  ```
  GET ecommerce_data/_search
  {
    "aggs": {
      "5_countries_w/_lowest ecommerce data": {
        "terms": {
          "field": "Country",
          "size": 5,
          "order": {
            "_count": "asc"
          }
        }
      }
    }
  }
  ```

  ![image-20240130103434037](https://i.imgur.com/qHreq6U.png)

##### Combined Aggregations

- 有些查詢需要合併不同aggregations來完成

- 例如：每日的營業總額是多少？

  `doc['UnitPrice'].value * doc['Quantity'].value`

  - script: 在ElasticSearch裡動態創建搜尋結果
  - for each document in our bucket, multiply **the value of field price** by **the value of the field Quantity**

  ```
  GET ecommerce_data/_search
  {
    "aggs": {
      "transactions_per_day": {
        "date_histogram": {
          "field": "@timestamp",
          "calendar_interval": "day"
        },
        "aggs": {
          "daily_revenue": {
            "sum": {
              "script": {
                "source": "doc['UnitPrice'].value * doc['Quantity'].value"
              }
            }
          }
        }
      }
    }
  }
  ```

  ![image-20240130105308021](https://i.imgur.com/UZZkMed.png)

- 可以建構任何combinations of aggregations來回答更複雜的問題

- 例如：取得每日營業額以及每日的unique countries數量

  - 需要在每個bucket中計算多個metric aggregations

- Example

  ```
  GET ecommerce_data/_search
  {
    "size": 0, 
    "aggs": {
      "transactions_per_day": {
        "date_histogram": {
          "field": "@timestamp",
          "calendar_interval": "day"
        },
        "aggs": {
          "daily_revenue": {
            "sum": {
              "script": {
                "source": "doc['UnitPrice'].value * doc['Quantity'].value"
              }
            }
          },
          "number_of_unique_countries_per_day": {
            "cardinality": {
              "field": "Country"
            }
          }
        }
      }
    }
  }
  ```

  ![image-20240130111250073](https://i.imgur.com/h9dr3DR.png)

  - Bucket 預設是以日期升冪排序，如果要改用每日營業額排序呢？
    - 最簡單的方法是改用每日營業額給dataset分成不同buckets

##### Sorting by metric value of a sub-aggregation

- 不用總是以時間區段、數字區段分組，也可以用 metric value of a sub-aggregation 分組

- 以下範例計算了每日營業額以及每日採購國家數量，如果要查截至今哪天的日營業額最高，只需要加入order參數，再用daily_revenue數值給buckets降冪排序

  ```
  GET ecommerce_data/
  ```

- Example

  ```
  GET ecommerce_data/_search
  {
    "size": 0, 
    "aggs": {
      "transactions_per_day": {
        "date_histogram": {
          "field": "@timestamp",
          "calendar_interval": "day",
            "order": {
              "daily_revenue": "desc"
            }
        },
        "aggs": {
          "daily_revenue": {
            "sum": {
              "script": {
                "source": "doc['UnitPrice'].value * doc['Quantity'].value"
              }
            }
          },
          "number_of_unique_countries_per_day": {
            "cardinality": {
              "field": "Country"
            }
          }
        }
      }
    }
  ```

  ![image-20240130113116910](https://i.imgur.com/lyQcd7Y.png)

## Part 5/6 Mapping

Mapping 定義了一份document以及其field是如何被索引以及儲存的

學習定義自己的mapping有以下幫助：

1. 優化ElasticSearch的效能
2. 節省空間

#### Index a Document

- Syntax

  ```
  POST {enter_name_of_index_here}/_doc
  {
    "field": "value"
  }
  ```

- Example

  ![image-20240130120732081](https://i.imgur.com/SqFzBv8.png)

##### Mapping Explained

- mapping決定了document以及其field如何被索引，且定義了儲存的type類型

- 包含 names list 以及對應的 field type

  ![image-20240130121351970](https://i.imgur.com/DOKrSQZ.png)

##### Dynamic Mapping

- 當user沒有事先定義mapping，Elasticsearch 在需要的情況下會預設建立或更新映射(mapping)，稱之為動態映射
- Elasticsearch 查看個別欄位，並且試圖從field內容推論出資料型別，給每個field分配一個型別，建立一個包含field name 與對應型別 (type) 的清單 (mapping)
- 檢視mapping `GET {name_of_the_index}/_mapping`
- field包含的[所有型別](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html)

##### Indexing Strings

- string 資料型別有兩種：1️⃣ Text    2️⃣ Keyword
- 預設的，每個字串被映射兩次，一次是text field，另一次是keyword multi-field
- 這兩種資料型別適用於不同類型的需求：
  - 關鍵字 (keyword) 用於精確匹配和聚合 (exact searches, aggregations, sorting)
  - 文本 (text) 用於全文檢索 (full-text searches)
    - Non-Case sensitive manner 不管大小寫

##### Text Field Type

1. Text analysis：文本經過Analyzer分成多個詞 (individual tokens)，不包含標點符號

   ![image-20240130145756669](https://i.imgur.com/yvhE0Vs.png)

2. Inverted index：一旦字串經過分析後，individual tokenu以及相關的doc_ID 會存在一個sorted list，稱為`inverted index`

3. 如果再新增一筆內容與前一筆 (doc_1) 一模一樣的 document (doc_2)，那麽只會增加 doc_id 欄

4. 假設doc_3內容是 `These chestnut are sourced from Turkey.`，那 Inverted index 就會長得像下面這樣

   | term      | doc_id    |
   | --------- | --------- |
   | are       | 1,2,**3** |
   | china     | 1,2       |
   | from      | 1,2,3     |
   | chestnuts | 1,2,**3** |
   | sourced   | 1,2,**3** |
   | these     | 1,2,3     |
   | turkey    | **3**     |

> Text field type is optimal for full-text search，因為只要client發出要查找包含 term `sourced` OR `from`的 documents的請求，Elastic Cluster 只要去 inverted index 裡面找出對應的 doc_id 就行。

##### Keyword Field Type

1. 建立了 keyword field 之後，不會分析內容，也沒有inverted index，而是使用 doc values 的資料結構來儲存資料

2. Doc values 適用於 Aggregations, sorting, exact searches 的檢索情境

   每新增一筆 document，其 doc_id 以及原封不動的 original string(保留大小寫) 會被存進 doc values

   | doc id | doc value   |
   | ------ | ----------- |
   | 4      | New Florida |
   | 5      | New Mexico  |
   | 6      | New Jersey  |


但你的使用場景並不是每次都需要這兩種 field types，這樣的話，預設設定會有點浪費，因為會減緩 indexing 速度，而且佔掉比較多的磁碟空間。自行定義mapping可以讓資料的儲存與檢索更有效率

##### 練習

專案：建構一個可以讓client管理農產品倉儲庫存的app

###### 需求：

1. 查詢產品名稱、產地國家、產品敘述
2. 辨別出採購紀錄頻率最高的產地來源國
3. 以類型分類農產品（水果 or 蔬菜）
4. 取得每月費用總結的資料

###### 樣本資料：

```json
{
    "name": "chestnut",
    "botanical_name": "Castanea sativa",
    "produce_type": "Fruit",
    "country_of_origin": "China",
    "date_purchased": "2024-02-05T02:16:24",
    "quantity": 325,
    "unit_price": 7.17,
    "description": "a large, brown nut that covered with burlike skin and grows on a tree, often cooked and eaten hot",
    "vendor_details": {
        "vendor": "The Lee's Lander",
        "main_contect": "Chipi Lee",
        "vendor_location": "TienJing, China",
        "preferred_vendor": true
    }
}
```

###### 行動方案：

1. 可搜尋產品名稱、產地來源國、產品敘述
   - fields: name, country_of_origin, description
   - 全文檢索 
   - name 以及 description 不需要聚合/分類/精確搜尋 ➡️ text only
   - country_of_origin: To be decided
2. 辨別出採購紀錄頻率最高的產地來源國
   - field: country_of_origin
   - Terms aggregration
   - 需要用到全文檢索以及聚合查詢 ➡️ text and keyword
3. 以類型分類農產品（水果 or 蔬菜）
   - field: produce_type
   - 需要分類 Sorting，不用全文檢索 ➡️ keyword only
4. 取得每月費用總結的資料
   - fields: date_purchased, quantity, unit_price
   - Date histogram aggregration
     - 建立以月份區分的buckets (date_purchased)
   - Metric aggregation
     - 計算每一筆採購的總額 (total = quantity * unit_price)
     - 將同月份buckets中，所有採購總額加總成 monthly expense

###### 根據所需功能優化的mapping

| String field(s)   | 適合的 field type |
| ----------------- | ----------------- |
| country_of_origin | Text and Keyword  |
| description_name  | Text only         |
| produce_type      | Keyword only      |

- `vendor_details`物件以及`botanical_name`field不會用到，可以disabled

##### 自定義 mapping

###### Rules

1. 如果沒有提前定義mapping，Elasticsearch會動態建立mapping
2. 如果要自建mapping，可以在創建索引時一起執行
3. 一個索引只定義一個mapping，一旦這個索引建立之後，只能在mapping增加新的field，無法變更既有field的mapping type
4. 如果一定要變更既有field的型別，一定要先建立一個新的索引+理想的mapping，再將所有文檔 reindex 進新的索引裡

###### 步驟一：將樣本document索引進一個test index

這個樣本文檔必須包含你想要定義的 fields，這些 fields 也必須包含與你想要定義的field type型別相近的值

- Syntax

  ```
  POST {name_of_test_index}/_doc
  {
    "field": "value"
  }
  ```

- Example

  ![image-20240130163918382](https://i.imgur.com/9gs99YU.png)

###### 步驟二：檢視動態映射 (dynamic mapping)

- Syntax

  ```
  GET {name_of_test_index}/_mapping
  ```

- Example

  ![image-20240130164326294](https://i.imgur.com/nFrNhiW.png)

- mapping fields 以英文字母排序，會顯示Elasticseach 動態映射的結果，可以將其作為自訂 mapping 的範本

###### 步驟三：編輯自訂mapping

1. 將上述結果複製貼到左邊輸入框，刪除第二行的 `"test_index": {` 以及對應的`}`

2. country_of_origin 要保留 keyword & text，不更改

3. description, name 要刪除 keyword multi field (參下圖 line 31 - 36 以及 line 30的逗號)

   ![image-20240130165006261](https://i.imgur.com/mDrZjuf.png)

4. product_type 要刪除 text type，只有keyword only

   Before:

   ![image-20240130165223787](https://i.imgur.com/e4HSUrL.png)

   After

   ![image-20240130165242919](https://i.imgur.com/p0bZuPZ.png)

5. botanical_name 以及 vendor_details

   - 因為搜尋用不到，故刪除兩個 types，加入"enabled" 參數並設為false

   - 這樣就不會給這兩個欄位建立 inverted index 或者 doc values

     ![image-20240130165923069](https://i.imgur.com/EHcUsg3.png)

###### 步驟四：用步驟三的自訂mapping，建立新的索引

- Syntax

  ```
  PUT {name_of_ur_final_index}
  {
    copy and paste edited mapping from step#3
  }
  ```

- Example

  - "enable**d**" 要打對，不然Elasticsearch會以為你沒有定義field

  - 另外這個 console 有提供 auto ident 工具幫你自動縮排

  ![image-20240130171331680](https://i.imgur.com/yCT81eF.png)



###### 步驟五：確認 final index 其中所有 field 對應的 type 都正確

`GET produce_index/_mapping`

✅ country_of_origin 用到 Text and Keyword

✅ description_name 只用 Text

✅ produce_type 只用 Keyword

❎ 計算每個產品的總費用，並存在新的 field `total_expense`（使用runtime fields)

✅ botanical_name & vendor_details 停用，避免建立不必要的inverted index或doc values

![image-20240130172025878](https://i.imgur.com/bGlMCTT.png)

###### 步驟六：將dataset 編入新的索引中

First document

![image-20240130172255688](https://i.imgur.com/vm0Vpcs.png)

Second document

![image-20240130172836248](https://i.imgur.com/dlHjJ10.png)

> 📍這裡不小心按了兩次，順便附上 deleteById的方法
>
> `DELETE {the_index_to_execute}/_doc/{the_doc_id_to_delete}`
>
> ![image-20240130173251915](https://i.imgur.com/s2gHirl.png)

Third document

- 假設這份產品有原定mapping沒有包含的field (`organic`)，會怎樣？正常新增

  ![image-20240130174048027](https://i.imgur.com/2ERquxU.png)

- 確認一下這個索引目前的mapping，發現 Elasticsearch 動態新增了一個 field

  ![image-20240130174413711](https://i.imgur.com/yNVvUOh.png)

###### 如果要變更既有的 field type，怎麼辦？

如果原本不開放搜尋的 botanical_name 現在需要支持全文檢索，只能建立新的 index，再將所有文檔重新編入新的索引

- 重編入新索引的Syntax

  ```
  POST _reindex
  {
    "source": {
      "index": "{name_of_the_index_OLD_mapping}"
    },
    "dest": {
      "index": "{name_of_the_new_index_NEW_mapping}"
    }
  }
  ```
  
- Example
  
  ![image-20240131100748782](https://i.imgur.com/3PJstQi.png)

###### Runtime Field

- Runtime is that moment in time when elasticsearch is executing your requests.

- 在runtime期間，可以建立一個暫時的欄位(temporary field)，並在裡面計算數值

- 如下步驟

  1. 建立`runtime field`把它加進既有index的mapping

     ```
     PUT {enter_name_of_index}/_mapping
     {
       "runtime": {
         "name_ur_runtime_field_here": {
           "type": "specify_field_type_here",
           "script": {
             "source": "specify_the_formula_u_want_executed"
           }
         }
       }
     }
     ```

     ![image-20240131101948036](https://i.imgur.com/PXzgLyf.png)

  2. 檢視更新後的 mapping

     - 這個runtime field並沒有列在properties裡，也沒有編進索引

     - 只有在執行請求時，會創建此欄位並計算值

       ![image-20240131102253402](https://i.imgur.com/zCNxMtQ.png)

  3. 測試runtime field執行情況

     - Syntax

       ```
       GET {enter_name_of_the_index}/_search
       {
         "size": 0,
         "aggs": {
           "name_ur_aggregations here": {
             "specify_the_aggregation_type_here": {
               "field": "name_the_field_u_want_to_aggregate_on_here"
             }
           }
         }
       }
       ```

     - Example

       剛建立的 runtime field 就在 line# 7

       ![image-20240131102726662](https://i.imgur.com/t3bSgry.png)



## Part 6/6 Troubleshooting errors

### 常見錯誤

#### Unable to connect

可能是 cluster 出問題或者是連線問題，要查看 network status 以及 cluster health

#### Connection unexpected closed

node 可能死了，或者是 network 問題，要重試請求

#### 5XX Errors

是Elasticsearch 內部server出錯，要查看 Elasticsearch log

#### 4XX Errors

是 client side 出錯，應檢查請求內容並修正，以下篇幅主要都是這方面的問題排除！

---

##### CRUD相關的錯誤

1. 404 no such index

   `GET common_errors/_doc/1` 查無此索引

2. 405 incorrect HTTP method for uri, allowed: \[x]

   - Example

     ```
     PUT common_errors/_doc
     {
       "field_one": "content_one"
     }
     ```

   - PUT 給一份document編入索引，並且指定一個ID，所以應該要寫成

     `PUT common_errors/_doc/{specified_document_id}`

   - POST 給一份document編入索引，Elasticsearch自行產生ID

3. 400 Unexpected Character

   - was expecting a comma to separate Object entries at [Source:...] line: x

   - Example

     ```
     POST common_errors/_update/1
     {
       "doc": {
         "field_1": "value_1"
         "field_2": "value_2",
       }
     }
     ```

   - 通常是物件的欄位之間忘記用逗號分開，或者不該有逗號的地方放了逗號

##### Query相關錯誤

1. 400 [x] query does not support [y]

   - *[range] query does not support [date]* --- 誤導
   - 建議去官方 [documentation](https://www.elastic.co/guide/index.html) 搜尋 range query 語法，就能發現`gte` `lte`應該用大括號包起來

   ```
   GET news_headlines/_search
   {
     "query": {
       "range": {
         "date":
           "gte": "2015-02-05",
           "lte": "2015-02-16"
       }
     }
   }
   ```

2. 400 Unexpected character

   - *was expecting double-quote to start field name* - 原因一樣很模糊
   - `"type": "phrase"` 應該放在 `multi_match`括號內

   ```
   GET news_headlines/_search
   {
     "query": {
       "multi_match": {
         "query": "party planning",
         "fields": [
           "headline",
           "short_description"
         ]
       },
       "type": "phrase"
     }
   }
   ```

3. 400 parsing_exception

   - Example 查詢2017-02-05發行的娛樂版新聞

     ```
     GET news_headlines/_search
     {
       "query": {
         "match": {
           "category": "ENTERTAINMENT",
           "date": "2018-02-05"
         }
       }
     }
     ```

   - 因為 `match query` 只能接收 document **單一個** field的檢索，如果有多個查詢條件，要改用 `Bool Query` (參 part 3/6, must, must_not, should, filter子句)

     - isEntertainment? true/false
     - isOn20180205? true/false ... 可以用 filter 子句，裡面裝兩個match queries

     Corrected example：

     ```
     GET news_headlines/_search
     {
       "query": {
         "bool": {
           "filter": [
             {
               "match": {
                 "category": "ENTERTAINMENT"
               }
             },
             {
               "match": {
                 "date": "2018-02-05"
               }
             }
           ]
         }
       }
     }
     ```

##### Aggregations相關錯誤

1. 400 Aggregation definition for \[x], expected a \[y]

   ```
   GET news_headlines/_search
   {
     "aggs": {
       "size": 0,
       "by_category": {
         "terms": {
           "field": "category"
         }
       }
     }
   }
   ```

   Error message:

   "Aggregation definition for [size starts with a [VALUE_NUMBER], expected a [START_OBJECT]."

   > START_OBJECT在這裡是指 `aggs`,  `自訂的聚合名稱`, `terms`, `field`
   >
   >  size 出現在不對的地方，應該加在 `aggs`外面

2. 400 Field \[x] of type \[y] is not supported for z type of aggregation

   - Date Histogram (fixed_interval): 以InvoiceDate分組，每8小時分一組
   - *Field [InvoiceDate] of type [keyword] is not supported for aggregation [date_histogram]*   - 因為這個欄位型別是keyword、不是date，故不支援依日期分組
   - Date histogram aggregation 只支援 date 或者 date range values

   ###### 與`_meta` field 相關的錯誤

   - 這個field通常出現在使用自訂mapping新建索引的時候

     ```json
     {
       "mappings": {
         "_meta": {
           "created_by": "ml-file-data-visualizer"
         },
         "properties": {...}
       }
     }
     ```

   - 用自建mapping新增索引時，不太需要加上`_meta`的內容，可以直接刪除

   ###### 關於 type date format 的知識

   - Elasticsearch是以 ISO8601 作為預設日期格式 (例：2023-02-16T17:39:05.123Z)
   - 如果文檔內容的時間格式與 ISO8601 不同，Elasticsearch就會拋錯
   - "format" 內容的定義方式依照 [DateTimeFormatter](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html) 之標準

3. 400 Found two aggregation type definitions in [x]: y and z

   - Elasticsearch能夠合併不同 aggregation 來回答更複雜的問題

   - 例如說要取得每日營業額以及the number of unique customers per day

     1. 先將資料分組成 daily buckets
     2. 在每個bucket中，計算營業額以及the number of unique customers per day

     ```
     GET ecommerce_data/_search
     {
       "size": 0,
       "aggs": {
         "transactions_per_day": {
           "date_histogram": {
             "field": "InvoiceDate",
             "calendar_interval": "day"
           },
           "daily_revenue": {
             "sum": {
               "script": {
                 "source": "doc['UnitPrice'].value * doc['Quantity'].value"
               }
             }
           },
           "number_of_unique_customers_per_day": {
             "cardinality": {
               "field": "CustomerID"
             }
           }
         }
       }
     }
     ```

   - Error message: *Found two aggregation type definitions in [transactions_per_day]: [date_histogram] and [daily_revenue]*  - occurred at line 9

   - 原因：aggregation請求的結構不正確。應該是aggregations (計算日營業額與去重的來客數量) within aggregations (依一天為單位分層)

   - 解法：再用一個 `"aggs": {}` 把 `daily_revenue`以及`number_of_unique_customers_per_day`包在大括號裡
