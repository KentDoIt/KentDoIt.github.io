---
title: Hexo 管理爬蟲 robots.txt（五）
categories: Hexo
tags: Hexo 優化
description: 此篇介紹如何設置 robots.txt 來管理爬蟲檔案爬取的權限。
abbrlink: 3578178281
date: 2022-05-04 20:26:06
---

## 範例的環境版本

hexo 版本：6.0.1
hexo-cli 版本：4.3.0
next 版本：7.8.0

## robots.txt

> 設置搜尋引擎爬蟲哪些是可以爬的路徑哪些是不能爬的，也能增強 SEO。

Step1：根目錄 source 資料夾底下新增 robots.txt 檔案

![](https://imgur.com/z1hRLbs.jpg)

Step2：設置檔案參數

User-agent : 爬蟲程式名字，必要項目

- 可以在每項規則中指定一或多個 user-agen
- *：運行於所有搜尋引擎，也可以只允許指定搜尋引擎，例如`User-agent: Googlebot`。

Allow：允許爬取路徑，至少要有一個

- 相對路徑；若為目錄，則必須以 / 作為結尾。

Disallow：不允許爬取路徑，至少要有一個

- 相對路徑；若為目錄，則必須以 / 作為結尾。

Sitemap：網站地圖的路徑，非必要

- 使用絕對路徑。

Crawl-delay：同伺服器每次連續請求所需的要間隔秒數，非必要

```txt source/robots.txt
User-agent: *
Allow: /
Allow: /archives/
Allow: /categories/
Allow: /tags/ 
Disallow: /vendors/
Disallow: /fonts/
Disallow: /vendors/
Disallow: /fancybox/
Sitemap: https://kentdoit.github.io/sitemap.xml
```

{% note default %}
[ray 大](https://israynotarray.com/other/20210627/3588736352/)：『建議不要忽略 js、css 與 fonts 相關的檔案與資料夾，否則可能在 Google Console Search 會收到一些問題提示』。
{% endnote %}

延伸閱讀：[快速掌握 robots.txt 用途](https://israynotarray.com/other/20210627/3588736352/)

---

## Front-matter

{% note default %}
前面介紹了透過 robots.txt 設置不允許爬蟲獲取路徑，那如果有特定的頁面不希望被加入 sitemap.xml 中，可以添加 Front-matter 參數。
{% endnote %}

在頁面的 markdown 中 Front-matter 設定 sitemap: false。

```diff text.md
title: 測試
date: 2022-05-01 14:10:25
categories:
tags: 
description:
+ sitemap: false
```

{% note default %}
否則預設重新部署後，若有更新路徑都會自動更新到 sitemap.xml 中。
{% endnote %}

---

## reference

[Hexo 基本配置](https://blog.yucheng.me/post/hexo-configuration/#sitemap-xml-%E6%90%9C%E5%B0%8B%E5%BC%95%E6%93%8E%E7%88%AC%E8%9F%B2)
[快速掌握 robots.txt 用途](https://israynotarray.com/other/20210627/3588736352/)