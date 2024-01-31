---
title: "Lesson 06: Linux 筆記 - Shell Script! final"
date: 2023-04-22T09:10:52+08:00
author: "celine"
tags: ["linux", "security"]
categories: ["StudyNote"]
---

> Shell Script 與排程
>
> Linux 安全應用演練及總複習

### shebang 定義
 - 指定由哪一個程式進行解讀此一檔案
 - 必須放在
 - #!/bin/bash


### 執行Script
- 有執行權限，可用 ./{file}執行
    - `./myscript.sh`
- 沒有執行權限
    - `sh myscript.sh`
- 執行 Script 時顯示除錯變數
  
### VARIABLES

- 開頭可以是底線或字母
- 字串建議都用雙引號框起來，減少失誤
    - `script = "Horizon"`

🎁

---
## Shell Script
#### 1. `if ... elif ... else ... fi`
```shell=
#!/bin/bash

LANG=C #強制輸出的語系

x=10

if[ ${x} -gt 10 ]; then
    echo "x greater than 10."
elif [ ${x} -ge 0] && [ ${x} -le 10 ]; then
    echo " x between 0 and 10."
else
    echo " out of range. "
fi
```
- `-gt` : greater than 
- `-ge` : 
- `-le` : 
- 注意 `[` 和 `]` 的左右**必須要有空白**，否則會出錯

#### 2. `for ... do ... done`
```shell=
#!/bin/bash

LANG=C

x="a    b    c    d    e    1    2    3"

for e in ${x}; do
    if [ "${e} == "e" ]; then
        break
    else
        echo ${e}
    fi
done
```
- 是 `tab` 鍵就會把他拆開來變成
- `chmod +x for.sh`
- break: 跳出迴圈
  continue: 跳過該輪
  
#### 3. `while ... do ... done`
```shell=
#!/bin/bash

x=10
n=0

while [${n} -le ${x} ]; do
    echo ${n}
    ((n++))
done
```
- 如果要當作數字作加減，必須用兩個小括號包起來
- 做一件事情睡一秒
```shell=
#!/bin/bash

n=o
while true; do
    echo ${n}
    ((n++))
    sleep 1
```
#### 4. switch case
- switch 一個區段的表示方式 `;;`
- default 以上皆不符合，落入此區段 `*)`
- debug模式的指令下法:`sh -x ./case.sh`

```shell=
#!/bin/bash

LANG=C

x=e

case "${x}" in
    a)
        echo "value of x is a."
    ;;
    b)
        echo "value of x is b."
    ;;
    c)
        echo "value of x is c."
    ;;
    d)
        echo "value of x is d."
    ;;
    *)
        echo "out of range"
    ;;
esac
```

#### 5. 重複兩次以上, 做成function

```shell=
#!/bin/bash
LANG=C

function say() {
    local w
    w="${1}"
    // w="${1}"
    echo "x: ${w}"
}

x=c

case ${x} in
    a)
        say ${x}
    ;;
    b)
        say ${x}
    ;;
    c)
        say ${x}
    ;;
    d)
        say ${x}
    ;;
    *)
        echo "out of range"
    ;;
esac
```


---
## Shell Script 猜數字

- `shuf` 產生隨機輸出
    - Syntax : 
        - file shuf: `shuf <options> <file>` |
        - range shuf: `shuf -i <LOW>-<HIGH> <options>`
        - list shuf: `shuf -e <option>... <arg>`
- `read` 讀取輸入值
    - Syntax : `read <options> <arguments>`
      ** -p \<prompt>** : Outputs the prompt string before reading user input
- `export` 引入當作檔案一部分,不會產生新的process id

#### 需求

- Script 啟動時，提供一變數，最小為 1 最大為 100 做為猜測目標值 n。
- 玩家每猜數字 g，若猜中直接顯示 Bingo 後離開。
- 玩家沒猜中
    - 玩家每猜數字 g，猜的數字若 > 1 且 < n 則最小值應設為 g。
    - 玩家每猜數字 g，猜的數字若 > n 且 < 100 則最大值應設為 g。
    - 重覆遊戲直到猜中。
    
#### Solutions
<details>
<summary>偷看解答</summary>

##### 1. guessNum.sh
  - vi
    ```shell=
    #!/bin/bash
    export LANG=C
    
    max=100
    min=1
    n=$(shuf -i ${min}-${max} -n1)
    g_cnt=1
    
    echo "Target: ${n}"
    
    while true; do
    
        read -p "Enter number: " guess
    
        if [ ${guess} -eq ${n} ]; then
            echo "Bingo!"
            echo "You guess ${g_cnt} time(s)."
            exit
        else


            if [ ${guess} -gt ${min} ] && [ ${guess} -lt ${n} ]; then
                min=${guess}
            fi
            if [ ${guess} -gt ${n} ] && [ ${guess} -lt ${max} ]; then
                max=${guess}
            fi
    
            echo "Between ${min} and ${max}."
    
            ((g_cnt++))
    
        fi
    done
    ```
  - cmd bash
    ```bash
    student$ chmod +x guessNum.sh
    student$ ./guessNum.sh
    ```
##### 2. 改為引用設定檔案 newGuessNum.sh
  - `-f` 辨別檔案存不存在
  - `-d` 辨別目錄存不存在  
  - `export`  
  - bash
    ```bash
    student$ mkdir guess; cd guess
    student$ cp ../guessNum.sh ./
    ```
  - config
    ```vi
    max=10
    min=1
    ```
  - newGuessNum.sh
    ```shell=
    #!/bin/bash
    export LANG=C
    
    if [ -f ./config ]; then
        source config
    else
        echo "Can not find config"
        exit
    fi
    
    n=$(shuf -i ${min}-${max} -n1)
    g_cnt=1
    
    echo "Target: ${n}"
    
    while true; do
    
        read -p "Enter number: " guess
    
        if [ ${guess} -eq ${n} ]; then
            echo "Bingo!"
            echo "You guess ${g_cnt} time(s)."
            exit
        else
    
            if [ ${guess} -gt ${min} ] && [ ${guess} -lt ${n} ]; then
                min=${guess}
            fi
            if [ ${guess} -gt ${n} ] && [ ${guess} -lt ${max} ]; then
                max=${guess}
            fi
    
            echo "Between ${min} and ${max}."
    
            ((g_cnt++))
    
        fi
    done
    ```
  - bash
    ```bash
    student$ ./newGuessNum.sh
    ```

</details>

---
## 登入告警

- `env | grep SSH` vs `set | grep SSH`
   ![env_整個環境 vs set_使用者自己的](https://i.imgur.com/jqBLc8v.png)


@Server
```
root# vi /etc/profile.d/class-login.sh
```
```bash=
#!/bin/bash
login_from=$(echo ${SSH_CLIENT} | awk '{print $1}')
logger "You are ${USER}. Login from ${login_from}"
```

@Server

Login as student.

Login as root

@Desktop
```bash
root# ls -lh /var/log/172.16.2.X/*/*
Jan 23 15:30:33 desktop0 student[6904]: You are student. Login from 10.6.23.150
Jan 23 15:32:06 desktop0 student[6970]: You are root. Login from
Jan 23 15:32:46 desktop0 student[7030]: You are root. Login from
Jan 23 15:32:51 desktop0 student[7123]: You are student. Login from 10.6.23.150
Jan 23 15:36:06 desktop0 student[7204]: You are root. Login from
```
![log位於student.log檔案裡面](https://i.imgur.com/FWUjQNb.png)


---
## `systemd`服務

> `/etc/rc.d/rc.local` 開機後最後一個動作 vs `systemctl`

@Server
```bash
root# cd /opt/
root# vi lab.sh
```
```vim=
#!/bin/bash 
date >> /tmp/time-lab.log

exit $?
```
> ## `$?` 
> 通常用來判斷上一支程式是否成功執行
> 是指上一個程式碼的執行結果，通常為整數 (如果為0，通常status:ok)

```bash
root# chmod +x lab.sh 
root# ./lab.sh 
root# echo $?
root# cat /tmp/time-lab.log 
```
```bash
root# cd /usr/lib/systemd/system
```

```bash
root# vi lab.service
```
> systemetl status sshd 
> 可以得到
> `ls -l /tmp/` show total
> cp sshd.service lab.service
> system status lab.service
> system start lab.service

```vim=
[Unit]
Description="LinuxClass systemd.timer lab."

[Service]
ExecStart=/opt/lab.sh
```

```bash
root# cat /tmp/time-lab.log 
```
```bash
root# systemctl status lab.service

root# systemctl start lab.service
root# systemctl status lab.service

root# ls -l /tmp/
root# less /tmp/time-lab.log 
```
![](https://i.imgur.com/59sP3n7.png)

---
## `systemd`計時器

@Server

#### Create job script.
```bash
root# mkdir /opt/lab; cd /opt/lab
```
```bash
root# vi echo-time.sh
```
```vim=
#!/bin/bash
export LANG=C
export PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin

_d=$(date)
echo ${_d} >> /tmp/timer-lab.log

exit $?
```
```bash
root# chmod +x echo-time.sh
```
![create_job_script](https://i.imgur.com/yLIfPmZ.png)


#### Create service

```bash
root# cd /usr/lib/systemd/system
```
```bash
root# vi class-timer-lab.service
```
```vim=
[Unit]
Description="LinuxClass systemd.timer lab."

[Service]
ExecStart=/opt/lab/echo-time.sh
```
```bash
root# systemctl status class-timer-lab.service
```
```bash
root# systemctl start class-timer-lab.service
```
![create_service](https://i.imgur.com/2f4jzaG.png)


#### Create timer
```bash
root# cd /usr/lib/systemd/system
```
```bash
root# vi class-timer-lab.timer
```
```vim=
[Unit]
Description="Timer config for LinuxClass lab"

[Timer]
OnCalendar = *-*-* *:*:0
Unit=class-timer-lab.service

[Install]
WantedBy=multi-user.target
```
> run-level 0~6
> 其中一個是multi-user

> Ref: [systemd.time — Time and date specifications](https://www.freedesktop.org/software/systemd/man/systemd.time.html)

```bash
root# systemctl start class-timer-lab.timer
root# systemctl status class-timer-lab.timer
root# tail /tmp/time-lab.log
```
![create_timer](https://i.imgur.com/mhstiVH.png)


> Podman 因為是 daemonless，可以搭配 systemctl 做管理

> #### 📚 推書時間:
> 1. ✨**圖解 Linux 核心工作原理** | 透過實作與圖解學習OS與硬體的基礎知識
> 2. 資安專家的 nmap 與 NSE 網路診斷與掃描技巧大公開
> 3. 實戰 Linux 系統數位鑑識 

> [Link to an interactive kernel map](https://makelinux.github.io/kernel/map/)

####  使用者排程
https://fedoramagazine.org/systemd-timers-for-scheduling-tasks/
https://cfarm.blog.aznc.cc/使用-systemd-timer-代替-crontab/


---
## 作業系統效能分析

#### 效能分析
客戶提供了 sysstat 的記錄檔，經視覺化處理後得到每台主機相對應的圖表。
1. I/O TPS
2. Swap Used%
3. Swap In/Swap Out
4. Load Average

試分析主機運作情況。

1. 各主機是否正常運作。
2. IO 是否需要更新。
3. 記憶體是否足夠。

@Server
- two root users
- 以下的bash: 用以測試or密碼學中產生"熵"
  ```bash=
  root# uptime
  root# dd if=/dev/urandom of=/dev/null 
  ```
  - `dd`: data duplicator
  - `if`: input file
  - `/dev/urandom`: for generating endless stream of pseudo-random bytes
  - `of`: output file
  - `/dev/null`: file that discards all data written into it
  ![dd if](https://i.imgur.com/yoI31b9.png)

#### `sysstat sar` - 本機監控的工具

- `sar <options> < <interval> <count> >`
![top](https://i.imgur.com/eJAYpVv.png)


---
# Bash TextBook Note

##### 重導向與管線(page 7)

##### 變數
- 雙引號: 裡面的變數會自己展開
- 單引號: 只會被當作字串

##### 位置參數
- 看有幾個參數輸近來，不包含自己
    ```bash=
    echo $#
    ```
    
#### 攻防基本原理
- (p34) CIA 保密性、完整性、可用性
    - 備份: 完整性
    - DDClass、NLB(Network_Load_Balance): 可用性
    
- 網路安全五大原則: CIA + 不可否認性 + 驗證

#### 攻擊的生命週期 
- page 39

#### 資料蒐集常用指令
- `cut`
- `file`
- `head`
- 建立 log server
- 蒐集系統資訊
    - commands
        | Linux cmd         | Win Git Bash | Purpose                   |
        | ----------------- | ------------ | ------------------------- |
        | uname -a          | uname -a     | 作業系統版本資訊          |
        | cat /proc/cpuinfo | systeminfo   | 系統硬體相關資訊          |
        | ifconfig          | ipconfig     | 網卡資訊                  |
        | route             | route print  | 路由表資訊                |
        | arp -a            | arp -a       | 顯示位址解析表(ARP Table) |
        | netstat -a        | netstat -a   | 顯示網路連線 *`ss`\       |
        | mount             | net share    | 顯示檔案系統              |
        | ps -e             | tasklist     | 顯示執行中程序            |
- awk(比`cut`指令好用) sed
  > sed & awk 程式設計 (O'reilly, 紫色那本)
- linux處理xml(p.83): 改用 linux xmllint 較優
- linux處理json(p.85): 可改用 [linux JQ 工具](http://bit.ly/2HJ2SzA)

#### 資料分析 (p. 91)
- sort: 排序
- head: 
- uniq: 篩選掉相鄰且重複的資料行數
  > honeypot 蜜罐系統
  
#### 檔案系統監看 (p. 139)
- 偵測檔案是否有被異動
- 整章的工作目的都可以被AIDE取代

#### VirusTotal (p. 165)

#### 指令稿隱蔽 (p. 191)
- 使用語焉不詳的命名慣例，同時重複使用相同的名稱命名(局部/全域變數都取同名)，再把全部內容串成一行，然後再把註解都拿掉
- 哩金派，金派
    ```bash=
    str="reboot"
    eval $str
    ```
- `exec`、`exe`避免
    - php, python
    - 容易被 inject 攻擊
    
#### 使用者、群組、權限 (p.229)
- chmod chown ...

#### Linux檔案權限 (p. 236)
- ✨✨ 0-7授權

#### 工具: 系統可用性監看 (p. 245)
- 我ping你，你不回，但是我不能因為這樣就判定對方已經死掉。應該去監控該port是否還活著
- ping 走的是 ICMP 協定

#### 如何知道Server裡面裝了什麼東西 (p. 251)
- rbm, dkbg


> #### 📚**推書第二單**
> 精通Shell程式設計
> LinuxShell程式設計與管理實務(臥龍小三)

> **自主練習方法**
>
> vmware player 裝兩台 vm
