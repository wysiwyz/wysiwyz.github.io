---
title: "Operating_system"
date: 1993-01-26T14:30:01+08:00
author: "celine"
tags: ["OperatingSystem"]
categories: ["StudyNote"]
---

# 01. Multiprogramming System

(一）定義：系統允許多個 jobs (processes) 同時執行，即是

主要目的：提高CPU utilization

作法：透過Job scheduling (or CPU scheduling 工作的排班或者CPU的排班) 技術達成

e.g. 當執行中的process在waiting for I/O completed，則 OS 可以將 CPU 切換給另外一個 process 執行，避免 CPU idle

即只要系統內有夠多的 Jobs 存在，則 CPU idle 機會下降



(二）Multiprogramming Degree

相當於<u>系統內之 processes 數目</u>

一般而言，如果 Degree 越高，則 cpu utilization 越高

Note: Chapter 8 virtual memory Thrashing 例外狀況除外



(三）多個 processes 同時執行之方式有兩個：

1. Concurrent execution 並行

   一顆CPU，大家輪流交錯使用，晚上7-8點給P1，做不下去切給其他的 

2. Parallel execution 平行

    多顆CPU或者多核 (core) ，每一個工作都在不同的cpu平行執行

![image-20240126144825311](https://i.imgur.com/JTe4SFh.png)

> Multiprogramming 跟一顆cpu或者多顆cpu無關，單一 user 也可以 multiprogramming，不限定多個 users。
>
> 允許多個工作同時進行就稱為 multi-programming。

# 02. Time-Sharing System 分時系統

> Time-Sharing騙局 - 讓每個user都以為有一台 dedicated to myself 的 computer

(一）Def: 恐龍本又稱為Multitasking 

“It's a logical extension of multiprogramming system.”

與Multiprogramming的最大差異：**CPU的切換頻率極高** (CPU switching highly frequently)

Time-sharing system features 還有：

1. 強調對user的response time要短（e.g. < 一秒）

2. 適用於 “user interactive” computing/environment (一來一往的使用者互動)

3. CPU scheduling（CPU排班）採用 RR 排班法則 (ch4)

4. Memory有限，如何讓user覺得電腦都是他們專屬使用呢？

   使用virtual memory技術，擴展 “logical memory space”

5. 使用“spooling”技術實現I/O Devices的共用，



**Mainframe**（主機, 例如IBM的vex360、vex370）

銀行的外匯交易、貨幣準備金、遠程外匯交割都在這上面實施，因為造價很貴，所以常會從主機拉終端機 (terminal) 出來，通常是terminal+keyboard很多台

![image-20240126150020729](https://i.imgur.com/DdzGIP1.png)



掃毒軟體、下載都不是屬於 user interactive 的情況

因為交易要經過一連串的互動才會完成，所以要求 response time 短

RR排班可以讓 cpu 使用對不同的 user 保障公平，也造就切換頻率高

不管你程式寫多大，我都讓你執行，邏輯的記憶體無窮寬廣



**Physical Memory** (e.g. RAM) v.s. **Logical Memory**

\* Logical 通常是指使用者這一端

  user看記憶體 - logical memory 

  user看檔案目錄 - logical directory 邏輯的目錄

  作業系統在管理檔案配置的目錄，那個就叫做實體目錄 ... (memory disk詳談)



Spooling - 把磁碟當成極大的緩衝區，類似於現在的 buffer 技術（工作原理大同小異）

Spooling示例：

1. 1. user下達列印xxx檔案的命令

1. 2. 把這個檔案送到磁碟disk

1. 3. 系統告訴你列印完成（事實上還沒開始印，系統不會通知你在queue的第幾順位）

1. 4. 接下來printer才一個一個開始印



在這種環境裡頭，user最care的就是回應時間 (response time)

假使一個中繼結果不需要user的反饋，最好也是要回覆給user

非本科系學生等待時間平均七秒 ... enter/esc/ctrl+c/ctrl+break …

而本科系比較長，會給他解釋的時間 ... 網路擁塞、主機今天交易多 ...

> 銀行系統如果正常要4秒鐘回來的話
>
> 超過一般的標準時間之後就要抽一張詞
>
> 『系統正在處理中，今日業務稍多，請不要中斷 ...』
>
> 『今日系統交易異常繁忙，您可以考慮取消或者再等待一下，即將有結果 ...』
>
> 『今日電信網路故障 ...』

# 03. Multiprocessors system

(一）Def: 又叫 Multiprocessing system or Parallel system or Tightly-Coupled system (緊密耦合的系統)

主要特徵 (features) 如下：

(1) 一個機器 (or Motherboard) 內有多顆 processors (or CPUs)

(2) 這些 CPUs 彼此共享此機器的 Memory、Bus、I/O-Devices、power-supplier, etc.

(3) 通常受同一個 clock 之時脈控制

(4) 由 同一個 OS 管理

(5) processors 之間的溝通大都採 “shared memory” 方式 (ch6詳談)

【圖示】

![image-20240126150547935](https://i.imgur.com/TYVtMJa.png)

(二）Benefits（好處）:

(1) Increased Throughput 產能增加

(2) Increased Reliability 可靠度提升

(3) Economy of Scale 運算能力擴充時符合經濟效益

**產能增加**，可以支持多個工作在不同CPUs上平行執行 (parallel computing)

Note: 雖然可以增加產能，但N顆CPUs的產能絕對小於原來一顆CPU產能*N倍

​    原因有二：<1> Resource competition資源競爭，兩顆CPU等一個bus

​            <2> Processors之間的Communication會抵銷產能

**可靠度提升**，萬一若某個CPU壞了，則system不會因而停頓，因為還有其它CPUs仍可以運行

Note: 名詞解釋 <1> Graceful degradation 漸進式滅亡

​              系統不會因為某些HW/SW元件故障而停頓，仍然保有持續運作之能力，

​              此性質稱為 graceful degradation，又稱為 Fail-Soft 軟失敗

​           <2> Fault-Tolerant system 容錯系統

​              具有graceful degradation性質的系統，稱為fault-tolerant system

達成容錯技術要符合兩條件: Backup system獨立系統>=2｜Switching smoothly無縫平穩切換

**運算能力之擴充，符合經濟效益**

比較基準：N顆CPUs在一部機器內，與N部機器相比，成本較為便宜。因爲這些CPU共享同一個機器裡頭的 Memory, Bus, I/O-Devices, etc.



(三）Multiprocessors system 可以再細分為兩種 subtypes

(1) SMP (Symmetric MultiProcessors) 對稱

- 定義：每一個processor之工作能力是相同的（identical）

  每一個CPU皆有對等的權利來存取資源

- 優點：1️⃣ 可靠度較ASMP高      2️⃣ 效能較高

- 缺點：SMP的<u>OS設計開發較為複雜</u>，例如互斥存取之機制的設計

(2) ASMP (Asymmetric MultiProcessors) 非對稱

- 定義：每個processor之工作能力不盡相同，通常採取“Master-Slave”架構 (恐龍本: Boss-Employee)，Master processor 負責工作分派以及資源分配，監督 slaves 等管理性工作；其它 slave processor 負責執行工作

- 優點：ASMP的OS設計開發較為簡單，因為他跟simple-CPU的OS版較為類似，開發容易，成本較低 

- 缺點：1️⃣ 相較於SMP的可靠度較低

  ​              2️⃣ 效能較低（因為Master CPU是瓶頸）

  多個slaves與master各有大量訊息交換，master做決策，行政效率較差，如果是為了搶市場，通常會先推 ASMP

(四）Multiprocessors system v.s. MultiCores CPU (多核）

- 主要是硬體的差異；就OS而言，沒有差異

- 將一個Core視為logical CPU來看待

- 例：主機板裝了四顆雙核的CPUs，對OS而言，即是有 4*2 = **8** 顆CPUs可用

- 優點

  ‧ Power saving

  ‧ 速度較快

   (同一個晶片內資料的傳輸速度)

  ![image-20240126151044539](https://i.imgur.com/EPOxKdr.png)

# Distributed system 分散式系統

(一）

- 定義：又叫 <u>Loosely-Coupled</u> system 鬆散耦合

- 主要特徵如下：

  1. 多部機器彼此透過 network (LAN, WAN, etc.)（or Bus）相互串連

  2. 每部機器之CPU有各自私有的Memory, Bus, I/O-Devices, etc. 並非共享

  3. 各個CPU之clock時脈控制不一定相同

  4. 各個CPU上的OS也不見得相同 (你Linux, 我Mac, 它Windows)

  5. 各個processors之間的溝通大都採用“<u>Message Passing</u>”方式

     Step1: 建立通訊鏈結 communication link

     Step2: Message 訊息之間相互傳輸

     Step3: 釋放 link

     ![image-20240126151528731](https://i.imgur.com/kMYropH.png)

(二）構建分散式系統之理由（好處）

1. Increased Throughput - 因為它支持平行運算 parallel computing

2. Increased Reliability

3. <span style="color: yellow;">Resource Sharing</span> - 因為資源共享，導致成本降低

   - 說明：支持 “client-server” computing model 之實施
   - Server: 提供某些服務的機器
   - e.g. MailServer, FileServer, Domain name server (DNS), 
   - PrinterServer (早期), Computing server (提供高速運算能力的伺服器）
   - Client: 本身不提供服務，且它需要某些服務時，則發請求給 server
   - Server服務完，再將結果傳回client 
   - Note: “peer-to-peer” model  peer - 同時具有 server 以及 client 的角色

4. <span style="color: yellow;">Remote Sites Communication</span> 需求被滿足

   e.g. email, FTP via internet

> #3 跟 #4 是 distributed system 有別於 multiprocessor system 擁有的好處

# Real-Time System 即時系統

(一）分為兩種：

1. Hard real time system
2. Soft real-time system

(二）Hard real-time system

- 定義：This system must ensure the critical tasks complete on time.  即工作必須在規定的時間限制內完成，否則即算失敗

- 舉例：軍事防衛系統、核能安控系統、工廠自動化生產、機器人控制，等等。

  military defense, nuclear safety control, manufacturing automation, robot control

【圖示】感測器：壓力、溫度、一氧化碳偵測、雷達、聲納

![image-20240126152400575](https://i.imgur.com/x902W2L.png)

#### 設計考量：

1. 所有時間延遲之因素皆須納入考量，確保這些時間加總能滿足 (<=) 時間deadline要求

   例如：sensor data transmission 傳輸速度、運算速度、signal的傳輸

2. 所有會造成處理時間過久或者無法預測之設備或機制，宜少採用或不用

   例如：磁碟disk宜少用或不用（運作速度比較久）、 絕對不採用virtual memory

3. 就 cpu scheduling 設計(ch4)而言，須先考量 schedulable 與否，再進行排程

   例如：rate-monotonic, EDF scheduling (cpu排班設計在詳談）

4. Time sharing system 無法與之並存

   肯定是走priority機制，不會RR scheduling

5. OS 所造成的任何 dispatch latency etc. 宜降低

   Note：一般實務上，hard real-time system 鮮少有OS (OS幾乎不存在），尤其是 embedded real-time system（嵌入式的體積一定要小）

6. 現行商用 OS 不支援 Hard real-time features，通常是客製化設計

   （商用OS：Linux, Unix, Windows, AppleOS, Solaris, etc.)

（三）Soft real-time system

- 定義：This system must ensure **the real-time process get the highest priority** than the others, and **retain this priority level** until it completed. 

  要取得最高優先權，並且維持住不可衰退

- 舉例：Multimedia system, simulation system, VR system, etc.  

  工作有先後執行卡關的關係，例如先播音訊再放影片，科學模擬遵照步驟，等等

- 系統設計考量：

  1. 就cpu scheduling設計 (ch4) 角度而言：

     1. 必須支持 preemptive priority scheduling 可插隊優先權排班法則
     2. 不可以提供 “Aging” 技術（aging: 防止低優先權的 starvation, 逐步調高優先權）

  2. 盡量降低 kernel 的分配延遲時間 Dispatch latency time (ch4)

  3. 可支援 virtual memory 並存，但是要求 real-time process 的全部頁面 (pages) 必須皆待在 memory 中，不可以被當作犧牲頁面 swapped out，直到完工

  4. 與 Time-sharing system 可以並存，例如：Solaris作業系統

  5. 一般商用系統 OS 皆支持 soft real-time system 的特徵

     （改變一下OS架構參數，就可以達成以上要求）

# 04.Batch System 批次系統

> Operating System 的前身為 resident monitor

- 定義：將一些<u>較不緊急</u>、<u>定期性</u>、<u>非交談互動性</u>之 jobs，累績成堆，再分批送入系統處理

- 舉例：庫存盤點、報稅、掃毒工作、磁碟重組、下載音樂、銀行清算系統等等

- 主要目的：
  - 提高 resource utilization，尤其是冷門時段/非尖峰時期的資源利用率
  - 不適合用在 real-time system，或者user-interactive application (e.g. game)



#### HandHeld system 手持式系統

(一）舉例：PDA個人數位化助理、Smartphone智慧型手機、Pad平板

(二）Hardware天生之限制，帶來Software必須配合之處

![image-20240126153137477](https://i.imgur.com/Z9dD5mA.png)

# 05. I/O運作與Hardware(Hw) resources Protection

![image-20240126153446919](https://i.imgur.com/JXjD5sC.png)

### I/O 運作方式

#### Polling I/O (詢問式 I/O)

- 定義：又稱為 Busy-waiting I/O (忙碌等待I/O) 或者 programmed I/O (可程式化I/O)

- 如下步驟：

  1. user process 發出 I/O-request

  2. OS 收到請求後，(Blocking I/O或者Non-Blocking I/O可能）會暫停此 process 執行，並執行對應的 system calls

  3. Kernel的I/O-subsystem會Pass此請求給<span style="color:yellow;">Device driver (設備的驅動程式, Software (Sw))</span>

  4. Device Driver依照此請求，設定對應的 I/O Commands 參數給 <span style="color:yellow;">Device Controller (設備控制器(Hw))</span>

  5. Device Controller 啟動，監督 I/O-Device 之 I/O 運作進行

  6. 在此之時，OS（可能）將 CPU 切給另外一個 process 執行

  7. 然而，cpu在執行process工作過程中，卻要不斷去polling Device Controller，以確定I/O運作是否完成或有I/O-error

     > 沒人告訴CPU其I/O做完了沒，設備控制器不會主動告知

- 缺點：

  CPU 耗費大量時間 (80~90%) 用於 polling I/O Device Controller 上，並未全用於 process execution (10~20%)上，故 CPU utilization 低 (真正用在process execution的比例) ，throughput 不高

【圖示】

![image-20240126155133275](https://i.imgur.com/RlOfzex.png)

#### Interrupted I/O (中斷式 I/O)

運用中斷的技巧來處理I/O設備與CPU之間溝通的方式

- 定義：步驟如下
  - Step 1 ~ Step 6 同前面敘述
  - Step 7：當I/O運作完成，設備控制器會發出 “I/O-Completed” Interrupt 通知 OS (CPU)
  - Step 8：OS收到中斷之後，（可能）會先暫停目前Process的執行，如果中斷的優先權很低，或有更緊急的事要做，就可能先不處理這個中斷
  - Step 9：OS必須查詢 “Interrupt Vector”(中斷向量表)，確認何種中斷發生，同時也要找到該中斷的 ISR (Interrupt Service Routine，服務處理程式) 的位址
  - Step10：Jump to ISR 位址，執行 ISR 
  - Step11：ISR 完成後，控制權返回 kernel (return control to kernel)，kernel 也許作一些通知工作
  - Step12：恢復（resume）原先中斷之前的工作執行，或交由 cpu scheduler 決定

- 優點：

  CPU不須耗費時間用於polling I/O-Device，而是可以用於process execution上。因此，CPU utilization有所提升，且提高了Throughput，改善了system performance

- 缺點：

  1. 中斷的處理仍然需要耗費一些CPU的時間 (overhead)，所以若I/O運作時間 (開始-結束) 小於interrupt處理時間，則使用Interrupted I/O就不划算， 因此，polling I/O仍然有其必要性

     > 假設家裡是強力瓦斯爐，開水要燒滾只要一分鐘，從沙發回想笛音是什麼意思（查表, Interrupt處理）也要一分鐘 ... 這樣用中斷就不划算，在旁邊等 (busy-waiting) 更佳

  2. 若中斷發生的頻率太高，則大量的中斷處理，會佔用幾乎全部的cpu time，系統效能會很差 

     > 例如找到一個完美的笛音壺但水量只有5ml，泡一杯咖啡要250ml（為了接下來的DMA鋪路）
  
  3. CPU 仍須耗費一些時間，用於監督/負責 I/O-Device 與記憶體之間的資料傳輸過程設備 (DiskRead/DiskWrite Buffer register）的資料傳輸到Memory是由 ISR 軟體執行 (Step10)，仍要消耗CPU時間
  
     > 該找瑪莉亞幫你泡咖啡了！
     >
     > 該存錢買咖啡機了！－**DMA介入**

​                                         

#### DMA (Direct Memory Access I/O)

(一）定義：DMA controller負責I/O-Device與Memory之間的Data transfer資料傳輸工作，過程中無須CPU之參與監督，cpu有更多時間用於processor execution上

- 優點：

  1. CPU utilization更高

  2. 適合用在Block-Transfer oriented I/O-Device (以區塊傳輸為導向的I/O設備) 上

     代表中斷發生的頻率不至於過高

     例：磁碟的控制器會與DMA控制器合作，這過程不需要cpu的監督

     例：**不**適合用在Byte-Transfer oriented I/O-Device

- 缺點：

  1. 引進DMA controller會增加硬體設計的複雜度

     Complicated the hardware design

     原因：DMA Controller會與CPU競爭 (爭奪) Memory 以及 Bus 的使用權

     當 DMA controller 佔用了 memory 或 bus 時，CPU 要被迫等待

##### [補充]

1. DMA Controller通常採用 “Cycle Stealing” 技術（或Interleaving (per the恐龍本)) 與CPU輪番交錯使用 memory 以及 bus。如果CPU與DMA controller發生conflict (衝突-同時要用memory以及bus)，則會給 **<u>DMA</u>** 比較高的優先權。                 

   > 瑪莉亞不僅會泡咖啡，還會燒菜！

2. 通常系統會給予 “對該資源需求量、頻率等較**小**” 的對象有較高的優先權

   會獲得1️⃣平均等待時間較小 2️⃣平均產能比較高 的好處

   > 最短工作優先

# 06. 機器指令 Stages

3. 機器指令 Stages FO：Fetch Operand

   - 有記憶體變數，立即/直接/相對定址

   | IF                | DE        | FO            | EX        | WM                     |
   | ----------------- | --------- | ------------- | --------- | ---------------------- |
   | instruction Fetch | Decode    | Fetch Operand | Execution | Write result to Memory |
   | 抓指令            | 解OP code | 抓運算元      | 執行      | 寫入資料進記憶體變數   |

   |      | CPU會Memory Access                     | DMA要用Memory                                            |
   | ---- | -------------------------------------- | -------------------------------------------------------- |
   | IF   | 會（必要）                             | 衝突 (Conflict, 爭奪memory)                              |
   | DE   | 不會                                   | OK                                                       |
   | FO   | 可能會                                 | OK或者有衝突（如果有衝突的時候，會給予DMA比較高的優先權) |
   | EX   | 不會                                   | OK                                                       |
   | WM   | 可能會（有可能會用memory, 也可能不用） | OK或者有衝突                                             |

#### DMA之six steps圖示

1. <u>Device driver</u> is told to transfer disk data to <u>buffer</u> address X

   設備驅動程式被告知要傳輸資料到記憶體buffer，Buffer之起始位置為 X

2. <u>Device driver</u> tells <u>disk controller</u> to transfer C bytes from disk to Buffer at address X 

   設備驅動程式告知磁碟控制器, 從磁碟裡傳輸C bytes的量進buffer add.3

3. <u>Disk controller</u> initiates DMA transfer 
4. <u>Disk controller</u> sends each byte to DMA controller
5. DMA transfer bytes to Buffer X increasing memory address (X+1) and decreasing C until C=∅ (C byte的量, counter C-1)

6. When C = ∅, DMA interrupts CPU to signal transfer completion

   ![image-20240126160627526](https://i.imgur.com/WVnkbbc.png)

#### Life cycle of I/O-request via Interrupted I/O

(經由中斷式I/OS) 之I/O請求的生命週期

<span style="color: lime;">🚧 這段不太懂 35:34 - 50:21 (06)</span>



# 07. Blocking and Non-Blocking I/O

1. Blocking-I/O: Process suspended until I/O completed 

   - 發出請求之後暫停，直到I/o完成後才繼續process

   - 優：Easy to use and understand

   - 缺：Insufficient for some needs

2. Non-Blocking I/O:

   播放電影的時候，總共有1GB，先抓取2mb的資料 [ 20:51 的圖有改版🚧 ]

   - I/O calls returns as much as available 

     e.g. user interface data copy (buffered I/O)

   - Implemented via multi-threading

   - Returns quickly with count of bytes read or written

3. Asynchronous I/O:

   Process runs while I/O executes

   - Difficult to use

   - I/O subsystem signals process when I/O completed

     發送I/O request之後，控制權馬上還給user process

     發送在執行，I/O設備也在執行

【圖示】

![image-20240126161629760](https://i.imgur.com/pgPdJ4W.png)

##### 小差異：

- Non-blocking I/O:

  I/O 完成資料有多少就丟多少回來(即少量 returns as much as possible)

  要求讀100bytes，每25bytes, kernel 就會通知process 

- Asynchronous I/O: 

  整個I/O完成，才通知process

  讀完整個100bytes完成後才通知process

  

#### Interrupt機制與種類

（一）當Interrupt發生，OS之處理步驟如下：

1. OS收到中斷後，（若此中斷要被立即處理的話）則OS會暫停目前Process之執行，且保存其 Status and Registers Contents

2. OS會依 Interrupt ID.(No.) 去查詢中斷向量表 (interrupt vector) 確認何種中斷發生，並且找出其 ISR 的位址

3. Jump to ISR位址，執行ISR

4. ISR 完成之後，控制權返回 kernel

5. OS 會恢復（resume）中斷之前 process 之執行

   ![image-20240126162224778](https://i.imgur.com/bJv8atO.png)

（二）Interrupt種類 ✨✨

- **[分類一]** 分為三種：

  1. External interrupt 外部中斷:

     CPU以外的周邊設備，控制卡，etc. 所發出來的

     例如：”I/O Completed”, “I/O-error”, “machine-check”

  2. Internal interrupt:

     CPU在執行Process過程中，遭遇重大錯誤而引發

     例如：Divide-by-zero, 執行非法的特權指令, etc.

  3. Software interrupt:

     user process 在執行中，若須要OS提供服務時，必須發出此類中斷，

     目的是通知作業系統，請它執行對應的服務請求

     例如：I/O-request

     Note: 好比是“服務鈕”

- **[分類二]** Interrupt 與 Trap兩種

  ![image-20240126163005722](https://i.imgur.com/SIlx8FA.png)

  - 例：設備發出“I/O-Completed”, “I/O-error”, “machine-check”, etc.  以及 “Time-out” by Timer（硬體計時器）

  - Trap 用途主要有二：

    1. Catch the arithmetic error 程式執行遭遇重大錯誤

       例如：Divide-by-zero、執行非法的特權指令（該指令不該是你執行）、illegal memory access     

    2. user process執行須要OS提供服務時，也會發Trap通知OS

       例如：I/O-request

- **[分類三]** 分為兩類              `中斷之間也有優先權高低`

  1. **maskable interrupt** 可遮罩中斷：

     此類中斷發生可以被忽略 (ignored, 假裝沒聽到) 或延後處理 (queued, 知道了, 打完就去)，不一定要馬上處理

     e.g. Software interrupt (優先權較低)

  2. **non-maskable interrupt** 不可遮罩中斷：

     此類中斷必須立刻處理，通常比較緊急

     e.g. internal interrupt（程式執行遭遇重大錯誤）, I/O-error, etc.

     

#### Hardware Resources Protection 硬體資源保護

〖基礎建設〗

- 必須要先有這兩種東西：
  1. Dual-modes operation 雙重模式運作
  2. Privileged instructions



#### Dual-modes operation

（一）定義：system 之運作模式至少要可以被區分為兩種模式

1. kernel mode:

   - 又叫作 system mode, supervisor mode, 也有人稱之為privileged mode

     （早期有，現移除的 monitor mode）

   - 代表此刻是 kernel 取得系統的控制權（i.e. 取得CPU執行)

   - 允許特權的指令 (privileged instructions) 在此模式下執行

2. user mode:
   - 代表user process取得CPU執行，此模式下**不允許**執行特權指令，若執行產生Trap

> 此外，dual-modes 必須要有硬體的支持才可實現
>
> 例如：CPU內會有Mode Bit，用以區分現在是哪個模式



##### Privileged instruction（特權指令）

- 定義：

  任何可能會造成系統重大危害的指令，可以設為特權指令

  特權指令只可以在 kernel mode 執行，不可以在 user mode 執行，

  一旦在 user mode 下執行，會發生 Trap 通知 OS，將此 user process terminates

- 例如

  - Turn-off（disable) interrupt 把中斷的機制關掉

  - Clear memory

  - I/O-instructions (for I/O protection)

  - Timer 值 Set / change (for CPU protection)
  - Base/limit register 修改/set (for memory protection)
  - Change mode from user mode to kernel mode

##### Quests

- 下列哪些是特權指令？

  (1) Set value of Timer 

  (2) Read the clock

  (3) Clear Memory

  (4) Turn off interrupt

  (5) Switch from user to monitor mode

  > Ans: (1)(3)(4)(5)

- 保護作業系統是至關重要的事情，因為要做到這樣的保護，才會有Dual-Modes

  然而為了讓user flexibility最大化，對user process的限制也要越少越好，

  以下七條通常可以被列入特權指令，但有些好像不太需要，哪些必須？

  (1) Change to user mode

  (2) Change to monitor mode

  (3) Read form monitor memory

  (4) Write into monitor memory

  (5) Fetch an instruction from monitor memory

  (6) Turn on timer interrupt

  (7) Turn off timer interrupt

  Ans: to be discussed

為何構成protection之基礎 ...

1. [User Mode] user process 透過 Trap 發出請求
2. [Mode change] to kernel mode
3. [Kernel mode] kernel 執行 I/O 指令 / 特權指令
4.  I/O 指令 / 特權指令將結果傳回 kernel
5. [Mode change] to user mode
6. 將結果傳回 user process 



#### Hardware Resources Protection

1. I/O protection
2. Memory protection
3. CPU protection



##### I/O Protection

- 目的：由於I/O運作較為繁瑣複雜，為了**降低 user processes 操控 I/O 之複雜度**，及避免user process 對 I/O Devices 之不當操作，所以才有 I/O protection

- 作法：將所有 I/O 指令設為特權指令 (privileged instruction)，配合 Dual Modes，**一律讓 user process 委託 kernel 執行 I/O 運作**

##### Memory Protection

- 目的：防止 user process 存取其它 user processes 之 memory area 及 kernel memory area

- 作法：（以Contiguous Memory Allocation為例）

  針對每一個 process，kernel 會提供一套 Registers，稱為 Base/Limit Register

  其中 Base register: 記錄 Process 之**起始位址**

  ​          Limit register: 紀錄 Process 之**大小** 

  將來，Process執行，會進行下列的checking:

  ![image-20240126165148804](https://i.imgur.com/wvzvDdh.png)

  檢查memory位址是否大於等於 Base(起始位址, e.g. base=1000)，如果小於base則為非法的記憶體存取，接著確認最後一個合法的位址為何 (if Base = 1000, limit = 500) ?

  最後位址 = 1000 + 500 - 1 = 1499 

  若 memory 位址不小於 Base + limit，則也是非法的記憶體存取 

  以上兩項檢查由硬體完成，Hardware = MMU (記憶體管理單元, Memory Management Unit)

  此外，Base與Limit register值之set/change須設為“特權指令”



##### CPU Protection

- 目的：防止 user process 無限期/長期佔用 CPU，而不釋放

- 作法：

  利用 Timer 實施，同時 OS 會規定 process 使用 CPU 時間之最大配額值 (Maximum Time Quantum) 當 process 取得 CPU 之後，Timer 初值即設為 maximum time quantum 值，隨著 process 實行時間增加，Timer 值會逐步遞減，直到 Timer 值為零，Timer 會發出Time-Out” interrupt 通知 OS，OS 便可強迫此 process 放掉 CPU。

  此外，Timer值之set/change（設定/修改）也須設為特權指令。



## Ch3. OS之Development

- OS應提供的服務項目 (3-5, 3-6 what is the OS? What r the services provided by OS?)
- OS之服務元件種類 (3-3)
- System Calls之介紹 🌠
- OS之Structure種類 (🌠作業系統模組之間的架構）
  - Simple                                          (e.g. DOS)
  - More Complex than simple (e.g. UNIX)
  - **Layered** Approach                (e.g. OSI 7層)
  - **Microkernel** 🌠微核心       (e.g. Mach)
  - Module                                         (e.g. 只收需要的)
  - Hybrid。                                      (e.g. 上述無法分類）

- 架構設計原則：policy 與 mechanism 宜分開
  - policy策略 - 要呈現什麼行為 what
  - mechanism機制 - 怎麼做出這個行為 how

- Virtual Machine 介紹：一開始是為了測試開發完的OS

- 

##### 〖軟體系統設計的兩大命題〗

- 高內聚：同樣或類似的功能越聚在一起越好

- 低耦合：模組之間的關聯性要越低越好

> 系統分析
>
> 軟體工程
>
> - Waterfall
>
>   - Analysis 分析/瞭解需求
>
>   - Design 
>
>     - 架構
>
>     - 細部設計 Algo + DS
>    - U.I.     
>   
>  - Coding/Implementation
>   
>  - Testing 
>   
>  - Maintain
>   

# 09. System call

- 定義：

  - 作為執行中user proess與kernel之間的溝通界面，當user process須要OS提供某種服務時，會先trap通知OS，並代入system call ID(No.) 以及所須參數，接著，OS執行對應的system call，完成後，將服務結果return to user process
  - It's a programing interface to the services provided by the O.S.

  - Note: 類比 
    - Trap => 服務鈕（包廂服務鈴)
    - System call => 服務項目（KTV_kernel叫車,點餐,送BDC,作餐等20種服務）

【圖示】

![image-20240126170503851](https://i.imgur.com/GW2DMdB.png)

> User process引發system call(`open()`)，引發sys.call一定伴隨Trap，sys.Call界面查表，執行對應的指令，實作後回傳結果
>
> C library 查 printf 對應的 table，System Call Interface通常是走API規格

- System call 作業系統提供的服務項目種類. (3-8 ~ 3-9)

  1. **Process Control**       

     e.g. 建立, 終止, 暫停, 恢復執行process, set/read attribute

  2. **File Management**   

     e.g. 建立, 讀檔, 寫檔, 開檔, 關檔, 更改檔名, 變更檔案指標, 刪除

  3. **Device Management**    

     設備管理

  4. **Information of Maintenance**  

      e.g. 取得系統日期/時間, 取得process屬性等等

  5. **Communications** 

     程序process之間的通訊, 而且只針對“message passing”方式提供服務

  6. **Protections**

     硬體資源的保護, 檔案存取權利的控制HW resources protection, File access control, etc.

- System call 之參數 (parameters) 傳遞的三種方式

  1. 利用**暫存器** (Registers) 保存參數

     - 優點：(1) 最簡單 (2) 存取速度最快 (without memory access)

     - 缺點：不適用於大量參數之情況

  2. 利用 **Memory**，以 Block (Table) 儲存這些參數，並且將此表格的起始位置於一個 register 中，pass 給 OS

     - 優點：適用於大量參數

     - 缺點：存取速度比較慢 (need memory access)，且操作較為麻煩

  3. 利用 Stack (系統的堆疊）將參數push入此Stack，OS再 pop from Stack，以取得參數

     - 優點：也適用於大量參數之狀況，操作也簡單

     - 缺點：堆疊Stack空間要留大一點，否則可能會overflow，堆疊如果平均少量，用暫存器；如果大量，就用記憶體


#### OS之系統架構 (Structure) 分類

1. **Simple**

   例：MS-DOS (resident system program, device driver, etc. 無dual-mode設計)

2. More complex than simple

   例：UNIX  

   - Limited by Hardware functionality 受限於硬體功能性
   - The original UNIX had limited structuring
   - The UNIX包含兩個separate parts: 
     1. System Programs
     2. The kernel
   - Beyond simple but not fully layered

   ![image-20240126171044726](https://i.imgur.com/Gb4MPfz.png)

3. **Layered approach**

   - 定義：

     1. 採取 Top-Down 方式切割系統功能/元件，以降低複雜度

     2. 元件/模組之間依照呼叫關係分層，即上層可以使用下層功能，但下層不可以使用上層功能（上可以call下，下不能call上）

     3. 測試時使用 Bottom-up 方式來進行測試除錯，比較容易知道bug是落在哪一層

     4. 層次的劃分沒有明確規定，通常第零層是硬體，最外層是user interface

        ![image-20240126171244198](https://i.imgur.com/stqQdbu.png)

        

   - 優點：
     1. 降低設計複雜度 
     2. 有助於分工
     3. 測試、除錯、維護容易

   - 缺點：

     1. 很難做到很精確的層次劃分

     2. 若層次數太多，則系統效能可能會非常的差 

        If too many layer, then system performance would be very poor.

4. **Microkernel** 🌠 微核心

   ![image-20240126171721680](https://i.imgur.com/rVjyr8e.jpg)

   - 由CMU（卡內基-美隆大學）率先提出，代表產品為 Mach O.S.

   - 定義：將kernel中一些Non-essential services（比較不是那麼重要的服務）自kernel中移除（不在kernel mode中運行），改成在user side提供服務，在user mode中，以system programs的方式存在，如此一來，可以得到一個比較小的kernel，稱之為microkernel

   - 一般而言，microkernel提供下列三個minimum services：
     1. Process control
     2.  Memory management（不包含virtual memory）
     3.  Process Communications（提供message passing服務而已）

   - 優點

     1. Easier to extend a microkernel
     2. Easier to port the the O.S. to new architectures (硬體架構)
     3. More reliable 
     4. More secure

     > 說明如下：
     >
     >  1️⃣ 服務的增加跟刪除是容易的，因為這些服務是在user side執行，所以服務的增刪不須要牽扯到 kernel也要更動，即使要，也是小幅度修改
     >
     >  2️⃣ 因為kernel很小，所以作業系統移植到新的硬體平台之更動幅度不大
     >
     >  3️⃣＆4️⃣ 萬一某個服務在執行中掛掉了，充其量只是相當於一個 user process 掛掉而已，對hardware、kernel 以及其它 user processes 沒有不良影響，所以更加安全可靠

   - 缺點

     Performance overhead of user space to kernel space communication

     效能較差，因為充斥著大量user mode與kernel mode之間訊息的傳遞

     > Microkernel的相反詞 **Monolithic kernel**
     >
     > 定義：
     >
     > - 所有的services皆須run in kernel mode
     >
     > - 大部分商用作業系統都是 monolithic kernel
     >
     > - Monolithic kernel之優缺點與微核心相反

5. Module

   ![image-20240126172335461](https://i.imgur.com/ysuqo39.jpg)

   - Many OS implement loadable kernel modules

   - Use object-oriented approach
   - Each core component is separate
   - Each talks to the others over known interface
   - Each is loadable as needed within the kernel
   - 簡記：Similar to layers but with more flexible, 效能也更好
   - Examples: Linux, Solaris, etc.

6. Hybrid 混合型

   - 現代的作業系統很難存粹歸屬於某一型

   - 例: Linux and Solaris 是 Monolithic (所有服務都在 kernel mode 運行）且也是Modular approach for dynamic loading

   - 例: Windows mostly Monolithic, 有時針對不同客戶需求加上microkernel for subsystem

   - 例: Apple MacOS 也是混合的，kernel包含：

     - Mach: microkernel的代表

     - 部分的 BSD UNiX

     - I/O kit

     - Dynamic loadable module (叫作 kernel extension)

       ![image-20240126172537764](https://i.imgur.com/Y1SPbT0.png)

# 10. Virtual Machine

- 定義：利用Software技術，模擬出一份與底層硬體一模一樣的功能界面之抽象化機器 (abstract machine) 稱之

【圖示】

1. **Host**: Underlying hardware system, Host OS, Host hardware

2. **VMM** (Virtual Machine Manager) 或Hypervisor: Creates and manages/runs virtual machines專門負責建立、管理、執行VM的模組

3. **Guest**: Process provided with virtual copy of the host Guest Applications, Guest OS

![image-20240126172800689](/Users/wysiwyz/Library/Application Support/typora-user-images/image-20240126172800689.png)

> [恐] 之其它英文：
>
> - Abstract hardware of a single computer into several different execution environments
>
> - Similar to layered approach, but layer creates virtual machine (VM)
>
> Hardware 主要有CPU, Memory, I/O-Devices
>
> 如何讓VM以為自己也有一顆CPU？Time-sharing 
>
> Memory？Virtual Memory
>
> I/O-Devices？Spooling, buffering

- 優點/好處

  1. 作為測試開發中的OS一個良好的負載平台，具有下列好處
     1. 其它user, user processes工作仍可以持續運作，不須暫停
     2. 萬一測試中的OS不穩定、掛掉或是失敗了，也不會影響Host hardware OS其它user process之工作，因為只是相當於一個user process fails而已，會對系統有重大危害

  2. 同一部Host Hardware可以執行多個OS，running on多個virtual machines可節省成本
  3. Consolidation: 在cloud computing environment中，我們會用有限的機器（硬體），建立為數眾多的virtual machines，我們可以依VM上的applications之執行負擔輕重，調動Host Machines資源作因應的支援
  4. VM較為安全：如果VM被病毒入侵，不至於擴散，因為VM之間是獨立的
  5. 可以Freeze, suspend, running VM 及clone (複製) VM

#### VMM的implementation

1. Type 0 Hypervisor: 

   - Hardware-based solutions via firmware

   - e.g. IBM, LPARS and Oracle, LDOMS, etc

2. Type 1 Hypervisor:

   - OS-like software: e.g. VMWare ESX, Joyent SmartOS, Citrix XenServer

   - General purpose OS that provides VMM functions (services)

     e.g. MicroSoft Window Server with HyperV, RedHat Linux with KVM

3. Type 2 Hypervisor:

   - Applications level provides VMM functionality

   - e.g. Parallel DeskTop (install Windows on Apple), Oracle VirtualBox

![image-20240126173533373](https://i.imgur.com/WMKlkgB.png)



# 11. VMM(Hypervisor) Implementation

| | VMM               | level of implementation |
|-| ----------------- | ----------------------- |
|1.| Type 0 Hypervisor | hardware level          |
|2.| Type 1 Hypervisor | kernel mode             |
|3.| Type 2 Hypervisor | user mode               |

共同特徵：它們建立的VM並不等於底層的硬體

其它變形如下：

4. Paravirtualization: 

   - The guest OS need modify to work in cooperation with VMM to optimize performance
   
   - 🐥It presents guest with similar but not identical to hardware(HW). 
   
     創造出來的硬體，呈現出來給guest與底層硬體並不完全一樣（只截取需要的部分）
   
   - 🐥Guest must be modified to run on paravirtualization HW. 
   
     Guest OS如果要在Paravirtualization運行的話，一定要修改過
   
5. Programming-environment Virtualization

      - VMMs do not virtualize read HW, but instead create an optimized virtual system
      - e.g. Java Virtual Machine (JVM), Microsoft .NET    
6. Emulators 模擬器
   
   - Allow application written from one hardware to run on a very different hardware, such as different type of CPU.
7. Application Containment
   - e.g. Oracle Solaris Zones, BSD Jails, IBM AIX WPARs
   - Solaris Zones早期是列在ParaVirtualization裡面 
   - 創造出來的是**執行環境**，而不是硬體

> JVM is a Specification, not an implementation. 
>
> JVM spec 規範了三點：
>
> 1. Class Loader   下載到client端
> 2. Class Verifier   驗證是否安全
> 3. Java Interpreter 如何在不同硬體架構上開發Java

#### Policy與Mechanism (政策/策略) 與 機制

- Policy：

  1. 定義 "What" to be provided 
  2. 經常改變

- Mechanism：

  1. 定義  "How" to do that
  2. The underlying mechanism 甚少改變或不變

- 設計原則：

  "Policy 與 Mechanism 宜 separate，以增進 system flexibility"

  政策與機制要隔離/獨立，這樣系統的維護/擴充/加強/管理會比較容易

- 舉例：硬體資源保護 --- I/O、Memory、CPU

  1. 運用Timer作為CPU protection ➡️ Mechanism   [附程式]  

     Maximum Time Quantum大小制定 ➡️ Policy   [參數]

  2. CPU排班採優先權排班 Priority scheduling ➡️ Mechanism

     Priority大小之定義 ➡️ Policy





## Ch4: Process Management 與 Thread Management

1. Process定義、與Program比較
2. 🌠 Process Control Block (PCB) 內容
3. 🌠🌠🌠🌠🌠 Process State Transition Diagram (STD)
4. 🌠🌠🌠 Scheduler種類：長期排班器、短期排班器、中期排班器
5. 🌠🌠🌠 Context switching
6. 🌠 Dispatcher, Dispatching latency 分派器與分派延遲
7. 🌠🌠🌠🌠🌠 **Process Control Operations**（以 UNIX system call 為主的程式追蹤）
8. 🌠🌠🌠 評估CPU scheduling performance之五個criteria
9. 🌠🌠🌠🌠🌠 各種CPU排班法則介紹以及相關名詞（7個排班法則） (Starvation, Aging, preemptive, non-preemptive, Convoy effect)
10. 特定系統排班設計 (✨✨multiprocessors system, ✨✨✨ real-time system)
11. Thread Management



# 12. Process

- 定義：A program in execution 執行中的程式 [恐龍本]
  - Process建立之後，其主要組成有：
    - Process No. (ID)
    - Process State
    - Code section, data section
       (這兩個合起來就是process memory space)
    - Programming Counter (PC, 程式計數器 - 內放下一條指令的位址）
    - Stack (保存參數、區域變數、返回位址,etc.)
    - CPU registers value, etc.
  - 是OS分配 Resource 之對象單位（Resource: CPU, I/O-Devices, Memory)

- 與Program（程式）比較

  | Process       | Program                              |
  | ------------- | ------------------------------------ |
  | 執行中的程式  | Just a File stored in storage device |
  | Active entity | Passive  entity                      |

#### Process Control Block (PCB) 內容

- 定義：OS為了管理所有Processes，會在 kernel memory 中，替每一個process各字，準備一個 Block（表格）記錄 process 之所有相關資訊



#### PCB主要內容有（八個）

1. Process No.(ID): 處理程式編號是 **unique**（唯一的）
2. Process state: e.g. ready, running, wait, etc.
3. Programming Counter: 內放the **next** instruction’s address
4. CPU registers: e.g. 計錄使用到暫存器的值 

​              e.g. Accumulator, PSW (process status word), Stack Top, etc.

5. CPU scheduling info:

   e.g. process優先權值 process arrival time (FIFO), etc.

6. Memory Management info

    (隨OS Memory記憶體管理方法不同，紀錄不同資訊）

    e.g. Base/limit register, 或 Page Table 或 segment table

7. Accounting Info 

   例: process已用多少cpu time，哪些資源，還剩多少資源、cpu time可用, etc.

   > Note: 
   >
   > 目的 (1) 記帳、收錢 (2) Administrator 調校效能的依據

8. I/O Status info

   例: process已發出多少I/O-request, 完成狀況如何, 佔用哪些I/O resources (目前) etc.

#### Quests

- 下列哪一項並未隸屬於PCB？

  1. Process number

  2. CPU register

  3. I/O-Device Queue 

     ➡️這是OS或device driver在管的。容易與I/O-Status Info混淆。

  4. CPU scheduling info

  5. Memory Management info

- Which of the following is not encoded in the process control block?

  1. process state

  2. the bitmap of this process

     ➡️Bitmap是磁碟在用的（Disk可用空間管理方法之一)

  3. cpu register

  4. memory management

#### Process的State Transition Diagram 

- STD, 狀態轉換圖

- 目的：描述process之lifecycle

- 版本有三種
  1. 恐龍本       5個 status 之STD ➡️ 看不到medium中程
  2. Stalling版 7個 status STD 
  3. UNIX STD (Stalling版）



##### 恐龍本：五個 Status 之 STD

![image-20240126181011915](https://i.imgur.com/bqxOpsF.png)

| State                           | Description                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| New (create)                    | Process被建立，已分得PCB空間，尚未載入memory（或尚未取得memory資源）(有身分證但不知道住哪) |
| Ready                           | Process已在memory中，且在ready queue內，具有資格爭奪CPU      |
| Running                         | Process取得CPU，執行中                                       |
| Wait (blocked)                  | Process在waiting queue中，waiting for I/O completed or event occurs. 不會與其它 process 爭奪/競爭cpu |
| Exit (terminate/ zombie/ abort) | Process完成工作 正常結束 或 異常終止，此時可能其PCB空間尚未回收，因為要等其父親 (Parent Process) collect收集完該子Process之成果後，才會回收PCB這個表格的空間。其它資源(memory, cpu, I/O-Devices)已回收 |

| Transition | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| 1️⃣          | 也叫Admit, 當Memory space足夠時, 可以由long-term scheduler (in batch sys.) 決定將此Job載入到memory中 |
| 2️⃣          | 叫 dispatch，由 short-term scheduler (cpu scheduler) 決定讓高優先權 process 取得 cpu |
| 3️⃣          | 叫time-out/interrupted, etc. 執行中的Process因某些事件發生而被迫放棄cpu，回去ready Queue. e.g. time-out, interrupt發生, 高優先process到達, 排到隊伍你位置的前面 |
| 4️⃣          | 叫 wait for I/O-Completed or event occurs                    |
| 5️⃣          | I/O-Completed 或 Event occurs                                |
| 6️⃣          | Process完工或異常處理                                        |

#### Stalling

- Medium term scheduling

- Which of the five status has the property that its process is in memory?

  〔New〕  〔**Ready**〕  〔**Running**〕  〔**Wait**〕  〔Exit〕



| Transition              | Description                                                  |
| ----------------------- | ------------------------------------------------------------ |
| Blocked/suspend         | Process 被 swap out 到 Disk 中暫存，即 blocked (asleep) in Disk |
| Ready/suspend           | event occurs or I/O-Completed.                               |
|                         | ready in Disk                                                |
| Suspend/ (swap out).  1️⃣ | 當記憶體memory空間不足，又有其它高優先權process須更多記憶體空間時，會由中期排班器 medium-term scheduler 決定將 blocked process |
| Activate/ swap in       | swap-out到 Disk，以空出記憶體空間 memory space (記憶體->磁碟) 當memory space有空，medium-term scheduler可以將它們 swap in 回memory中，ready for execution |
| Suspend/ (swap out). 2️⃣  | 支持transition之理由有兩個: <br>1️⃣ 所有blocked processes皆swap out後, 記憶體空間仍不足時 |

























