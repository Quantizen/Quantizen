title: Kindle鏈
date: 2016-04-22 20:05:13
categories: read
tags: kindle
---

> 讀書，以使過往最大效率利用，當下最有效存在，未來最有力掌控

<!--more-->

# 下載書籍

1. Libgen，Bookzz等下載，主要外文書籍；
2. 百度雲盤，新浪微盤等下載；

# 製作書籍

1. 精裝可用Tex，[如這樣一些模板][kindletex];
2. 簡易可用Pandoc，[這裏有教程][kindlepandoc]，三四步的事兒；

# Rss推送

## Calibre 抓取

可以使用電腦軟件 Calibre 抓取推送，缺點是需要開着電腦以及這個軟件的時候才行。

## 服務應用

利用[KindleEar][KindleEar]，在[GAE][gae]上搭建自己的推送服務應用。我的服務是在：[http://kindle-solo.appspot.com/](http://kindle-solo.appspot.com/)，如有需要我可以多創建幾個帳號。

1. 有問題先在[FAQ][faqear]中搜索；
2. 參考[大智若魚的介紹][gongmkindle]，[匆匆那年的詳細介紹][ptbsarekindle]。

注意：

1. 生成和上傳app時要用終端代碼，GUI總是出錯。
2. Kindle E-mail要寫自己的kindle的郵箱。

## Rss生成

## 知乎專欄

參見[知乎日報中的介紹][zhihurss]或一般其他的Rss製作方法。目前比較好的生成知乎專欄 rss 的方法是 [lilydjwg 提供的服務](https://rss.lilydjwg.me/)。

## 一般網頁

可用 [Feed43](http://feed43.com/) 生成 rss 鏈接。步驟爲：

1. 註冊。
2. 登錄，並 點擊 [Create new feed](http://feed43.com/feed.html?action=new)。
3. 填寫要訂閱的網頁地址，Encoding 一般爲`utf-8`
4. 定義 Global search rule （搜索範圍） 及 item search pattern （搜索模式）。`{*}`表示任意字符，{% raw %}<code>{%}</code>{% endraw %} [^nunjucks]表示要匹配的內容。
5. 定義輸出模式，Feed description 好像是必填項。
6. Optional features 中可以 Change file name 以方便記憶，即自定義 feed 鏈接的網址名。

微信並沒有好的 rss 提供，可以[結合傳送門和 Feed43 自己製備](http://blog.csdn.net/lzuacm/article/details/11734051)。如果知乎專欄的方法也失效了，也可以考慮此方法。但估計不會，微信之所以打擊 rss 可能是因爲要維護自己的客戶端。

注意：

1. 訂閱自己 Hexo 博客的時候總是不顯示內容，原因是沒有在`_config.yml`中設置`yourcite.com`爲自己的網址。

# 刪除存檔

在[我的內容](https://www.amazon.cn/mn/dcw/myx.html#/home/content/booksAll/dateDsc/)中可以刪除存檔的電子書，包括電子書（購買的電子書），個人文檔（自己發送到 kindle 郵箱的）等。麻煩之處在於每次只能操作十個條目，方便的方法可以[通過`javascript`實現](https://www.zhihu.com/question/20246215/answer/14470675)（答案中腳本不一定能用）。

如果不想將個人文檔存檔，則可到[設置](https://www.amazon.cn/mn/dcw/myx.html#/home/settings/payment)中找到個人文檔設置，點擊編輯存檔設置，取消即可。


[^nunjucks]: Hexo 在處理 {% raw %}<code>{%</code>{% endraw %} 時會有 [bug](https://github.com/hexojs/hexo/issues/1777)。

[kindletex]: http://www.latexstudio.net/archives/509
[kindlepandoc]: http://www.imwsh.net/2014/01/use-pandoc-kindlegen-make-beautiful-ebooks/
[KindleEar]: https://github.com/cdhigh/KindleEar
[faqear]: http://kindleear.appspot.com/static/faq.html
[gongmkindle]: http://gongm.in/2014/12/deploy-kindle-ear-on-mac/
[ptbsarekindle]: http://ptbsare.org/2015/06/06/gae%E4%B8%8A%E6%90%AD%E5%BB%BAkindleear%E5%AE%9E%E7%8E%B0%E8%87%AA%E5%8A%A8rss%E6%8A%93%E5%8F%96%E8%AE%A2%E9%98%85/
[zhihurss]: http://daily.zhihu.com/story/7891850
[gae]: https://console.cloud.google.com/home/dashboard?project=kindle-solo
