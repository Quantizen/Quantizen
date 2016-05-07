title: 第一個小蜘蛛
date: 2015-11-28 11:38:34
categories: [tec]
tags: [python, alfred]
---

> 多少在Linux上折騰的騷年，最後都歸於mac的麾下

日前需要下載包含“quantum contextuality”的所有PRL的文章，手動一個個下載會很無聊。於是想用一個workflow解決，但目前只寫出了Python程序，還沒有融合到Alfred中。

<!--more-->

解決思路是：

1. 在[arxiv][arxiv]中搜索`發表雜誌是PRL`+`摘要中包含quantum contextuality`的所有文章；
2. 獲取相應文章的pdf文件的鏈接，需要用到正則表達式進行匹配；
3. 下載到特定文件夾；

```bash
import re
import requests
import urllib

#the rules of pipei
p = re.compile(r'(?<=/pdf/).+?(?=\")')

#read the html in the url
r = requests.get('http://arxiv.org/find/all/1/AND+jr:+AND+Lett+AND+Phys+Rev+abs:+AND+quantum+contextuality/0/1/0/all/0/1')
data = r.text

#find all the links of pdf files and download them to a folder
link_list = re.findall(p,data)
for url in link_list:
    urllib.urlretrieve('http://arxiv.org/pdf/'+url+'.pdf', '要下載到的文件夾/'+url+'.pdf')
```

感慨：

1. 爲什麼[^why]不直接在PRL中下載呢？因爲PR系列下載文章之前需要一個驗證：點擊Einstein的圖片。而且即使驗證過了，雖然能在網頁中正常打開pdf文件，但用`urllib，wget`下載的都不是pdf文件。以前還覺得這個很有趣，但現在成了跨不過去的鴻溝。
2. 還是arxiv人性化，會在頁面上自動給出搜尋結果的鏈接： 
```code
http://arxiv.org/find/all/1/AND+jr:+AND+Lett+AND+Phys+Rev+abs:+AND+quantum+contextuality/0/1/0/all/0/1
```
3. 正則表達式入門較簡單，基本的語法規則，但能寫出正確的匹配規則並進一步優化規則還是有點困難的。希望熟能生巧吧。		

<p><iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=298 height=52 src="http://music.163.com/outchain/player?type=2&id=19192372&auto=0&height=32"></iframe></p>

---------

[wuyidexinsheng写抓取知乎数据的爬虫][wuyidexinsheng]，可以通过邮件等多种方式监测自己知乎的更新，比如特别关注的一个人的更新。[这里][hereint]有对其代码的解释。

参考：[Python正则表达式指南][link195025]。

[link195025]:http://www.cnblogs.com/huxi/archive/2010/07/04/1771073.html


[arxiv]:http://arxiv.org

[^why]: 爲什麼`爲什麼`在簡繁轉換的時候`爲`保持不動呢？為什麼`為什麼`在簡繁轉換的時候`為`卻正常呢？原來「為」是繁體的書寫體，「爲」是繁體的印刷體。 

[hereint]:http://www.bkjia.com/ASPjc/945869.html
[wuyidexinsheng]:http://blog.csdn.net/wuyidexinsheng/article/details/42964707
