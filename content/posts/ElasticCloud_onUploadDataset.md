---
title: "〔Debug〕ElasticCloud 上傳type為date的csv檔案"
date: 2024-01-29T17:21:01+08:00
author: "celine"
tags: ["ElasticCloud"]
categories: ["StudyNote"]
---

### 錯誤內容：

當dataset其中一個field看起來像是日期可以分成多個buckets，實際上會拋出以下錯誤

```json
{
  "error": {
    "type": "illegal_argument_exception",
    "reason": "Field [InvoiceDate] of type [keyword] is not supported for aggregation [date_histogram]"
  }
}
```

### 問題釐清：

上傳檔案時，3/30/2011 16:34 或者 12/1/2010 9:10 這類的欄位內容被辨識為 `"type": "keyword"`，須要在上傳檔案時點擊 Override settings，勾選 `Contains time field` 再加上時間戳記格式以及對應的 time field，這樣就能正常做 bucket aggregations 了

 ![image-20240129171708505](https://i.imgur.com/Xm7MltO.png)

 ![image-20240129171438559](https://i.imgur.com/QOrSbt1.png)

###   實際結果：

- 看起來 Elastic 幫你增加了一個@timestamp的欄位，而欄位內容確實是與 InvoiceDate 一致

  !(https://i.imgur.com/OFv0uCh.png)

  ![image-20240129173425349](https://i.imgur.com/fV5XOep.png)

### 重編索引 - 使用Kibana console

1. 先 PUT  建立一個自訂索引，細節參考另一篇Elasticsearch筆記，注意`"type":"date"`需要再接上原本source index欄位的日期格式，不然會無法重編索引

2. 接著 `POST _reindex` 加入來源與目標 index 的參數，顯示下圖右結果就成功了

![image-20240131110556154](https://i.imgur.com/n8aJmDt.png)
