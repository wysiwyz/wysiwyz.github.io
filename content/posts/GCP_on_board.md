---
title: "GCP on board"
date: 2024-01-10T13:01:59+08:00
author: "celine"
tags: [""]
categories: ["StudyNote"]
---

## Mod1_GCP 簡介

- 什麼是雲端運算？

  1. **自助式隨選服務** - 無需人為介入即可取得資源
  2. **廣泛的網路存取權** - 隨時隨地都可存取
  3. **資源集區** (共享資源的概念) - 提供者可將資源分享給客戶
  4. **迅速彈性** - 隨需快速取得更多資源
  5. **依使用的服務計費** - 用多少，付多少

- 雲端技術如何發展而成？未來方向為何？

  - 實體/主機代管：由使用者設定、管理與維護
  - [Phase2] 虛擬化：由使用者設定，由供應商代管與維護
  - [Phase3] 無伺服器：全自動化

- GCP 運算架構 [採用代管基礎架構] ↔️ [採用動態基礎架構]

  | Compute Engine | Kubernetes Engine | App Engine | Cloud Functions | 代管服務 |
  | -------------- | ----------------- | ---------- | --------------- | -------- |
  | IaaS           | 混合式            | PaaS       | 無伺服器邏輯    | 自動     |

- GCP 在全球設立187個網路邊緣位置（POP，網際網路交換中心），並投資鋪設20+條海底電纜

- GCP 依區域和可用區劃分

  - 區域region、可用區zone

  - 每個區域裡有多個可用區zone (大部分3個)，一個資料中心就是一個區域region

    ![europe-west-zone](https://i.imgur.com/Z1nifa5.png)

    ![image-20240110131544139](https://i.imgur.com/llzPbGw.png)

- Google善盡環保責任

  - 100%落實碳中和政策(自2007年起) 

    > Data Center 2030年達成零碳排

  - 全球數一數二的再生能源採購企業

  - 資料中心率先通過 ISO 14001 認證

- 貼近客戶需求的計價方式

  - 以秒/毫秒計費：適用於運算、資料處理和其他服務
  - 續用折扣：虛擬機器的使用時間超過當月的25%，就會自動套用此折扣 (upto 30%)
  - 承諾使用折扣：以較低的價格取得長期穩定的工作負載 (around 50%off)
  - 先占使用折扣：以較低價格取得可中斷的工作負載
  - 自訂VM執行個體類型：僅為應用程式所需的資源付費 (pricing calculator)

- 開放式API和開放原始碼讓客戶能改用其他供應商的服務

  - 開放式API；與開放原始碼服務相容
    - Cloud Bigtable
    - Cloud Dataproc（兼容Hadoop）
  - 採用開放原始碼以打造豐富生態系統
    - TensorFlow
    - Kubernetes
    - Forseti Security
  - 適合多供應商環境的技術
    - Google Operations Suite
    - Kubernetes Engine (支援混合雲)

- Google技術基礎架構將安全性納入設計考量 (由下而上⬆️)

  | 分層         | 重要安全措施(僅列舉部分)                                     |
  | ------------ | ------------------------------------------------------------ |
  | 作業安全性   | 入侵偵測系統、降低內部風險的技術、員工使用U2F、軟體開發作法 (e.g.紅隊演練) |
  | 網際網路通訊 | Google Front End；加入阻斷服務功能的設計 (預防ddos attack)   |
  | 儲存服務     | 靜態資料加密 (金鑰加解密)                                    |
  | 使用者身分   | 支援U2F的中央身分識別服務                                    |
  | 服務部署     | 服務間通訊加密 (e.g. RPC, 磁碟加密)                          |
  | 硬體基礎架構 | 硬體的設計和來源、安全的啟動堆疊、設施安全                   |

  - Google Front End(GFE)：確保 TLS 傳輸層安全

- 選用GCP的優點

  - 提供高度安全、穩定可靠且彈性十足的基礎架構，幫助開發人員輕鬆部署

- 運算服務

  1. Compute Engine
  2. Kubernetes Engine (代管K8S的平台)
  3. App Engine
  4. Cloud Functions

- 儲存服務

  1. Bigtable
  2. Cloud Storaage
  3. Cloud SQL
  4. Cloud Spanner
  5. Cloud Datastore

- 大數據

  1. BigQuery
  2. Pub/Sub
  3. Dataflow
  4. Dataproc
  5. Datalab

- 機器學習

  1. Natural Language API
  2. Vision API
  3. AI Platform
  4. Speech API
  5. Translate API

Q1 創新GCP計價方式

1. by sec
2. ?
3. ?

Q2 GCP 使用好處

1. 善盡環保責任
2. 對開放原始碼技術的承諾
3. 穩固的基礎架構

---

### 1. 資源階層結構

![image-20240110134418510](https://i.imgur.com/THAfGBG.png)

#### 雲端安全

![image-20240110134553331](https://i.imgur.com/RtgrrVM.png)

#### 資源階層定義了信任界線

- 依機構架構將資源分組
- 階級中的每一層都可作為信任界線，達到資源隔離的效果（往下繼承）

#### 有三個屬性可用於識別專案

| 專案ID     | 專案名稱 | 專案編號   |
| ---------- | -------- | ---------- |
| 全域不重複 | 可以重複 | 全域不重複 |
| 由您選擇   | 由您選擇 | 由GCP指派  |
| 無法變更   | 可變更   | 無法變更   |

#### 資料夾提供靈活的管理功能

- 資料夾將機構下的專案分門別類
- 資料夾可以包含專案和/或其他資料夾
- 資料夾可用於指派政策

#### 機構節點可組織專案

organization node

#### IAM資源階層示例

![image-20240110135301298](https://i.imgur.com/xcJBRTH.png)

- 如果bookself專案有對Cloud Storage寫入的身份，而組織節點有對Cloud Storage讀取的身份，那麼bookself實際可讀可寫Cloud Storage（UNION)

### 2. IAM

誰可以對什麼資源做什麼事情

#### IAM政策可以套用至四種主體

![image-20240110135650081](https://i.imgur.com/w7aLQZY.png)

#### 可以做什麼：IAM角色是一組相關聯的權限

![image-20240110135736393](https://i.imgur.com/uRBSjz6.png)

#### 可用的資源：使用者會取得階層中特定項目的角色

定在某個節點上，他就會往下繼承，決定在哪一個服務生效

![image-20240110135851630](https://i.imgur.com/eSD5oIv.png)

#### IAM角色有三種類型

1. 基本角色：會套用到專案中的所有GCP服務

   ![image-20240110140003019](https://i.imgur.com/duTw9hg.png)

2. 預先定義角色：會套用至專案中的<u>特定GCP服務（e.g. no-sql bigtable</u>)

   ![image-20240110140236684](https://i.imgur.com/IdKAs9f.png)

3. 自訂角色：可以讓您定義一組精確的權限

   通常建議企業給員工設定原則為最低權限模型

   ![image-20240110140358161](https://i.imgur.com/Z7MWpwN.png)

#### 可以使用哪些工具來管理具備管理權限的GCP user？

![image-20240110140707211](https://i.imgur.com/zmbhfEw.png)

#### 如果已經有其他公司目錄，怎麼辦

- Google Cloud Directory Sync

![image-20240110140846315](https://i.imgur.com/JGkYKKx.png)

### 3. 與GCP互動

#### 有四種GCP互動的方式

![image-20240110141002706](https://i.imgur.com/I3rg1aX.png)

##### (1) Google Cloud Platform Console

可以滿足大部分需求（但不是全部）

支援git協定的版本控制系統 --- cloud source repositories

- 可集中管理所有專案資料的控制台
- 開發人員工具
  - Cloud Source Repositories
  - Cloud Shell
  - Test Lab (行動應用程式測試)
- 可存取產品API
- 管理和建立專案

##### (2) Google Cloud SDK

命令列工具

- SDK 包括適用於 Cloud Platform 產品和服務的 CLI 工具
  - `gcloud`, `gsutil`(Cloud Storage), `bq` (BigQuery)
- 可做為 Docker 映像檔
- 可透過 Cloud Shell 使用
  - 在 Compute Engine 執行個體上執行的 Cloud SDK 容器化版本

##### (3)Cloud Console行動應用程式

在DevOps 或 SRE 時會用到

- 管理虛擬機器和資料庫執行個體
- 管理Google App Engine中的應用程式
- 管理帳單
- 透過可以自訂的資訊主頁，以視覺化方式呈現專案

##### (4) 符合REST樣式的API

- 以程式輔助的方式存取產品和服務
  - 通常使用JSON作為交換格式
  - 使用OAuth 2.0 進行驗證與授權
- 透過Google Cloud Platform Console啟用
- 為了協助控管支出，大多數的服務都包含每日配額和頻率(限制)
  - 您可以申請調高配額和頻率

- 另外也有支援GRPC(Google Remote procedure call)的 API

#### Questions

- Q: 如果Google Cloud IAM政策授予您專案層級的擁有者權限，則您對專案內資源的存取權限，可能會因為資源具有較嚴格的政策而受限，正確或錯誤？
  - A: 政策是父政策與資源政策的聯集。如果父政策的限制性較低，則會覆寫限制性較高的資源政策。

![image-20240110141629724](https://i.imgur.com/F8qqvCh.png)

![image-20240110141644371](https://i.imgur.com/BFQE9DU.png)

---

## Mod2_VM 與儲存空間

### 1. 虛擬私人雲端(VPC)網路

#### Virtual private cloud

- 每個虛擬私人雲端網路都位於GCP專案中 (專案建立都會有預設的VPC)

- 您可以佈建 Cloud Platform 資源，使這些資源彼此連接或隔離

  

#### Google Cloud虛擬私人雲端網路是全球性資源，但子網路是區域性資源

- 子網路可以用內部ip相互通訊
- us-east1區域內建立了 us-east1-b 與 us-east1-c ，位於不同的可用區

![image-20240110142204242](https://i.imgur.com/t3RleDC.png)

### 2. Compute Engine

#### Compute Engine 提供代管虛擬機器

一定要先有網路才能建VM。RFC1918的內部通訊。

![image-20240110142604844](https://i.imgur.com/f2fW6vR.png)

#### Compute Engine 貼近客戶需求的計價方式

- 以秒計費、續用折扣、承諾使用折扣
- 先占執行個體
- 儲存空間提供高總處理量，且不額外收費
- 自訂機器類型：僅支付所需硬體的費用

#### 使用Compute Engine擴充或縮減

![image-20240110142958558](https://i.imgur.com/qQfdyVR.png)

### 3. 重要的虛擬私人雲功能

#### 虛擬私人雲端網路的拓撲(topology)由您掌控

路由表：如何把流量從一個VM傳送到網路中的另一個VM，是內建的不用管理

防火牆：『全球分散式的』防火牆，可以限制某台VM連進連出的流量限制

- 使用**路由表**來轉送網路內的流量，甚至轉送跨子網路的流量
- 使用**防火牆**來控制允許的網路流量
- 透過共用虛擬私人雲端 (Shared VPC) 與其他GCP專案共用整個網路或獨立子網路
- 使用虛擬私人雲端對等互連 (VPC Peering)，讓GCP專案中的網路相互連結

#### 有了全球通用的Cloud Load Balancing，你的應用程式就能為世界各地的使用者呈現單一前端

解決流量分配的問題

這裏使用名詞是Load Balancing （負載平衡這件事）而不是Load Balancer（負載平衡裝置），GCP提供的是負載平衡服務（由軟體控制）

自動失效轉移的功能：當該區域的計算服務失效，負載平衡會自動移轉到健康的後端

![image-20240110143805199](https://i.imgur.com/llzPbGw.png)

#### Google虛擬私人雲端提供一系列的負載平衡選項

![image-20240110144308032](https://i.imgur.com/1eu1Xie.png)

區域性的負載平衡：後端只能放在單一區域

#### Cloud DNS可用性高且可擴充

`8.8.8.8`

- 建立代管區域，接著新增、編輯和刪除DNS記錄
- 使用符合REST樣式的API或指令列介面，以程式輔助方式管理區域和記錄

#### Cloud CDN (內容傳遞聯播網)

content delivery network 

- 利用 Google 遍及全球的邊緣快取技術，在靠近使用者的位置快取內容
  - 好處：
    1. 使用者的延遲感受較低
    2. 這個請求不會消耗運算資源
    3. 不會需要輸出到 Internet 上，費用較低
- 如果想使用不同的CDN，也可以使用CDN Interconnect

### Question

![image-20240110145043269](https://i.imgur.com/vgVyHST.png)

Cloud Virtual Network, Cloud Interconnect, Cloud DNS, Cloud Load Balancing, CDN

![image-20240110145130061](https://i.imgur.com/FzrfXST.png)

Calculate by second, Customized machine type, spot VM

---

### 儲存空間選項

#### 1. Cloud Storage

##### Cloud Storage 是二進位大型物件儲存空間

用多少付多少，不用預先估容量

定址到這個物件的 key 是以網址形式呈現

資料加密不能停用，也不需要特別啟用他，不會另外多收錢

第三方儲存工具 FUSE：可以幫你把Cloud Storage以檔案系統方式呈現，但Cloud Storage底層還是物件式儲存

- 高效能、網際網路規模
  - 管理簡單
- 無需管理容量
- 靜態資料加密
- 預設會加密從Google傳輸到端點的資料
- 提供線上和離線匯入服務



##### Cloud Storage 檔案會歸入不同的<u>值區(bucket)</u>

![image-20240110145857356](https://i.imgur.com/EpFgRxk.png)

##### 有幾種方法可以將資料移入Cloud Storage

1. 線上移轉：`gsutil`或`gcloud`指令上傳，或者在主控台上傳(拖曳)
2. Storage Transfer Service 移轉服務：可以管理、排定批次移轉作業 (TB等級規模)
3. Transfer Appliance 移轉裝置：租一個儲存裝置，線下寄過去 (PB等級規模)

![image-20240110150421159](https://i.imgur.com/7ksOmji.png)

##### Cloud Storage 可以與其他 GCP 服務搭配使用

![image-20240110150742707](https://i.imgur.com/pYrlWwu.png)

- 使用BigQuery或CloudSQL匯入匯出資料

- Compute Engine映像檔、開機指令腳本

#### 2. Cloud Bigtable

##### Cloud Bigtable 是代管 NoSQL

Google mail, map, analytics 都會用到

![image-20240110150949843](https://i.imgur.com/24RGJ2b.png)

##### 選用Cloud Bigtable的優點

如果資料庫很龐大(>1TB)，為半結構或結構化的資料

要求低延遲，高處理量

要求NoSQL，不需要關聯式語意

- 複製 (Replicated) 儲存空間
- 傳輸中的資料和靜態資料都會加密
- 以角色為依據的存取控制清單
- 驅動主要應用程式，例如Google Analytics(分析) 和 Gmail



#### 3. Cloud SQL & Cloud Spanner

##### Cloud SQL 是代管RDBMS

SQL資料庫修補patch，備份工作交給GoogleCloud，也可以控制哪些IP才能存取資料庫，某種程度上是取代了DBA的工作

- 提供MySQL, PostgresSQL, SQL Server資料庫式服務
- 自動複製 (Replication)
  - 可以是GCP外部，或者Compute Engine裡面的RDS
- 代管備份
- 垂直資源調度 (讀取和寫入)
- 水平資源調度 (讀取)
- Google 安全性

##### Cloud Spanner 是可水平擴充的RDBMS

- Cloud Spanner支援：
  - 自動複製
  - 全球同步一致性
  - 具有高可用性的代管執行個體
  - SQL (ANSI 2011及擴充功能)

要高可用、關聯式、資料量大、每秒鐘IO請求大-> use this

#### 4. Cloud Datastore

##### Cloud Datastore是可水平擴充的NoSQL DB

查詢作業速度是依照結果集大小而定，不是依資料集大小
 (aka Datastore模式底下的Firestore)

![image-20240110152037350](https://i.imgur.com/f09dYkB.png)

#### 5. 比較儲存選項：

##### 技術詳細資料

OLAP(線上分析處理), OLTP(線上交易處理)

![image-20240110152211966](https://i.imgur.com/DNfkwvd.png)

##### 用途

![image-20240110152654195](https://i.imgur.com/M54H9nQ.png)

#### Questions

- Question：您的應用程式會為大型影片檔案轉碼，應該先考慮使用哪種儲存服務？
  - Answer: Google Cloud Storage

- Question：您要從裝有感測器的裝置中串流大量資料，應該先考慮使用哪種儲存服務？
  - Answer: IoT, 大量資料串流 ➡️ Google Cloud Bigtable

---

## Mod3_容器與應用程式開發、部署與監控

### 1. 簡介

#### IaaS可以讓你透過虛擬化硬體來共用資源

![image-20240110153905606](https://i.imgur.com/g9FBIWq.png)

三台機器有各自系統，可以自己安排執行期、中繼軟體、磁碟空間, etc.

#### 但靈活性會導致啟動所需的時間（分鐘）和資源（GB）增加

每個副本要去複製一次，導致速度變慢

![image-20240110154109182](https://i.imgur.com/eGhzqYz.png)

#### App Engine可讓使用者存取程式設計服務以及客戶端函式庫

![image-20240110154213772](https://i.imgur.com/q1M9Iz3.png)

#### 隨著對應用程式需求的增加，此平台可以依據工作附載和基礎架構，以快速且獨立的方式擴充應用程式

可以擴充但無法微調底層架構

![image-20240110154304917](https://i.imgur.com/SSquScS.png)

#### 容器提供IaaS靈活性和PaaS可擴充性

![image-20240110154404457](https://i.imgur.com/OYNklVL.png)

#### 容器可以設定、彼此獨立且極為輕便可攜

假設build出了綠色的容器，可以直接把他丟到雲端（容器作為交付元件）

![image-20240110154551694](https://i.imgur.com/zG56SU0.png)

#### 透過共用主機設定，即可將容器部署在稱為叢集的一組伺服器上

使用大量的容器建立一個應用程式，讓容器具有模組化功能，可以在多台主機獨立擴充微型服務。容器可以增減，主機也可以增減。

![image-20240110154650136](https://i.imgur.com/lBFnsxG.png)

### 2. K8S和GKE

#### Kubernetes

##### Kubernetes讓你可以在多台主機上輕鬆自動化調度管理多個容器

![image-20240110154955173](https://i.imgur.com/cpVe7MO.png)



##### 第一步：以容器的形式建構應用程式並加以執行

![image-20240110155107454](https://i.imgur.com/djfjgqg.png)



##### 如何在Kubernetes中執行這個應用程式？

`requirements.txt`: python app 需要哪些相依性套件

`Dockerfile`: 指定如何將程式碼封裝成映像檔的步驟

- 指定環境
- 在指定的虛擬環境安裝
- 從本機複製到空中的虛擬環境
- 在空中環境執行
- 指定進入點`ENTRYPOINT`

![image-20240110155150620](https://i.imgur.com/TzMl0BL.png)



##### 接著以映像檔的形式建構容器並加以執行

![image-20240110155651045](https://i.imgur.com/y9zvCzc.png)



##### 使用Kubernetes API在稱為叢集的一組節點上部署容器

建立叢集的指令如下

```terminal
gcloud container clusters create k1
```

![image-20240110155839444](https://i.imgur.com/RsxTbZ1.png)

##### 以下是啟動Kubernetes Engine的方法

在GKE叢集中，你可以指定：

- 機器類型
- 節點數
- 網路設定等等

##### 在節點上部署容器時，使用Pod來包裝容器

Pod: 太空艙、綠豆莢(一個豆莢裡很多綠豆)、是包裝容器的抽象化單元，是可以部署的最小單元

![image-20240110160342349](https://i.imgur.com/yQnDm7F.png)



##### 可以使用`kubectl run`指令執行Pod中的容器

```terminal
kubectl run nginx --image=nginx:1.15.7
```

![image-20240110160417725](https://i.imgur.com/7o60Tox.png)

##### 您可以使用Deployment管理用於應用程式或工作負載的一組Pod副本，並確保所需的數量正在執行，並且狀態良好

```terminal
kubectl get pods
```

就會列出蓋叢集所有pod的清單



##### 預設情況下，Pod只能在叢集內使用，並且會取得臨時IP

將deployments暴露在外，建立負載平衡器

```terminal
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

![image-20240110160751470](https://i.imgur.com/oj0Ezh5.png)



##### 符合前述IP的所有用戶端請求都會轉送至Service物件後方的Pod

deployment物件：維持pod的可用性

![image-20240110160854348](https://i.imgur.com/ooVEfOV.png)

##### 如果要取得Service的公開IP，請執行`kubectl get services`

![image-20240110160954664](https://i.imgur.com/xF1UI3h.png)



##### 如要調整Deployment的資源配置，請執行 `kubectl scale`

擴充（帶參數：三個複製）手動擴充

![image-20240110161118826](https://i.imgur.com/arpb7oc.png)



##### 自動擴充Service後面的Pod

![image-20240110161232779](https://i.imgur.com/xo12DHz.png)



##### 宣告式作業

declarative 真正強的地方，以組態檔設定

```terminal
kubctl get deployment nginx -o yaml
```

replicas: 3 -> 這個deployment需要維護三個nginx pods的副本

selector -> 把符合條件的pods圈選起來，歸類進Deployment物件

但 pod 也要有對應的 label (`app:nginx`)才能被選到

![image-20240110161321513](https://i.imgur.com/DFyXTma.png)



##### 宣告式作業---套用變更

![image-20240110161708259](https://i.imgur.com/WWTVxRp.png)



##### 執行`kubectl get pods`以查看上線的Pod

![image-20240110161749724](https://i.imgur.com/YzeGlWG.png)



##### `kubectl get deployments`或`describe deployments`，以確保適當數量的副本執行中

![image-20240110161840940](https://i.imgur.com/av29yze.png)



##### `kubectl get services`再次確認service的公開IP

達成分散負載、擴充服務的目標

![image-20240110162005026](https://i.imgur.com/ZW7cFSr.png)



##### 滾動式更新策略

`kubectl rollout`、`kubectl apply`

一個一個建立新版本，建立一個新的就砍一個舊的，會有新舊版同時存在的情況

![image-20240110162101259](https://i.imgur.com/HqQA8tW.png)

### Questions

- 請列舉兩個使用容器部署應用程式的理由
  1. 跨開發、測試和實際工作環境的可攜性
  2. 遷移工作負載更簡單
  3. 便於建立微型服務（鬆散耦合）
  4. 靈活性
- Kubernetes讓您可以在不同雲端供應商叢集裡管理容器，正確或錯誤？
  - correct，而且GKE現在已經是跨平台產品了。Kubernetes Engine可以在不同環境中管理K8S。
- GCP提供高速容器映像檔案儲存服務，可與Kubernetes Engine搭配使用，正確或錯誤？
  - correct. 稱為 Artifiact Registry (原本的 Container Registry 停用了🥲)

---

### 雲端中的運算環境

#### 1. AppEngine

![image-20240110162811159](https://i.imgur.com/xyNF0g6.png)

#### 2. Standard GAE

![image-20240110162844665](https://i.imgur.com/aQk4Gca.png)

##### Requirements

![image-20240110162919930](https://i.imgur.com/zZ5argG.png)

#### 3. Flexible GAE

##### AppEngine彈性環境

也可以自訂執行期

因為是用VM執行映像檔案，如果啟用除錯模式，可以ssh連進去彈性環境給你的VM，除錯完在停用

![image-20240110163024237](https://i.imgur.com/alqcbC5.png)



##### 比較 App Engine

![image-20240110163429125](https://i.imgur.com/HHy4sdG.png)

#### 4. GC Endpoint & Apigee Edge

![image-20240110163644650](https://i.imgur.com/I50fP68.png)

##### Cloud Endpoints

- 分散式的API管理系統
- ESP 可延伸式服務代理伺服器，提供低延遲高效能

![image-20240110163740246](https://i.imgur.com/i6lJfMR.png)



##### Apigee Edge

依照客戶等級限流

![image-20240110164015861](https://i.imgur.com/f1L2LZq.png)

#### 5. Cloud Function

##### Cloud Function

使用微型商業邏輯建立程式，或連到其他雲端服務

![image-20240110164131250](https://i.imgur.com/OLg7Rpr.png)

> Cloud Run: 計費模型跟 cloud function 一樣。cloud run 可以用容器映像檔案部署，cloud function 底層是由 cloud run 幫你完成執行

#### Questions

- 請列舉使用App Engine彈性環境（對比App Engine標準環境）的3項優勢
  - 彈性環境允許SSH存取及磁碟寫入，並支援第三方二進位檔案（也允許自訂堆疊及背景程序）
- Cloud Endpoints 與 Apigee Edge有哪些不同之處？
  - Cloud Endpoints可協助您建立和維護API；Apigee Edge可協助您保護API並透過API盈利

---

### 在雲端開發部署與監控

#### 1.雲端開發

##### Cloud Source Repositories

建立不限數量的 repositories，避免將安全性金鑰退上remote

![image-20240110164710448](https://i.imgur.com/f7rhao0.png)

#### 2.部署基礎架構與程式碼(IaC)

infrastructure as code 其中一種是 Terraform，它是開源的

![image-20240110164923232](https://i.imgur.com/vVMTVMI.png)

#### 3.  監控：主動式監測

##### Operations

Monitoring, Logging訊息紀錄, Profiler效能分析, Error reporting例外通知, Trace延遲

![image-20240110165132379](https://i.imgur.com/oe8PA05.png)

![image-20240110165253546](https://i.imgur.com/NrQdmgI.png)

---

## Mod4_大數據與機器學習

### 1.大數據

![image-20240110165640592](https://i.imgur.com/oE5Rd7v.png)

##### Cloud Dataproc

![image-20240110165703174](https://i.imgur.com/CcIr9D1.png)

##### Cloud Dataflow

比較新一代的是Cloud Dataflow，整合批次處理和串流資料處裡

![image-20240110165736753](https://i.imgur.com/9AXOPa2.png)

##### Dataflow 管道

來源不一定要是GCP上面的 -> 轉換 -> 接收器 sink

![image-20240110165833512](https://i.imgur.com/Lch4UGR.png)

##### BigQuery

可以做ELT，查詢速度快，資料量龐大(幾十TB)

![image-20240110165939325](https://i.imgur.com/ey7cLZw.png)

結合分析和儲存服務 (所以費用也分兩種)

但儲存費用跟 Cloud Storage 差不多，且長期儲存(90天以上) 打折



![image-20240110170024438](https://i.imgur.com/gl4gYcT.png)



##### Cloud Pub/Sub

保證接收者至少收到一筆，或者精準收到一筆

串流資料緩衝接收第一步

![image-20240110170141865](https://i.imgur.com/k4s5Al1.png)



##### Vertex AI Workbench

提供代管的 Jupyter 環境

![image-20240110170319731](https://i.imgur.com/4sMaeJb.png)

##### 為何要用Vertex

跟其他服務整合在一起了，存取資料時，身分驗證會很簡單

![image-20240110170408401](https://i.imgur.com/zSWaxnL.png)

---

### AI平台

#### AI解決方案

訓練深度神經網路：TensorFlow

加速訓練模型：Vertex AI

準備資料 ➡️ 評估 ➡️ 部署 ➡️ 模型監控（避免飄移）

![image-20240110170517467](https://i.imgur.com/DOpP0Ad.png)

#### Cloud Vision API

專門處理圖片辨識：地標、不適當圖片

![image-20240110171007398](https://i.imgur.com/cOIQPjX.png)



音訊轉為文字檔案

![image-20240110171149583](https://i.imgur.com/zYpUoAi.png)



##### Cloud Natural Language API

了解這段話內容，字詞間的交叉參照關係，辨識位置事件，辨識情緒

![image-20240110171211700](https://i.imgur.com/AEEXXE5.png)

##### Cloud Translation API

![image-20240110171310719](https://i.imgur.com/AbyaILN.png)

##### Cloud Video Intelligence API

Vision API的影片版本

![image-20240110171347948](https://i.imgur.com/ZXS675h.png)

#### Questions

- 何時會使用 Cloud Dataproc？
  - 您可以透過這項服務將內部部署Hadoop工作遷移到雲端，還可以用於資料採集和雲端資料分析。
- 請列舉 Cloud Dataflow 的兩個用途
  1. ETL
  2. 自動化調度管理
- 請列舉 Google AI 平台的三個用途
  1. 詐欺偵測
  2. 情緒分析
  3. 內容個人化

---

## Conclusion

![image-20240110171620399](https://i.imgur.com/fLo3nbC.png)

![image-20240110172056832](https://i.imgur.com/2GHawsl.png)

![image-20240110172146826](https://i.imgur.com/Y2EN73s.png)

##### 參考資料

[Module 1 video](https://www.youtube.com/watch?v=mMuZyn5BYCQ)
[Module 2 video](https://www.youtube.com/watch?v=WHZPd0Q5wvM-)
[Module 3 video](https://www.youtube.com/watch?v=zFhZPr2URYQ)
[Module 4 video](https://www.youtube.com/watch?v=4ppPqan3IEs)
[Module 5 video](https://www.youtube.com/watch?v=KYali6POmDU)
