---
title: Python批量处理中文文本并解析关键词
tags:
  - Python
  - 数据处理
permalink: 1518136467
date: 2018-02-09 08:34:27
---
介绍几个处理文本的模块。
<!-- more -->
本文的使用环境在Ubuntu17.10、Pycharm 2017.3编辑器，以及Python3.6.3。

- **Wordcloud, 可以将文本中的关键词总结并输出词云**
https://github.com/amueller/word_cloud
- **jieba，像英语文章的单词那样将中文文本分成若干个词组**
- **docx，python处理doc、docx文本的模块**
http://python-docx.readthedocs.io/en/latest/index.html
- **snownlp，可以对文本进行正负面情感分析取值**
https://github.com/isnowfy/snownlp
- **pyLDAvis，直观地在默认网页显示关键词的可交互动态图**
- **textrank4zh，可以总结关键词出现的频率、关键词、关键句**
https://github.com/letiantian/TextRank4ZH

-------------------

### 模块安装和导入

> pip3 install wordcloud
> pip3 install jieba
> pip3 install python-docx
> pip3 install snownlp
> pip3 install pyLDAvis
> pip3 install textRank4zh

### 读入文本
若是docx文本，以下为处理一个文本的代码，批量处理可用数组储存文件名再遍历

```python
import docx
file=docx.Document("example.docx")#可修改文件名
text=" "
for para in file.paragraphs:
    text+=para.text
```
若是单个txt文本，
```python
text = "example.txt" #可以修改文件名
with open(text,"r",encoding='gbk',errors='ignore') as f:
    mytext=f.read()
```
### 生成词云
```python
from os import path
from wordcloud import WordCloud
import os
d = path.dirname(__file__)#该文件所在的文件夹路径

font=os.path.join(os.path.dirname(__file__), "arialuni.ttf")#arialuni.ttf为字体的包

# 读入文本
mytext = "santi.txt"   #可更换文件名
with open(mytext,"r",encoding='gbk',errors='ignore') as f:
    text=f.read()

# Generate a word cloud image加载词云
wordcloud = WordCloud().generate(text)

# Display the generated image:
# the matplotlib way:
import matplotlib.pyplot as plt
plt.imshow(wordcloud)
plt.axis("off")

# lower max_font_size
wordcloud = WordCloud(font_path=font,max_font_size=40).generate(text)
plt.figure()
plt.imshow(wordcloud)
plt.axis("off")
plt.show()

# The pil way (if you don't have matplotlib)
#image = wordcloud.to_image()
#image.show()
```
### 提取关键词和分类
```python
from sklearn.feature_extraction.text import TfidfVectorizer,CountVectorizer
from sklearn.decomposition import LatentDirichletAllocation
#从文本中提取1000个最重要的特征关键词
n_features = 1000
tf_vectorizer = CountVectorizer(strip_accents = 'unicode',
                                max_features=n_features,
                                stop_words='english',
                                max_df = 0.5,
                                min_df = 10)
tf = tf_vectorizer.fit_transform(df.content_cutted)
#用LDA算法将主题分为5类
n_topics=5
lda = LatentDirichletAllocation(n_topics=n_topics,max_iter=50,
                               learning_method='online',
                               learning_offset=50,
                               random_state=0)
lda.fit(tf)
#输出五个主题的前若干个关键词的函数定义
def print_top_words(model,feature_names,n_top_words):
    for topic_idx,topic in enumerate(model.components_):
        print("Topic #%d:" % topic_idx)
        print(" ".join([feature_names[i]
                        for i in topic.argsort()[:-n_top_words -1 :-1]]))
        print()
#输出每个主题的前20个关键词
n_top_words=20
tf_feature_names=tf_vectorizer.get_feature_names()
print_top_words(lda,tf_feature_names,n_top_words)
#关键词和主题生成可交互的动态图
import pyLDAvis
import pyLDAvis.sklearn
pyLDAvis.enable_notebook()
pyLDAvis.sklearn.prepare(lda, tf, tf_vectorizer)
data = pyLDAvis.sklearn.prepare(lda, tf, tf_vectorizer)
pyLDAvis.show(data)
```

### 统计关键词出现的频率、关键词组和摘要
可直接参考github https://github.com/letiantian/TextRank4ZH。



### 情感分析
可直接参考github https://github.com/isnowfy/snownlp。
