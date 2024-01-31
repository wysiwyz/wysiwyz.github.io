---
title: "Lesson 02: Linux 筆記 - UGO, ACL, Samba, NFS, SFTP"
date: 2023-03-17T16:44:11+08:00
author: "celine"
tags: ["linux", "security"]
categories: ["StudyNote"]
---

> 請假六小時，來補課arrrhh

### 何謂索引式檔案系統

+ 沒有索引的時候，磁碟越大要掃描越久
+ 自從有了索引，大大增加了檔案讀取的效率

### 日誌系統

+ 確保檔案操作完成
+ 減少檔案操作中斷的異常問題（寫入到一半停電之類的）
+ 目前流行的檔案系統都有支援日誌功能
  + ext3/ext4
  + XFS
+ 可以減少檔案損毀發生機率
+ 參考書
  + 大師養成起手式：從核心真正了解Linux運作原理
  + 圖解Linux核心工作原理（五星推推⭐<sup>5</sup>）

### 掛載項目

+ 在上層權限缺失的情況下，可以由mount point層面做最後一層限制
+ man fstab
+ Options:
  + noexec  
    + 檔案掛載時，要限制不能執行使用者放置的可執行檔
  + nosuid
  + nodev（目前比較少用）

> Linux如果是作為log server，可能會有放置檔案上限問題

### UGO 

+ User Group Other

  ![image-20230317115143678](https://i.imgur.com/Qn6zkb2.png)

  

+ 由三組位元所組成（八進制）

  + `l` for link, `d` for directory, 
  + `-`一般檔案(執行檔也是), `c`或`d`裝置檔

  ```
  drwxr-xr-x,
  dr-xr-x---,
  ```
  
+ 數字:

  | ❔    | r    | w    | x    | -    |
  | ---- | ---- | ---- | ---- | ---- |
  | 數值 | 4    | 2    | 1    | 0    |

  > ⚠️`666`& `777` 注意
  >
  > 允許每個人都能讀寫檔案或目錄，可能會造成file tempering(檔案篡改)

+ [SUID & SGID & SBIT](https://dywang.csie.cyut.edu.tw/dywang/linuxsecurity/node39.html)



### 檔案系統 特殊權限

#### 1. SetUID

##### nosuid

在根目錄建立一個secure folder，設定成777，查看`/secure/`目錄的權限，

接著修改為可讀寫屬性，清空裡面檔案

```bash
root# cd /secure/
root# chmod 777 /secure/  
root# ls -ld /secure/
drwxrwxrwx. 2 root root 6 {Date} {Time} /secure/
root# mount -o remount /dev/sdb /secure
root# rm -rf *
```

複製 `/usr/bin/cp` 檔案並重新命名為`cp-lab`

```bash
root# cd /secure
root# cp /usr/bin/cp ./cp-lab
root# ls -ls
-rwxr-xr-x. 1 root 149K {Date} {Time} cp-lab
```

切成 user2 使用者，複製`/etc/passwd`到 sample1 裡面，觀察owner是誰

```bash
root# su - user2

user2# cd /secure
user2# ./cp-lab /etc/passwd ./sample1

user2# ls -lh
sample1 owner is user2

user2# exit
```

> #### 寫`hello.sh`
>
> ```bash
> vi hello.sh
> ```
>
> ```
> #!/bin/bash
> 
> export LANG=C
> 
> echo Hello
> ```
>
> 完成 hello.sh 編輯
>
> ```bash
> $ cat hello.sh
> ```
>
> 查看內文
>
> ```bash
> $ ls -lh hello.sh
> -rw-r--r--  
> ```
>
> 發現是不可執行檔
>
> ```bash
> $ ./hello.sh
> -bash: ./hello.sh: Permission denied
> $ chmod +x hello.sh
> ```
>
> Voilaa! 現在可執行了
>
> ```bash
> $ ./hello.sh
> Hello
> $ ls -lh hello.sh
> -rwxr-xr-x. 1 root root 40 {Date} {Time} hello.sh
> $ mount -o remount,noexec /dev/sdb /secure2
> $ mount | grep sdb
> /dev/sdb on /secure2 type xfs {rw, noexec, ...}
> $ ./hello.sh
> -bash: ./hello.sh: Permission denied
> $ sh hello.sh
> Hello
> $ which sh
> /usr/bin/sh
> ```

lsblk {directory/file} 列出快設備訊息

blkid {directory/file} 查找塊設備詳情 (可以看UUID)

高頻用法: 備份檔案 `cp -a {file_name} {file_name}_backup`

`mount -a` 的時候畫面通常不會有其他訊息

# [🥝🥝🥝🥝補課時間戳記](https://www.youtube.com/watch?v=sKIUs2_rMhw&list=PLi8AVVmcS1cINASR5MbKKbrDSHrGkat0s&index=31)

#### 2. SetGID

```bash
root# cd /secure/
root# rm -rf *

root# groupadd lab
```







