---
permalink: 1650697899
title: 通过CSS选择器定位元素
tags: 学习笔记
date: 2022-04-23 15:11:39
---
如何以CSS选择器去定位页面上的元素

<!-- more -->
CSS选择器，可以直接在浏览器控制台element选中元素copy selector，但是往往选中的元素非常长，类似于"div > div:nth-child(1) > div > div > h3 > div > a"，为了写出精简的选择器，并且可以通过选择器大概猜测出在页面的位置，学习通过id或class等去定位元素非常有必要。

常规定位
```javascript
document.querySelectorAll('.content') // 选择所有class="content"的元素
document.querySelectorAll('#content') // 选择所有id="content"的元素
document.querySelectorAll('span') // 选择所有<span>元素
document.querySelectorAll('.content,.item') // 选择所有class="content"元素和class="item"元素
```

通过父元素定位子元素
```javascript
document.querySelectorAll('.content .item') // 选择class="content"元素的class="item"子元素
document.querySelectorAll('.content>.item') // 选择所有父级是class="content"元素的class="item"子元素
```

通过子元素索引去定位元素
```javascript
document.querySelectorAll('.content:ntn-child(2)') // 选择class="content"元素的父元素的第二个子元素
document.querySelectorAll('.content:ntn-last-child(2)') // 选择所有class="content"元素的父元素的倒数第二个子元素
```

:not选择器
```javascript
document.querySelectorAll('.red:not(p)') // 选择所有class="red"而非<p>的元素
```

通过子元素去定位父元素，可以使用has伪类，但是样式表不能使用
```javascript
document.querySelectorAll('span:has(> .red)') // 选择拥有class="red"的<span>元素
```

现在组件复用前端页面中非常常见，因此定位时尽量不用一些常规的class，比如class = "select"，class = "finger"，尽量用一些这个组件特有的，以产品功能定义作为class命名的最佳。

如果遇到一些经过循环渲染的元素，个人认为，比较好的方式是通过索引和文案可以达到精准定位。