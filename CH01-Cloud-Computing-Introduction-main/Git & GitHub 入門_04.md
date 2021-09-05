# Git 分支中階教學

## 還原技巧

- 回頭觀看版本內容：`git checkout 編號`
- 返回最新的版本：`git checkout master(分支名稱)`
- 還原工作目錄上已更改的檔案 ：`git checkout -- <file>`
- 索引檔案>還原到工作目錄：`git reset HEAD`
- 還原前兩個版本：`git reset HEAD^^`
- 還原前兩個版本，所有更新檔案都**放棄**：`git reset HEAD^^ --hard`
- 觀看詳細歷史紀錄：`git reflog`
- [git reset 參數介紹](https://gitbook.tw/chapters/using-git/reset-commit.html)

## checkout 與 reset 差異

- checkout 是移動 HEAD
- reset 是移動 branch

## 發 [PR](https://github.com/hsiangfeng/1107-Pull-request) 流程

1. Ray 是開發者，並擁有一個 master 開發分支
2. 洧杰很喜歡這個專案，但發現有個小問題
3. 於是他 fork 了專案，在  master 上新增了一個 commit 並下了 PR
4. Ray 認為洧杰 真是天才，於是 merge 了他的 PR

```
練習：請試著發 PR 給 Ray
```

## commit 衝突

兩個 commit 在合併時，檔案裡的某一行有重複修改到時，就會觸發

## 本地衝突

- 1.洧杰跟 Ray 一起在同個 repo 開發，只有一條 master 線
- 2.洧杰先部署了一個 commit 到 master
- 3.Ray 更新了第六行程式碼，並在下班前立馬 push
- 4.洧杰也改了第六行，但他效率比較差，下班後 push 時才發現，Ray 這個ㄐ歪人先 push 了！
- 5.洧杰只好先 pull 下來，結果竟然跟自己的 commit 衝突
- 6.洧杰只好邊罵髒話，邊解決衝突，整理好後再 push