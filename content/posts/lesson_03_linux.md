---
title: "Lesson 03: Linux 筆記 - 磁碟加密、Apache, Nginx, SSL"
date: 2023-03-18T09:30:38+08:00
author: "celine"
tags: ["linux", "security"]
categories: ["StudyNote"]
---

### LUKS 加密服務

#### 適用對象

+ 移動式設備資料保護

+ notebook, USB磁碟加密

+ 提供32組插槽使用

  > 把partition切成小塊加密，裡面放密碼、key_file
  >
  > 可以使用設定的密碼解密或者用key解密

#### 注意事項

+ LUKS並不對檔案本身進行加密(針對整個partition,整個D槽)
+ LUKS對於單一磁碟取走後能夠起保護作用
+ KUKS對於已經解密之磁碟內容不起保護作用

#### 指令

+ `cryptsetup`
  + `luksFormat` 建立加密磁碟
  + `luksOpen` 開啟加密磁碟
  + `close` 關閉已經開啟的加密磁碟

> luks - 解密
>
> mount - 掛起來
>
> umount - 卸載 
>
> ​    (順便同步, 如果未正確卸載可能會造成檔案毀損)

```bash
lsblk /dev/sdb
cryptsetup luksFormat /dev/sdb
cryptsetup luksOpen /dev/sdb secDisk
mkfs.xfs /dev/mapper/secDisk
mount /dev/mapper/secDisk /mnt
touch /mnt/myfile
umount /mnt
cryptsetup close secDisk
```

#### Dump/Status

+ more cmds

  

#### 使用插槽



#### 刪除插槽



> memcached的使用
>
> [Luks資料加密架構與資料救援方法](osslab.tv/research-luks/)
>
> [Linux檔案加密_GPG_GnuPG](https://www.redhat.com/sysadmin/getting-started-gpg)
>
> [LUKS加密裝置](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/security_hardening/encrypting-block-devices-using-luks_security-hardening)
>
> 一定要備分 一定要備份 一定要備份

---

## 磁碟加密

```bash
sudo su -
ssh student@server58
id
sudo su -
```

+ 安裝 cryptsetup

+ 將 /dev/sdb 設定加密磁碟區

  ```bash
  cryptsetup luksFormat /dev/sdb
  ```

+ 查看磁碟資訊

  ![image-20230318105936415](https://i.imgur.com/Z2HoDuB.png)

  

+ 啟用加密磁碟，將`/dev/sdb`掛載為secDisk

  ![image-20230318110421399](https://i.imgur.com/TCjsTrk.png)

  

+ 格式化

  > mkfs.xfs {the_Disk_to_be_formatted}

  ```bash
  blkid /dev/mapper/secDisk
  mkfs.xfs /dev/mapper/secDisk
  blkid /dev/mapper/secDisk
  ```

  ![image-20230318111139632](https://i.imgur.com/AHP6NKa.png)

  

+ 掛載使用

  > `mnt`被設計來臨時掛載使用

  ```bash
  mount /dev/mapper/secDisk /mnt
  df -h /mnt
  cd /mnt/
  ls
  touch a b c d e f
  ls -lh
  cd
  ```

  ![image-20230318111613052](https://i.imgur.com/5u6hL0O.png)

  

  ![image-20230318111710595](https://i.imgur.com/un7l5F6.png)

  

+ 卸載 `/mnt`

  > 要先 `cd`跳出目錄才能 `umount`，否則會有以下錯誤訊息
  >
  > ```
  > umount: /mnt: target is busy.
  > ```

  ```bash
  umount /mnt/
  ls -lh /dev/mapper/
  ```

  ![image-20230318111858395](https://i.imgur.com/ifEy9L5.png)

  

+ 關閉加密磁碟

  ```bash
  cryptsetup luksClose secDisk
  ls -lh /dev/mapper
  ```

  ![image-20230318112403700](https://i.imgur.com/kDskBQG.png)

   

#### 新增 Password 到插槽

+ 查看現有的插槽

  > `luksDump` 查看目前存了啥密碼

  ```bash
  cryptsetup luksDump /dev/sdb
  cryptsetup luksDump /dev/sdb | grep luks2
   
  ```

+ 新增密碼

  ```bash
  cryptsetup luksAddKey /dev/sdb
  // 輸入之前存的密碼
  // 建立新密碼(passphrase for key slot)
  // 重新輸入新密碼(verify passphrase)
  ```

  ![image-20230318113349477](C:\Users\tpi\AppData\Roaming\Typora\typora-user-images\image-20230318113349477.png)

  ```bash
  cryptsetup luksDump /dev/sdb | grep luks2
  ```

+  

+  

+  



#### 新增 Key 到插槽

+ 使用密碼把 key1 插到插槽

  ```bash
  dd if=/dev/urandom of=key1 bs=1K count=64
  cryptsetup luksAddKey /dev/sdb key1
  (這裡輸入第一個or第二個passwd都可,)
  ```

  ![image-20230318113740963](https://i.imgur.com/GfaZ525.png)

+ 查看使用的插槽數

  ![image-20230318114149051](https://i.imgur.com/LWHxLKZ.png)

  

+ 使用 key1 把 key2 插到插槽

  ![image-20230318114448911](https://i.imgur.com/T3QGasX.png)

  

#### 刪除插槽

+ 使用 key1 刪除插槽3 (key2)

  ![image-20230318114947146](https://i.imgur.com/pEy9TWI.png)

>  `man cryptsetup`
>
> man + {xxxxxx} 就是個 user manual 可以查看文件



---

#### Image 加密

@Server

Disk Image Cryption

![image-20230318131241693](https://i.imgur.com/tDcBmV4.png)

 

![image-20230318131441744](https://i.imgur.com/i9hxFyx.png)

![image-20230318131705159](https://i.imgur.com/QZmlV7Q.png)

複製 Image 到 Desktop

[Server]      [Desktop]

![image-20230318132306522](https://i.imgur.com/alExjRK.png)

![image-20230318132602606](https://i.imgur.com/CF7RD4L.png)

![image-20230318133421392](https://i.imgur.com/tEnLGN5.png)

> `c100`、`s100` 沒什麼意義，只是一個名稱(block device)

![image-20230318134446942](https://i.imgur.com/9Rfhe7v.png)

---

## 開機自動掛載加密磁碟

> key 為什麼要放在 /etc/ 目錄裡？為了避免非super用戶篡改資料。

@Server

> `;`如果前面的指令執行完，接著執行下個指令

> cp -a 的-a指的是包含權限

![image-20230318140133033](https://i.imgur.com/cOUHbGG.png)



![image-20230318140013366](https://i.imgur.com/SeflM1E.png)



備份開機掛載設定檔

```
貼到 /etc/fstab 最尾巴新檔
...
(預設的)
/dev/mapper/secDisk /sec xfs defaults 0 0

(包含之前設定內容)
/dev/mapper/secDisk /sec xfs _netdev,nofail,defaults,x-systemd.device-timeout= 1 0 0
```

> 重開機之後要把硬碟掛在 sec 目錄
>
> 檔案格式為 xfs
>
> ![image-20230318140957907](https://i.imgur.com/Ph5zMsS.png)

關機前測試

```bash
df -h /sec
mount -a
df -h /sec
reboot
df -h /sec/
```



![image-20230318142322693](https://i.imgur.com/FiGXHTv.png)

### 取消自動掛載

@Server

```
cat /etc/fstab_backup > /etc/fstab
reboot
```

---

## Apache安裝





---

### Apache + SSL(secure socket layer)

##### HTTPS準備

1. Server產生**憑證請求檔**
2. 向 SSL_x509的CA認證中心申請**簽證**，裝在Server上
3. 憑證檔案
   + SSL x.509 憑證格式
     + 常用`.DER`格式，Java使用的是`.JKS`(二進制)

##### Apache設定檔案

+ /etc/httpd/conf.d/ssl.conf

##### Nginx設定檔案

+ /etc/nginx/conf.d/default.conf



SSL1.0 -> SSL2.0 -> SSL3.0 -> 

TLS 1.0 -> TLS 1.1 -> TLS 1.2(金融政府最低限制) -> TLS1.3

##### SSL連線四個步驟



![image-20230318153244671](https://i.imgur.com/DGtVV33.png)

##### 設定ssl使用憑證位置

![image-20230318153829980](https://i.imgur.com/h7Sjq4W.png)

##### 設定防火牆

```bash
root# firewall-cmd --permanent --add-service=https
root# firewall-cmd --reload
root# firewall-cmd --list-all
```

##### 重新啟動 httpd 服務

```bash
systemctl restart httpd
```

![image-20230318154218959](https://i.imgur.com/6Rx0IhT.png)

> SSL建議簽署時間為一年
>
> ![image-20230318154454326](https://i.imgur.com/eHKe87S.png)

##### 新增信任的根憑證

@Client [root]

+ 將CA加入作業系統信任憑證中

  ```bash
  cd /etc/pki/ca-trust/source/anchors/
  curl -OJL http://192.168.60.253/ssl-key/ca/ca.crt
  ```

+ 重新載入信任憑證

  ```bash
  update-ca-trust
  ```

+ 用戶端再次確認

  ![image-20230318155639877](https://i.imgur.com/St4iLHQ.png)

> [PKI架構、CA、中間人攻擊](https://www.informationsecurity.com.tw/article/article_detail.aspx?aid=7327)

---

### Nginx 安裝

#### 環境清除

@Server (停止服務、移除相依)

```bash
root# systemctl stop httpd
root# dnf remove httpd
```

#### 服務設定

安裝套件

```bash
root# dnf install -y nginx
```

設定首頁，啟動Nginx

```bash
root# cd /usr/share/nginx/html/
root# echo Hello server58 in nginx > index.html
root# systemctl enable --now nginx
```

使用 curl 或 firefox 檢驗服務

```bash
student $ curl http://server58.linux.class
```

![image-20230318162624478](https://i.imgur.com/Fx5dOHJ.png)

#### ![image-20230318162521199](https://i.imgur.com/aqQWbeT.png)

---

### Nginx + SSL

1. 建立憑證目錄
2. 下載憑證
3. 設定 https 預設站台 `cd /etc/nginx/conf.d/`
4. 新增設定檔案 `vi default.conf`
5. 測試設定檔案 `nginx -t`
6. 重啟服務 `systemctl restart nginx`

![image-20230318163407424](https://i.imgur.com/wfK9Rrz.png)

![image-20230318163258070](https://i.imgur.com/e8dMSbg.png)

#### 用戶端測試

1. curl 

   ![image-20230318163512524](https://i.imgur.com/32JYjH4.png)

2. Firefox 

---

### Strict Transport Security (HSTS)

@Desktop

![image-20230318163954322](https://i.imgur.com/vxx2Yra.png)



@Server

在nginx增加header輸出

```bash
vi /etc/nginx/conf.d/default.conf
```

line 14 add

```text
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
```

```bash
root# nginx -t
root# systemctl reload nginx
```

![image-20230318164142207](https://i.imgur.com/AnW558P.png)

@Desktop

檢查是否支援 HSTS

![image-20230318164237949](https://i.imgur.com/8EkMHob.png)

#### Apache 設定 HSTS

@Server

1. 重裝 Apache

   ```bash
   root# systemctl stop nginx; dnf remove -y nginx
   root# dnf install -y httpd mod_ssl;
   ```

2. 新增設定

   ```bash
   root# cat /etc/httpd/conf.d/ssl.conf.rpmsave > /etc/httpd/conf.d/ssl.conf
   ```

![image-20230318165828265](https://i.imgur.com/0vSZBIL.png)

###### vi /etc/httpd/conf.d/ssl.conf

![image-20230318165554752](https://i.imgur.com/yhCCbGA.png)

驗證

@Desktop

![image-20230318170010681](https://i.imgur.com/1LpiLQv.png)



#### SSL Report 

Qualys

![image-20230318170154800](https://i.imgur.com/Nr7FTae.png)
