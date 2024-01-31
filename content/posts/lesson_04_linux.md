---
title: "Lesson 04: Linux 筆記 - CA憑證、SELinux"
date: 2023-04-08T09:15:16+08:00
author: "celine"
tags: ["linux", "security"]
categories: ["StudyNote"]
---

### Strict Transport Security (HSTS)

##### Apache

##### Nginx

---

### HTTPS 加密流程

+ Client 連到 Server 時，Server 將 Public key 傳給 Client
+ Client 使用 Public key 產生隨機碼，和 Server 建立加密通道。
  + Client 用 Public key 進行傳輸資料加解密，Server 用 Private key 進行傳輸資料加解密
+ 實際的資料使用對稱式加密方式，透過已經建好的加密通道進行傳輸
+ https 使用了非對稱式與對稱式加密

---

### Khoor, Olqxa

> 怎麼在廣大的領域中發現小小的弱點，突破當前使用之演算法的弱點 -- Dr. Mathematics

凱薩加密演算法

+ 字母位移預設三個字

@Desktop

```bash
root# mkdir demo; cd demo/
root# vi caesar-cipher.sh
```

shellscript

```vim
#!/bin/bash

# 宣告加密和解密時使用的位移表
declare -A shift_table=(
  ["a"]="d"
  ["b"]="e"
  ["c"]="f"
  ["d"]="g"
  ["e"]="h"
  ["f"]="i"
  ["g"]="j"
  ["h"]="k"
  ["i"]="l"
  ["j"]="m"
  ["k"]="n"
  ["l"]="o"
  ["m"]="p"
  ["n"]="q"
  ["o"]="r"
  ["p"]="s"
  ["q"]="t"
  ["r"]="u"
  ["s"]="v"
  ["t"]="w"
  ["u"]="x"
  ["v"]="y"
  ["w"]="z"
  ["x"]="a"
  ["y"]="b"
  ["z"]="c"
  ["A"]="D"
  ["B"]="E"
  ["C"]="F"
  ["D"]="G"
  ["E"]="H"
  ["F"]="I"
  ["G"]="J"
  ["H"]="K"
  ["I"]="L"
  ["J"]="M"
  ["K"]="N"
  ["L"]="O"
  ["M"]="P"
  ["N"]="Q"
  ["O"]="R"
  ["P"]="S"
  ["Q"]="T"
  ["R"]="U"
  ["S"]="V"
  ["T"]="W"
  ["U"]="X"
  ["V"]="Y"
  ["W"]="Z"
  ["X"]="A"
  ["Y"]="B"
  ["Z"]="C"
)

# 宣告加密和解密的函數
encrypt() {
  encrypted=""
  for (( i=0; i<${#1}; i++ )); do
    char="${1:$i:1}"
    if [[ $char =~ [a-zA-Z] ]]; then
      shifted_char="${shift_table[$char]}"
      encrypted="$encrypted$shifted_char"
    else
      encrypted="$encrypted$char"
    fi
  done
  echo "$encrypted"
}

decrypt() {
  decrypted=""
  for (( i=0; i<${#1}; i++ )); do
    char="${1:$i:1}"
    if [[ $char =~ [a-zA-Z] ]]; then
      for shifted_char in "${!shift_table[@]}"; do
        if [[ ${shift_table[$shifted_char]} == "$char" ]]; then
          decrypted="$decrypted$shifted_char"
          break
        fi
      done
    else
      decrypted="$decrypted$char"
    fi
  done
  echo "$decrypted"
}

# 判斷是要加密還是解密
if [[ $# -lt 2 ]]; then
  echo "Usage: $0 (encrypt|decrypt) text"
  exit 1
fi

if [[ "$1" == "encrypt" ]]; then
  encrypt "${2}"
elif [[ "$1" == "decrypt" ]]; then
  decrypt "${2}"
else
  echo "Usage: $0 (encrypt|decrypt) text"
  exit 1
fi
```

chmod

```bash
root# chmod +x caesar-cipher.sh
```

加密字串

```bash
root# ./caesar-cipher.sh decrypt "Khoor, Olqxa"
```



---

### 演算法與選擇

+ 大部分演算法為數學公式的集合
+ 世界上沒有永遠破解不了的演算法，主要在於時間與算力
+ 加密使用的演算法，應在不同目的選擇合適而非嚴謹的方法

### 弱演算法應完全不用?

+ 已經被證實容易被破解的演算法，而非不能用
+ MD5被認為是比較弱的演算法，但是是不是代表不能用？
  + 檔案一致性
  + 資訊內容遮罩
  + 密碼加密

### 非對稱式加密

+ 可用演算法
  + SHA256/348
  + ECC
  + 在Server, Client都支援的情況下，加密傳輸優先權：
    + ECDHE > ECDH(Client) > DH(Server提供dhparm)

### 對稱式加密

+ 可用演算法
  + AES
  + ChaCha20
  + GCM

### 憑證特性

+ 憑證在資安的CIA上具有不可否認性

### 憑證架構

```
根憑證(PKI, root) → 中繼憑證 → 終端憑證
   └─────────憑證鏈(chain)─────────┘
```

### 根憑證說明

+ 預先儲存於作業系統中
+ 可以透過系統更新修改可信任的憑證（經由憑證廠商申請）

##### 憑證詳細資料

![image-20230408111756496](https://i.imgur.com/OSu9VzW.png)

---

### 查看憑證內容

@Server

```bash
root# cd /etc/httpd/conf/ssl/
```

```bash
root# openssl x509 -in server58.linux.class.crt -text -noout
Certificate:
    Data:
        ....
```

@Desktop

```bash
student$ cd /etc/pki/ca-trust/source/anchors/
```

```bash
student$ openssl x509 -in ca.crt -text -noout
Certificate:
    Data:
        ...
```

---

### 建立內部PKI

#### RootCA 憑證設定檔

@Desktop

```bash
root# cd
root# mkdir ssl-ca
root# cd ssl-ca
```

建立 CA v3 設定檔案

```bash
root# vi v3.cfg
```

```vim
# OpenSSL root CA configuration file.
# Copy to `/root/ca/openssl.cnf`.

[ ca ]
# `man ca`
default_ca = CA_default

[ CA_default ]
# Directory and file locations.
dir               = /home/centos/tmp/linuxclass/RootCA
certs             = $dir/certs
crl_dir           = $dir/crl
new_certs_dir     = $dir/newcerts
database          = $dir/index.txt
serial            = $dir/serial
RANDFILE          = $dir/private/.rand

# The root key and root certificate.
private_key       = $dir/private/ca.key.pem
certificate       = $dir/certs/ca.cert.pem

# For certificate revocation lists.
crlnumber         = $dir/crlnumber
crl               = $dir/crl/ca.crl.pem
crl_extensions    = crl_ext
default_crl_days  = 30

# SHA-1 is deprecated, so use SHA-2 instead.
default_md        = sha256

name_opt          = ca_default
cert_opt          = ca_default
default_days      = 7500
preserve          = no
policy            = policy_strict

[ policy_strict ]
# The root CA should only sign intermediate certificates that match.
# See the POLICY FORMAT section of `man ca`.
countryName             = match
stateOrProvinceName     = match
organizationName        = match
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional

[ policy_loose ]
# Allow the intermediate CA to sign a more diverse range of certificates.
# See the POLICY FORMAT section of the `ca` man page.
countryName             = optional
stateOrProvinceName     = optional
localityName            = optional
organizationName        = optional
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional

[ req ]
# Options for the `req` tool (`man req`).
default_bits        = 2048
distinguished_name  = req_distinguished_name
string_mask         = utf8only

# SHA-1 is deprecated, so use SHA-2 instead.
default_md          = sha256

# Extension to add when the -x509 option is used.
x509_extensions     = v3_ca

[ req_distinguished_name ]
# See <https://en.wikipedia.org/wiki/Certificate_signing_request>.
countryName                     = Country Name (2 letter code)
stateOrProvinceName             = State or Province Name (full name)
localityName                    = Locality Name (eg, city)
0.organizationName              = Organization Name (eg, company)
organizationalUnitName          = Organizational Unit Name (eg, section)
commonName                      = Common Name (eg, your name or your server's hostname)
emailAddress                    = Email Address

# Optionally, specify some defaults.
countryName_default             = TW
stateOrProvinceName_default     = Zhongshan Dist
localityName_default            = Taipei City
0.organizationName_default      = LinuxClass
organizationalUnitName_default  = Class
emailAddress_default            = ca@linux.class

[ v3_ca ]
# Extensions for a typical CA (`man x509v3_config`).
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true
keyUsage = critical, digitalSignature, cRLSign, keyCertSign

[ v3_intermediate_ca ]
# Extensions for a typical intermediate CA (`man x509v3_config`).
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true, pathlen:0
keyUsage = critical, digitalSignature, cRLSign, keyCertSign

[ usr_cert ]
# Extensions for client certificates (`man x509v3_config`).
basicConstraints = CA:FALSE
nsCertType = client, email
nsComment = "OpenSSL Generated Client Certificate"
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer
keyUsage = critical, nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = clientAuth, emailProtection

[ server_cert ]
# Extensions for server certificates (`man x509v3_config`).
basicConstraints = CA:FALSE
nsCertType = server
nsComment = "OpenSSL Generated Server Certificate"
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer:always
keyUsage = critical, digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth

[ crl_ext ]
# Extension for CRLs (`man x509v3_config`).
authorityKeyIdentifier=keyid:always

[ ocsp ]
# Extension for OCSP signing certificates (`man ocsp`).
basicConstraints = CA:FALSE
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer
keyUsage = critical, digitalSignature
extendedKeyUsage = critical, OCSPSigning
```

#### 產 ca 憑證

產生 CA Private/Public Key

```bash
root# openssl req -config v3.config -new -x509 -days 7500 -keyout ca.key -out ca.crt
```

```bash
root# ls -l
```

#### 建立新憑證組

建立用戶端 Private 與 Request 檔案

```bash
root# openssl req -nodes -newkey rsa:2048 -sha256 -keyout lab-server58.linux.class.key -nodes -utf8 -out lab-server58.linux.class.csr
```

> CommonName 設定 lab-server58.linux.class

#### 簽憑證

```bash
root# cd ~/ssl-ca
```

設定簽證設定檔

```bash
root# vi csr-v3.cfg
```

```vim
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
```

使用CA簽屬用戶端憑證

```bash
root# openssl x509 -req -days 365 -in lab-server58.linux.class.csr -extfile csr-v3.cfg -CA ca.crt -CAkey ca.key -CAcreateserial -out lab-server58.linux.class.crt -sha256
```

```bash
root# ls -l
```

```bash
root# cp lab-server58.crt /etc/httpd/conf/ssl
```

> CA 簽 CSR，get CRT

#### 套新憑證

1. 將`lab-server58.linux.class.crt`與`lab-server58.linux.class.key`複製到`Server`
   @Desktop

   ```bash
   root# echo "put lab-server58.linux.class.crt" | sftp root@server58:/etc/httpd/conf/ssl/
   root# echo "put lab-server58.linux.class.key" | sftp root@server58:/etc/httpd/conf/ssl/
   ```

2. 參考 Apache + SSL 設定 Apache 憑證

   > 有重置的話要再裝一次 Apache & SSL



#### Client 驗證

@Desktop

1. 把新的 ca.crt 放到 `/etc/pki/ca-trust/source/anchors/`，執行 update-ca-trust 
   ```bash
   root# cd ~/ssl-ca
   root# cp ca.crt /etc/pki/ca-trust/source/anchors/
   root# sudo update-ca-trust
   ```

2. 在 `/etc/hosts` 新增 lab-server58.linux.class 對應

3. FireFox:
   [https://lab-server58.linux.class](https://lab-server58.linux.class/)

---

### SELinux 簡述

+ SELinux: 系統安全防禦的預設值
+ 因為處理網頁無法開啟的問題更為複雜，所以一開始導入時，管理人員傾向於關閉SELinux，以讓服務正常運作

本文使用 Apache `httpd` 服務來說明 SELinux 對於行程的影響，其邏輯也能在其它服務中適用。

有關於 SELinux 對 `httpd` 運作的影響，大致有如下範圍：

- 目錄能否有權限存取
- 程式把檔案寫入指定的目錄位置
- 程式能否經由網路和別的系統服務交換資料
- `httpd` 能否在其它自訂的連埠服提供服務

要處理 SELinux 對 httpd 的運作影響，必須先區分幾個類別：

1. 檔案與目錄權限

   檔案或目錄只有在正確的 SELinux 安全標籤才可以操作。

2. 網路連接埠

   程式只有在 SELinux 允許的連接埠才能提供服務。

3. Apache 功能

   針對服務所提供的特別功能，必須要在 SELinux 中設定開關才能使用。

Apache 在 SELinux 中的關鍵字，多以 `http_` 字眼出現，在操作 SELinux 條入之前，大部份使用 `semanage` 與 `setsebool` 進行設定。`semenage` 為查修條件的工具，另一個則是用來開關 `httpd` 可用功能的開關。

`semanage` 預設並不會安裝，主要是期望管理者能夠依標準原則來處理系統服務，比如檔案應放在預先設好的路徑而非另行指定。話雖如此仍可以另行設定 SELinux 條件來新增或修改其條件，讓系統管理更有彈性。

若要安裝 `semanage` 指令工具可以進行如下操作：

```bash
student$ sudo dnf -y install policycoreutils-python-utils
```

除了 `semanage` 之外，我們還需要 `sesearch` 工具來找出作業行程能夠存取目錄與檔案的相關資訊，`sesearch` 被收錄在 `setools-console` 套件中，使用下列方式可以將它安裝好：

```bash
student$ sudo dnf install -y setools-console
```

由於 SELinux 對於行程的規範非常多，在此小節上，僅針對檔案與目錄權限、網路連接埠與 Apaache 功能做示範。

### 檔案與目錄權限

檔案與目錄權限是受 SELinux 規範最多的部份，在設定 SELinux 政策之前，必須以行程為首往下找出可以用的標籤規則，再從這些規則中找出有哪些目錄是符合的。

使用下列方式找出有關 `httpd` 的 SELinux 標籤：

```bash
student$ sudo ps -uxZ | grep -w httpd
system_u:system_r:httpd_t:s0    root     19053  0.0  0.5 230556  5352 ?        Ss    3月03   0:38 /usr/sbin/httpd -DFOREGROUND
```

上述結果中找到有關 `httpd` 行程所被設定的行程標籤為 `httpd_t`，接著再利用找出來的標籤找出所符合的行程可以存取哪些目錄檔案標籤，使用 `sesearch` 工具可以找出這些項目：

```bash
student$ sudo sesearch -s httpd_t --allow
~~ 以上略 ~~
allow httpd_t httpd_sys_content_t : lnk_file { read getattr } ; 
allow httpd_t httpd_sys_content_t : dir { ioctl read getattr lock search open } ; 
allow httpd_t httpd_sys_content_t : file { ioctl read getattr lock map open } ;
~~ 以下略 ~~
```

因為找出的項目有成千上萬，但經由確認後可以發現有關 `httpd_sys_content_t` 可以針對目錄與檔案進行開啟的行為（`open`）。接下來我們可以使用 `httpd_sys_content_t` 為關鍵，找出符合該檔案標籤有哪些路徑。

使用 `semanage` 再指定的標簽可以找出相關資訊，如下範例所示：

```bash
student$ sudo semanage fcontext -l | grep httpd_sys_content_t
/srv/([^/]*/)?www(/.*)?                            all files          system_u:object_r:httpd_sys_content_t:s0 
/var/www(/.*)?                                     all files          system_u:object_r:httpd_sys_content_t:s0 
/etc/htdig(/.*)?                                   all files          system_u:object_r:httpd_sys_content_t:s0 
/srv/gallery2(/.*)?                                all files          system_u:object_r:httpd_sys_content_t:s0 
/var/lib/trac(/.*)?                                all files          system_u:object_r:httpd_sys_content_t:s0 
/var/lib/htdig(/.*)?                               all files          system_u:object_r:httpd_sys_content_t:s0 
/var/www/icons(/.*)?                               all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/glpi(/.*)?                              all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/htdig(/.*)?                             all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/drupal.*                                all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/z-push(/.*)?                            all files          system_u:object_r:httpd_sys_content_t:s0 
/var/www/svn/conf(/.*)?                            all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/icecast(/.*)?                           all files          system_u:object_r:httpd_sys_content_t:s0 
/var/lib/cacti/rra(/.*)?                           all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/ntop/html(/.*)?                         all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/nginx/html(/.*)?                        all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/doc/ghc/html(/.*)?                      all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/openca/htdocs(/.*)?                     all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/selinux-policy[^/]*/html(/.*)?          all files          system_u:object_r:httpd_sys_content_t:s0
```

以上找到的結果，就是預設可以放置網頁檔案的位置（如: `/var/www(/.*)?` 項目），這些位置可以讓 Apache 行程讀取目錄與檔案的內容。

### 網路連接埠

`httpd` 所能佔用的網路連接埠被 SELinux 的 `http_port_t` 所規範，必須在項目所允許的列表中才能夠啟用該連接埠，否則在 `httpd` 啟動的時候就會發生錯誤。

要檢查目前 `httpd` 被允許使用哪些連接埠，可以使用下列方式：

```bash
student$ sudo semanage port -l | grep http_port_t
http_port_t                    tcp      80, 81, 443, 488, 8008, 8009, 8443, 9000
```

從上列所顯示的連接埠中，除了眾所皆知的 80、443 之外，還包含了如 81、488⋯⋯等項目可以使用。

如果要增加可用連接埠，一樣必須使用 `semanage` 工具進行設定，為 `http_port_t` 標籤新增一個連接埠讓 `httpd` 可以使用。

假設預計要為 `http_port_t` 新增一個 tcp 8888 的連接埠，可以使用下列方式達成：

```bash
student$ sudo semanage port -a -p tcp 8888 -t http_port_t
```

完成後再重新列表一次，確認 tcp 8888 已經被加入到 `http_port_t` 標籤中：

```bash
student$ sudo semanage port -l | grep http_port_t
http_port_t                    tcp      8888, 80, 81, 443, 488, 8008, 8009, 8443, 9000
```

完成之後，您可以在 `httpd` 的設定檔（如 `/etc/httpd/conf/httpd.conf`）中，將 Listen 8888 加入讓 `httpd` 可以使用 TCP/8888 的連接埠。

> 若要刪除先前所指定的可用連接埠，將 `-a` 參數改為 `-d` 就可以將該項目刪除。

### Apache 功能

Apache 提供了多個功能，某些功能會被 SELinux 所規範，SELinux 將 Apache 的功能設成多個開關，若有必要時再開啟讓 Apache 能夠正常提供。

這些功能開啟與關閉的關鍵字為 `on` 與 `off`，使用 `boolean` 的型態所指示。

若要顯示 SELinux 規範的 Apache 功能有哪些，可以使用 `semanager` 來檢視：

```bash
student$ sudo semanage boolean -l | grep -w httpd
httpd_can_network_relay        (off  ,  off)  Allow httpd to can network relay
httpd_can_connect_mythtv       (off  ,  off)  Allow httpd to can connect mythtv
httpd_can_network_connect_db   (off  ,  off)  Allow httpd to can network connect db
httpd_use_gpg                  (off  ,  off)  Allow httpd to use gpg
httpd_dbus_sssd                (off  ,  off)  Allow httpd to dbus sssd
httpd_enable_cgi               (on   ,   on)  Allow httpd to enable cgi
httpd_verify_dns               (off  ,  off)  Allow httpd to verify dns
httpd_dontaudit_search_dirs    (off  ,  off)  Allow httpd to dontaudit search dirs
httpd_use_cifs                 (off  ,  off)  Allow httpd to use cifs
httpd_manage_ipa               (off  ,  off)  Allow httpd to manage ipa
~~ 以下略 ~~
```

以上所列的訊息中，在第 2 欄位使用括弧所包起來的項目，分別為 **現行狀態** 與 **預設值**。假設要讓 `httpd` 可以使用 cifs 協定進行檔案存取，那麼就必須把 `httpd_use_cifs` 設定為 `on`。

設定 SELinux boolean 可以使用 `setsebool` 工具進行設定：

```bash
student$ sudo setsebool httpd_use_cifs on
```

完成後再次檢查 `httpd_use_cifs` 是否被正確設定：

```bash
student$ sudo  semanage boolean -l | grep -w httpd | grep -w httpd_use_cifs
httpd_use_cifs                 (on   ,  off)  Allow httpd to use cifs
```

請注意所輸出之項目，第二欄位由括弧所包起來的項目中，現行狀態若為 `on` 代表已啟用功能，若是預設狀值仍為 `off` 的話代表在下次系統重開機時會關閉該功能，必須再次執行啟用的設定。

如果經過測試後確定要讓指定的功能永久啟用（重開機時也會啟用）那麼就可以使用 `-P` 參數讓該設定永久生效，使用的方式如下：

```bash
student$ sudo setsebool -P httpd_use_cifs on
```

> 針對 SELinux 對 Apache 各功能的項目，可以參考 httpd_selinux(8) 有完整的說明。

---

### SELinux

查看目前系統的 SELinux 有沒有跑起來

```bash
root# sestatus
```

![image-20230408151627269](https://i.imgur.com/nLSA6jJ.png)

---

## 目標

- 瞭解 SELinux 對於 File Context/Port/Function 的管理方法。
- 使用工具確認是否違反 SELinux 政策。
- 使用 `-Z` 參數、semanager、sesearch、getsebool、setsebool、chcon、restorecon 等 SELinux 工具的使用。

## SSH 改 Port

這個 LAB 用來演練修改服務的使用 Port 與 SELinux 需要調配的項目。

@Server

```bash
root# dnf install -y policycoreutils-python-utils-2.9-20.el8.noarch
root# cp /etc/ssh/sshd_config{,_backup}
```

設定將 Port 改成 `8866`

```bash
root# vi /etc/ssh/sshd_config
修改 Port

...

Port 8866
root# systemctl restart sshd
FAIL
root# ausearch -m avc
```

列出 sshd 能夠使用的 Port

```bash
root# semanage port -l | grep ssh_port_t
```

新增 sshd 能夠使用的 Port

```bash
root# semanage port -a -p tcp 8866 -t ssh_port_t
```

重新啟動服務

```bash
root# systemctl restart sshd
OK
```

設定防火牆允許 tcp/p8866 連入

```bash
root# ss -ntulp | grep 8866
root# firewall-cmd --permanent --add-port=8866/tcp
root# firewall-cmd --reload
```

將 Port 改為原本的 22

@Desktop

```bash
root# ssh -p 8866 root@server58
```

@Server

```bash
root# vi /etc/ssh/sshd_config
修改 Port

...

Port 22
root# systemctl restart sshd
```

移除防火牆可連入 tcp/8866

```bash
root# firewall-cmd --permanent --remove-port=8866/tcp
root# firewall-cmd --reload
```

移除 sshd 可使用 Port 8866 的設定

```bash
root# semanage port -l | grep ssh_port_t

root# semanage port -d -p tcp 8866 -t ssh_port_t

root# semanage port -l | grep ssh_port_t
```

## Apache 預設目錄

這個 LAB 用來演練修改網頁服務對於 檔案/目錄 的 `fcontext` 操作方法。

@Server

```bash
root# dnf install -y policycoreutils-python-utils
root# dnf install -y setools-console
```

安裝與啟用 httpd

```bash
root# dnf install -y httpd
root# systemctl enable --now httpd
root# firewall-cmd --permanent --add-service=http
root# firewall-cmd --reload

root# echo HelloX > /var/www/html/index.html
```

@Desktop

Firefox: [http://server58/](http://server58/)
HelloX

@Server

```bash
root# mkdir /web

root# echo HelloX > /web/index.html

root# cd /etc/httpd/conf
root# cp httpd.conf httpd.conf_backup
```

修改網頁根目錄

```bash
root# vi httpd.conf
修改下列行
...

Line 122: DocumentRoot "/web"
Line 127: <Directory "/web">
Line 134: <Directory "/web">
root# mv /etc/httpd/conf.d/welcome.conf{,_disable}
root# systemctl restart httpd
```

@Desktop

Firefox: [http://server58](http://server58/)
Forbidden

@Server

檢查存取檔案是否被 SELinux 阻擋。

```bash
root# ausearch -m avc | tail
----
time->Tue Aug 24 22:34:49 2021
type=PROCTITLE msg=audit(1629815689.260:493): proctitle=2F7573722F7362696E2F6874747064002D44464F524547524F554E44
type=SYSCALL msg=audit(1629815689.260:493): arch=c000003e syscall=4 success=no exit=-13 a0=7f67b4045630 a1=7f67ac7ef890 a2=7f67ac7ef890 a3=7f67ac7f04f0 items=0 ppid=13555 pid=13559 auid=4294967295 uid=48 gid=48 euid=48 suid=48 fsuid=48 egid=48 sgid=48 fsgid=48 tty=(none) ses=4294967295 comm="httpd" exe="/usr/sbin/httpd" subj=system_u:system_r:httpd_t:s0 key=(null)
type=AVC msg=audit(1629815689.260:493): avc:  denied  { getattr } for  pid=13559 comm="httpd" path="/web/index.html" dev="dm-0" ino=254636 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:default_t:s0 tclass=file permissive=0
----
time->Tue Aug 24 22:34:49 2021
type=PROCTITLE msg=audit(1629815689.260:494): proctitle=2F7573722F7362696E2F6874747064002D44464F524547524F554E44
type=SYSCALL msg=audit(1629815689.260:494): arch=c000003e syscall=6 success=no exit=-13 a0=7f67b4045700 a1=7f67ac7ef890 a2=7f67ac7ef890 a3=1 items=0 ppid=13555 pid=13559 auid=4294967295 uid=48 gid=48 euid=48 suid=48 fsuid=48 egid=48 sgid=48 fsgid=48 tty=(none) ses=4294967295 comm="httpd" exe="/usr/sbin/httpd" subj=system_u:system_r:httpd_t:s0 key=(null)
type=AVC msg=audit(1629815689.260:494): avc:  denied  { getattr } for  pid=13559 comm="httpd" path="/web/index.html" dev="dm-0" ino=254636 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:default_t:s0 tclass=file permissive=0
```

> SELinux 擋住 `/web/index.html` 存取

查看 httpd 使用的 process 被貼上的 `context`

```bash
root# ps auxZ | grep httpd
```

查看預設根目錄中的 `context`

```bash
root# ls -lhdZ /var/www/html
drwxr-xr-x. 2 root root system_u:object_r:httpd_sys_content_t:s0 24 Aug 22 11:40 /var/www/html
```

查看 `httpd` 行程 context 對於哪些指定的 `檔案/目錄` context 有哪些已知的權限。

```bash
root# sesearch -s httpd_t --allow | grep httpd_sys_content_t
allow daemon httpd_sys_content_t:dir { getattr open search };
allow httpd_t httpd_sys_content_t:dir { add_name getattr ioctl lock open read remove_name search write }; [ ( httpd_builtin_scripting && httpd_unified && httpd_enable_cgi ) ]:True
allow httpd_t httpd_sys_content_t:dir { getattr ioctl lock open read search };
allow httpd_t httpd_sys_content_t:file { getattr ioctl lock map open read };
allow httpd_t httpd_sys_content_t:lnk_file { getattr read };
```

列出 fcontext 中預設被貼上 context 的目錄規則。

```bash
root# semanage fcontext -l | grep httpd_sys_content_t
/etc/htdig(/.*)?                                   all files          system_u:object_r:httpd_sys_content_t:s0 
/srv/([^/]*/)?www(/.*)?                            all files          system_u:object_r:httpd_sys_content_t:s0 
/srv/gallery2(/.*)?                                all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/doc/ghc/html(/.*)?                      all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/drupal.*                                all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/glpi(/.*)?                              all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/htdig(/.*)?                             all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/icecast(/.*)?                           all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/nginx/html(/.*)?                        all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/ntop/html(/.*)?                         all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/openca/htdocs(/.*)?                     all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/selinux-policy[^/]*/html(/.*)?          all files          system_u:object_r:httpd_sys_content_t:s0 
/usr/share/z-push(/.*)?                            all files          system_u:object_r:httpd_sys_content_t:s0 
/var/lib/cacti/rra(/.*)?                           all files          system_u:object_r:httpd_sys_content_t:s0 
/var/lib/htdig(/.*)?                               all files          system_u:object_r:httpd_sys_content_t:s0 
/var/lib/trac(/.*)?                                all files          system_u:object_r:httpd_sys_content_t:s0 
/var/www(/.*)?                                     all files          system_u:object_r:httpd_sys_content_t:s0 
/var/www/icons(/.*)?                               all files          system_u:object_r:httpd_sys_content_t:s0 
/var/www/svn/conf(/.*)?                            all files          system_u:object_r:httpd_sys_content_t:s0 
```

將新的位置加入 httpd 可使用的規則中。

```bash
root# semanage fcontext -a -t httpd_sys_content_t '/web(/.*)?'

root# semanage fcontext -l | grep httpd_sys_content_t | grep web
/web(/.*)?                                         all files          system_u:object_r:httpd_sys_content_t:s0
```

使用 restorecon 還原並檢查結果。

```bash
root# ls -ldZ /web

root# restorecon -R /web

root# ls -ldZ /web
```

@Desktop

Firefox: [http://server58](http://server58/)
HelloX

## Apache Mount NFS

這個 LAB 用來演練啟用服務功能對於 SELinux 需要進行開關的項目。

在 Desktop 中建立 NFS 服務並啟用。

@Desktop

```bash
root# dnf install -y nfs-utils

root# mkdir /web
root# echo HelloX from Desktop > /web/index.html
root# vi /etc/exports
/web 172.16.2.X(ro)
root# systemctl enable --now nfs-server
root# firewall-cmd --permanent --add-service=nfs
root# firewall-cmd --reload
```

在 Server 中掛載 Desktop 的 NFS 分享目錄。

@Server

```bash
root# dnf install -y nfs-utils
root# mount -t nfs 172.16.1.X:/web /web
```

@Desktop

Firefox: [http://server58](http://server58/)
Forbidden

@Server

在 Server 中查看 SELinux 阻檔原因

```bash
root# # ausearch -m avc | tail
----
time->Tue Aug 24 22:54:35 2021
type=PROCTITLE msg=audit(1629816875.274:575): proctitle=2F7573722F7362696E2F6874747064002D44464F524547524F554E44
type=SYSCALL msg=audit(1629816875.274:575): arch=c000003e syscall=4 success=no exit=-13 a0=7f67b40436d0 a1=7f67ad7f1890 a2=7f67ad7f1890 a3=7f67ad7f24f0 items=0 ppid=13555 pid=13559 auid=4294967295 uid=48 gid=48 euid=48 suid=48 fsuid=48 egid=48 sgid=48 fsgid=48 tty=(none) ses=4294967295 comm="httpd" exe="/usr/sbin/httpd" subj=system_u:system_r:httpd_t:s0 key=(null)
type=AVC msg=audit(1629816875.274:575): avc:  denied  { getattr } for  pid=13559 comm="httpd" path="/web/index.html" dev="0:48" ino=68301488 scontext=system_u:system_r:httpd_t:s0 tcontext=system_u:object_r:nfs_t:s0 tclass=file permissive=0
----
time->Tue Aug 24 22:54:35 2021
type=PROCTITLE msg=audit(1629816875.275:576): proctitle=2F7573722F7362696E2F6874747064002D44464F524547524F554E44
type=SYSCALL msg=audit(1629816875.275:576): arch=c000003e syscall=6 success=no exit=-13 a0=7f67b40437a0 a1=7f67ad7f1890 a2=7f67ad7f1890 a3=1 items=0 ppid=13555 pid=13559 auid=4294967295 uid=48 gid=48 euid=48 suid=48 fsuid=48 egid=48 sgid=48 fsgid=48 tty=(none) ses=4294967295 comm="httpd" exe="/usr/sbin/httpd" subj=system_u:system_r:httpd_t:s0 key=(null)
type=AVC msg=audit(1629816875.275:576): avc:  denied  { getattr } for  pid=13559 comm="httpd" path="/web/index.html" dev="0:48" ino=68301488 scontext=system_u:system_r:httpd_t:s0 tcontext=system_u:object_r:nfs_t:s0 tclass=file permissive=0
```

> SELinux 拒絕 httpd 存取 nfs

檢視 SELinux 針對 httpd 的功能開關。

```bash
root# getsebool -a | grep httpd_use_nfs
httpd_use_nfs --> off
```

啟用 httpd 可使用 NFS 的功能。

```bash
root# setsebool -P httpd_use_nfs on
```

@Desktop

Firefox: [http://server58](http://server58/)
HelloX from Desktop

---

> # 很值得參考的文檔
>
> 如果以上lab太多錯難以除去，那就看這篇吧🥲
>
> [httpd_selinux(8) - Linux man page](https://linux.die.net/man/8/httpd_selinux)







