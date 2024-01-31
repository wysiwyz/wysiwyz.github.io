---
title: "Lesson 00: Linux 筆記 - 資安原則、OpenSCAP"
date: 2023-02-11T10:08:41+08:00
author: "celine"
tags: ["linux", "security"]
categories: ["StudyNote"]
---

### 資安是一個循環

+ 這次會過不代表下次會過，每年都要複驗
+ ISO27001 改版（LA - lead auditor）
+ Log server
+ Windows 的 GPO policy 是什麼
+ `.twcertcc` : https://www.twcert.org.tw/tw/mp-1.html

### 營利目標

+ 依公司法，企業營運是為了向股東負責
+ 企業營運
  + 以**永續經營**與**營利**為目標
  + 發展產品或服務解決客戶之問題，或者滿足其需求
  + 有利企業發展的事才考慮執行

### 機密性

+ 產生營利的過程中，衍生出只有甲乙雙方才能知道的事件
+ 營運過程中，衍生無法完全透明的內部資訊
+ 營運過程中，產生敏感度較高的數位物件 (e.g. 財務、薪資)
+ 營運過程中，人事時地物的產生與其產生結果，具有不同層級的影響力

### 目標

+ 保護營運資產不被未授權的人存取或者破壞，使用IT方式達成目標
+ 常用手法
  + 人員階級、職等、業務範圍管理
  + 實物管理（區域進出換證、資產盤點）
  + 數位資訊管理

### CentOS 8 安全措施

+ 作業系統安全
  + 作業系統安裝
  + 系統紀錄
  + 檔案稽核
  + 檔案系統權限配置
  + 系統防火牆
+ 應用服務安全
  + Web SSL
  + MariaDB (MySQL ... audit 要付費)
  + Samba(smb)/NFS

> [AP windows] (NFS) ----> [AP linux] (NFS)
>
> * NFS: 可以做到細度、哪個 IP 可以對資料做存取
> * SRC
> * cifs (smb)(smb3): 445/TCP 加密 
>
> ---
>
> linux 777 是什麼?



### 課程網路 2/2

+ VM

  + desktop IP: 10.6.X.5

  + server IP: 10.6.X.11

    (X = **代號 30**)

### Cent OS

+ Cent OS Linux (到 version 7) 現在改叫 Cent OS Stream

### RedHat 歷史

1. Redhat Linux ~ 9.0 (到第九版就結束了)
2. 第九版之後，分成兩條線: 
   1. **Fedora** (遊戲場?) - 未來 RHEL 會長的樣子
   2. **RHEL 8** ($)
      - 因為有 GPL license，有掛此排的 Open Source 軟體都要公開 source code (`srpm`)
3. RHEL 8 再 build 出兩種
   1. **Rocky Linux** - 更版 1~2 天
   2. **AlmaLinux** (背後有 cloud linux) - 弱點修補較及時, 更版比 rocky linux 較快

### REPO 來源

+ 統一的安裝來源 (容易部署、容易維護)
+ 確保安裝環境一致
+ 節省外部流量
+ http repo 
  + http://10.6.X.5/repo/BaseOS/
  + http://10.6.X.5/repo/AppStream/



### OpenSCAP

檢查目標主機的安全性設設定是否符合需求

+ https://csrc.nist.gov/
+ 工具 `dnf install -y openscap-scanner`
+ 指引參照 `dnf install -y scap-security-guide`

---

## 🔰 Course Notes 

**下次上課不要帶課本**

---

大主倉: https://archive.kernel.org/centos-vault/Chat 

GPT: Linux + Python (每個月 20 元)

---

VM:

desktop IP: 10.6.30.5

server IP: 10.6.30.11

---

VMware Workstation 16 player 

New 一個 desktop

Network Adapter: NAT

Network Adapter 2: Bridged (Automatic)選 4 GB 就好

Power on

---

Rocky Linux

課程提供之 VM 登入密碼:

​	ID: student

​	PW: 

---

登入後語系都選 next ...

Activities (右上角) → Terminal 

```
$ sudo su -
[sudo] password for student:
[root@desktop-X ~]# head -n 1 /etc/shadow
root:!::0:99999:7:::
```

---

改成 desktop.30 ，如以下截圖![img](https://lh6.googleusercontent.com/hZhsxQuAKe-UXtgzrOQfVWbIW2ZYQEQqOrnGhCm92XYdr5QVU4_uhD0osfbdPIcO457kSOgQXkO6Bi2IUnYN35CTAH3F7HOtONal-XjWVj4hC8wcSSt8eqrElQD0iYMdH_yrassMyRvN11u9eyiMZp4)
接著輸入 reboot 重新開機

+ `nmtui` 進入編輯選單
  + 選第二張 network adapter，改 Manual ，點 show
  + Addresses: 10.6.30.5/24 (這裡的 24 指的是遮罩 8 bytes *4)
  + Automatically connect，按空白鍵勾選它 [V]，點OK

```bash
$ nmcli connection show
NAME  UUID                         TYPE       DEVICE
$ nmcli connection up ens192   # 這是使第二張網卡生效
Connection successfully activated...
```

+ 對照 ens192 的 MAC address

![img](https://lh4.googleusercontent.com/W_9IX70XVauw1_LIP7y5B7BhxDYzR2ScDxdDPhagLJUxugqEBFj82e-Ns0TiaqXGt_Rz3itSxj8Dx1MK_ZGa1ZyhQOawY6SAtkxMlDtie80Yi_fzs9S_S7Blk4sXPffbOAnRd6a70i2EFtTDexgeIRY)

```bash
$ ip addr
$ nmtui
$ nmcli connection show
$ nmcli connection up ens192
$ dnf list
[baseos]
name=Class Base30
baseurl=http://10.6.X.5/repo/BaseOS/
enabled=1     # 1 啟用 、 0 停用
gpgcheck=0

```

![img](https://lh3.googleusercontent.com/YPMewhOPa0Xvw79t5e6_7o6VnM29ksmNsYJsar6GYvs314DjJEySNVWE-LlfRSuBbVGyppfK9Et1fAUsRAWVyyKHplgwoTljh57if_J5arSWwxmWpb45VT5wUF5XdKQErweUAtjpSjrDoo8HJnOmgQs)

1. 變更 baseurl 的方法一：`vi local.repo`![img](https://lh4.googleusercontent.com/IJNkfEp9fOE_jhYy52tW02jd2qAZpzn3M-i1uQpTbNX_ze2v07y_wprBz3O0wi1YUwJmhm1p3bpNmW9zz3Tgxf266cWdyVT3FwdZsRkPUeE3IcnWt6WdWLy8Atm4yokpMLh0_jhDLC1Xfr10uBHbV9I)

2. 變更 baseurl 的方法二：

   ```bash
   $ sed -i -e 's/10.6.X.5/10.6.30.5/g' /etc/yum.repos.d/local.repo
   $ cat local.repo
   $ curl http://10.6.30.5
   $ systemctl enable --now httpd
   $ dnf list  # 會出現一串藍色字清單
   $ systemctl status
   $ history # 可以列出之前打的指令
   ```

![image-20230211183048859](https://i.imgur.com/MxJ9feX.png)

## 開另外一個 VMware Workstation 16 Player 視窗，建立 server

1. Step 01

![img](https://lh6.googleusercontent.com/Y09X-7ABL4zxgMljxg7rHIMoVKwAA1X09oZqkXG8oFWmB6u3UZ2cQOjDRVnZzir6IQoKEtE_H0zkLv3LwofIyF7-yteYEfVp1aUEExU79bTwwhKc0bYQNzNMoOGjUHHwwi4Xz7Hf2ELYgFWbvQy9is0)

2. Step 02

![img](https://lh3.googleusercontent.com/HbSbtBIiXvwGmK-z1RN64QIpTJ5rclh1UsWSFvIs-RXnAaZ8fTrydQef-tvE1c6BLC8j40B05nrEw0fICECZTkqedlD3S83Kc-lZyLIKSaV2YGNGM6fC7AWCxVCLac5NXLVWamscCHoXJYHBTJTFyx8)

3. Step 03

   ![img](https://lh4.googleusercontent.com/SsztfCnEi2_nXcg9aBMIdb7an5Y5pqZHxjpAC0OfiLGpEKdtUu3JR7kETEsknPP4Eg2DXkJ3H4S2xUt6tRjGugsaDnLiKhuC_AwKiL7BVBFYyGHmzfPUP8nX5HV4cX8TUntkX5UH7tBOpTMTm1m2iSE)

4. Step 04

   ![img](https://lh4.googleusercontent.com/_7r4EiY6MQKB5OB2di6abTQAXLKutsq0bS7psYlt-fjsL3cgsp2vRZlPTM-NZLUcWzUVB1ZRbo0_hvkXk-5G-cMOoAmKij9sHsljJ5e2GxTdo1xq541wBm63S4_pZBWan77u0DQLQANIKojZzQ6v4gI)

5. Step 05

   ![img](https://lh3.googleusercontent.com/k01VnrCgbEG3-58tQCEvtC_RW4vWOLteCLheIppRDUul-fkKAET8XFBVihl_jLiOPgCJbVeEZag--2YunKqUDNHZRb7g5bTZAHbI0ez4_lcX1OiwcHwFhY_g580ywO29k8f8SWq1O42Qsy3UMEscEuc)

6. Power On

7. Editing ens192 (bridged network adapter)

   General - 勾選 Connect 

   ![img](https://lh6.googleusercontent.com/D7qydIUos_STgWWVhaqelwhqTj_fQjyV7EKeviF13I5ifJASg0NnFBoq7HffS3YCBS1XZKSBC7mJEMKvNFvgXih5K8BSbdJMA3ULjd4mBMah_SikK9q60KqpqF2vYmtMsN0pl9Vhc3eO5wFo6tMMFuk)


Power On

8. IPv4 Settings 增加 Addresses 以及 Netmask 24

   ![img](https://lh3.googleusercontent.com/E-cdXHR-NsFaiU9_ozHQ6sp8JbuOnxwzUWCOIl9MaHZRWWBVeE1Dfj_WzgHXR102_DKJfJsU4LCUoqomyi0Xx1jgvC2FLgXi1x1mv9i_KLMEPiRy74xeyIgPQe_vKGCZc7QBA43gFT0wSQ8UD7jd3GI)

9. ens192 network adapter SWITCH ON

   ![img](https://lh4.googleusercontent.com/-mUG-pWLcMs8HK8DDSXWmA99gts5moDHGylQ69bfCRgCqhOmSSUCSd6C2mi4JF9l4exLVf1umfsqJFhIlYxndTyP3XVzp-DLoHW_pq9GijTQbeV5URq6oNHjiFYjUmqYsqryO0QUtDX_dA4sqidxvck)

10. 設定 Root pcode，方便記憶這裡使用 1qaz1qaz

11. Installation Source (SOFTWARE 中間第一個)

    ![img](https://lh6.googleusercontent.com/8QnAsETziErtcs-TJjWYH352XFQiHkaNHsRUnUD4TUgafHWBufu4m0Nzz_huAIBllg6-j6EeITZTlqwKodPNzw0PlNtQGRKjhL9Gm87Huuvzp8apUAILuxPrKQFEwloatT3IwBZeR5JAPrnXF6RaWZE)

12. Software Selection (SOFTWARE 中間第二個)：選 minimal install![img](https://lh4.googleusercontent.com/qWa6Eu9S2LaUlQRsjhmBh_SXM5G6hkv4OGimK1qDc-bozDXbey387-VHP0tbfpDjN17ERkCxP6dggZsFOopqV9VhuemxHploiOvbltaMZSHmVHsszzihCfagBsTxXyy7jdZVt4eQ-d0PUZ54s3JKktk)
13. 中間第一個 + additional repo，填入 http URL `10.6.30.5/repo/AppStream`![img](https://lh3.googleusercontent.com/yxHkI23BwRwKOcy8WGv_ASGjBFHJkulUpR1ZrEnnoq6jgHtgLsn1gtTztgUpsURWluQ4XRpcxXPO3u3-9mTbFglTSdT82p3S4odmV0ILnYmVGLiV7xmsG7ulHlGszFmVnCrsXTHgzbYo68IBn7EnpK0)

14. 右上第一個 : 磁碟分割Storage Configuration 選 [custom] → [DONE] → 跳進坑之後再點 <u>I would like to automatically…</u> → [DONE]

15. Security Policy 據說要付費，不用選，中間上有個按鈕把它 [switch OFF]![img](https://lh5.googleusercontent.com/PFWVIm3QYOpKdeowuz7PXbFOexCQbNeTlYUYcK-uEMY3n9rjPmiaioIx50tgj4JWjCiMo3itpDi_NX1EzoTxGo5gy7vJnqIZXvGYKbA3ZmahOxrdDa2YWUKIrUqt1LWq1KtPz0CyYrrEI5tjv7Ixv6E)

16. 然後就可以 Begin Installation 了

    ![img](https://lh4.googleusercontent.com/l0lySatyvIFXSggRUav1kNE0d7waJlRbdYzeqAytbb50C4hI7Z7I65YV36K0g0kYO5J_EPP1qPOuYAXlWkcFsO_pwEKsMl7Qa1JrP4SRZ36LllWhsHea7X4g79vnCzPENJxdmFVvBbEOpYyOxkwzWbg)


​    

##  從 **desktop** 端 輸入 ssh root@10.6.30.11

desktop 端是今天開的第一個 VMware Player 視窗

![img](https://lh5.googleusercontent.com/KdtCF7tZxD0RBCq4XaACBh8GNWRF6wGWQU4SCqcSHrU6XE8kwy5r0sDaLQbJgG_bSlNckChcFJzmmQ3C79cUc1EFO6VRSxL_NqQUC74HFxW4XPwwH2Sy--56TNa05LLBcrzKdS0b6NPnj25LTAEWRAE)

1. Copy [baseos] & [appstream] 內文

   ```bash
   $ cd /etc/yum.repos.d/
   $ ls
   $ cat local.repo
   ```

   ![img](https://lh5.googleusercontent.com/VQD6UCNKbwIK3YQv_zpoelRvPsOuExKl9VHpNZuJtp7CLXPivLmkBL6TqUpSw8GQ3DhGPRmc_6dgJrFoxCX8QoxfmhoD9gAoZlNUCC6aOKPTHFV94jievTbqLmWbMGCnZ0nXLWsUGgySZdBTqf0SQm4)

   

   

2.  `sudo su -` 切到 root 使用者下載 OpenSCAP 合規確認

   ```bash
   $ sudo su -
   $ dnf install -y openscap-scanner
   $ dnf install -y scap-security-guide
   $ oscap info /usr/share/xml/scap/ssg/content/ssg-rl8-ds.xml | grep Id:
     # 查看參考指引的指令
   $ oscap xccdf eval --report ~/report.html --profile xccdf_org.ssgproject.content_profile_standard /usr/share/xml/scap/ssg/content/ssg-rl8-ds.xml
     # eval : 執行
   ```

   ![img](https://lh4.googleusercontent.com/Bizh621nygH29RYMBcTyRyNq81v6xM3hy_7ddvM9aYFJ42Qst7q5jniZ0vQpfasJGsoph6AeBCufgDpbkpVyQnGDsfcerwCTbJQtI52B0_AypUsEQ9ooTaU-eav6ACh7UcFnShEChni-PdjO8ighYU0)

3. 開始掃![img](https://lh4.googleusercontent.com/aYio4m9l2Pf7qUoWVbCnKJbkSI9zyXeiiioLaCg_BFvAF206U-U8XRAm2NZQqAybrcVuqOwDhZCrBRHUFDYff0vWyeL-9mld-mkF1JwsloE-k8O486AmvjqZU4iDa7C-dknoqNuv8xJqJZuUOr4Z8_k)

4. 檢視 Report

   ```bash
   $ cp ~/report.html ~student/
   $ chown student ~student/report.html
   ```

5. 使用 Firefox 開啟 ~/report.html![img](https://lh5.googleusercontent.com/eJfNUu8JYnMaRLS0TII6entFKeyplRiYdJPqsiQF6cwp3vZGQpE8XKob5aJjS4sTADAuuPvI7SaX_TVR8uhmO0-jckJiZQc1NQXYU-5F6YP-kkHP8sodtdGDE_5VxjZPqrO4OPkUuOJWZnMufCSS_Lg)

6. 看報告![img](https://lh5.googleusercontent.com/N05tFiHIEuXOEJDhO7IhQweID1lAHPUopr3TEq90T7P2JcMpV2YFu8C6tWnZhy1xEKWNtfJx1EUSX5y6ZV-ZTqV40lQgzKZMDW9GHqNQng_07eULQGM_rZay5sfGl-MmrUwd-73KgCp6-7JNkMSS78w)

7. 修補 

   ```bash
   $ vi /etc/pam.d/system-auth
   $ cp /etc/pam.d/system-auth{,_backup}
   $ grep nullok /etc/pam.d/system-auth
   $ sed -i -e 's/nullok//g' /etc/pam.d/system-auth
   $ grep nullok /etc/pam.d/system-auth
   ```

   ![img](https://lh4.googleusercontent.com/Qj7uB4mK5ls75rP3oudk4QuqdVtAScuwbDEQ_u0gVhznfLbGaFXfqvxj_kUuEyQaLPXZSBRgyCwbaIpMHgDmwt45vd34yq31YDxrMTYNRb0vK9747NeXeDQbui1eRuA8fnWhDuj6yyBmpp988BOvycQ)

8. 重新掃描，查看是否修正

   ```bash
   $ oscap xccdf eval --report ~/report2.html --profile xccdf_org.ssgproject.content_profile_standard /usr/share/xml/scap/ssg/content/ssg-rl8-ds.xml
   ```

9. 檢視 Report 第二版

   ```bash
   $ cp ~/report2.html ~student/
   $ chown student ~student/report2.html
   ```

   + **Version #1**

     ![img](https://lh6.googleusercontent.com/H0gZHXwkBmw84cxm1Wz1L4AKiuFRFKfZoqoEcMqiPOFzjuBlNvLkI-JQpwt3FRw5oy0EVPdH02Eq5c89jmxZSsU1dkUBm-yW-aUlF9WIZxtaxkZc3Fn5f_lXZRjcc44I7Bdyhfbrh2zsSe_DxlsRSJ0)

   + **Version #2**![img](https://lh4.googleusercontent.com/4k-vxukvT9ZmuSP3Xk0kS1GdDSaJpimydt9VtGmO1YnAVBcDRjwUr42T3fstjx5ViuBgYFCno-IOZMRdbmUn_w4aN70Zzz7woswsb6NfwCny5hPRRF8Kwp7b39EYyQOdsA-l28jcrbhbiBp3rsoJ178)

10. 安裝 scap workbench

    1. Download

       ```bash
       $ dnf install -y scap-workbench
       ```

    2. Open SCAP Security Guide : 選 Rl8

       ![img](https://lh5.googleusercontent.com/NsdsfTwAYjnm48IRAiIstO8jWKdufBJU06aWjPWOYXYEHyEY7jSe_L-SdUhdP0wCu-c6H37rmekeE6IdeiHMmBueLIx2eCexBhydq1zyefha68hz0hpw0LTFH_8bV-wzg1Z9jN7ENmNsIWZzdJDmlII)

    3. Customization **Profile** 選 SSSPRH En Linux 8(56)

       ![img](https://lh6.googleusercontent.com/Uheksew0c6C8Ry5XMh_WihHTFo4BjOpGd9r36oiRay_GzIc45wbAp4w85oCmCq-VW88sUgFRAsVYGJbh5w2jiTZFfImwdqd6eHTjWhTojG5EtjKRcBrdjnJKisnvUGPsyDygvlOohlo9evIcb6rgzJA)

    4.  Rules客製化：

       1. 可以作policy調整，點右邊的 [Customize]

    5. 還可以掃遠端：Target 選 [Remote Machine (over SSH)

       ![img](https://lh5.googleusercontent.com/944tKpcoC2DyCZudKI0vKFEJSyAK5gnNKQ_F-wpDDFDYjerBy2lczm2vNo7u_d3-nVleKMbrqyOGU-pN-4vEdsD6BcIc4pZx9Fg2MsXH1yyCTHhEvYUGo4FOGF8CJqpLGKy_J9D5qrXXGAOYIAX4ZiQ)

    6. WorkBench 掃完的畫面長這樣!![img](https://lh4.googleusercontent.com/2N5K9dIjgxCtnPepNbj_Gc40XgmO1qbWkjz67qgqWyH5lG-Ysab81u5RgWF2eUCy0YUb0WSgK-awtLa94opd7Y0XlkN7Zk1a_onkmqDtEHAHxm7mIHouX_fqxrFc2Q9GqKLNa1ZnrQOGfmAgx7qccL0)

## 磁碟分割

+ /boot - 1G (不用給多)

+ /var - 動態，看需求(資料庫,etc.) 通常 20~40G

+ /var/log - 5G

+ audit - 2G (如果爆掉可能是被攻擊)

+ /tmp, /var/tmp - 5G

+ /home

+ ps. swap 給 4G ~ 8G

  ![image-20230211195201490](https://i.imgur.com/4HCoJyE.png)

## 政府組態基準說明文件

 ( TWGCB-01-008 )

![img](https://lh5.googleusercontent.com/ZKH6pCqx7nv771lxnJgeHSj6iJoQERvQK0lREEd7SnHx-wfSsLz2SiM31AocaGIpGShjak7M-xXACnEgWUB-F4l9mEmIZaYHZWk-55AW5bodf7Jbbs1HmvSjNG-xq1tNylEoqnHng9yWohZC7eqJ0dY)

## VI 文字編輯器

### 一、準備個檔案

```bash
$ cd rm sample
$ cp /etc/passwd ./sample
```

```bash
$ vi sample
```

### 二、操作 Vi 說明

Linux 發行版中，許多服務都要經過編輯器修改設定才能正確運作，所以學習文字編輯器能夠 **有效提升** Linux 管理效率。

`vi` 文字編輯器歷史悠久，在大部分 Linux 發行版本都有內建。`vi` 不僅能在 Linux/Unix 中應用，也適用於 *BSD、macOS 等系統。

`vi`編輯器常用的功能不外乎：

+ 文字編輯
+ 游標移動
+ 存檔
+ 文字搜尋
+ 離開

`vi`編輯器主要的三個模式：

+ 編輯模式
+ 一般模式
+ 指令模式

離開模式的方法是使用鍵盤上的 [ESC] 鍵。

### 三、一般模式

+ 控制文件上的游標

  | KEY   | FUNCTION                    |
  | ----- | --------------------------- |
  | `[k]` | 游標往上                    |
  | `[j]` | 游標往下                    |
  | `[h]` | 游標往左                    |
  | `[l]` | 游標往右                    |
  | `1G`  | 游標移到第 1 行             |
  | `5G`  | 游標移到第 5 行             |
  | n`G`  | 游標移到第 n 行 (`n`= 數字) |
  | `G`   | 游標移到最後行              |
  | `^`   | 游標移到行首                |
  | `$`   | 游標移到行尾                |

+ 進行文件操作

  | KEY   | FUNCTION                  |
  | ----- | ------------------------- |
  | `yy`  | 複製                      |
  | `2yy` | 複製 2 行                 |
  | n`yy` | 複製 n 行 (`n`= 數字)     |
  | `p`   | 在下一行貼上 (小寫p)      |
  | `P`   | 在上一行貼上 (大寫P)      |
  | `x`   | 剪下一個字元              |
  | `2x`  | 剪下 2 個字元             |
  | n`x`  | 剪下 n 個字元 (`n`= 數字) |
  | `dd`  | 刪除一整行                |
  | `2dd` | 刪除二行                  |
  | n`dd` | 刪除 n 行 (`n`= 數字)     |

  

### 四、指令模式

在一般模式中直接輸入 `:` 開頭的指令就會進入指令模式

| KEY       | FUNCTION                          |
| --------- | --------------------------------- |
| `:w`      | 存檔                              |
| `:q`      | 離開                              |
| `:wq`     | 存檔後離開                        |
| `:q!`     | 不存檔離開                        |
| `:w!`     | 強制存檔 (需要有相關權限)         |
| `/{word}` | 尋找字串 (`{word}`代表要找的字串) |

### 五、編輯模式

使用以下方式可以從 **一般模式** 切換為 **編輯模式**

| KEY  | FUNCTION                                     |
| ---- | -------------------------------------------- |
| `i`  | 在目前位置開始編輯                           |
| `a`  | 在游標所在位置的下一個字元開始編輯           |
| `o`  | 在目前所在行數的**下方**新增一行後，開始編輯 |
| `O`  | 在目前所在行數的**上方**新增一行後，開始編輯 |

從**編輯模式**回到**一般模式**，可以按 [ESC]

### 六、注意事項

啟動 `vi` 時，會在文件位置產生一個暫時性的交換檔，該交換檔會在 `vi` 關閉時自己刪掉。所以如果關閉方式不正確，那該交換檔就會被留下來，重新再該檔案的時候可能會出現警告訊息。
