---
title: "跟著黑哥學 Kubernetes"
date: 2023-10-09T14:24:47+08:00
author: "celine"
tags: [""]
categories: ["StudyNote"]
---

0-1 應用部署方式演變

0-2 kubernetes 介紹

本質是一組服務器集群，每個服務器都稱為一個節點，可以在每個節點中的容器安裝程序

### 0-3 kubernetes 組件

一個 k8s 集群分為控制節點master_node，工作節點worker_node構成，每個節點上都安裝不同的組件

##### Master: 集群的控制平面，負責集群的決策（管理）

1. **API Server**：資源操作的唯一入口，接收用戶輸入的命令，提供認證、授權、API註冊和發現等機制
   K8S 的整個訪問入口
2. **Schedule**r：負責集群資源調度，按照預訂的調度策略將Pod調度到相應的node節點上
   根據算法決定安裝nginx的請球要分配給哪一個node節點（負責算）
3. **ControllerManager**：負責維護集群的狀態，比如程序部署安排、故障檢測、自動擴展、滾動更新等
   如何將 nginx 安裝請求交給 node1節點（負責安排幹活）
4. **Etcd**：負責存儲集群中各種資源對象的信息
   用來紀錄請求信息儲存在資料庫中（也可以改用mysql）

##### Node: 集群的數據平面，負責為容器提供運行環境 [worker_node]

1. **Kubelet**：負責維護容器的生命週期，即通過控制docker，來創建、更新、銷毀容器
   接收控制節點發來的訊息，並傳送命令給 worker node 裡面其他的組件_docker

2. **KubeProxy**：負責提供集群內部的服務發現和負載均衡

   訪問程序的入口

3. **Docker**：負責節點上容器的各種操作

##### 以部署一個 nginx 服務來說明 kubernetes 系統各個組件調用關係

1. 首先要明確，一旦 kubernetes 環境啟動之後，master 和 node 都會將自身的信息存儲到 etcd 數據庫中
2. 一個 nginx 服務的安裝請求會首先被發送到 master 節點的 apiServer 組件
3. apiServer 組件會調用 scheduler 組件來決定到底應該把這個服務安裝到哪一個 node 節點上
   此時，它會從 etcd 中讀取各個 node 節點的信息，然後按照一定的算法進行選擇，並將結果告知 apiServer
4. apiServer 調用 controller_manager 去調度 Node 節點安裝 nginx 服務
5. Kubelet 接收到指定之後，會通知 docker，然後由 docker 來啟動一個 nginx 的 pod
   pod 是 kubernetes 的最小操作單元，容器必須跑在 pod 中至此
6. 一個 nginx 服務就運行了，如果需要訪問 nginx，就需要通過 kube-proxy 來對 pod 產生訪問的代理
   這樣，外界用戶就可以訪問集群中的 nginx 服務了

---

### 0-4 kubernetes 概念

**Master**：集群控制節點，每個集群需要至少一個 master 節點負責集群的管控
	安排工作的

**Node**：工作負載節點，由 master 分配容器到這些 node 工作節點上，然後 node 節點上的 docker 負責容器的運行
	幹活的

**Pod**：kubernetes 的最小控制單元，容易都是運行在 pod 中的，一個 pod 中可以有 1個或著多個容器
	程序跑在容器裡面，容器運行在 pod 裡面；一個 pod 裡面可以有多個容器

**Controller**：控制器，通過它來實現對 pod 的管理，比如啟動 pod、停止 pod、伸縮 pod 的數量等等
	管理 pod 的工具。不僅僅是一種控制器，每種控制器都有適合的應用場景

**Service**：pod 對外服務的統一入口，下面可以維護者同一類的多的 pod

​    配置 ipvs 功能 --- 在 kubernetes 中，service 有兩種代理類型，一種是基於 `iptables` 的，一種是基於 `ipvs` 的，兩者比較的話，ipvs 的性能明顯要高一些，但是如果要使用它，需要手動載入 ipvs 模塊

**Label**：標籤，用於對 pod 進行分類，同一類 pod 的運行環境
	e.g. `app:tomcat` -可以用來當選擇器的條件

> 標籤與選擇器的機制：標籤定義一批屬性，選擇器透過這些屬性把這些東西化成一類

**NameSpace**：命名空間，用來隔離 pod 的運行環境
	在一個 k8s環境中，所有 pod 是可以相互訪問的，要限制可訪問性可以透過 nameSpace區分來達成

---

### 0-5 環境搭建_環境規劃

如何搭建 k8s 的集群環境

#### 0-5-1 環境規劃

##### 0-5-1-1 集群類型

Kubernetes 集群大體上分為兩類：一主多從和多主多從

+ 一主多從：一台Master節點和多台Node節點，搭建簡單，但是有單機故障風險，適合用於測試環境
  ```mermaid
  graph LR;
      Master1 --- Node1;
      Master1 --- Node2;
      Master1 --- Node3;
  
  ```

+ 多主多從：多台Master節點和多台Node節點，搭建麻煩，安全性高，適合用於生產環境

  ```
  [Master1]---┬---[Node1]
              |
  [Master2]---┼---[Node2]
              |
  [Master3]---┴---[Node3]
  ```

##### 0-5-1-2 安裝方式

kubernetes 有多種部署方式，目前主流有 kubeadm、minikube、二進制包

+ minikube：一個用於快速搭建單節點kubenetes的工具
+ kubeadm：一個用於快速搭建kubenetes集群的工具
+ 二進制包：從官網下載每個組件的二進制包，依次去安裝，此方式對於理解kubenetes組件更加有效

##### 0-5-1-3 主機規劃

| 作用   | IP地址          | 操作系統                 | 配置                    |
| ------ | --------------- | ------------------------ | ----------------------- |
| Master | 192.168.109.100 | Centos7.5 基礎設施服務器 | 2顆CPU  2G內存  50G硬盤 |
| Node1  | 192.168.109.101 | Centos7.5 基礎設施服務器 | 2顆CPU  2G內存  50G硬盤 |
| Node2  | 192.168.109.102 | Centos7.5 基礎設施服務器 | 2顆CPU  2G內存  50G硬盤 |

#### 0-6 環境搭建_主機安裝

需要安裝三台 Centos 服務器（一主二從），然後在每台服務器中分別安裝 docker (18.06.3)，kubeadm (1.17.4), kubelet (1.17.4), kubectl (1.17.4) 程序

##### 0-6-1 主機安裝

pass practice

> 0-7 環境搭建_環境初始化
>
> Vmware 起三個 VM, master *1, worker *2
>
> 0-8 環境搭建_集群所需組建安裝
>
> 0-9 環境搭建_集群安裝
>
> 0-10 環境搭建_網路插件安裝
>
> 0-11 環境搭建_環境測試
>
> 0-12 資源管理介紹

#### 0-13 yaml 語言介紹

YAML是一個類似XML、JSON的標記性語言，他強調以**數據**為中心，並不是以標記語言為重點，因而YAML本身的定義比較簡單，號稱一種人性化的數據格式語言

Xml太麻煩了，要以對象的嵌套表示

    ````xml
    <heißenburg>
        <age>23</age>
        <address>Tainan</address>
    </heißenburg>
    ````

```yml
heißenburg:
  age: 15
  address: Tainan
```

###### Note

1. 書寫yaml切記`: ` 後面要加一個空格

2. 如果需要將多段yaml配置放在一個文件中，中間要使用 `---` 分隔

3. 下面是一個 yaml 轉 json 的網站，可以通過它驗證 yaml 是否書寫正確

   https://www.json2yaml.com/convert-yaml-to-json

YAML 的語法比較簡單，主要有下面幾個：

+ 大小寫敏感
+ 使用縮進 (縮排, indentation) 表示層級關係
+ 縮排 (indentation) 不允許使用 tab，只允許空格
  只對低版本的 yaml 有限制，高版本也可以用 tab 鍵
+ 縮排的空格數不重要，只要相同層級的元素左對齊即可
+ `#`表示註解

YAML 支持以下幾種數據類型：

+ 純量：單個的、不可再分的值

  ```yml
  # 純量：一個簡單的值、字符串、布林值、整數、浮點數、null、時間、日期
  # 1 布林類型
  c1: true 
  c11: True
  # 2 整數型
  c2: 234
  # 3 浮點數型
  c3: 3.14
  # 4 null類型
  c4: ~    # 使用~表示null
  # 5 日期類型
  c5: 2017-02-16    # 日期使用ISO 8601格式，即yyyy-MM-dd
  # 6 時間類型
  c6: 2017-02-16T19:02:23+03:00    
      # 時間使用ISO 8601格式，時間和日期之間使用T連接，最後使用+代表時區
  # 7 字符串類型
  c7: heißenburg  # 簡單寫法，直接寫值，如果字符串中間有特殊字符，必須使用雙引號或者單引號包裹
  c71: 'heißenburg honey'
  c8: hima
      laya
      yosemite    # 字符串過多的情況可以拆成多行，
  ```

+ 對象：鍵值對的組合，又稱為映射 (mapping) / 哈希 (hash) /  字典 (dictionary)

  ```json
  {
    "jingle": {
      "age": 23,
      "account": "yoroshiku"
    }
  }
  ```

  ```yml
  # 對象
  # 形式一 (推薦)
  jingle:
    age: 23
    account: yoroshiku
  # 形式二 (了解)
  ingrid: {age: 26,account: sesamestreet}
  ```

+ 數組：一組按次序排列的值，又稱為序列 (sequence) / 列表 (list)

  ```json
  {
    "morality": [
      "禮義",
      "廉恥"
    ]
  }
  ```

  ```yml
  # 數組
  # 形式一（推薦）
  morality:
    - 禮義
    - 廉恥
  # 形式二（了解）
  morality: [禮義,廉恥]
  ```


---

### 0-14 資源管理方式_介紹

+ 命令式對象管理：直接使用命令去操作 kubernetes 資源
  單純查詢，不做修改的時候，通常會使用命令式對象管理

  `kubectl run nginx-pod --image=nginx:1.17.1 --port=80`

+ 命令式對象配置：通過命令配置和配置文件去操作 kubernetes 資源

  `kubectl create/patch -f nginx-pod.yaml`

+ 聲明式對象配置：通過 apply 命令和配置文件去操作 kubernetes 資源
  可以把10個yaml檔案存在一個目錄，再對目錄執行更新操作

  `kubectl apply -f nginx-pod.yaml`
    `apply` - 用於創建和更新資源（有就更新，沒有就創建）

| 類型           | 操作對象 | 適用環境 | 優點           | 缺點                                   |
| -------------- | -------- | -------- | -------------- | -------------------------------------- |
| 命令式對象管理 | 對象     | 測試     | 簡單           | 只能操作活動對象，<br />無法審計、跟蹤 |
| 命令式對象配置 | 文件     | 開發     | 可以審計、跟蹤 | 項目大時，配置文件多，<br />操作麻煩   |
| 聲明式對象配置 | 目錄     | 開發     | 支持目錄操作   | 意外情況下難以調試                     |

---

### 0-15 資源管理方式_1

###### `kubectl` 命令

​    `kubectl`是Kubenetes集群的命令行工具，通過它能夠對集群本身進行管理，並能夠在集群上進行容器化應用的安裝部署，`kubectl`命令的語法如下：
``` terminal
kubectl [command] [type] [name] [flags]
```

+ `command` 指定要對資源執行的操作，例如 create、get、delete  <span style="color:lightgreen;">[必填]</span>
+ `type` 指定資源類型，比如 deployment、pod、service  <span style="color:lightgreen;">[必填]</span>
+ `name` 指定資源的名稱，名稱大小寫敏感 <span style="color:yellow;">[可選]</span>
+ `flags` 指定額外的可選擇參數    <span style="color:yellow;">[可選]</span>

```terminal
# 查看所有 pod
kubectl get pod

# 查看某個 pod
kubectl get pod pod_name

# 查看某個 pod，以 yaml 格式展示結果
kubectl get pod pod_name -o yaml
kubectl get pod nginx-pod-697786dc74-bsdbr -o json  # 以json格式展示成果
```

###### 常用的 command 們

| 命令分類 | 命令    | 命令作用 |
| -------- | ------- | -------- |
| 基本命令 | create  | *創建* 一個資源 |
| －       | edit    | *編輯* 一個資源 [修改] |
| －       | get     | *獲取* 一個資源 |
| －       | patch   | *更新* 一個資源 [打補丁更新] |
| －       | delete  | *刪除* 一個資源 |
| －       | explain | *解釋* .  展示資源文檔 |
| 運行和調試 | run | *運行* .  在集群中運行一個指定的鏡像 |
| － | expose | *暴露* 資源為 Service |
| － | describe | *描述* .  顯示資源內部信息<br />例如IP為何、容器的鏡像、ID、啟動中經過的歷程 |
| － | logs | 輸出容器在 pod 中的日誌 |
| － | attach | 進入運行中的容器 |
| － | exec | *執行* .  執行容器中的一個指令 |
| － | cp | *複製* .  在Pod內外複製文件 |
| － | rollout | *首次展示* .  管理資源的發布 |
| － | scale | *規模化* .  擴 (縮) 容Pod的數量 |
| － | autoscale | *自動調整* Pod的數量 |
| 高級命令 | apply | 通過文件對資源進行配置 |
| － | label | *標籤* .  更新資源上的標籤 |
| 其他命令 | cluster-info | *集群信息* .  顯示集群信息 |
| － | version | *版本* .  顯示當前 Service 和 Client 的版本 |

###### 常用的資源類型

Kubenetes 中所有的內容都抽象為資源，可以通過命令進行查看 

```powershell
kubectl api-resources
```

| 資源分類       | 資源名稱                 | 縮寫   | 資源作用        |
| -------------- | ------------------------ | ------ | --------------- |
| 集群級別資源   | nodes                    | no     | 集群組成部分    |
| ---            | namespaces               | ns     | 隔離pod         |
| pod 資源       | pods                     | po     | 裝載容器        |
| Pod 資源控制器 | replicationcontrollers   | rc     | 控制pod資源     |
| ---            | replicasets              | rs     | 控制pod資源     |
| ---            | deployments              | deploy | 控制pod資源     |
| ---            | daemonsets               | ds     | 控制pod資源     |
| ---            | jobs                     |        | 控制pod資源     |
| ---            | cronjobs                 | cj     | 控制pod資源     |
| ---            | horizontalpodautoscalers | hpa    | 控制pod資源     |
| 服務器發現資源 | services                 | svc    | 統一pod對外接口 |
| ---            | ingress                  | ing    | 統一pod對外接口 |
| 存儲資源       | volumeattachments        |        | 存儲            |
| ---            | persistentvolumes        | pv     | 存儲            |
| ---            | persistentvolumeclaims   | pvc    | 存儲            |
| 配置資源       | configmaps               | cm     | 配置            |
| ---            | secrets                  |        | 配置            |

##### 命令式對象管理 操作案例 (Demo)

```powershell
# 創建一個namespace
[root@master ~] kubectl create namespace dev
namespace/dev created

# 獲取namespace
[root@master ~] kubectl get ns
NAME							STATUS	AGE
default						Active	21h
dev								Active	21s
kube-node-lease		Active	21h
kube-public				Active	21h
kube-system				Active	21h

# 在此namespace下創建並運行一個nginx的pod
[root@master ~] kubectl run pod --image=nginx -n dev
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/pod created

# 查看新創建的pod
 [root@master ~] kubectl get pod -n dev
delete pod pod_name -n dev 
# 刪了之後 k8s 會自動 deploy 新的 pod, 要刪整個 namespace dev 才行（...略）
```

---

### 0-16 資源管理方式_2

##### 命令式對象配置

1. 建立一個 nginxpod.yaml 

   ```yml
   apiVersion: v1
   kind: Namespace
   metadata:
     name: dev
   
   ---
   
   apiVersion: v1
   kind: Pod
   metadata:
     name: nginxpod
     namespace: dev
   spec:
     containers:
     - name: nginx-containers
       image: nginx:1.17.1
   ```

2. 執行 create command，創建資源

   ```powershell
   [root@master ~] kubectl create -f nginxpod.yaml
   namespace/dev created
   pod/nginxpod created
   ```

3. 





---

0-17 資源管理方式_3

0-18 資源管理方式_結語

1-1 實戰入門_Namespace

1-2 實戰入門_Pod

1-3 實戰入門_Label

1-4 實戰入門_Deployment

1-5 實戰入門_Service

1-6 Pod解析_結構和定義

1-7 Pod解析_基本配置

1-8 Pod解析_鏡像拉取策略

1-9 Pod解析_啟動命令

1-10 Pod解析_環境變量

1-11 Pod解析_端口設置

1-12 Pod解析_資源配額

2-1 Pod解析_生命週期 _概述

2-2 Pod解析_生命週期 _創建和終止

2-3 Pod解析_生命週期 _初始化容器

2-4 Pod解析_生命週期 _鉤子函數

2-5 Pod解析_生命週期 _容器探測

2-6 Pod解析_生命週期 _容器探測補充

2-7 Pod解析_生命週期 _重啟策略

2-8 Pod解析_調度 _概述

2-9 Pod解析_定向調度

2-10 Pod解析_親和性調度 _概述

2-11 Pod解析_親和性調度 _nodeAffinity

2-12 Pod解析_親和性調度 _podAffinity

2-13 Pod解析_親和性調度 _podAntiAffinity

2-14 Pod解析_調度 _污點

2-15 Pod解析_調度 _容忍

3-1 Pod 控制器_概述

3-2 Pod 控制器_ReplicaSet

3-3 Pod 控制器_Deployment _基礎

3-4 Pod 控制器_Deployment _擴容器

3-5 Pod 控制器_Deployment _升級策略

3-6  Pod 控制器_Deployment _版本回退

3-7 Pod 控制器_Deployment _金絲雀發布

3-8 Pod 控制器_HPA (上)

3-9 Pod 控制器_HPA (下)

3-10 Pod 控制器_DaemonSet

3-11 Pod 控制器_Job

3-12 Pod 控制器_CronJob

3-13 Service_概述

3-14 Service_資源清單文件介紹

3-15 Service_實驗 _環境準備

3-16 Service_實驗 _ClusterIP 類型

3-17 Service_實驗 _HeadLines 類型

3-18 Service_實驗 _NodePort 類型

3-19 Service_實驗 LoadBalancer 類型

3-20 Service_實驗 ExternalName 類型

4-1 Ingress 介紹

4-2 Ingress案例_環境準備

4-3 Ingress案例_http代理

4-4 Ingress案例_http代理

4-5 數據存儲介紹

4-6 基本存儲_EmptyDir

4-7 基本存儲_HostPath

4-8 基本存儲_NFS

4-9 高級存儲_pv和pvc的介紹

4-10 高級存儲_pv

4-11 高級存儲_pvc

4-12 高級存儲_pv和pvc的生命週期

4-13 配置存儲_configmap

4-14 配置存儲_secret

4-15 安全認證_概述

4-16 安全認證_認證方式

4-17 安全認證_授權管理

4-18 安全認證_進入控制

4-19 DashBoard_部署

4-20 DashBoard_使用演示
