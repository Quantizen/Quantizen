title: Atom
date: 2016-05-01 11:34:45
categories: mac
tags: software
---

> 當接觸到一個新的編輯器的時候怎樣快速上手呢？

<!-- more -->

明確目的：自己用編輯器無非寫一些`.md, .tex`文件。

## 安裝插件

首要之急是安裝所需要的插件。安裝插件的快捷鍵`Command+Shift+P`[但發現安裝插件總是失敗](https://segmentfault.com/q/1010000000743953)，難道和偉大的牆有關？那麼只能手動安裝了，

```
cd ~/.atom/packages
git clone <你想安装的 Package 的仓库链接> # 比如：git clone https://atom.io/packages/emmet，github 版本有時會沒有 setting 項。
cd <Package 路径> # cd emmet-atom
apm install #有時好像需要 npm install
```
或許可以寫一個 Alfred 的 workflow 精簡一下。

## 插件管理

安裝完插件之後就需要調配以適合自己的習慣。

1. Open the Settings VIew (`Cmd+,` on OS X, `Ctrl+,` on other platforms)
2. Click the Packages tab
3. Type the name of the package into the filter box
4. Find the package in the list

搜索插件：[search atom packages](https://atom.io/packages/search?utf8=%E2%9C%93&q=)

插件更新：[auto-update-package](https://atom.io/packages/auto-update-packages)

## 插件推薦

### Markdown

1. markdown-assistant，qiniu-uploader：輔助工具，上傳圖片並生成外鏈而後插入。目前只支持截取的圖片，[不支持本地圖片](https://github.com/knightli/markdown-assistant/issues/4)。
2. markdown-previewer-plus，支持更多格式的預覽
3. markdown-writer：方便書寫，也可管理博客（需要插件 hexo-generator-atom-markdown-writer-meta，該插件並不支持新的 hexo）
4. markdown-scroll-sync：單向同步滾動

### Tex

1. language-latex：語法高亮
2. latexer：自動補全，包括公式引用參考文獻
3. latex-plus：編譯，內置瀏覽，更新緩慢
4. latex-tool：編譯，[調用Skim瀏覽](https://github.com/msiniscalchi/atom-latextools/issues/26)，更新較快，仿 [ST3 的同名插件](https://github.com/SublimeText/LaTeXTools)。

## 操作快捷鍵

1. 不同文件間切換：Go to the Atom menu > Open Your Keymap and add:
```
'body':
  'ctrl-tab': 'pane:show-next-item'
  'ctrl-shift-tab': 'pane:show-previous-item'
```
2. 移動到文件開始：`cmd+up`；移動到文件結尾：`cmd+down`。
3. 選取文件中當前單詞所有出現：`ctrl-cmd-G`。
4. 多行操作：`cmd-shift-L`。
5. 切換編碼：`ctrl-shift-U`。
6. 插入`linK, Image, Table`：`cmd+shift+k, i, t`。
7. 註釋 html 代碼：`cmd+/`。
8. `esc`關閉彈出的彈出的面板。

因爲不同插件之間的快捷鍵可能有衝突，這點需要注意。

## 聯繫 Alfred

[Alfred workflow](https://github.com/franzheidl/alfred-workflows/tree/master/open-with-atom)

## 對比 Sublime

### 優點

1. 相當於一個 web app，這使得內置預覽`.md, .pdf`文件等都很方便，也可以在不同 screen 中預覽，即取消 split。
2. 基於 Github，發展前景較好。
3. 可以[自定義代碼段 snippets](http://www.jianshu.com/p/2ee34d8da142)，這能替代一些鍵盤快捷工具，如`TextExpander`。如果平常使用英文輸入法，這將是很贊的一個功能。
4. 可類似 web 的界面自定義。

### 缺點

1. 打開較慢，每次都[打開一個目錄](https://github.com/atom/atom/issues/1847)。電腦發燙。
2. 插件`markdown-writer`依然不如 Sublime 的`markdown-editing`。但`markdown-writer`管理博客時對已有標籤的提示還是很好的，有時自己會忘了寫過什麼標籤了。

綜上，決定轉向 Atom。

## 參考

1. [关于 markdown， pandoc 和 LaTeX 的入门安利](https://towdium.github.io/blog/about-markdown-pandoc-latex/)
2. [新编码神器Atom使用纪要](http://www.jeffjade.com/2016/03/03/2016-03-02-how-to-use-atom/)
