---
title: "[筆記] 黑白箱弱點掃描淺淺談"
date: 2024-01-04T14:12:33+08:00
author: "celine"
tags: [""]
categories: ["StudyNote"]
---

## 黑白箱弱點掃描

##### 白箱：

- 又名為輔助邏輯驅動測試 (auxiliary and logic driven testing)、clear-box testing、open-box testing
- 在光譜上是黑箱測試的另一面，滲透測試者對source code以及相關架構設計文件有全部的存取權限
- 在大量可用的data中淘選過濾，目地在於找到潛在弱點，因此它是滲透測試中最耗時的一種
- 白箱滲透測試人員可以對static code進行分析
- 白箱測試可以手動執行，或借助於自動化工具掃出像是 SQL注入攻擊(SQL injection)、跨站腳本攻擊 (cross-site scripting)、緩衝區溢位 (buffer overflow) 等弱點。
- 以下是白箱測試中很重要的工具：source code analyzer, debuggers
- 另外 dynamic analysis tool 也很重要，可以找出static analysis可能會漏掉的弱點，像是目標系統錯誤配置造成的弱點 (misconfiguration of target systems)
- 白箱滲透測試提供了綜合的評估，包含程式的內部弱點與外部弱點

##### 黑箱：

- 又稱為 DAST test (動態分析資安測試 dynamic analysis security testing)
- 黑箱測試對於找出這樣的弱點很有幫助：input/output驗證問題, 伺服器配置相關的錯誤等。
- 黑箱測試的目標是要模擬真實世界的情境，並找出系統在不同情境下的行爲，例如網路攻擊、惡意輸入、阻斷服務攻擊DoS。
- 黑箱測試可以手動操作執行，或透過像是port scanners、fuzzers、web scanners 等自動化工具探測系統弱點。

深透測試目的是為了識別並補強可能會被攻擊者利用的弱點，因此黑箱測試是比較理想的滲透測試方法，因為大多數攻擊者並不知道目標網路的內部行為。

跟黑箱測試相反的則是白箱測試，測試人員有目標系統的完整資訊。

而灰箱測試則是提供測試人員有關目標系統『有限的』資訊，灰箱測試所模擬的資訊瞭解程度是「駭客透過調查與系統偵查足跡(footprinting)的方式，長期使用目標系統所能得到資訊」。

###### 參考資料

[黑箱、灰箱、白箱滲透測試是什麼](https://resources.infosecinstitute.com/topics/penetration-testing/what-are-black-box-grey-box-and-white-box-penetration-testing/)

[以黑箱測試改進應用程式的安全性](https://www.veracode.com/security/black-box-testing)

[白箱資安測試與黑箱測試的異同](https://www.linkedin.com/advice/3/what-key-differences-similarities-between-white-box#what-is-white-box-security-testing?)
