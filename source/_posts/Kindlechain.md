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

## Tex 轉 mobi

自己遇到將 Tex 轉到 kindle 閱讀的情況僅限於 Arxiv 上的文章，這個時候最好直接生成 pdf 文件。因爲：

1. 有很多數學公式和格式等，轉換過程不免造成丟失混亂；
2. 圖片格式是 pdf 或 eps 的時候轉換也會有問題，需要先將格式轉換成 png 等。如 `gs -dSAFER -dBATCH -dNOPAUSE -sDEVICE=png16m -dGraphicsAlphaBits=4 -sOutputFile=shockvalue2.png shockvalue2.eps`；

自己的 Tex 模板是

```
\documentclass[17pt,a5paper,oldfontcommands]{memoir} % font sizes: 8pt, 9pt, 10pt, 11pt, 12pt, 14pt, 17pt, 20pt.
\usepackage[hmargin=1.5em,bottom=4em]{geometry}
\usepackage{graphicx}
% http://tex.stackexchange.com/questions/187237/figure-caption-does-not-continue-on-new-page
\makeatletter
\newenvironment{figurehere}
{\def\@captype{figure}}
{}
\makeatletter
```
其中`oldfontcommands`因爲[`memoir`模板對`\it, \tt`等字體命令的識別問題](http://tex.stackexchange.com/questions/129272/memoir-class-and-bibtex)。

此外有人寫了一個 python 程序[Kindlize](https://bitbucket.org/nye17/kindlize)進行轉換

如果 Tex 文件沒有上述兩個問題，可以通過 Tex to md/html (to epub) to mobi 的過程進行轉換。

**Tex to md**

```
pandoc -f latex -t markdown -o bigqbism.md bigqbism.tex

```

**md to html**

```
pandoc -s --mathjax --number-sections --bibliography=foo.bib -o output.html input.md
```

Ref: [Convert Markdown to other formats via Pandoc](http://yihui.name/knitr/demo/pandoc/)

**html to mobi**

有在線 html to mobi 的應用， 如[convertio](https://convertio.co/zh/html-mobi/)。

**md to epub**

```
pandoc meta.md book.md -o output.epub
```

關於一些 epub 元數據如章節作者等，參見 [EPUB Metadata](http://pandoc.org/README.html#epub-metadata)。

Ref: [Creating an ebook with pandoc](http://pandoc.org/epub.html)

**epub to mobi**

可以利用 kindelgen 或 Calibre。

```
kindelgen output.epub
```

## md 轉 pdf

關鍵是 template，參見 [Pandoc demo 14](http://pandoc.org/demos.html)，這裏也有[一個 template](https://github.com/karlseguin/the-little-redis-book/blob/master/common/pdf-template.tex).

```
:: A4 pdf
pandoc meta.md book.md --toc --template=zhtw --latex-engine=xelatex ^
    --no-tex-ligatures -V documentclass=scrartcl -o A4.pdf
:: 6 inch pdf
pandoc meta.md book.md --toc --template=kindle --latex-engine=xelatex ^
    --no-tex-ligatures -V documentclass=scrartcl -o output.pdf
```

另推薦[用 pandoc 制作带弹出式注释的 EPUB 和 MOBI 电子书](http://sadhen.com/blog/2015/03/06/make-ebook-using-pandoc.html)，[其 github 代碼](https://github.com/sadhen/shiji)。

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

# 網頁至 kindle

推薦 Chrome 插件：[Send to kindle](https://chrome.google.com/webstore/detail/send-to-kindle-by-klipme/ipkfnchcgalnafehpglfbommidgmalan/related?hl=zh-CN)

# 本地文件至 kindle

推薦：[ksend](https://github.com/hanan198501/ksend)。如果推送郵箱是 Gmail，則需先設置 [Access for less secure apps](https://www.google.com/settings/security/lesssecureapps)，[討論見此處](https://github.com/nodemailer/nodemailer-wellknown/issues/3)。

# 其他

## 生詞本

> 目前，生词本支持从Kindle商店购买的内容和通过Kindle个人文档服务发送到Kindle的个人文档。通过USB传输到您设备上的个人文档目前不支持生词本功能。

具體見：[阅读辅助功能](https://www.amazon.cn/gp/help/customer/display.html?nodeId=201733850)

有些已經有個人文檔標籤的書籍需要用 Calibre [去除標籤](http://www.tech-recipes.com/rx/20147/kindle-fire-how-to-display-mobi-files-as-books-and-not-documents/)後發送至 kindle 郵箱纔可以。

[^nunjucks]: Hexo 在處理 {% raw %}<code>{%</code>{% endraw %} 時會有 [bug](https://github.com/hexojs/hexo/issues/1777)。

[kindletex]: http://www.latexstudio.net/archives/509
[kindlepandoc]: http://www.imwsh.net/2014/01/use-pandoc-kindlegen-make-beautiful-ebooks/
[KindleEar]: https://github.com/cdhigh/KindleEar
[faqear]: http://kindleear.appspot.com/static/faq.html
[gongmkindle]: http://gongm.in/2014/12/deploy-kindle-ear-on-mac/
[ptbsarekindle]: http://ptbsare.org/2015/06/06/gae%E4%B8%8A%E6%90%AD%E5%BB%BAkindleear%E5%AE%9E%E7%8E%B0%E8%87%AA%E5%8A%A8rss%E6%8A%93%E5%8F%96%E8%AE%A2%E9%98%85/
[zhihurss]: http://daily.zhihu.com/story/7891850
[gae]: https://console.cloud.google.com/home/dashboard?project=kindle-solo
