---
title: "Lesson 01: Linux 筆記 - Repository, 系統稽核/查詢"
date: 2023-03-04T09:54:41+08:00
author: "celine"
tags: ["linux", "security"]
categories: ["StudyNote"]

---

憑證具有 CIA 三要素

+ Confidentiality 機密性

+ Integrity

+ Availability

+ Others 

  + Authenticity

  + Accountability

  + Non-repudiation 

AAA, Triple A - 認證授權紀錄

基本等級設計 - 強制原則MAC、地方自治DAC、以角色為基準授權RBAC

## ---

ntpdate 舊時代的工具，別用

[Chrony_NTP_lab](https://www.stdtime.gov.tw/chinese/bulletin/NTP%20promo.txt)

![image-20230304115103043](https://i.imgur.com/9zUWt77.png)



### Linux history (morning)

![image-20230304115914285](https://i.imgur.com/Lc20nFT.png)



### Recommended 

+ `.GUTS` - jsurf
+ 實戰 Linux 系統數位鑑識 (GoTop)
+ 圖解 Linux 核心工作原理（GoTop）



```
server 172.
server desktopXX iburst
```

```
systemctl restart chronyd
systemctl enable chronyd
systemctl status chronyd
```

### 在 Docker 設定時區

https://confluence.atlassian.com/kb/how-to-set-the-timezone-for-docker-container-976780914.html#:~:text=The%20timezone%20value%20in%20Atlassian,e%20TZ%3D.

### 另外一個設定時區方式 

podman.io (官方比較少推)



---

### 兩種設定方式

官方主推指令方式 `getent group wheet`

![image-20230304140523402](https://i.imgur.com/0dSLkbT.png)



![image-20230304141702970](https://i.imgur.com/ImZMj34.png)

### 設定帳號不能登入系統

![image-20230304142728425](https://i.imgur.com/pldoJQn.png)

也可以在 `/opt/`目錄下寫個 `.sh` 檔案，讓用戶在 desktop 登入遭拒絕之前先出示文字 or 其它執行動作

![image-20230304144617332](https://i.imgur.com/HtlyJM8.png)



---

### 帳號停用/啟用

![image-20230304150206448](https://i.imgur.com/XnfPS47.png)



### 密碼更新政策

+ 設定user1每60天改密碼

  ![image-20230304150348214](https://i.imgur.com/hAVOhTV.png)

+ 設定 user1 密碼永久有效

  ![image-20230304150555001](https://i.imgur.com/ydMjeze.png)

+ 設定 user1 密碼至少要用 2 天才能修改

  ![image-20230304150731570](https://i.imgur.com/q9ykljU.png)

+ 設定 user1 密碼可隨時修改

  ![image-20230304150810672](https://i.imgur.com/DA0FJBV.png)

+  

> CTRL + L clear terminal command



---

#### 登入不用密碼

+ @Desktop

  ```terminal
  student$ ssh-keygen
  student$ ssh-copy-id sysadmin@server{XX}
  student$ ssh sysadmin@server{XX}
  ```

![image-20230304153804706](https://i.imgur.com/ceXOgei.png)

---

#### sshd 停用密碼登入



> -i 指定密碼確切位置
>
> ssh -i keybackup/id_rsa sysadmin@server{XX}
>
> 很少在 desktop 上面用 root



### 系統稽核

一些指令的簡單介紹

[Linux.die.net](https://linux.die.net/man/8/auditctl)

![image-20230304165045772](https://i.imgur.com/xaXaTYU.png)



#### 檔案完整性

搭配 `aide` 輔佐檢查檔案屬性是否有被修改，改了哪裡

那裡的hash不同(如果是修改，但內文不變，則hash不變，但有modified紀錄)

![image-20230304170418559](https://i.imgur.com/lfVUtt5.png)

> auditd 可以知道被觸發了什麼
>
> aide 可以比較具體的知道屬性哪裡被改了