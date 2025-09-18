---
title: Hexo Sitemap 建立索引 無法擷取
abbrlink: 2629412870
date: 2022-05-10 07:51:46
categories: Hexo
tags: Hexo SEO
description: 此篇會介紹 Sitemap 所遇到的一切問題。
---

{% note default %}
此篇內容為 [Hexo 設置 GA、Sitemap（四）](https://kentdoit.github.io/hexo/599670072/)的 Debug 延續篇，介紹如何解決無法擷取以及設置索引。
{% endnote %}

## 範例的環境版本

hexo 版本：6.0.1
hexo-cli 版本：4.3.0
next 版本：7.8.0

## 無法擷取、可讀取 Sitemap，但其中含有錯誤

![](https://imgur.com/UH0jdLH.jpg)

![](https://imgur.com/Ae8Sllt.jpg)

![](https://imgur.com/tYWXLmz.jpg)

{% note default %}
看到類似以上圖片狀況，都不需要擔心。下方會分享四種親身嘗試的方法。
{% endnote %}

---

### 擁有權驗證

方法一：添加其他驗證方式（本來只有用 GA）

Step1：取得 content 值

- 記得在複製內容中只擷取 content 值就好。

![演示四步驟取得 content](https://imgur.com/N623p1p.jpg)

Step2：修改 NexT 主題中 _config.yml 參數 google_site_verification

- 貼上取得的 content 值。

```diff .git/config
# Google Webmaster tools verification.
# See: https://www.google.com/webmasters
+ google_site_verification: 25miYVaewg4Qur_GPtoF9-GhCZZJNidmq2kqy6wRE9g
- google_site_verification:
```

Step3：重新部署

{% codeblock lang:shell line_number:false %}
hexo g -d
{% endcodeblock %}

Step4：重新部署後，檢查網站中是否包含 google-site-verification

![搜尋 google-site-verification](https://imgur.com/RVLo57e.jpg)

---

### 檢查 Sitemap 格式

方法二：檢查 Sitemap 格式
> 發覺我的 sitemap.xml 格式和官方範例不同，所以需要將多的標籤移除。

![格式比較](https://imgur.com/QnPnMb0.jpg)

Step1：將 node_modules/hexo-generator-sitemap/sitemap.xml 中的 `changefreq`、`priority` 標籤移除。

Step2：重新部署
{% codeblock lang:shell line_number:false %}
hexo g -d
{% endcodeblock %}

{% note default %}
參考這篇 [Google無法讀取sitemap(一般的HTTP錯誤) 解決方式](https://zenreal.github.io/posts/7993/#google%E7%84%A1%E6%B3%95%E8%AE%80%E5%8F%96sitemap-%E4%B8%80%E8%88%AC%E7%9A%84http%E9%8C%AF%E8%AA%A4-%E8%A7%A3%E6%B1%BA%E6%96%B9%E5%BC%8F) 所提到的解法，雖然我沒有這個問題。
{% endnote %}

![我有嘗試把所有多的標籤都移除呦，但也還是沒成功。](https://imgur.com/NkFfNmd.jpg)

---

### 建立 .nojekyll

方法三：建立 .nojekyll

Step1：進入根目錄 source 資料夾底下

{% codeblock lang:shell line_number:false %}
cd source
{% endcodeblock %}

Step2：建立一個空白的 .nojekyll 檔案

{% codeblock lang:shell line_number:false %}
touch .nojekyll
{% endcodeblock %}

![](https://imgur.com/BFbbf5V.jpg)

{% note default %}
接下來設定是為了讓每次部署產生的 public 資料夾中也能存在 .nojekyll 檔案
{% endnote %}

Step3：修改根目錄 _config.yml 參數

```diff _config.yml
# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include:
+  .nojekyll
exclude:
ignore:
```

Step4：重新部署

{% codeblock lang:shell line_number:false %}
hexo cl
hexo g -d
{% endcodeblock %}

{% note default %}
參考這篇 [Sitemap (XML) partially works on GitHub Pages and cannot be fetched by Google Search Console](https://github.com/orgs/community/discussions/21599) 所提到的添加 .nojekyll。
{% endnote %}

---

### 建立索引

> 為了將自己的網址透過 Google Search Console 建立索引，提交給 Google 使搜尋引擎能更容易地搜尋到我們的網站，這個也屬於提升 SEO 的其中一種方法。

方法四：網址審查建立索引

Step1：點擊網址審查，並在上方網址列貼上部落格網址（例如：https://kentdoit.github.io/）

Step2：按下建立索引

![兩步驟建立索引](https://imgur.com/VwNKDrB.jpg)

{% note default %}
過一陣子之後再到網址審查中檢查就會看到綠色勾勾了，也代表建立索引成功了～～～
雖然不確定是時間還沒到還是因為 .nojekyll，但至少之後用其他文章連結也有成功建立索引 (੭•̀ᴗ•̀)੭
{% endnote %}

![](https://imgur.com/iR7NAzQ.jpg)

{% note default %}
「已建立索引，但未提交至 Sitemap」看到這個沒關係，只是 google 還尚未更新我們的 Sitemap，等 Sitemap 狀態變為「成功」之後，全部的索引資料也都會一併更新。
{% endnote %}

---

### 自我檢查兩步驟

Step1: robots.txt 是否有設置 Sitemap

可以參考「[Hexo 管理爬蟲 robots.txt（五）](https://kentdoit.github.io/hexo/3578178281/)」來設置 robots.txt。

![](https://imgur.com/herVKkf.jpg)

Step2: google 搜尋自己的網站

格式：`site:` 網站網址

![](https://imgur.com/xRoMj9K.jpg)

{% note default %}
google 有搜尋到、robots.txt 有設置，基本上就可以不用擔心了，接下來就交給時間了。
我自己是等超過一個禮拜還好就不管了，一個月之後看就自動好了，Google Search Console 也會都有自動找到尚未加入索引的網址。
{% endnote %}

---

## conclusion

{% note default %}
如果沒有其他錯誤訊息，基本上`時間到了`狀態自動就會變綠色「成功」。
{% endnote %}

![](https://imgur.com/65rMzvx.jpg)

## reference

[解決 Hexo + Github Pages 新增 sitemap.xml 找不到頁面](https://blog.kyomind.tw/adding-sitemap-issue/)
[解決 Google Search Console 抓不到 Hexo 放在 Github Pages 的 sitemap.xml 的問題](https://blog.balabambe.com/2021/12/01/%E8%A7%A3%E6%B1%BA-Google-Search-Console-%E6%8A%93%E4%B8%8D%E5%88%B0-Hexo-%E6%94%BE%E5%9C%A8-Github-Pages-%E7%9A%84-sitemap-xml-%E7%9A%84%E5%95%8F%E9%A1%8C/)