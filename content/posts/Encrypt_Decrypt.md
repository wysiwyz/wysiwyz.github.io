---
title: "Encrypt, Decrypt"
date: 2023-02-01T14:01:22+08:00
author: ""
tags: [""]
categories: ["StudyNote"]
---

## 用途

1. 怕被別人看見
   + 弄成被看到也沒關係 [加密]
2. 想給特定人看
   + 避免對方也看不懂 [解密]

## 分類

| 類型 | 對稱加解密                                                   | 非對稱加解密                                                 |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 實例 | AES、3DES                                                    | RSA、ECDSA                                                   |
| ...  | 加解密方式寫在同一張紙上<br />[加解密方式] public key        | 加解密方式寫在不同紙上<br />[加密方式] public key ... client get by calling api<br />[解密方式] private key ... saved at Server side |
| 概論 | 簡單、速度快、安全性低                                       | 複雜速度慢、安全性較高                                       |
| 特色 | 1. Client 和 Server 用同一把密鑰加解密<br />2. `AES/ {ECB/CBC...}/PaddingPattern` |                                                              |
| 應用 | 1. 對大量明文的加密<br />2. Https 的 SessionKey              |                                                              |

## Appendix

### AES 加密結構說明

+ 金鑰長度(bit) : 
  + AES-128
  + AES-192
  + AES-256
+ 區塊間加密方式 : 
  + ECB - 可以被密推
  + CBC - 亂數 initial value 初始值
  + CFB - 亂數 initial value 初始值
  + OFB
  + CTR
+ 區塊填充方式: 
  + NoPadding - 不建議、會影響加密強度
  + ZeroBytePadding - 缺兩個填入 0
  + PKCS5Padding
  + PKCS7Padding

> Example: AES-256/CBC/PKCS7Padding
>
> | 密鑰長度 (bit) | 分組長度 (bit) | 建議輪數 |
> | -------------- | -------------- | -------- |
> | AES-128        | 128            | 10       |
> | AES-192        | 128            | 12       |
> | AES-256        | 128            | 14       |

### 其他

| 對        | 非對稱     | 混合      | 橢圓 | 雜湊算法 | 編碼               |
| --------- | ---------- | --------- | ---- | -------- | ------------------ |
| AES、3DES | RSA、ECDSA | RSA x AES |      | SHA3-256 | Base64、URLEncoder |

[延伸閱讀 - 區塊間加密方式](https://notes.andywu.tw/2019/%E5%AF%86%E7%A2%BC%E7%9A%84%E5%8A%A0%E5%AF%86%E6%A8%A1%E5%BC%8Fecb-cbc-cfb-ofb-ctr/)
