---
title: "[筆記] OWASP API Security Top 10"
date: 2024-01-15T20:28:58+08:00
author: "celine"
tags: ["security", "OWASP"]
categories: ["StudyNote"]
---

## What is OWASP Security Top 10

你會學到：API相關弱點的多樣類型、包含的來襲方向（attack vector）、弱點利用可能造成的技術影響

這些人都該來瞭解一下：API生態系的相關人士, bug bunty hunters, 開發人員, 滲透測試人員, 組織領導, 對API Security有興趣的人

（而且現在APISec University有一個certifcation bage提供給CASA 測驗通過的人，好像是個酷東西！）

### What is OWASP

- OWASP 是 Open Web Application Security Project 的縮寫，為非營利基金會，目標是改善應用程式的資訊安全。
- 創立於2001年12月1日，以Top10List的開源工具還有其他安全專案而聞名
- 創立者：Erez Yalon、Inon Sheedy
- OWASP API Security Top 10 首次於 2019 年 12 月發佈，基於以下幾點原因而啟動
  1. APIs迅速的興起：不用自己造輪子開發地圖、GPS、付款交易、驗證、溝通相關的程式，可以直接使用其他家專精開發且成熟完善的功能
  2. 安全重大缺口：以前用來作為企業級弱點管理的工具與技術（例如：程式、掃描工具與傳統網路資安監控工具）不是專為處理API相關資安風險而設計的，因此存在資料外洩(data breaches) 的風險
  3. 新的攻擊方向： 通常公開網路API忽略了所有過去幾十年隨著商機一起成長的資安手法。
     - 攻擊者不用再經歷MITRE網路攻擊鏈（bypass firewall, gain entry to the network, pivot around  to a system that contains data, find a way to exfiltrate that data
     - 攻擊者可以利用不穩定的API，直接存取資料源

> 一些上新聞的 API 意外事件
>
> - 2018 USPS data leak
> - 2019 Venmo public API scraping
> - 2021 Peloton API Data leak
> - 2021 Parler API Data leak
> - 2021 LinkedIn API Data leak
> - 2022 Coinbase Authorization flaw
> - 2022 Optus API Data Breach
> - 2022 Toyota API Exposure
> - 2023 EatonWorks Toyota Research
> - 2023 T-mobile API Data Exposure

### How is the Top 10 compiled?

根據OWASP API Security Project領導人 Paulo Silva所述，2023 OWASP API Security Top 10 list 由團隊成員所彙整，基於以下幾點：

- internal research
- publicly available data
- bounty platforms and news

### Mapped to External Sources

 The OWASP API security risks are associated with references to external sources, including CWEs (common weakness enumeration), other OAS projects, and the national institute of Standards and Technology (NIST Guidance).

大部分的參考來源於CWE (a list of common software and hardware vulnerabilities developed by the community and hosted by MITRE)，每個CWE由唯一識別號所區分 (CWEID)，對應到特定的一件弱點。

### Updates to the OWASP API Security Top 10

從OWASP API Security Top 10 2019年發佈以來，API使用度增加了，API相關的資料洩漏事件持續出現，而且新的API科技也推陳出新 ➡️ 需要更新版本

#### API 使用急速成長

- API 攻擊風險：Akamai 回報每天有 114M 起AP攻擊 (2021年)
- 2021 API市值：$2.2B
- 2031 API預估市值：$41.5B（10年20倍成長）
- 2022: 46M Postman collections, 3M API repos in GitHub

> "We aspire to follow the security trends that are relevent to APIs and have been developed in recent years.  If you try to hack or protect an API that was developed five years ago, it would make more sense to refer to the 2019 list."  -- Iron Shkedy, OWASP API Security Project leader  
>
> 如果要攻擊或保護的API是在五年以前開發的，那看2019年版會比較適合

#### 2019 vs 2023 差異比較

- `API08:2019 Injection` 以及 `API10:2019 Insufficient Logging & Monitoring` 兩個項目被還是很重要，只是被新來的推到前十名之外了

- 三個項目沒變

  - `API01:2023` Broken Object Level Authorization (又稱為 `BOLA`)
  - `API05:2023` Broken Function Level Authorization (又稱為`BFLA`)
  -  `API08:2023 (was API07:2019)` Security Misconfiguration

- 四個項目被重新命名了

  - `API02:2019` Broken User Authentication ➡️ `API02:2023` Broken Authentication 
    - to bring additional attention to the API authentication as a whole process
  - `API09:2019` Improper Assets Management ➡️ `API09:2023` Improper Inventory Management
    - to better align with industry terminology  
    - to cover a wider range of resources (Asset: tangible items valued by an organization; Inventory: a wider set of resources)

- 五個是新來的

  - 包含2019 OWASP Top 10的部分元素
    - `API03:2023` Broken Object Property Level Authorization 
      - 又稱`BOPLA`
      - 合併`3:2019`Excessive Data Exposure +`6:2019`Mass Assignment
    - `API04:2023` Unrestricted Resource Consumption
      - 與`4:2019` Lack of Resources and Rate Limiting 有關
  - `API06:2023` Unrestricted Access to Sensitive Business Flows
  - `API07:2023` Server Side Request Forgery [全新]
    - 沒有在2019版裡面，但有出現在2021 OWASP top ten list
    - 不是因為實際事件發生記錄，而是民調結果才使它上榜
  - `API10:2023` Unsafe Consumption of APIs [全新]

- Risk Rating：根據OWASP risk rating terminology (OWASP risk rating framework)

  | Theat Agents | Exploitability                        | Weakness Prevalence                      | Weakness Detectability                | Technical Impact                  | Business Impacts  |
  | ------------ | ------------------------------------- | ---------------------------------------- | ------------------------------------- | --------------------------------- | ----------------- |
  | API Specific | Easy: 3 \| Average: 2 \| Difficult: 1 | Widespread:3\| Common: 2 \| Difficult: 1 | Easy: 3 \| Average: 2 \| Difficult: 1 | Sever: 3 \| Moderate:2\| Minor: 1 | Business Specific |

- OWASP API Security Top 10的目的不是要給特定組織做風險分析，而是提供準則給組織考量風險因素

  | Risk         | Exploitability | Prevalence | Detectability | Technical | Overall |
  | ------------ | -------------- | ---------- | ------------- | --------- | ------- |
  | risk#1 ~ #10 | (1~3分)        |            |               |           | (加總)  |

- 風險的方程式：`risk = likelihood * impact`

  - likelihood : a relative value to be determined by the business

## The Top 10

### API1:2023 
#### Broken Object Level Authorization

1. BOLA: 最普遍且最嚴重的API弱點

2. 當API provider 不具備足夠的控制權來enforce authorization時會發生，

3. API使用者應該只能存取屬於他們自身的機敏資料，如果BOLA發生，攻擊者可以存取其他用戶的機敏資料

4. Attack Vector Descriptions

   > "Attackers can exploit API endpoints that are vulnerable to a broken object level authorization by manipulating the ID of an object that is sent within the request. Object IDs can be anything from sequential integers, UUIDs or generic strings. Regardless of the data type, they're easy to identify in the request target, either through the path or a query string parameter request headers, or even as a part of the request payload."

5. Security Weakness Description

   > "This has been the most common and impactful attack on APIs. Authorization and access control mechanisms in modern web applications are complex and widespread. Even if the application implements a proper infrastructure for authorization checks, developers might forget to use these checks before accessing a sensitive object. Access controlled detection is not typically amenable to automated static or dynamic testing."

6. Impacts Description

   > "Unauthorized access can result in data disclosure to unauthorized parties, data loss, or data manipulation. Unauthorized access to objects can also lead to full account takeover"

7. 簡單來說，如果id為1717的Himmel可以取得id為1716的Chelsea的個資，就是BOLA

8. 就以下範例而言，可以透過替換粗體字部份來測試

   - 常見、好利用、需要的技術不高

   | Options | API請求的URL                       | 試圖存取相似的物件                  |
   | ------- | ---------------------------------- | ----------------------------------- |
   | GET     | /api/user/**1**                    | /api/user/**7**                     |
   | GET     | /user/account/find?**user_id=22**  | /user/account/find?**user_id=23**   |
   | POST    | /company/account/**Apple**/balance | /company/account/**Google**/balance |
   | GET     | /admin/settings/account/**bman**   | /admin/settings/account/**hdent**   |


OWASP 預防性措施

   - Implement authorization with user policies and hierarchy 以用戶政策與階層實作授權
   - Verify users have access to the resources they're accessing 驗證用戶要存取資源時，擁有進入權
   - Use random object IDs wherever possible 可以的話，都使用隨機號的物件ID 
     - GUID for records ID 使用全域唯一識別碼

   - Test APIs to identify any authorization vulnerabilities 

---

### API2:2023 

#### Broken Authentication

- 指的是API驗證過程中的任何弱點

- Authentication是驗證API user身份的流程，可能是person, device 或system（驗證『真為其人』）

- 常用驗證方式：用戶帳號密碼 (username and password combination)、token、多因素驗證 (multifactor authentication)

  - 發生原因：API 供應者沒有實作強度不夠的驗證機制，或驗證步驟有誤

- Attack Vector Description

  > "The authentication mechanism is an easy target for attackers since it's exposed to everyone. Although more advanced technical skills may be required to exploit some authentication issues, exploitation tools are generally available."

- Security Weakness Description

  > "Software and security engineers' misconceptions regarding authentication boundaries and inherent implementation complexity make authentication issues prevalent. Methodologies of detecting broken authentication are available and easy to create."

- Impacts Description

  > "Attackers can gain complete control of other users' accounts in the system, read their personal data, and perform sensitive actions on their behalf. Systems are unlikely to be able to distinguish attackers' actions from legitmate user ones."

- Weak Password Policy

  - 允許用戶建立強度較弱的密碼
  - 允許用暴力攻擊嘗試猜出其他用戶密碼
  - 在不詢問密碼確認的情況下，允許用戶變更密碼
  - 在不詢問密碼確認的情況下，允許用戶變更帳戶電子信箱
  - 在URL連結裡揭露token或者密碼
  - GraphQL queries allow for many authentication attempts in a single request（？
  - 對於機敏的請求，沒有做驗證

- Credential Stuffing 憑證填充

  - Attack using large number of username and password combinations 使用大量的帳號密碼組合攻擊
  - Credentials used in these types of attacks are typically collected from data breaches 此類型的身分驗證資訊通常從資料洩漏中收集而來
  - Allows users to brute force many username and password combinations 允許用戶用很多帳號密碼的組合做暴力攻擊

- Predictable Tokens

  - Tokens obtained through a weak token generation authentication process 
    經由強度薄弱的產生權杖驗證步驟，取得權杖
  - Weak tokens can easily be guessed, deduced, or calculated by an attacker 
    攻擊可以透過計算、臆測、推論得到強度很弱的權杖
  - Using incremental or guessable token IDs 
    使用遞增或容易猜測的ID權杖

- Misconfigured JSON Web Tokens

  - JWTs commonly used for API authentication and authorization 
    常用於API驗證授權JWT
  - Provide flexibility to customize algorithms used for signing the token, the key/secret that is used, and the information used in the payload
  - Security misconfigurations:
    - API provider accepts unsigned JWT tokens  
      API供應者接受未經簽署的JWTs
    - API provider does not check JWT expiration 
      API供應者未確認JWT逾期日
    - API provider discloses sensitive information within encoded JWT payload 
      API供應者於 encoded JWT payload 裡揭露機敏資訊
    - JWT is signed with a weak key
      使用強度薄弱的key簽署JWT token

> "The future of digital systems is complexity and complexity is the worst enemy of security."  --- Bruce Schneider, Internationally Renowned Security Technologist

- API Authentication

  - REST requires APIs to be stateless 

  - APIs, therefore, require users to register to acquire tokens

  - Token is used for future requests

    從REST APIs的六個限制可以知道，RESTful API 是設計成無狀態的，為了達成無狀態，API供應者應該不用記住上一個請求到下一個請求的消費者。要實行此限制，APIs通常要求users經歷註冊步驟，來取得唯一的權杖，而這個產生出的權杖會用於接下來的幾個請求，作為驗證授權之用途

- Issues with Authentication

  - Insufficient token randomness (entropy) 如果不夠隨機亂數，駭客會猜得到
  - Vulnerabilities in registration process 
  - Password reset process 密碼重設步驟
  - Multi-factor authentication features 多因素驗證

- OWASP preventative measures

  - Know all the possible flows to authenticate to the API 
    瞭解所有取得API驗證的可能流程

  - Understand your authentication mechanism and how they are used - **OAuth is not authentication, and neither are API keys**

    瞭解驗證的機制，以及如何運用此機制。

  - Use standards for authentication, token generation, or password storage 
    不要自己造輪子，用既有標準

  - Treat credential recovery/forgot password endpoints as login endpoints in terms of brute force, rate limiting, and lockout protectionsㄛ

  - Require re-authentication for sensitive operations (e.g. changing the account owner email address/2FA phone number) 對於機敏資料的操作，必須要求重新驗證

  - 多利用 OWASP Authentication Cheatsheet

    - [Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)

  - 可以的話，實作多因素驗證 MFA

  - 實作反暴力攻擊的機制以減輕content stuffing, dictionary attacks, brute force attacks

  - 實作 account lockout/captcha 機制，以預防針對特定用戶的暴力攻擊

  - 實作 weak-password checks

  - API keys應該只能用來作為API clients authentication，不該用來 user authentication

---

### API3:2023 

#### Broken Oject Property Level Authorization

BOPLA 合併了 2019 OWASP API Security Top10 兩點: Excessive Data Exposure, Mass Assignment

- `API:2019`Excessive Data Exposure

  - 當API供應者對單一個請求回傳了整個data object 

  - Usually APIs filter data to what is being requested 
    通常API要負責過濾，只留下所請求的部份

  - Relying on customer/UI to filter creates risk of exposing sensitive information
    如果留給client端或前端過濾，會有機敏資訊暴露的風險

    

- `API:2019`Mass Assignment

  - Mass Assignement allows user input to alter sensitive object properties
    允許user調整機敏物件的配置
  - 例如：API使用特別的property來建立管理員帳號
    - 應該只有被授權的user可以提出『alter those administrative properties』的請求
    - If no restriction in place, then an attacker would be able to elevate their privileges and perform administrative actions by altering these properties.

- Attack Vector Description

  > "APIs tend to expose endpoints that return all object's properties. This is particularly valid for REST APIs. For other protocols such as GraphQL, it. may require crafted requests to specify which properties should be returned. Identifying these additional properties that can be manipulated requires more effort, but there are a few automated tools available to assist in this task."

- Security Weakness Description

  > "Inspecting API responses is enough to identify sensitive information in returned objects' representations. Fuzzing is usually used to identify additional (hidden) properties. Whether they can be changed is a matter of crafting an API request and analysing the response. Side-effect analysis may be required if the target property is not returned in the API response."

- Impacts Description

  > "Unauthorized access to private/sensitive object properties may result in data disclosure, data loss, or data corruption. Under certain circumstances, unauthorized access to object properties can lead to privilege escalation or partial/full account takeover."

- BOPLA Vulnerabilities 弱點

  - The API endpoint exposes properties of an object that are considered sensitive and should not be read by the user. (previously named: "Excessive Data Exposure")
  - The API endpoint allows a user to change, and/or delete the value of a sensitive object's property which the user should not be able to access (previously named: "Mass Assignment")

- Excessive Data Exposure

  - EDE is when API responds with more information than needed
  - Often occurs when the consumer is expected to filter results
  - 暴露過多資訊

- Test for Data Exposure

  - Excessive Data Exposure is very difficult to detect with automated scanner 
    很難透過自動化掃描器偵測出來
  - Often bypasses every security control in place to protect sensitive information
  - Can deliver sensitive data to an attacker simply because they use the API
    只要攻擊者使用這個API，就能收到機敏資料
  - API providers **must test** API endpoints and review information in response
    API供應者務必測試每個endpoint並審核回傳的信息

- Mass Assignment

  - API consumer includes more parameters than the app intended
    使用API那一方在發送請求時，傳了多餘的參數

    - `account_privilege_level` 變更權限等級
    - `account_balance` 變更餘額
    - `isAdmin` 設定為管理員角色(true)

  - Application adds these parameters to code variables or internal objects
    app使用這先參數建立變數或內部物件

  - A consumer may be able to edit object properties or escalate privileges

  - 例如說有一個 createAccount 的 api，偷加入isAdmin這個parameter

    ```json
    {
      "User":"hina",
      "Password": "GreatPasswd123"
    }
    {
      "User":"hedie",
      "Password": "GreatPasswd123",
      "isAdmin": "true"
    }
    ```

- OWASP Preventive Measures

  - Always make sure users should have access to the object's properties
    確保用戶應該有權存取物件設定檔案
  - Avoid using generic methods such as `to_json()` and `to_string()`
  - Avoid functions that automatically bind input into code variables, objects, or properties
  - Only allow changes to object's properties that should be updated by the client
  - Implement response valiation mechanism as an extra layer of security
  - Define and enforce data returned by all API methods
  - Minimize returned data structures, according to the business/functional requirements

---

### API4:2023 
#### Unrestricted Resource Consumption

It's an API issue where the API provider does not have safeguards in place to prevent excessive use of their API. API供應者沒有預防過於頻繁使用此API的控管措施。

The API provider could become a victim of a denial of service, attack or experience unnecessary financial costs. 可能使API供應者成為阻斷服務攻擊的受害者，或增加非必要的財務成本。

- Formerly "Lack of Resources and Rate Limiting" (OWASP 2019)
- Attack Vector Description
  - Exploited via simple API requests
  - Multiple request from single source, or across clouds
    可以從單一一台電腦或雲端運算設備傳送多筆並發(concurrent) 請求
  - Automated tool simulate DoS via high traffic loads
- Security Weakness Description
  - APIs commonly do not limit interactions or resource limits 
    未限制互動次數或資源存取次數
  - Test for resource consumption weakness via:
    - API requests that specify resource quantities
    - Analyzing response status/time/length
  - Also valid for batched operations
  - Evaluate cost impact based on service provider pricing
- Impacts Description
  - Exploitation can lead to Denial of Service
  - Economic impact due to higher infrastructure demands 
    雲端儲存空間須增加、CPU需求增加
- Source of Vulnerability
  - Missing/ inappropriate settings:
    - Execution timeouts
    - Max allocable memory
    - Max number of file descriptors
    - Max number of processes
    - Max upload of file size
    - \# of operations in single api client request
    - \# of records in single request
    - Third party provider's spend limit
- Summary
  - Every API request has technical and financial cost
  - Lack of rate controls increase risk of:
    - Denial of Service
    - Unnecessary financial cost
    - Degradation of service to users
  - 例如`RapidAPI`allows some API providers to also have infrastructure that automatically scales with the number of API requests

- OWASP Preventative Measures
  - Docker offers controls for memory, CPU, number of restarts, file descriptors, processes
  - Limit number of API calls within timeframe 在一段時間內最多可以call幾次api
  - Notify client when limit is exceeded
    超出次數時, 告知client上限為何以及累積次數何時歸零
  - Add server-side validation for inputs (query string and request body)
  - Define, enforce max size for inputs and payload
    - max length for strings, max number of elements in arrays


---

### API5:2023 
#### Broken Function Level Authorization

BFLA - API functions 具有不足的存取權控制。BOLA與存取資料有關，而BFLA就與調整/刪除資料有關。

- BOLA vs BFLA

  - BOLA: UserA 能夠檢視 UserB's 的帳戶資料
  - BFLA: UserA 能夠以 UserB 的帳戶名義執行交易

- Attack Vector Description

  > "Exploitation requires the attacker to send legitimate API calls to an API endpoint that they should not have access to as anonymous users or regular, non-privileged users. Exposed endpoints will be easily exploited."

- Security Weakness Description

  > "Authorization checks for a function or resource are usually managed via configuration or code level. Implementing proper checks can be a confusing task since modern applications can contain many types of roles, groups, and complex user hierarchies (e.g. , sub-users, or users with more than one role).  It's easier to discover these flaws in APIs since APIs are more structured, and accessing different functions is more predictable."

- Impacts Description

  > "Such flaws allow attackers to access unauthorized functionality. Administrative functions are key targets for this type of attack and may lead to data disclosure, data loss, or data corruption. Ultimately, it may lead to service disruption."

- BFLA Vulnerability

  - User of one privilege level can use functionality of another user
  - API providers often have different privilege levels
    - Public users, merchants, partners, vendors, admins, ...
  - Can be exploited for:
    - Unauthorized use of lateral functions
    - Privilege escalation
  - Targeted API functions - endpoints with:
    - Sensitive information
    - Resources that belong to another group
    - Administrative functionality like user account management

- BFLA 示例

  - Different endpoints for privileged actions
    - `/{userid}/account/balance` - user account information
    - `/admin/account/{userid}` - administrator endpoint
  - Admin endpoint can be exploited if API has improper access controls
    - An attacker can perform admin actions, account takeover
  - APIs don't always have admin endpoints
    - <u>HTTP request methods</u> can control access to functionality
    - 例如：GET, POST, PUT, DELETE
  - Attack can exploit unrestricted methods to exploit API functionality

- Testing for BFLA

  - Identify endpoints an attacker could use to their advantage, e.g.:
    - Alter user accounts
    - Delete user resources
    - Gain access to restricted endpoints

- OWASP Preventive Measures

  - Have constant, easy-to-analyze authorization module for all business functions
  - Deny all access by default, require explicit grants to specific roles for access to every function
    預設拒絕所有存取，某些特定角色如果要能存取每一項功能，需要顯性授權(explicit grants) 
  - Review endpoints against function level authorization flaws
    仔細檢查應用程式個端點，確保它們在功能層次授權上沒有漏洞
  - Ensure admin controllers inherit from an administrative abstract controller that implements authorization checks based on the user's group/role
    確保所有管理員相關的 controllers 都有相似的授權機制，藉由繼承抽象controller類別達成
  - Ensure admin functions inside a regular controller implement authorization checks based on the user's group/role
    一般的controllers要根據user屬於哪一群組或具有哪一身分，做額外的授權檢查

  





---

### API6:2023 
#### Unrestricted Access to Sensitive Business Flows

- It represents the risk of an attacker being able to identify and exploit API driven workflows. 代表攻擊者能夠識別並利用API驅動的工作流程的風險

- If vulnerable, an attacker will be able to leverage an organization's API request structure to obstruct other users. 攻擊者會藉由組織的API請求結構，來妨礙其它users，例如：
  - spam other users, deplete the stock of highly sought after items, prevent other users from using expected application functionality 
  
    給其他用戶發垃圾郵件、耗用高需求的資源的庫存、不讓其他users使用app預期要能提供的功能
  
- Attack Vector Description
  
  > "Exploitation usually involves understanding the business model backed by the API, finding sensitive business flows, and automating access to these flows, causing harm to the business."
  
- Security Weakness Description

  > "Lack of a holistic view of the API in order to fully support business requirements tends to contribute to the prevalence of this issue. Attackers manually identify what resources (e.g. endpoints) are involved in the target workflow and how they work together. If mitigation mechanisms are already in place, attackers need to find a way to bypass them."

- Impacts Description

  > "In general technical impact is not expected. Exploitation might hurt the business in different ways, for example: prevent legitimate users from purchasing a product, or lead to inflation in the internal economy of a game."

  例如：在購買流程中，如果web-app不控管購買流程，投機者(scalper)可能會自動化發送請求，把產品的庫存一下子清空。This is where mechanisms like a completely automated public Turing test to tell computers and humans apart, or better knowns as CAPTCHA, comes into play. 在流程中加入captcha等證明不是機器人的互動可以減緩流程

- OWASP Preventative Measures

  > The mitigation planning should be done in two layers:
  >
  > - Business - identify the business flows that might harm the business if they are excessively used
  > - Engineering - choose the right protection mechanisms to mitigate the business risk
  >
  > The following methods are used to slow down automated threats:
  >
  > - Device fingerprinting: deny service to unexpected client devices
  > - Human detection: use captcha or more advanced biometric solutions
  > - Non-human patterns: analyze traffic to detect non-human patterns
  > - Consider blocking IP addresses of Tor exit nodes, well-known proxies
  >
  > Secure and limit access to APIs consumed directly by machines

  


---


### API7:2023 
#### Server Side Request Forgery

user能夠控制應用程式取回的遠端資源，攻擊者可以用URL的方式提供他們的input。提供的URL可能暴露私有的資料、掃描內部網路、或者透過遠端程式碼執行危害目標server。

SSRF也是2021版OWASP的第十名，對api的威脅程度有增加的趨勢。

- Attack Vector Description

  > Exploitation requires the attacker to find an API endpoint that accesses a URI that's provided by the client. In general, basic SSRF (when the response is returned to the attacker), is easier to exploit than Blind SSRF in which the attacker has no feedback on whether or not the attack was successful.

- Security Weakness Description

  > Modern concepts in application development encourage developers to access URIs provided by the client. Lack of or improper validation of such URIs are common issues. Regular API requests and response analysis will be required to detect the issue. When the response is not returned (Blind SSRF) detecting the vulnerability requires more effort and creativity.

- Impacts Description

  > Successful exploitation might lead to internal services enumeration (e.g. port scanning), information disclosure, bypassing firewalls, or other security mechanisms. In some cases, it can lead to DoS or the server being used as a proxy to hide malicious activities.

- SSRF 大概就是

  - App沒有做輸入驗證，就接收了遠端的資源
  - 攻擊者可以控制server請求什麼資源
  - 允許攻擊者存取機敏資料，危害host
  - 攻擊者利用target server處理他們的請求
  - SSRF bug bunty payouts based on level of impact

- 兩種值得一提的SSRF類型

  1. In-Band SSRF
     - Server responds with resources specified by user
     - Attacker發送請求，包含一個URL（更新商店庫存），Server將密碼傳回給攻擊者
  2. Out-of-Band SSRF "Blind"
     - Server does not deliver information from specified URL

- OWASP 預防性措施

  - 將資源取得機制隔離在內部網路
  - 設定允許清單 (1) remote origins, (2) URL schemes, ports, (3) accepted media types
  - 停用 HTTP redirections
  - 使用URL解析器
  - 驗證並清理過所有client端提供的input data
  - 不要傳回 raw responses 給 client

---

### API8:2023 
#### Security Misconfiguration

與 host APIs 的系統相關的諸多弱點都算在這類裡面。當API security配置錯誤，可能會損害機密性、完整性以及API提供者的可取得性(CIA)

因爲配置錯誤相關的缺點繁多，這項弱點可能產生的影響小則資訊揭露，大到數據外洩。

- Attack Vector Description

  > Attackers will often attempt to find unpatched flaws, common endpoints, or unprotected files and directories to gain unauthorized access or knowledge of system.

- Security Weakness Description

  > Security misconfiguration can happen at any level of the API stack, from the network level to the application level. Automated tools are available to detect and exploit misconfigurations such as unecessary services or legacy options.

- Impacts Description

  > Security misconfigurations can not only expose sensitive user data, but also system details that can lead to full server compromise.

- Misconfiguration Examples

  - Misconfigured headers 標頭配置錯誤
  - Misconfigured transit encryption 傳輸中加密配置錯誤
  - Use of default accounts 使用預設帳戶
  - Acceptance of unnecessary HTTP methods
  - Lack of input sanitization 缺少輸入消毒
  - Verbose error messaging 冗長的錯誤訊息
  
- API Header Misconfiguration

  - API providers use headers for handling response and security requirements
  - Misconfigured headers can result in sensitive information disclosure, downgrade attacks, and cross-site scripting attacks
  - Many APIs use additional services to enhance metrics and security

- X-Powered-By Header

  - Reveals backend technology 揭示網站所使用的後端技術

  - Often advertise the exact supporting service and its version 廣宣服務版本號碼

  - Useful to find exploits published for that version of software 尋找該版本軟體的漏洞

- X-XSS-Protection Header

  - Prevents Cross-Site Scripting (XSS) attacks 預防跨站腳本攻擊
  
  - Common type of injection vulnerability 注入弱點的常見類型
  
  - Insert scripts to trick end-users to click malicious links 騙end-user點擊惡意網址
  
  - Value of 0 indicates no protections, 1 indicates protection enabled
  
  - Header clearly reveals whether a security control is in place 這個標頭清楚揭露了資訊管控是否到位
  
- X-Response-Time Header

  - X-Response-Time header is middleware that provides usage metric

  - If API isn't configured properly this header reveal existing resources

  - X-Response-Time may differ for existing vs non-existing records 

    有效帳號的回應時間很長，無效帳號的回應時間很短，可以依照此邏輯暴力破解出有效帳號

  - Example:

    - `/user/account/thisdefinitelydoesnotexist`
      - response time 25.5 ms

    - `/user/account/1021` (valid account)
      - response time 510 ms

- Transport Layer Security

  - APIs with sensitive info should used Transport Layer Security
  - TLS is fundamental way to protect data communication
  - Misconfigured encryption can pass sensitive API info in cleartest
  - Attacker could capture the responses and requests MITM attack

- Default Accounts and Credentials

  - Attackers will attempt default credentials
  - Could allow:
    - Access to sensitive information
    - Access to adminitrative functionality

  - Potentially lead to compromise of supporting systems

- HTTP Methods

  - Unnecessary HTTP methods can increase risk 
  - Application may not handle these methods properly
  - Potential disclosure of sensitive information

- Testing for Misconfigurations

  - Some Security Misconfigurations can be detected by web app scanners
  - Automated scanners will automatically check responses to determine:
    - Version information
    - Headers
    - Cookies
    - Transit encryption configuration
    - Parameters

  - Security misconfigurations can also be check manually

- OWASP Preventative Measures

  - Repeatable hardening process for a properly locked down environment
  - Review and update configurations across the entire API stack, including:
    - Orchestration files
    - API components
    - Cloud services (e.g. S3 bucket permissions)

  - Automated, continuous assessment of configuration/setting effectiveness
  - Ensure all API communication between client and API server is encrypted
  - Specify with HTTP verbs each API accepts; disable all others
  - APIs accessible from browser-based client should:
    - Implement Cross-Origin Resource Sharing (CORS) policy
    - Include applicable Security Headers

  - Restrict incoming content types/data formats to those that meet requirements
  - Ensure all servers in the HTTP chain process incoming requests in uniform manner
  - Define, enforce all API responses to prevent exception traces and other valuable info


---


### API9:2023 
#### Improper Inventory Management

暴露非生產環境且未支援的API版本，通常是因爲未支援的API version受到的保護安全與生產環境的API不一致。

- Attack Vector Description

  > Threat agents usually get unauthorized access through old API versions or endpoints left running unpatched and using weaker security requirements. Alternatively, they may get access to sensitive data through a 3rd party with whom there's no reason to shart data with.

- Security Weakness Description

  > Outdated documentation makes it more difficult to find and/or fix vulnerabilities. Lack of assets inventory and retirement strategies leads to running unpatched systems, resulting in leakage of sensitive data. It's common to find unnecessarily exposed API hosts because of modern concepts like microservices, which make application easy to deploy and independent (e.g. cloud computing, K8S). Simple Google Dorking, DNS enumerations, or using specialized search engines for various types of servers (webcams, routers, servers, etc.) connected to the internet will be enough to discover targets.

- Impacts Description

  > Attackers can gain access to sensitive data, or even take over the server. Sometimes different API versions/deployments are connected to the same database with real data. Threat agents may exploit deprecated endpoints available in old API versions to get access to administrative functions or exploit known vulnerabilities.

- Summary

  - Improper Inventory Management exposes unsupported API 暴露不支援的API

  - Old API versions are more likely to contain vulnerabilities

     舊api比較可能會包含弱點(沒有再升級/補強)

  - Can lead to other vulnerabilities:

    - excessive data exposure, information disclosure, mass assignment, improper rate-limiting, API injection

      表示API08只是駭客要做其它APISecurity risk之前的首個步驟

- Detecting Improper Inventory

  - Test with outdated API documentation, changelogs, and version history on repositories 
  - Version information in endpoints: `/v1/`, `/v2/`, `/v3/`
  - API in development paths: `/alpha/`, `/beta/`, `/test/`, `/uat/`, `/demo/`
  - Techniques: guessing, fuzzing, or brute force requests
  - Testing focuses on unsupported and non-production API versions
  - Typical new version paths:
    - `api.target.com/v3`
    - `/api/v2/accounts`
    - `/api/v3/accounts`
    - `/v2/accounts`

- Discovering Improper Inventory

  - API versioning could also be maintained as a header:

    - Accept: version=2.0
    - Accept api-version=3

  - Versioning can be set within query parameter or request body:

    - /api/accounts?ver=2

    - **POST** /api/accounts

      ```json
      {
        "ver": 1.0,
        "user": "happyhimmel"
      }
      ```

  - Non-production API versions examples:

    - `api.test.target.com`
    - `api.uat.target.com`
    - `beta.api.com`
    - `/api/private`
    - `/api/partner`
    - `/api/test`

- OWASP Preventative Measures

  - Inventory, document all API hosts and the API versions
    - production, staging, test, development
  - Inventory, document integrated services - role, data flow, sensitivity
  - Document API details: authentication, errors, redirects, rate limiting, CORS, endpoints (parameters, requests, and responses)
  - Auto-generate documentation, and include in CI/CD
  - Make API documentation available authorized consumers
  - Use external API protection firewalls for all versions, not just current
  - Avoid using production data with non-production API deployments
  - Perform risk analysis when publishing newer versions of APIs

---


### API10:2023 
#### Unsafe Consumptions of APIs

是表列十項中，唯一一點that focuses less on the risks of being an API provider and more on the risks of being API consumer，當應用程式接收第三方api提供的資料時，應該用與 user input相同的處理方式，即零信任原則(little-to-no-trust)

- Attack Vector Description

  > Exploiting this issue requires attackers to identify and potentially compromise other APIs/services the target API integrated with.
  >
  > Usually, this information is not publicly available or the integrated API/service is not easily exploitable.

- Security Weakness Description

  > Developers tend to trust and not verify the endpoints that interact with external or third-party APIs, relying on weaker security requirements such as those regarding  transport security, authentication/authorization, and input validation and sanitization. Attackers need to identify services the target API integrates with (data sources) and, eventually, compromise them.

- Summary

  - Most of API Top 10 focuses on API provider
  - APIs often path of least resistance
  - If attacker compromises 3rd API, then other businesses at risk
  - If API uses unencrypted connection then data exposed in clear text
  - 3rd party APIs may have less stringent security standards
  - Could also be vulnerable to injection, authorization, other attacks

- OWASP Preventative Measures

  - Assess API security of service providers

  - Ensure all API is encrypted

  - Always validate, sanitize data received 3rd party APIs

  - Do not blindly follow redirects


---

## Beyond the Top 10
### Injections

當攻擊者可以發送通常由『支援web-application的系統』所執行的指令

最常見的注入攻擊是SQL injection, cros site scripting 以及 operating system command injection

APIs are yet another attack vector for these critical attacks to be communicated from an attacker to the supporting system, supporting databases and systems.

- Attack Vector Description

  > Attackers take advantage of lack of logging and monitoring to abuse systems without being noticed.

- Security Weakness Description

  > Without logging and monitoring, or with insufficient logging and monitoring, it is almost impossible to track suspicious activities and respond to them in a timely fashion.

- Impacts Description (per OWASP 2019)

  > Without visibility over ongoing malicious activities, attackers have plenty of time to fully compromise systems.

- Summary

  - Logs reveal patterns in API usage, can also identify abuse
  - Logs provide audit trail of activities
  - Often required for compliance purposes
  - Ensure logs cannot be altered by attackers
  - Monitoring helps detect suspicious activities, anomalous behavior

- OWASP Preventative Measures

  - Log failed authentication, denied access, input validation errors
  - Write logs for a log management solution, include detail to identify malicious actor
  - Logs should be handled as sensitive data
  - Continuously monitor infrastructure, network, API functioning
  - Use a Security Information and Event Management (SIEM)
  - Configure custom alerts to detect suspicious activities earlier


- 

Business Logic Flaws







##### 延伸與參考資料

- API Bug Bounty Research

  - [PentesterLand Writeups Compilation](https://pentester.land/writeups)

  - [HackerOne Hacktivity](https://hackerone.com/hacktivity)

  - [Awesome Bugbounty Writeups Repo](https://github.com/devanshbatham/Awesome-Bugbounty-Writeups)

