---
title: "Git Bash - some awesome commands"
date: 2023-03-16T14:20:15+08:00
author: "celine"
tags: ["Git Bash"]
categories: ["StudyNote"]
---

> 紀錄一些 git bash 指令

1. `git log -oneline` 跟sourceTree_History_Description欄位基本一樣

   ![image-20230316142443067](https://i.imgur.com/k8WeUJx.png)

   

2. `git log` 跟樓上一樣, 但多了些author/date訊息

   ![image-20230316142421145](https://i.imgur.com/gNQhL0n.png)

### Interactive Rebase

+ 可用來修改local端的commit message
+ 刪除local端commit
+ 整理local端commits
+ 合併多個commits為一個
+ 將一個commit拆分為多個commits

> 注意：會重寫commit history，產生新的hash ID(i.e. new commit)
>
> 所以不能對已經push上remote repository的commits執行interactive rebase

1. `git rebase -i HEAD~3`

   + rebase interactive head till the three

   + 要回去幾個節點之前? base commit是哪個?

     1. copy hash id `9a23775`
     2. Head minus 3
   
   + 接著會出現一個editor window，可控制head+前兩個commit
   
   + `reword`
   
     + 不直接改commit msg，而是將pick改成reword，存檔關閉
   
       ![image-20230316152150688](https://i.imgur.com/N49E1nu.png)
   
       
   
     + 接著才會出現該commit id的編輯器視窗，修改內文後存檔關閉 (`esc`+`:wq`)
   
       ![image-20230316152031070](https://i.imgur.com/R131Dvu.png)
   
       ![image-20230316152123443](https://i.imgur.com/JmR3HPm.png)
       
       
   
   + `squash`
   
     + 將該行commit和它的上一行(前一個commit)合併成同一個，存檔關閉
       ![image-20230316152237018](https://i.imgur.com/Dj5c7XP.png)
     + 
     + 跳新視窗，寫入合併後的commit msg，存檔關閉
   
     + Voilà! 原本的兩個commits合併成一個惹！
   
       ![image-20230316190914876](https://i.imgur.com/L2QNeLr.png)
   
       
   
       ![image-20230316150048329](https://i.imgur.com/N4SIKQQ.png)
       
       
   
2. `git commit amend` 可修改最近一筆local端本分支的 msg

### Cherry-Picking - 合併特定一筆/特定幾筆commits

```
[j1]<--[j3]<--[j2]--<branch_R>
  ↖
   [j2]<--[j4]--<branch_J>
```

主要還是用 rebase 跟 merge，只有特殊情境才用

##### Example

天！小白把變更 commit 到 master 分支了，但是團隊老大說過要 commit 在 feature_newsletter 才對！

小白可以這麼做...

```bash
$ git checkout feature_newsletter
Switched to branch 'feature_newsletter'
$ git cherry-pick <that_hash_id>
```

Voilà！newsletter signup page 被接木到 feature_newsletter 惹！

想重寫歷史 ~~毀屍滅跡~~ 的話，小白還可以...

```bash
$ git checkout master
Switched to branch 'master'
$ git reset --hard HEAD~1
```

> `HEAD~1`(head till the one): 在這裡指的是one behind the head

### Reflog - every movement of the HEAD Pointer (日誌)

可列出各種 actions history
例如 commit/checkout/merge/rebase/cherry-pick/reset 

##### Example_1

1. 小綠討厭分支內的其中幾個commits

2. 小綠使用`git reset`之術

   ```bash
   $ git reset --hard <想刪除的commits『之前』的commit_hash_id>
   HEAD is now at <貼上的該commit_hash_id> <該commit msg>
   ```

3. 小綠後悔了

4. 小綠😭😱

解決之道：

```bash
$ git reflog
<commit_id> (HEAD -> xxx, xxxx) HEAD@{0}: reset: moving to <commit_id>
<another_cmt_id> 
```

可以看到過去的 actions(從上而下，新到舊_chronologically)

想要回到過去的話，只要複製line# 3的commit_id，再執行以下作法其中一個

+ `git reset`
+ `git branch happy-ending <another_cmt_id>` 
  開新的分支

##### Example_2 - 使用`git reflog`之術復原已刪除的分支 feature/login

小青的客戶說這個產品不需要login功能了，小青信以為真把它刪了

因為有沒合併到其他分支的commit，需要使用 force delete

![image-20230316165331703](https://i.imgur.com/O748uRt.png)

經過一夜，三心二意的客戶告訴小青他們要重測 feature/login 分支的API了

這時候可以查看`git reflog`

![image-20230316165530356](https://i.imgur.com/4Ph5tir.png)

複製 `9abcca5`，用此 command_id 開新的分支

> 嫌視窗佔滿可以輸入`clear`(put that on top)，
> 作用跟 `CTRL + L` clear terminal command 差不多

```bash
$ git branch feature/login 9abcca5
```

### Submodules - repo nested inside another parent repo

複製貼上第三方程式碼的缺點

1. 外部程式碼跟自己的code可能會衝突/搞混
2. 手工更新外部程式碼，反覆同樣行為

##### Example

1. 在某repository目錄下建立一個目錄，名稱為`lib` or `vendor folder` 之類的

   ```bash
   $ mkdir lib
   $ cd lib
   $ git submodule add <遠端倉庫的網址 https://github.com/xxx/yyy>
   ```

   ![image-20230316173931365](https://i.imgur.com/2RS3hJk.png)

2. [注意] submodule `lib`目錄下的東西並不存在於parent repository
   parent repository 只會存：

   + sub module 的遠端 URL
   + 本地端的路徑
   + checkout revision

3. 加入sub-module之後，會發現 parent repo目錄內除了`.gitignore`，還多了一個`.gitmodules`檔案

   ```bash
   $ cat .gitmodules
   [submodule "lib/ToProgress"]
   			path = lib/ToProgress
   			url = https://github.com/xxx/yyy
   ```

   還可以查看 `.git/config` 多一個配置

   ![image-20230316174413999](https://i.imgur.com/Qulj4qZ.png)

---

```bash
$ git fetch 
$ git branch -a 
  //列出可checkout的所有分支
$ git checkout -b develop origin/develop
  //從遠端拉分支到本地端 

$ git add .
  //全部stage
$ git add <要stage的file>
  //stage partially
  
$ git commit -m "[UAT][accident] mod validation"
$ git commit --amend

$ git push
```

