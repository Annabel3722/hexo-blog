---
title: Python访问微信好友的打开方式
post.intro: 简单介绍两个python的处理微信接口的模块
comments: true
tags: Python
permalink: 1514739600.html
abbrlink: 2991835141
date: 2018-01-01 01:00:00
---
简单介绍两个python的处理微信接口的模块
<!-- more -->
-------------------------------------------------------------
近来微信用户越来越多，处理信息也越来越需要批量处理，下面就简单地介绍两个Python的处理微信接口的库吧
---
###### 本文的运行测试环境在Ubuntu17.10，Python3.6.3版本
---------------------------------------------------------------
### itchat
###### 这是API的介绍网址[https://itchat.readthedocs.io/zh/latest/](https://note.youdao.com/)
###### 支持微信机器人收集微信好友的个人信息、消息的收发处理、包括文字、图片等等的回复
###### 下面举个栗子，做微信好友个性签名的词云

```python3
from __future__ import print_function
import itchat
import re
import os

font=os.path.join(os.path.dirname(__file__), "arialuni.ttf")  #字体格式需要另行下载，位置为当下位置

itchat.login() #此处会有二维码生成，登录微信网页版的入口
text=""
for i in friends[1:]:
    signature=i['Signature'].strip().replace("span","").replace("class","").replace("emoji","")#此处使用正则表达式过滤一些标签名和表情包
    rep=re.compile("1f\d.+")
    signature=rep.sub("",signature)
    text+=signature

#结巴中文分词，把文本分成若干个有意义的词组
import jieba
seg_list=jieba.cut(text,cut_all=True)  #全模式分词
result=" ".join(seg_list)

#加载并生成词云
import matplotlib.pyplot as plt
from wordcloud import WordCloud
wordCloud = WordCloud(font_path=font,max_font_size=40).generate(result)
plt.figure()
plt.imshow(wordCloud)
plt.axis("off")
plt.show()
```

-----------------------
### wxpy
###### 这是API的介绍网址[https://wxpy.readthedocs.io/zh/latest/](https://note.youdao.com/)
###### 下面简单举个栗子，给我的家乡湛江好友们群发个祝福

```
from wxpy import *
bot=Bot()
zhanjiang=bot.friends().search(city="湛江")

for each in zhanjiang:
   each.send("祝愿湛江的各位小伙伴们元旦快乐~")
```
###### 同样地会有二维码生成，登录微信网页版就可以自动群发了，发送对象为个人信息备注城市为湛江的小伙伴们

###### PS:个人的建议是：虽然这个比较好玩，但是重要的话还是要自己实打实地发送出去哦，远方的亲人朋友可以打个电话寄个信，近邻可以面对面聊聊~

*最后祝福大家元旦快乐!2018年快乐!*
