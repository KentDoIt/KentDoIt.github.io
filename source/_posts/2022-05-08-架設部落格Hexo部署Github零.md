---
title: 架設部落格 Hexo 部署 Github (零)
categories: Hexo
tags: Hexo 部落格
description: 此篇會手把手介紹如何透過 Hexo 架設部落格部並署到 Github。
abbrlink: 1074258594
date: 2022-04-24 22:03:56
---

## Hexo 環境

Step1：下載 Hexo 環境

{% codeblock lang:shell line_number:false %}
npm install hexo-cli -g
{% endcodeblock %}

Step2：檢查 hexo 版本

{% codeblock lang:shell line_number:false %}
hexo version
{% endcodeblock %}

![](https://imgur.com/eskxR6o.png)

Step3：建立 Hexo（projectname 替換成自己的名稱）

{% note default %}
之後會用於 github repository，建議這邊 projectname 設置為 `自己 github 帳號+.github.io`
例如：[kentdoit.github.io](https://kentdoit.github.io/)。（github pages 會自動將英文轉小寫）
{% endnote %}

{% codeblock lang:shell line_number:false %}
hexo init projectname
{% endcodeblock %}

![Hexo 建立完成畫面](https://imgur.com/UQw03Y3.png)

Step4：進入 Hexo 目錄

{% codeblock lang:shell line_number:false %}
cd projectname
{% endcodeblock %}

Step5：安裝相依套件

{% codeblock lang:shell line_number:false %}
npm install
{% endcodeblock %}

Step6：在本地端把 server run 起來

{% codeblock lang:shell line_number:false %}
hexo server
{% endcodeblock %}

{% note default %}
成功的話會看到下方畫面（預設主題為 landscape）
{% endnote %}

![](https://imgur.com/P8oLEQt.png)

> 在本地端啟動 Hexo 伺服器，預設路徑為：`http://localhost:4000/`
> 這種方法只能本地瀏覽，無法從外部瀏覽，那怎麼做才能從外部瀏覽？

{% note default %}
免費仔當然就是用 {% label danger@免費%} github pages 啦～～
{% endnote %}

## 部署到 GitHub

Step1：安裝 Git 部署套件（預設 Hexo 沒有安裝）

{% codeblock lang:shell line_number:false %}
npm install hexo-deployer-git --save
{% endcodeblock %}

Step2：修改 `deploy` 參數

```diff _config.yml
deploy:
-  type: 
+  type: git # 模式
+  repo: h$$

$$ttps://github.com/KentDoIt/KentDoIt.github.io.git # 自己 GitHub repo 連結
+  branch: master # 分支
```

Step3：部署指令

{% codeblock lang:shell line_number:false %}
hexo d -g
{% endcodeblock %}

![403 error](https://imgur.com/o6lXKWf.png)

失敗的話把 repo 改為用 SSH（例如：[git@github.com](mailto:git@github.com):KentDoIt/KentDoIt.github.io.git）

Step4：查看 pages

- github 專案底下 → 點擊右上角 齒輪 Settings → 左側 Pages

{% note default %}
剛上傳會需要等一段時間，等到狀態變為綠色後點擊連結就可以看到和 local 端一樣的畫面。
{% endnote %}

![pages 狀態比較圖](https://imgur.com/CyCxriT.png)

## Hexo 目錄架構

> 下方會介紹幾個 Hexo 目錄架構中比較常用到的資料夾以及檔案。

```yaml
├── .deploy_git
├── .git
├── node_modules
├── public           # hexo g 後生成靜態網站
├── scaffolds        # 指令 new 產生的草稿、頁面、文章模板設置
|   ├── draft.md
|   ├── page.md
|   └── post.md      # 生成文章的模版
├── source
|   ├── _drafts
|   └── _posts       # 文章資料夾（markdown 檔）
├── themes           # theme 版型資料夾
|   └── landscape    # 預設 theme
|       └── _config.yml  # 版型樣式設定參數
├── .gitignore
├── _config.yml      # 基本部署設定
├── db.json
├── package.json
└── package-lock.json
```

node_modules：依照 package.json 所安裝的套件。

- 對應的指令：`npm install`

public：編譯產生靜態檔案後存放的位置。

- 對應的指令：`hexo g`

scaffolds：存放 layout 模版，Hexo 會根據 scaffolds 模板來建立檔案。（預設 layout 沒輸入就是 post）

- 對應的指令：`hexo new [layout] <title>`

source：新增文章後存放的位置。

- 對應的指令：`hexo new [layout] <title>`

themes：存放 Hexo 主題的位置，（預設 themes 為 landscape），Hexo 會依據放在該資料夾底下的主題與 _config.yml 設置來編譯產生靜態檔案。

_config.yml：Hexo 編譯網站的設定檔。

package.json：管理相依套件的版本。

.gitignore：部署時要忽略特定檔案或資料夾。

package-lock.json：主要用途是記錄當前安裝的每一個套件版本，確認是否安裝到正確的套件版本。（基本上不會動到這個檔案，只有在新增、刪除以及更新套件時，它才會有變化）

## reference

- ray [Hexo教學 Next主題設定與頁面功能(Ver. 8)](https://israynotarray.com/hexo/20190411/932826160/)
- [[教學] 使用 GitHub Pages + Hexo 來架設個人部落格](https://ed521.github.io/2019/07/hexo-install/)
- ray [(4) 試著學 Hexo - 認識 Hexo 目錄結構](https://israynotarray.com/hexo/20200917/636983586/)
