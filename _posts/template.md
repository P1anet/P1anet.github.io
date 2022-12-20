---
layout: post # page, default, keynote, null
title: "title"
subtitle: "🎞  \"subtitle\" "
date: YYYY-MM-DD # hh:mm:ss
author: "Kenshin"
header-style: text
header-img: "img/***.jpg"
header-img-credit: "@WebdesignerDepot"
header-img-credit-href: "medium.com/@WebdesignerDepot/poll-should-css-become-more-like-a-programming-language-c74eb26a4270"
header-mask: 0.3
header-bg-css: "linear-gradient(to right, #24b94a, #38ef7d);"
# iframe:     "//huangxuan.me/js-module-7day/" # for keynote
nav-style:  "invert"
navcolor:   "invert"
hidden: true
catalog: true # default
published: false
multilingual: true
lang: en
mathjax: tru
plchart: true
tags:
  - tag1
  - tag2
---

[link1](#linked)
...

<p id = "linked"></p>

[link2](http://***/)

![image1](/img/in-post/***.png)

---上为空行即分割线，---上为文字即标题，标题前自带分割线

`highlight`
**bold** <!-- old_fashioned: __bold__ -->
_italic_ <!-- old_fashioned: *italic* -->

- <!-- old_fashioned: * -->

> blockquote

<div class="visible-md visible-lg">
<img src="https://huangxuan.me/js-module-7day/attach/qrcode.png" width="350"/>
<small class="img-hint">你也可以通过扫描二维码在手机上观看</small>
<a href=https://github.com/Huxpro/js-module-7day>github</a>
</div>

<img src="http://www.mobilexweb.com/wp-content/uploads/2015/09/back.png" alt="backbutton" width="320" />

<img class="shadow" width="320" src="/img/in-post/post-js-version/keep-calm-and-learn-javascript.png" />
<small class="img-hint">来学 JavaScript 吧！</small>

```html
<p> p is paragraph! </p>

<style>
p { color: red;}
</style>
```

<pre>
保留 文本 中的
空格 ⇌和 换行
</pre>

<iframe src="https://huangxuan.me/forcify/" style="
    width:100%;
    height:500px;
    border: 0;
"></iframe>

<iframe 
  id="chart"
  src="https://huangxuan.me/PL-chart/"
  frameborder="0" 
  scrolling="no" 
  style="width: 100%;height: 100%">
</iframe>


<!-- Chinese Version -->
<div class="zh post-container">
    {% capture about_zh %}
    {% include posts/2017-07-12-upgrading-eleme-to-pwa/zh.md %}
    {% endcapture %}
    {{ about_zh | markdownify }}
</div>

<!-- English Version -->
<div class="en post-container">
    {% capture about_en %}
    {% include posts/2017-07-12-upgrading-eleme-to-pwa/en.md %}
    {% endcapture %}
    {{ about_en | markdownify }}
</div>


| table | 111111 | 222222 | 33333 |
| ----- | :----: | :----- | ----: |
| 等    |   中   | 左     |    右 |

<div>
    <blockquote>
        line1
        <br>line2
    </blockquote>
    <br>
    <a href="http://taobao.com" class=" external" target="_blank" rel="nofollow noreferrer">
        <span class="invisible">http://</span>
        <span class="visible">taobao.com</span>
        <span class="invisible"></span>
        <i class="icon-external"></i>
    </a>
    <br>
    <a data-hash="8f7d284bb1a97deaa4533a6190206ecb" href="http://www.zhihu.com/people/8f7d284bb1a97deaa4533a6190206ecb" class="member_mention" data-editable="true" data-title="@覃浩tommy" data-tip="p$b$8f7d284bb1a97deaa4533a6190206ecb">@覃浩tommy</a>
    <br> <!-- \n -->
    <img class="shadow" src="/img/in-post/***.jpg" width="260">
    <br>
    <b>bold</b>
    <br>
    <i>italic</i>
    <br>
    <u>underline</u>
    <br>
    <p>paragraph</p>
    <ul>
        <li>list1</li>
        <li>list2</li>
    </ul>
    <ul>
        <ul>
            <li>list1</li>
            <li><b>list2</b></li>
        </ul>
    </ul>
    
</div>
