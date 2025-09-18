---
title: Hexo 設置 GA、Sitemap（四）
abbrlink: 599670072
date: 2022-05-01 14:10:25
categories: Hexo
tags: Hexo SEO
description: 此篇介紹如何為部落格設置 GA 以及 Sitemap。
---

{% note default %}
會把這篇內容放在前面是因為我深受其害，因此我誠心建議這些設定越早弄完越好。
{% endnote %}

## 範例的環境版本

hexo 版本：6.0.1
hexo-cli 版本：4.3.0
next 版本：7.8.0

## Google Analytics

> 下面分成兩個方向來設置：設置追蹤碼、埋追蹤碼。

{% note default %}
想看詳細過程介紹可以參考：Gui [Day 12：為 Hexo 裝設 Google Analytics，追蹤你的部落格流量（使用 Next 佈景主題）](https://guiblogs.com/hexo30-12/)
{% endnote %}

### 設置追蹤碼（製評估 ID）

Step1：[Google Analytics](https://analytics.google.com/analytics/web/provision/#/provision) 申請帳號
Step2：複製評估 ID（`G-` 開頭）
Step3：修改 NexT 主題中 _config.yml 參數

```diff themes/hexo-theme-next/_config.yml
# Google Analytics
google_analytics:
-  tracking_id: # <app_id>
+  tracking_id: #貼上剛剛複製的評估 ID
  # By default, NexT will load an external gtag.js script on your site.
  # If you only need the pageview feature, set the following option to true to get a better performance.
  only_pageview: false
```

### 埋追蹤碼

Step1：複製追蹤碼

![步驟演示：資料串流 →  全域網站代碼 → 複製](https://imgur.com/mjvgxfs.jpg)

Step2：檔案 themes/hexo-theme-next/layout/_partials/head/head.swig 最後貼上追蹤碼

```diff themes/hexo-theme-next/layout/_partials/head/head.swig
{{ next_config() }}
+ <!-- Global site tag (gtag.js) - Google Analytics -->
+ <script async src="https://www.googletagmanager.com/gtag/js?id=G-K8QW91NNBQ"></script>
+ <script>
+   window.dataLayer = window.dataLayer || [];
+   function gtag(){dataLayer.push(arguments);}
+   gtag('js', new Date());
+ 
+   gtag('config', 'G-K8QW91NNBQ');
+ </script>
```

Step3：重新部署

{% codeblock lang:shell line_number:false %}
hexo g -d
{% endcodeblock %}

{% note default %}
完成上面兩個設置就可以重新部署後再次檢查 GA 是否成功。
{% endnote %}

![進到 GA 中點選左邊側邊欄報表，有看到數字 1 就成功了！](https://imgur.com/60AW6rA.jpg)

---

## Sitemap

> Sitemap 用來記錄該網站中所有 url 路徑，以及每一個路徑最後修改的時間戳。

{% note default %}
Sitemap 檔案會提供網頁中的相關資訊給搜尋引擎，Google 的爬蟲會定期爬 sitemap.xml 檔案，若有更新他就會去爬你的網站並加入到搜尋引擎中。
{% endnote %}

Step1：安裝指令

{% codeblock lang:shell line_number:false %}
npm install hexo-generator-sitemap
{% endcodeblock %}

Step2：根目錄 _config.yml 在最後面添加新參數

- 之後只要指令有 `hexo g` 時，就會自動生成 sitemap.xml。

```diff .git/config
+ # Sitemap
+ sitemap:
+   path: sitemap.xml
```

Step3：重新部署

{% codeblock lang:shell line_number:false %}
hexo d -g
{% endcodeblock %}

Step4：[Google Search](https://search.google.com/search-console/about?hl=zh-tw) 設置加入網站連結

- GA 屬於一種驗證方式，剛剛有成功設置 GA 所以會自動驗證成功。

![](https://imgur.com/MCx0dFO.jpg)

Step5：提交 sitemap

- 在新增 Sitemap 欄位中輸入 `sitemap.xml`

![](https://imgur.com/D2VrXHx.jpg)

Step6：檢查 sitemap

- 檢查自己的網址後面加上 `/sitemap` 或 `/sitemap.xml` 後查看是否有內容。

![https://kentdoit.github.io/sitemap](https://imgur.com/2yYRM6p.jpg)

{% note default %}
如果**運氣好**到這邊應該是成功了，~~運氣不好~~會發覺網址看有內容，但是狀態為`無法擷取`。
{% endnote %}

![](https://imgur.com/Ae8Sllt.jpg)

{% note default %}
礙於 debug 篇幅過長，將過程記錄在另一篇 [Hexo Sitemap 建立索引 無法擷取](https://kentdoit.github.io/hexo/2629412870/) 文章中。
{% endnote %}

---

## reference

[Hexo搜尋引擎優化](https://israynotarray.com/hexo/20190514/2072033203/)
[Hexo 搭建系列 - SEO優化篇](https://zenreal.github.io/posts/7993/)
[Hexo 架站攻略 - SEO 優化](https://syj0905.github.io/hexo/20190906/285599935/)
