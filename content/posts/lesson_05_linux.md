---
title: "Lesson 05: Linux 筆記 - Log Server, MariaDB, nmap"
date: 2023-04-15T09:10:47+08:00
author: "celine"
tags: ["linux", "security"]
categories: ["StudyNote"]
---

> Log Server 實作
>
> MariaDB 權限與加密, nmap 檢測工具介紹

User ID Report
```bash=
aureport -u -i --failed | less
```
---

### 1.Write message to syslog

BIOS/UEF ➡ grub2 ➡ kernel ➡ systemd ➡ journalctl

+ `rsyslog` : 如果選 minimal installation，err不會由rsyslog接，改由`journalctl`接error接收

#### 將訊息寫入系統紀錄中
@Server
```bash
root# logger hello World
```
```bash
root# tail /var/log/messages
```
```bash
root# journalctl
```
```bash
root# journalctl --since "2023-04-15"
```
```bash
root# reboot
```

#### 將`journalctl`紀錄持久化

> 推書時間: 
> 實戰Linux系統數位鑑識 (江湖海) 
> (Practical Linux Forensics: A guide for digital investigators, by Bruce Nikkel)


```bash
root# journalctl
```
> 尋找 "hello World"，會發現找不到

```bash
root# grep hello /var/log/messages
```
> 找得到 hello World

```bash
root# mkdir /var/log/journal
```
```bash
root# logger hello world
```
```bash
root# journalctl
```
```bash
root# reboot
```
```bash
root# journalctl
```
> 找出"hello World"

#### 自訂記錄等級與標籤
@Server
在紀錄中設定`class`Tag
```bash
root# logger -t class "hello myapp log"
root# logger -t class -p 'user.err' "[Error] hello myapp log"
```
```bash=
root# grep class /var/log/messages
root# journalctl -t class
```
在紀錄中，設定嚴重等級
```bash!
root# logger -t class -p 'user.info' "Test log messages"
root# grep class /var/log/messages
```
使用`journalctl`找出tag為`class`的記錄
```bash
root# journalctl -t class
```
> (screen1)

使用 `journalctl` 找出tag為`class` 且嚴重性為'err'的記錄
```bash
root# journalctl -t class -p err
```
> (screen2)

---
### 2. 建立簡單 Rsyslog Server

安裝 rsyslog 套件

@Desktop, @Server
```bash
root# dnf install -y rsyslog
```

@Desktop
```bash
root# cd /etc/rsyslog.d
```

設定接收 `rsyslog` 資訊
```bash
root# vi server.conf
```
```bash=
module(load="imudp") # needs to be done just once
input(type="imudp" port="514")

module(load="imtcp") # needs to be done just once
input(type="imtcp" 
port="514")
```
```bash
root# systemctl restart rsyslog
```
```bash
root# firewall-cmd --permanent --add-port=514/tcp
root# firewall-cmd --permanent --add-port=514/udp

root# firewall-cmd --reload
```

@Server
設定 sysrlog client
```bash
root# cd /etc/rsyslog.d
```

設定將 log 使用 TCP 協定傳送到 Desktop
```bash!
root# vi client.conf
```
```bash=
*.* @@172.16.1.X:514
```
> (screenshot3)
```bash
root# systemctl restart rsyslog
```
```bash
root# logger "Test message from Server"
```

@Desktop, @Server
```bash
root# less /var/log/messages
Apr 15 14:19:08 serverb root[5228]: Test message from Server
```
> ##### `sudo su -/-i` 跟 `sudo su` 的差別: 
> 少了減號就不會登入(真正變成)那個user
> DigitalOcean: [How To Use Journalctl to View and Manipulate Systemd Logs](https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs)
> NXLog

---
### 3. Syslog 應用

#### 建主 syslog Server
@Desktop
```bash
root# dnf install -y rsyslog
```
```bash
root# cd /etc/rsyslog.d/
```
```bash
root# vi server.conf
```
```bash=
module(load="imudp") # needs to be done just once
input(type="imudp" port="514")

module(load="imtcp") # needs to be done just once
input(type="imtcp" port="514")

$template RemoteLogs,"/var/log/%FROMHOST-IP%/%HOSTNAME%-%$NOW%/%PROGRAMNAME%.log" 
*.* ?RemoteLogs
```

> Ref: https://www.rsyslog.com/doc/master/configuration/properties.html

```bash
root# firewall-cmd --permanent --add-port=514/tcp
root# firewall-cmd --permanent --add-port=514/udp

root# firewall-cmd --reload
```
```bash
root# systemctl restart rsyslog
```
#### 設定用戶端 Client
@Server
```bash
root# dnf install -y rsyslog
```
```bash
root# cd /etc/rsyslog.d/
```
```bash
root# vi client.conf
```
```vim=
#*.* @172.16.1.58:514 # Use @ for UDP protocol 
*.* @@172.16.1.58:514 # Use @@ for TCP protocol

& ~
```
```bash
root# systemctl restart rsyslog
```
#### 測試
@Server
```bash
root# logger -t linuxclass "test message from root"
```
```bash
student$ logger -t linuxclass "test message from server"
```
@Desktop
```bash
root# ls -lR /var/log/172.16.2.58/
```

#### 讀取指定檔案內容
@Server
```bash
root# cd /etc/rsyslog.d/
```
設定 log 檔案，位於 `/tmp/applog.txt`
```bash!
root# touch /tmp/applog.txt
```
```bash!
root# vi 01_watch_log.conf
```
```
module(load="imfile")

input(type="imfile"
      File="/tmp/applog.txt"
      Tag="mytag:"
      Facility="local0")
```
取消 STOP
```bash
root# vi client.conf
```
```bash=
#*.* @172.16.1.X:514 # Use @ for UDP protocol 
*.* @@172.16.1.X:514 # Use @@ for TCP protocol
```
測試寫入檔案到 log
```bash
root# echo Hello 1 >> /tmp/applog.txt
root# echo Hello 2 >> /tmp/applog.txt
root# echo Hello 3 >> /tmp/applog.txt
root# echo Hello 4 >> /tmp/applog.txt
```
查看 Log 是否寫入
```bash
root# ls -lR /var/log/172.16.2.58/
```
##### 💤允許特定網段/IP通過
```bash
sudo firewall-cmd --add-rich-rule='rule family="ipv4" service name="ssh" source address="10.10.1.125" accept'
```
##### firewall-cmd 的小史💥
```
           [firewalld]
                 ↓
         [firewall-cmd]
                ↙↘
          (EL7)    (EL8/9)
          ↙          ↘
      [iptables]   [nftables]
          ↘          ↙
        [kernel+notfilter]
```

---
### 4. MariaDB 帳號管理

> MariaDB/MySQL 資料庫實體檔位置 ➡ `/var/lib/mysql/`

@Desktop

安裝資料庫
```bash
root# dnf install -y chrony
root# systemctl enable --now chronyd
root# timedatectl set-timezone Asia/Taipei

root# dnf install -y mariadb-server
root# systemctl enable --now mariadb

root# firewall-cmd --permanent --add-service=mysql
root# firewall-cmd --reload
```
```bash
root# mysql_secure_installation
```
> 密碼設為 `rocky`

連線資料庫
```bash
root# mysql -u root -p -h localhost
```

建立資料庫
```bash
MariaDB> create database classdb;
```

建立使用者
```
MariaDB> GRANT ALL ON classdb.* TO 'dbuser'@'172.16.2.58' IDENTIFIED BY 'rocky' REQUIRE SSL;
MariaDB> GRANT ALL ON classdb.* TO 'user1'@'172.16.2.58' IDENTIFIED BY 'rocky';
```
列出帳號與資庫資訊
```
MariaDB> SELECT User, Db, Host from mysql.db;
+--------+---------+---------------+
| User   | Db      | Host          |
+--------+---------+---------------+
| dbuser | classdb | 172.16.2.58  |
+--------+---------+---------------+
1 row in set (0.004 sec)
```
列出使用者權限
```
show grants for 'dbuser'@'172.16.2.58';
+-------------------------------------------------------------------------------------------------------------------------------+
| Grants for dbuser@172.16.2.58                                                                                               |
+-------------------------------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `dbuser`@`172.16.2.58` IDENTIFIED BY PASSWORD '*128977E278358FF80A246B5046F51043A2B1FCED' REQUIRE SSL  |
| GRANT ALL PRIVILEGES ON `classdb`.* TO `dbuser`@`172.16.2.58`                                                                |
+-------------------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.002 sec)
```


@Server
```bash
root# dnf install -y mariadb
```
```bash
student$ mysql -u dbuser -p -h desktop58
FAIL!
```
```bash
student$ mysql -u user1 -p -h desktop58
```

@Desktop

產生稽核語法
```bash
student$ mysql -u root -p -h localhost
```

```
MariaDB> select distinct concat('SHOW GRANTS FOR ', QUOTE(user), '@', QUOTE(host), ';') as query from mysql.user;
+-------------------------------------------+
| query                                     |
+-------------------------------------------+
| SHOW GRANTS FOR 'root'@'127.0.0.1';       |
| SHOW GRANTS FOR 'dbuser'@'172.16.2.58';    |
| SHOW GRANTS FOR 'root'@'::1';             |
| SHOW GRANTS FOR 'root'@'localhost';       |
+-------------------------------------------+
4 rows in set (0.001 sec)
```


---
### 5. MariaDB packet interception

#### 從 Server 查看封包
##### Server
@Terminal-1
```bash
root# dnf install -y tcpdump
```
```bash
root# tcpdump host 172.16.1.58 and port 3306 -X
```
##### Desktop
```bash
root# dnf install -y tcpdump
root# tcpdump port 3306 -X
```

#### 連線資料庫
@Server
@Terminal-2
```bash
root# dnf install -y mariadb
```
@Terminal-2
```bash
root# mysql -u user1 -h 172.16.1.58 -p
MariaDB> show databases;

MariaDB> use information_schema;
MariaDB> show tables;
```
@Desktop

查看 `tcpdump` 結果

---
### 6. MariaDB SSL連線加密

@Desktop
```bash
root# mkdir mariadb-ssl; cd mariadb-ssl
root# mkdir ca server client
```
#### Build CA

建立 RootCA
```bash
root# cd ca
root# ls -lh

root# openssl genrsa 4096 > ca.key
root# openssl req -new -x509 -nodes -days 365000 -key ca.key -out ca.crt

root# root# ls -lh
```
> Common Name 設為 `DB CA`

#### Build server certificate

建立 Server 用憑證

```bash
root# openssl req -newkey rsa:2048 -days 3650 -nodes -keyout server.key -out server.csr
root# openssl rsa -in server.key -out server.key
```
> Common Name 設為 `DB Server`

簽署憑證

```bash
root# openssl x509 -req -in server.csr -days 3650 -CA ca.crt -CAkey ca.key -set_serial 01 -out server.crt
```

#### Build client certificate

建立用戶端憑證
```bash
root# openssl req -newkey rsa:2048 -days 3650 -nodes -keyout client.key -out client.csr
root# openssl rsa -in client.key -out client.key
```
> Common Name 設為 `DB Client`

簽署憑證
```
root# openssl x509 -req -in client.csr -days 3650 -CA ca.crt -CAkey ca.key -set_serial 01 -out client.crt
```
#### 驗證
將簽好的憑證複製到相關目錄
```bash
root# openssl verify -CAfile ca.crt server.crt client.crt
```
```bash
root# cp ca.crt ../server
root# cp ca.crt ../client

root# mv server* ../server
root# mv client* ../client
```

#### 設定 MariaDB 憑證
@Desktop
```bash
root# cd /root/mariadb-ssl

root# cp -R server /etc/my.cnf.d/ssl/
root# chown -R mysql /etc/my.cnf.d/ssl/
```
套用 Server 憑證
```bash
root# cat > /etc/my.cnf.d/50-server.cnf <<EOF
[mysqld]
ssl-ca=/etc/my.cnf.d/ssl/ca.crt
ssl-cert=/etc/my.cnf.d/ssl/server.crt
ssl-key=/etc/my.cnf.d/ssl/server.key
EOF
```
重新啟動 MariaDB
```bash
root# systemctl restart mariadb
```
檢查憑證套用成功
```bash
root# mysql -u root -p -h localhost

MariaDB> show variables like '%ssl%';
+---------------------+----------------------------------+
| Variable_name       | Value                            |
+---------------------+----------------------------------+
| have_openssl        | YES                              |
| have_ssl            | YES                              |
| ssl_ca              | /etc/my.cnf.d/ssl/ca.crt         |
| ssl_capath          |                                  |
| ssl_cert            | /etc/my.cnf.d/ssl/server.crt     |
| ssl_cipher          |                                  |
| ssl_crl             |                                  |
| ssl_crlpath         |                                  |
| ssl_key             | /etc/my.cnf.d/ssl/server.key     |
| version_ssl_library | OpenSSL 1.1.1g FIPS  21 Apr 2020 |
+---------------------+----------------------------------+
10 rows in set (0.003 sec)

MariaDB> create database classdb;

MariaDB> GRANT ALL ON classdb.* TO 'dbuser'@'172.16.2.X' IDENTIFIED BY 'rocky' REQUIRE SSL;
MariaDB> GRANT ALL ON classdb.* TO 'dbuser2'@'172.16.2.X' IDENTIFIED BY 'rocky';

MariaDB> exit;
```
#### Client 端連線

@Server
```bash
root# mkdir ~/client/
```
@Desktop
```bash
root# cd /root/mariadb-ssl/client

root# echo "PUT * " | sftp root@server58:/root/client
```
@Server
```bash
root# cd
root# ls -lh client
```
```bash
root# dnf install -y chrony
root# systemctl enable --now chronyd
root# timedatectl set-timezone Asia/Taipei
```
```bash
root# cp -R client /etc/my.cnf.d/ssl/
```
```bash
root# vi /etc/my.cnf.d/client.cnf
```
```=
在 [client] 區段中新增

...
[client]
ssl-ca=/etc/my.cnf.d/ssl/ca.crt
ssl-cert=/etc/my.cnf.d/ssl/client.crt
ssl-key=/etc/my.cnf.d/ssl/client.key
```
```
root# mysql -u dbuser -h 172.16.1.X -p 

MariaDB> status;
--------------
mysql  Ver 15.1 Distrib 10.3.28-MariaDB, for Linux (x86_64) using readline 5.1

Connection id:		14
Current database:	
Current user:		dbuser@172.16.62.105
SSL:			Cipher in use is TLS_AES_256_GCM_SHA384
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server:			MariaDB
Server version:		10.3.28-MariaDB MariaDB Server
Protocol version:	10
Connection:		172.16.62.5 via TCP/IP
Server characterset:	latin1
Db     characterset:	latin1
Client characterset:	utf8
Conn.  characterset:	utf8
TCP port:		3306
Uptime:			24 min 23 sec

Threads: 6  Questions: 18  Slow queries: 0  Opens: 18  Flush tables: 1  Open tables: 11  Queries per second avg: 0.012
--------------
```
#### 連線時指定憑證位置
```bash
root# vi /etc/my.cnf.d/client.cnf
```
```=
在 [client] 區段中移除

...
[client]
#ssl-ca=/etc/my.cnf.d/ssl/ca.crt
#ssl-cert=/etc/my.cnf.d/ssl/client.crt
#ssl-key=/etc/my.cnf.d/ssl/client.key
```
```bash
root# cd client
```
```bash
root# mysql -u dbuser -h 172.16.1.58 -p --ssl-ca=ca.crt --ssl-cert=client.crt --ssl-key=client.key
```

參考 [5.MariaDB 封包截取](#5.-MariaDB-packet-interception) 驗證加密後結果。

---
### 7. MariaDB 密碼原則

#### 參考文件
[Simple Password Check Plugin](https://mariadb.com/kb/en/simple-password-check-plugin/)

#### Desktop
##### 啟用模組
修改 /etc/my.cnf.d/mariadb-server.cnf，在 [mysqld] 區段中，填入
```
plugin_load_add = simple_password_check
```
> (screen_shot)

##### 查看規則
@Desktop
```bash
root# mysql -u root -p -h localhost
```

```bash
MariaDB [(none)]> show variables like '%simple_password%';
+-----------------------------------------+-------+
| Variable_name                           | Value |
+-----------------------------------------+-------+
| simple_password_check_digits            | 1     |
| simple_password_check_letters_same_case | 1     |
| simple_password_check_minimal_length    | 8     |
| simple_password_check_other_characters  | 1     |
+-----------------------------------------+-------+
4 rows in set (0.001 sec)
```

##### 驗證規則
```bash
MariaDB [(none)]> select user,host from mysql.user;
+---------+-------------+
| user    | host        |
+---------+-------------+
| root    | 127.0.0.1   |
| dbuser  | 172.16.2.X  |
| dbuser2 | 172.16.2.X  |
| user1   | 172.16.2.X  |
| root    | ::1         |
| root    | localhost   |
+---------+-------------+
6 rows in set (0.001 sec)

MariaDB [(none)]> SET PASSWORD FOR 'user1'@'172.16.2.X' = PASSWORD('abc');
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
MariaDB [(none)]> SET PASSWORD FOR 'user1'@'172.16.2.X' = PASSWORD('rocky');
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
```

---
### 8. MariaDB 稽核

#### 參考文件
> [MariaDB Audit Plugin](https://mariadb.com/kb/en/mariadb-audit-plugin/)

#### 啟用稽核模組與設定
@Desktop
```bash!
root# cp -a /etc/my.cnf.d/mariadb-server.cnf{,_backup}
```
```bash!
root# vi /etc/my.cnf.d/mariadb-server.cnf
```
```=
在 mysqld 區段加入
...

plugin_load_add = server_audit
server_audit_logging = ON
server_audit=FORCE_PLUS_PERMANENT
server_audit_events=connect,query
server_audit_file_path=/var/lib/mysql/server_audit.log
```
```bash
root# systemctl restart mariadb
```

#### 測試
@Server
```bash
root# cd ~/client
root# mysql -u dbuser -h 172.16.1.38 -p --ssl-ca=ca.crt --ssl-cert=client.crt --ssl-key=client.key
```
@Desktop
```bash
root# /var/lib/mysql/server_audit.log
```

---
### 9. Port 掃描
1. 掃描 TCP
    ```
    root# nmap -p- server58
    ```
2. 掃描 UDP
    ```
    root# nmap -sU -p- server58
    ```
    > 這個掃描會很久

3. 猜測作業系統版本
    ```
    root# nmap -O server58
    ```
    > (...Nmap scan report..)

4. 掃描 server58 中開了哪些 1024 以下的 TCP Port，並列出作業系統
    ```bash
    root# nmap -p0-1024 -O server58
    ```
    
> ### Teleport服務_跳板機
> Teleport(https://goteleport.com)
