# Git 分支 (branch)

## 為什麼要用分支？

* 多人協作時，不可能都在 master
* 可以讓 master 都是正式版資料，可以開分支來做測試或開發，藉此不影響正式主機分支

## branch 介紹

* 分支就像是便利貼，貼在某個 commit 上
* 分支合併，主要是兩個 commit 進行合併

## 開分支流程

新增分支：`git branch 分支名稱`
查看分支：`git branch` 
切換分支：`git checkout 分支名稱`
刪除分支：`git branch -d 分支名稱` 、-D 是強制刪除
還原上個版本：`git reset HEAD^`

開發情境：我有一個正式主機 master 分支，有一個開發分支叫做 develop

## 合併分支 && 快轉機制

合併分支：`git merge 分支名稱`
取消快轉：`git merge 分支名稱 --no-ff`
觀看線圖：`git log —oneline -graph`
還原合併前狀態：`git reset —hard ORIG_HEAD`

### 快轉機制

* 取消快轉：兩個不同任務分支合併時，取消快轉
* 預設使用快轉：本地與遠端兩條相同的任務分支時，可以用快轉

## 解決衝突流程

HEAD 是當前 HEAD 分支位置，develop 是你想合併的分支

```
<<<<<<< HEAD
      <h1>我是標題</h1>
=======
      <h2>我是大標題</h2>
>>>>>>> develop
```

* 取消 merge 衝突狀態： `git merge --abort`

## 其他操作


將特定 commit 貼上分支：`git branch 新分支名稱 commit編號`

## 常見分支名稱

* master 預設分支
* develop 開發分支
* feature 開發新功能分支

## 遊戲分享

* [https://learngitbranching.js.org](https://learngitbranching.js.org/)
* [挑戰關卡](https://drive.google.com/drive/folders/1koW25onvGTHtnHuob2UR1x6Jxywo34Om?usp=sharing)
    * 請寫成 BLOG
    * 請留言分享一個 Git 分支圖，讓其他學員練習

## 作業一：Git 分支練習

請依序做此[練習題](https://drive.google.com/drive/folders/198UAQzQ4T66fYWrH5H9RJUgbmtUVbNzK?usp=sharing)，並截圖 10~15 關總練習的練習畫面。


## 作業二：Git 遠端分支練習

### 請依照下面兩個指示設計，並**截圖 Sourtree 畫面與 GitHub Repo 網址**

1. 請用 Git Branch 去做出下圖，有兩個本地分支 (master、dev)，一個遠端 GitHub 分支 (origin/master)
2. 左側的三個 JS 功能如下，不需寫 HTML，**只寫 JS 邏輯即可，或是自訂功能也可以**。

![Image: 74670846_2786531548033139_2906708143650111488_n-1.jpg](images\74670846_2786531548033139_2906708143650111488_n-1.jpg)

## JS 三個功能

以下題目都用 console.log 顯示結果
1.印出乘法表，從 3~15，從 3x1、3x2~一直到 15x15
2.請建立一個 BMI 函式，裡頭有兩個參數(身高,體重)，執行時會印出對應 BMI，並取小數點後一位。範例如下


> *input: BMI(178,70)*

> *console.log：25.2*

3.請用 for 迴圈，1 加到 10，最後用 console 印出總數
此為 sourcetree 提示