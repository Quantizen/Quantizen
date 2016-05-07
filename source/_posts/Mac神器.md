title: Mac 神器
categories: [tec]
tags: [technology, mac]
date: 2015-11-21

---

> 工欲善其事，必先利其器。

> Every tool carries with it the spirit by which it had been created.<cite>Werner Heisenberg</cite>

很大的坑，慢慢填....但愿软件节省的时间能弥补折腾软件的时间。

<!-- more -->

## Alfred

目前觉得像个人助手，Cortana和Siri是语音版的Alfred。

1. [workflow收集网站][workflow]；
2. [入门][introduction]；
3. 比如谷歌搜索的设定：
```
https://www.google.com/search?client=safari&rls=en&q={query}
```
4. `option+command+c`剪切板歷史。

### 常见问题及解答

1. [如何修改alfred的默认搜索引擎？][searchengine]
2. 文件搜索：空格＋文件名

### Hexo new

爲了創建新的博文，需要到`hexo`所在文件夾下執行`hexo new "filename"`，而後`open source/_posts/filename.md`。現在用`applescript`寫了自己的第一個`Alfred`插件，用以實現這些步驟:
```bash
tell application "Finder"
    set thePath to "/Users/Eden/Documents/Git/hexo"
    tell application "Terminal"
        activate
        do script "cd " & thePath & "; hexo new \"{query}\"" & "; open source/_posts/{query}.md"
    end tell
end tell
```
`Script Behavior`一項修改其中的`Queue Delay`爲`Custom delay after...`，並勾去`Always run immediately...`。

相應也可以定義一個`hexo server`的插件。之所以和`hexo new`分開是因爲`hexo server`不能重複執行，但有時我們需要創建不止一個新的博文。

### 顯示Arxiv搜索結果

[Arxiv居然有API接口][Arxivapi]，想到很多Alfred插件都是從各個網站API獲得的數據，所以想嘗試自己接一下Arxiv的API。其使用文檔看似有點長，其實很容易看進取。

```python
import re
import requests

#the rules of pipei
ptitle = re.compile(r'(?<=<title>).+?(?=</title>)')
pid = re.compile(r'(?<=<id>).+?(?=</id>)')
pdate = re.compile(r'(?<=<updated>).+?(?=</updated>)')
pcut = re.compile(r'(xml).+?(?=<entry>)')

#read the html in the url
r = requests.get('http://export.arxiv.org/api/query?search_query=au:del_maestro&sortBy=lastUpdatedDate&sortOrder=descending&start=0&max_results=10')
#replace does not work for unicode
data = (pcut.sub('',r.text.encode("utf-8").replace('\n',''))).decode('utf8')

#find all the links of pdf files and download them to a folder
titles = re.findall(ptitle,data)
ids = re.findall(pid,data)
dates = re.findall(pdate,data)
result = [u'<?xml version="1.0"?>', u'<items>']
for i in range(len(titles)):
    result.append(u'<item uid="arxiv' + str(i) + u'" arg="' + ids[i] + u'">')
    result.append(u'<title>' + titles[i] + u'</title>')
    result.append(u'<subtitle>'+ dates[i].decode('utf8')+u'</subtitle>')
    result.append(u'<icon>arxiv.png</icon>')
    result.append(u'</item>')

result.append(u'</items>')
xml = ''.join(result)
print xml.encode("utf-8")
```
期間遇到的困難是：

1. 對unicode編碼的字符串匹配時，匹配對象包含`\n`好像會出錯。所以有個語句替換了所有`\n`。
2. 對unicode編碼的字符串不能替換刪除等操作，可以先轉碼再替換再轉碼。
3. 最大的困難是Arxiv的API提供的直接是`xml`文件而非`json`，反而不能套用很多用到`workflow`包的例子。最終[luiyezheng的這篇博客][luiyezheng]帶入了門，作者後續還有很多給出詳細過程的博文，可以好好學習一下。

以上純屬玩耍，更加推薦的方法是用rss閱讀器訂閱相應搜索結果的feed。

### 推薦`workflow`

1. [網易雲音樂][music163alfred]搜索，[本地播放][af163local]。後者需要安裝[`chrome-cli`][ghchromecli]這個神器，在終端中運行`Chrome`。

參考: AppleScript的[Wikibook][link183255]。

[link183255]:https://en.wikibooks.org/wiki/AppleScript_Programming

Mac OS 自帶的Automator支持Diction，能夠實現語音輸入。是不是很像Siri呢？Alfred好像不支持，[也有人嘗試][someonetry]。

## aText

文本助手。

比如写博客时有时会插入音乐`it's amazing`，以往操作是：

>网易云音乐中搜索，选择，点击生成外链播放器，选择样式，复制粘贴。

而现在：

>网易云音乐中搜索，选择，地址栏中复制音乐id，`wymusic`，缩减了不必要的几步。其中触发快捷键`wymusic`设定是：
```
<p><iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=298 height=52 src="http://music.163.com/outchain/player?type=2&id=【clipboard】&auto=0&height=32"></iframe></p>
```

希望功能：

1. 支持粘贴板扩展，有时需要对先后拷贝的几个东西一起处理。
2. 支持插入到文档开头或结尾，而不限于光标所在处。
3. 1，2两点举个例子就是写`Markdown`格式的有链接的文本。需要对三个先后复制的文本一起处理，并将链接部分放至文档末尾。 目前這點是用編輯器ST3的插件[MarkdownEditing][stmegithub]實現的。

## TextExpander

文本助手，比`aText`強大在可以調用一些腳本。`TextExpander 5`設置支持中文輸入法:

1. 在用戶名目錄下新建文件夾`Dropbox`；
2. 點擊`TextExpander 5`的`Preferences`，`Sync`，勾選`Sync with TextExpander 4 ...`。
3. 退出`TextExpander 5`，下載並打開[TEIMPrefSetter][TEIMPrefSetter]，去除中文。
4. 重啓`TextExpander 5`，反做第二步，第一步。

[有用的中文介紹][techinese]。設置的第一個快捷項是插入鏈接的
```bash
[%|][link%H%M%S]

[link%H%M%S]:%clipboard
```
沒必要給每一個鏈接都取一個名字，用時間作爲編號出問題的概率是`1/(24*60*60)`。對`Alfred`寫的`applescript`腳本在`Keyboard Maestro`中也能使用。

[techinese]:http://sspai.com/27479
[TEIMPrefSetter]:https://smilesoftware.com/downloads/TEIMPrefSetter.zip

## 输入法RIME

[Mac 里的文字输入效率][mactext]中讨论的很好，关键是`输入法+快速输入文本应用`。快速输入文本应用已经选用aText，而关于输入法本不想折腾，但看到
> 那些平时被我们容忍甚至忽略的细微点滴终究会汇聚成河，时间还很长为什么不做一点改变呢？

[Rime][rime]是一个备受推崇的开源输入法，安装时用的[Homebrew cast][brewcast]的方式，具体参考[Rime定制指南][rimediy]。首要的是如何调出并切换方案：修改`switcher`中的`hotkeys`，增加` - "Control+s"`；另，翻页是`［/］`，因为`Tab/Shift+Tab`在Sublime中不能用；最后切换中英文是`Caps`，因为`Shift`在Mathematica中配合`Enter`用于执行。另，一定要注意`.yaml`格式的配置文件中空格一定要写对，配置后没有生效要先检查这一点。

其他参考[^original]：

1. [鼠须管的调教笔记][teachrime]
2. [词库添加及配置][dictrime]
3. [配置参考][refrime]

## Pocket

虽然Safari有reading list的功能，但最好还是一个和浏览器无关的应用。于是就选择了[Pocket][pocket]，Safari，Chrome等都有插件。


## Popclip

Popclip是一个[文本任意门][textany]，[popclip介绍][popclip]。平常自己最多就是选定文本后搜索，用Alfred多出一步就可解决，没必要多装一个软件。而像选定翻译反而不如系统的手势方便。

## Keyboard Maestro

也是通过快捷键自动完成一系列操作的工具，目前无需求。[介紹][kmintro]。

[kmintro]:http://sspai.com/28721

## Copyless

粘贴板增强工具，无太大需求。

## 1Password

挺有用的，但是还是不会选择记录和个人信息相关的密码。

## DEVONthink

一直希望个人的文件能像网页那样，内容也能被检索，因为文件名设置的再好也不能包含所有文件的信息。DEVONthink或许是一个可行的方案。

## Omnifocus

被称为“外脑”（out brain）的事务规划管理软件，希望能用来解放一部分大脑。另，DEVONthink和Omnifocus的云存储用支持[WebDAV的坚果云][webdavjgy]。

## Gitit

個人維基的建立也是很重要的，有利於將知識系統化。我選擇個人維基有兩個標準：

1. 基於`Markdown`文件，這樣書寫方便且結合`Sublime`的插件能實時查看效果。
2. 通過網頁展示，這樣能夠實現多標籤打開。凡是美妙的東西都不是一根弦。
3. 維基不同於筆記，在於能否在不同頁面間方便跳轉，次級標題也能跳轉。

目前我只接觸過：

1. [`MDwiki`][mdwiki]，轉換有問題，比如不支持腳註。再者不支持次級標題鏈接，只能鏈接到一個頁面。這樣的話，只能算作個人筆記，`Hexo`的[`Wixo`][wixo]主題也能做到。優點是安裝簡單，直接`git clone`就可以。
2. [`Quiver`][quiver]，基於本地的編輯器，不支持多標籤，也是只能鏈接到一個頁面。新鮮點的特點是`Code Cell`，並不像[`Jupyter`][jupyter]可以運行，只是看的話`Pandoc`也能做到。
3. [`Gitit`][Gitit]，作者`jgm`就是`Pandoc`的作者，其他的都不用說了吧。缺點是安裝然後弄明白稍微複雜。
4. [Simiki][simiki]，Simple wiki，不知都實現了[哪些功能][simikif]。

## Dash

Dash是語言文檔管理器，當用到的語言或擴展較多的時候很有用。吐槽一下`Python`的`Numpy`等的說明文檔一百多兆那麼大，都沒敢下。

語言的文檔能直接找到，`Python`的宏包的文檔需要[生成後導入][pythonpac]。

## Sublime text 或 Atom

[ST3 快捷鍵][sublimetsc]。

## Homebrew

Homebrew 管理沒有圖形界面的軟件，[Homebrew-cask](https://github.com/caskroom/homebrew-cask/blob/master/USAGE.md)管理有圖形界面的軟件，如 QQ 等。快捷之處在於可以一條命令 `brew cask install app1 app2 ...` 安裝很多軟件，省去一個個手動處理的麻煩。[Homebrew 切換國內的源](https://www.zhihu.com/question/31360766)：

```
cd /usr/local
git remote set-url origin git://mirrors.ustc.edu.cn/brew.git
```

常用命令：

```
brew install / search / uninstall / update / upgrade / tap / outdated / list / cleanup / info / doctor / prune
brew cask install / search / uninstall / info / list / cleanup
```

注意：先`update`而後`upgrade`。

Homebrew-cask [暫時沒有 upgrade 的功能](https://github.com/caskroom/homebrew-cask/issues/4678)，[或可用`install --force`替代](https://github.com/caskroom/homebrew-cask/blob/master/USAGE.md#updatingupgrading-casks)。

## Quicklook

不用打開，空格鍵直接預覽的自帶軟件。推薦的插件有：

1. `brew cask install qlmarkdown` 安裝 Markdown 預覽（QuickLook）插件.
2. `brew cask install qlcolorcode` 代碼塊高亮.
3. `brew cask install qlvideo` 視頻預覽插件.
4. [以及一些其他的](http://sspai.com/31927).

## 在线应用

1. [格式表格生成][tablegenerator].
2. [Google trends][googletrends].
3. [各網站音視頻下載][audioredio].
4. [碩鼠下載音視頻][flvcd].
5. [知乎專欄 rss](https://github.com/lilydjwg/morerssplz).

> 工欲善其事，勿只善其器。


<p><iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=298 height=52 src="http://music.163.com/outchain/player?type=2&id=18784004&auto=0&height=32"></iframe></p>

## Chrome plugins

1. Surfingkeys，類似的有vimium及chromium-vim，但Surfingkeys支持自定義。`alt-s`用於開關插件。

## Shortcat

用鍵盤操縱所有應用的界面，但還不成熟，也太極致了。

參考：

[Best app][bestapp]

[^original]:[Mac OS自带输入法的一些技巧][osinput]

[pocket]:https://getpocket.com
[workflow]:http://alfredworkflow.com
[introduction]:http://blog.okeyang.com/blog/2015/07/15/alfredpei-zhi/
[popclip]:http://mp.weixin.qq.com/s?__biz=MjM5MjAyNDUyMA==&mid=205638015&idx=2&sn=bf9f37e955620cc872531ec8beedc93e&3rd=MzA3MDU4NTYzMw==&scene=6#rd
[textany]:http://www.virgoer.com/archives/1210

[searchengine]:http://www.zhihu.com/question/22055110

[webdavjgy]:http://help.jianguoyun.com/?p=2064


[refrime]:http://www.dreamxu.com/install-config-squirrel/
[dictrime]:http://www.jianshu.com/p/cffc0ea094a7
[teachrime]:https://medium.com/@scomper/鼠須管-的调教笔记-3fdeb0e78814#.d82g9z3ar
[rimediy]:https://github.com/rime/home/wiki/CustomizationGuide
[brewcast]:http://caskroom.io
[osinput]:http://zhan.renren.com/applelife?gid=3602888498002502491&checked=true
[rime]:http://rime.im/download
[mactext]:http://sspai.com/31525
[mdwiki]:https://github.com/Dynalon/mdwiki
[quiver]:https://itunes.apple.com/us/app/quiver-programmers-notebook/id866773894?mt=12
[Gitit]:https://github.com/jgm/gitit
[wixo]:http://hahack.com/codes/hexo-theme-wixo/
[jupyter]:https://jupyter.org
[tablegenerator]: http://www.tablesgenerator.com/html_tables
[googletrends]: https://www.google.com/trends/?hl=zh-CN
[audioredio]: https://www.hello1995.com/
[bestapp]: https://github.com/hzlzh/Best-App
[music163alfred]: https://github.com/goodbest/Alfred_Workflow_Music163
[af163local]: https://github.com/iveney/music.163.workflow/
[ghchromecli]: https://github.com/prasmussen/chrome-cli
[simiki]: https://github.com/tankywoo/simiki
[simikif]: https://github.com/tankywoo/simiki/issues/46
[stmegithub]: https://github.com/SublimeText-Markdown/MarkdownEditing
[flvcd]: http://www.flvcd.com/url.php
[Arxivapi]: http://arxiv.org/help/api/user-manual
[luiyezheng]: http://luiyezheng.github.io/%E4%BC%98%E7%A7%80app%E6%8E%A8%E8%8D%90/2015/12/26/Alfred1.html
[pythonpac]: http://technosophos.com/2014/03/12/importing-python-docs-into-dash.html
[sublimetsc]: http://qiudeqing.com/tools/2015/05/31/sublime-text-3.html
[someonetry]: http://www.alfredforum.com/topic/5413-alfred-dictation-keyboard-free-power/
