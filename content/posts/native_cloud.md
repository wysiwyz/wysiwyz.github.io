---
title: "Cloud native - the next gen"
date: 2023-03-17T14:02:55+08:00
author: "celine"
tags: ["Terraform", "JEnv", "cloud_native"]
categories: ["StudyNote"]
---

> 現在沒有雲原生可是不行的，了解一下下

### Cloud native

1. 公有雲、私有雲、混合雲
2. 動態環境 - Node上下, **stateless vs stateful**
3. 可擴展 - auto scaling (HPA) 
4. 聲明式 - **ansible**, **terraform**, K8S yaml
5. 容器、微服務、無服務器 - K8S, OCP, Cloud Run, Cloud Function
6. 最小化用戶的操作負擔 - auto healing (liveness, readiness)

> 所有的服務最好都是stateless

### Stateless vs Stateful

1. Stateless
   + 無狀態，每次請求都是獨立
   + example
     + JWT
     + .map()
     + Vue/React/Angular service
     + Deployment (K8S)
2. Stateful
   + 有狀態，每次請求與過去相關
   + example
     + Session
     + .reduce()
     + MySQL
     + Volume/StatefulSet(K8S)

#### 聲明式 - Ansible

#### 聲明式 - Terraform



CKA Kubernetes 認證使用命令式 `kubectl run nginx -image=nginx` 

實務上用聲明式 



Cloud native 的時代

Spring boot 3

1. Java 17 baseline 
2. Support for generating native images with GraalVM(AOT)
3. Improved Observability w/ Micrometer & Micrometer Tracing

### Java Virtual Machine

+ Pros:

  + 跨平台 - 一次編譯，到處運行
  + Interpreter + JIT

+ Cons:

  + 啟動速度慢

  ![image-20230317162605636](https://i.imgur.com/r1t6i38.png)

### GraalVM

+ High performance:

  + 執行更快

+ Cloud Native:

  + 雲原生

+ Polyglot:

  + 支援多語言

+ Instant startup, low footprint:

  + 直接把Java Program編譯成machine code, 執行起來體積更小，啟動更快

  ![image-20230317163126936](https://i.imgur.com/8E8lvR3.png)



### 比較傳統 JVM 與 AOT <sub>ahead-of-time</sub>

|                  | JVM                                                         | AOT                                                         |
| ---------------- | ----------------------------------------------------------- | ----------------------------------------------------------- |
|                  | ![image-20230317164238308](https://i.imgur.com/c6A9CJd.png) | ![image-20230317164254967](https://i.imgur.com/wvGbhtO.png) |
| 打包速度         | FAST                                                        | SLOW                                                        |
| 編譯為機械碼時機 | RUNTIME                                                     | BUILD                                                       |
| 啟動時間         | LONG                                                        | SHORT                                                       |
| 執行速度         | DEPENDS(JIT+INTEPRETOR)                                     | FAST                                                        |



### Java Application on Lambda

Lambda或Cloud Run等Serverless服務，只要一段時間沒使用，就會服務中止

所以每次有新的請求就會重新啟動

傳統JVM啟動時間長，造成成本上升，第一次請求回應時間慢

![image-20230317164115097](https://i.imgur.com/cOsxfQB.png)



```terminal
./gradlew bootBuildImage
docker run --rm -p 8080:8080 demo:1.0.0
...

terraform init
terraform plan
terraform apply
```

![image-20230318091905548](https://i.imgur.com/x8ZQb8S.png)

![image-20230317144930777](https://i.imgur.com/dNeJMxo.png)

### JEnv

+ Java版本管理工具，跟nodeJS管理版本87%像

+ Commands:

  ```
  jenv versions
  ```

  > 查所有可用java版本

  ```
  jenv local {version_name}
  ```

  > 指定此環境使用的java版本

  

> Further Reading:
>
> + [Terraform on Google Cloud](https://cloud.google.com/docs/terraform/get-started-with-terraform)
> + [Oracle JVM HotSpot_JVM架構](http://www.ohsdba.cn/index.php?m=Article&a=show&id=454)
> + [釜底抽薪_AOT](https://ithelp.ithome.com.tw/articles/10297355)
> + [Lambda_Performance_Optimization](https://aws.amazon.com/tw/blogs/compute/operating-lambda-performance-optimization-part-1/)
> + [GraalVM](https://www.graalvm.org/)
> + [古古說GraalVM](https://kucw.github.io/blog/2019/10/java-graalvm/)

















