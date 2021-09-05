# Git 遠端  Repository 操作

## 常用環境設定

* 編輯器更換：`git config --global core.editor "code --wait"`
* Git 縮寫：
    * `$ git config ``--``global`` ``alias``.``co checkout`
        `$ git config ``--``global`` ``alias``.``br branch`
        `$ git config ``--``global`` ``alias``.``st status
        $ git config --global alias.ci commit`

* 觀看所有 config 的設定：
    * Mac：`~/.gitconfig`
    * Win：`C:\Users\$USER`
* [git log 詳細資訊](https://git-scm.com/book/zh-tw/v2/Git-%E5%9F%BA%E7%A4%8E-%E6%AA%A2%E8%A6%96%E6%8F%90%E4%BA%A4%E7%9A%84%E6%AD%B7%E5%8F%B2%E8%A8%98%E9%8C%84)

## Git 與 Github 是什麼？

* Git 是一個分散式版本控制軟體，可藉由它產生一個**儲存庫( git Repository)**。
* Github：支援 git 程式碼存取和**遠端托管儲存庫**的平台服務
* 關係像是本地端有一個 index.html，但可以放到 dropbox、Google Drive 進行雲端託管

## 熱門遠端儲存庫(Github VS Bitbucket VS Gitlab)

* [GitHub](https://github.com/)：擁有 GitHub Pages 功能，可擁有私人數據庫，免費方案是 3 人以下
* [Bitbucket](https://bitbucket.org/product/pricing)：可擁有私人數據庫，免費方案是五人以下團隊
* [GitLab](https://about.gitlab.com/)：自架 Git 伺服器，有提供 web 視覺化管理介面，常用於企業內部開發

懶人包解釋：

* 公司專案的小型團隊可用 Bitbucket
* 想要有一個公開對外網站的話，可用 GitHub

## 遠端儲存庫(Repository)操作

* 註冊遠端儲存庫：`git remote add origin 遠端儲存庫網址`
* 更新資料到遠端 master 分支：`git push -u origin master`

* -u 是指他預設會推到哪個遠端數據庫服務
* origin 可以改它的遠端數據庫名稱，例如 **git push -u github master**

## Git 版本細節

* branch ：分支，預設分支叫做 master
* HEAD：指標 
* origin：預設遠端儲存庫名稱
* 回頭觀看版本內容：`git checkout 編號`！
* 返回最新的版本：`git checkout master(分支名稱)`

## 還原技巧

![Image: 18333fig0201-tn.png](images\18333fig0201-tn.png)

### 新增檔案時，檔案還沒加追蹤時，清空工作目錄

* 顯示此次清除的檔案：`git clean -n`
* 強制清除檔案：`git clean -f `

**檔案已加入追蹤，清空工作目錄**

* 還原工作目錄上已更改的檔案 ：`git checkout -- <file>`

**檔案加入到索引，退到工作目錄**

* 加入索引的檔案還原到工作目錄：`git reset HEAD`

### 版本還原

* 還原前兩個版本：`git reset HEAD^^`
* 還原前兩個版本，所有更新檔案都放棄：`git reset HEAD^^ --hard`
* 觀看詳細歷史紀錄：`git reflog`
* 還原到特定 commit：`git reset commit編號 --hard`
* [git reset 參數介紹](https://gitbook.tw/chapters/using-git/reset-commit.html)



## 參考文件

* [Git 與 Github 版本控制基本指令與操作入門教學](https://blog.techbridge.cc/2018/01/17/learning-programming-and-coding-with-python-git-and-github-tutorial/)