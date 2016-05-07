title: Tex中的參考文獻
date: 2016-01-24 14:17:02
categories: tex
tags: tex
---

> 工具可以塑造人，可以拯救人。

<!--more-->

# 概述

文獻管理工具我用的是`Zotero`，可以方便地導出爲`.bib`文件用到Tex中。 在`Tex`中寫參考文獻，目前所知有四種方法：

1. `thebibliography`，手工在文檔中編寫格式等；
2. `bibtex`，文獻管理。支持應用廣泛，但格式`.bst`文件難編寫修改；
3. `biber`，需要結合`biblatex`使用。目的是取代`bibtex`作爲`biblatex`的引擎，`bibtex`在其中只起到了排序和生成`.bbl`文件的作用，所以其格式`.bst`文件並不是用於`biblatex`；
4. `natbib`，只有`bibtex`作爲引擎，重新寫了`cite`等命令以豐富引用樣式；

[SE上的綜合評價][sebiblat]是：`biblatex`是大勢所趨，但投稿什麼的還是用`bibtex`吧，如APS等雜誌提供有[`.bst`文件][apsbst]。

# 安裝格式文件

## 安裝`.bst`文件

一般能在`CTAN`上找到，然後用`tlmgr install`安裝即可。手動：

1. 下載BST文件，將其復制到`$(TEXMFLOCAL)/bibtex/bst/`，其中`$(TEXMFLOCAL)`是你的機器上的本地的`texmf`目錄，您可以通過執行`kpsewhich --var-value=TEXMFLOCAL`獲得該目錄路徑；
2. 通過執行`texhash`刷新`texmf`目錄索引，註意執行該命令可能需要系統管理員權限；
3. 如果您使用Linux或Mac OS X操作系統，您也可以通過項目所提供的Makefile腳本進行安裝，只需在項目所在目錄下運行以下命令：`sudo make`，註意需要通過sudo切換root權限。

## 安裝`.bbx, .cbx`文件

一般能在`CTAN`上找到，然後用`tlmgr install`安裝即可。手動可以參考SE上[這個回答][seinstallbbx]。

# 使用例子

## `Bibtex`

```
\documentclass[aps,prl,reprint]{revtex4-1}

\begin{document}
\title{Some random title}
\author{Me}
\email{mail@example.com}
\author{Myself}
\author{Someone Else}
\affiliation{A University}

\begin{abstract}
Here I tell what I have done... And I have done a lot but it is hard to tell what exactly I have done...
\end{abstract}

\maketitle
\cite{article-minimal}

\bibliographystyle{apsrev4-1} % Tell bibtex which bibliography style to use
\bibliography{xampl} % Tell bibtex which .bib file to use (this one is some example file in TexLive's file tree)

\end{document}
```

## `Biblatex`

```
\documentclass[]{article}

\usepackage[
    style=phys,
    sorting=ynt, %排序方法為year name title
    %biblabel=brackets,
    %sortlocale=de_DE,
    %natbib=true,
    url=false, 
    doi=false,
    %eprint=false,
    firstinits=false,
    maxnames=50 %盡量顯示所有作者
]{biblatex}

\DeclareNameAlias{default}{first-last} %作者名稱格式
\DeclareFieldFormat{labelnumberwidth}{{#1}\adddot} %文獻序號格式
      \setlength{\biblabelsep}{10 pt}

\addbibresource{mypublish.bib}

\usepackage[]{hyperref}
\hypersetup{
    colorlinks=true,
}

\begin{document}
\nocite{*}  
\printbibliography[title={all reference}] %這兩句打印所有參考文獻
\citetitle{reference1}
\end{document}
```

個人決定使用`biblatex`的方案。投稿或傳送給其他人時，將生成的`.bbl`文件中的內容拷到或引用到主文件中並註釋掉`bibtex，biblatex`的內容即可。

`biblatex`參考文獻格式設計很好寫，具體功能實現可以從別人的格式定義文件中找尋方法。

>有意義的是想法設計，臟活累活都應交給代碼。 

參考：

1. [biblatex 參考文獻和引用樣式][bibcasper]；
2. [`natbib`自然科學引文和參考文獻][natbibzh]；
3. [使用 BibTeX 生成參考文獻列表][introbibt]介紹`bibtex`的原理；
4. [REVTeX 4.1][revtex41]使用指南；
5. [工具的作用][gjdzy]；
6. [Biblatex: submitting to a journal][subbibl]；


[apsbst]: https://journals.aps.org/files/revtex/revtex4-1.zip
[bibcasper]: http://mirrors.ustc.edu.cn/CTAN/macros/latex/contrib/biblatex-contrib/biblatex-caspervector/doc/readme.pdf
[sebiblat]: http://tex.stackexchange.com/questions/25701/bibtex-vs-biber-and-biblatex-vs-natbib
[natbibzh]: http://www.latexstudio.net/wp-content/uploads/2015/02/natbib-zh.pdf
[seinstallbbx]: http://tex.stackexchange.com/a/10720/34983
[introbibt]: http://liam0205.me/2016/01/23/using-bibtex-to-generate-reference/
[revtex41]: https://journals.aps.org/revtex
[gjdzy]: http://www.jianshu.com/p/8cfd2c340114
[subbibl]: http://tex.stackexchange.com/questions/12175/biblatex-submitting-to-a-journal
