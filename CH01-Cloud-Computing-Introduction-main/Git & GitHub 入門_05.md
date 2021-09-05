# Git Flow、GitHub Flow

發問時請切到成全體嘉賓與觀眾。

## 常見合作流程

- [Git flow](https://nvie.com/posts/a-successful-git-branching-model/)
- [GitHub flow](https://guides.github.com/introduction/flow/)
- 發問區：https://app.sli.do/event/janodfuz

## GitHub flow

1. 在 master 建立分支來新增功能
2. 開始開發功能
3. 發 PR (Pull Request) 申請合併
4. 討論與檢視程式碼
5. 部署(Deploy)
6. 合併(Merge)

### 規範

- 任何在 master 上的 commit 都是可以部署的
- 假使程式碼需要討論，請 PR 後在 comment 進行討論

## 本日協作流程

1.本地建立 node 、Git 環境，增加 (.gitignore)
2.commit 1：建立環境
3.commit 2：修改 title
4.新增 dev branch
5.新增 GitHub Repo，並 push
6.新增 heroku Repo，並 push
7.Ray 抓 GitHub dev 分支，並開 feature 分支 (中間洧杰做 heroku 部署設定)
8.commit 兩個後，push feature 分支，並開 pr 請求合併 dev
9.管理員合併
10.志誠 抓 GitHub dev 分支，並開 feature 分支
11.commit 兩個後，push feature 分支，並開 pr 請求合併 dev
12.管理員合併，並抓下來看內容都 ok
13.管理員 fetch 下來，用 master 合併 origin/dev 分支，並 push 到 GitHub