---
title: javascript onmousemove事件遇到iframe
id: .nan
categories:
  - web开发
date: 2016-10-05 14:41:35
tags:
---

今天在做论文的时候，遇到了个问题：
html代码是这样的：

```html
    <div class="main">
      <div class="titlebar"></div>
      <div class="content">
        <iframe></iframe>
      </div>
    </div>
```

我给titlebar添加了功能：按住titlebar，然后移动整个main，移动过程需要document的mousemove事件。如果移动得快了的话，会出现停顿，必须将鼠标移到main外面才能继续移动。
想了半天，原来这个问题的根本原因在于，移动时鼠标会跑进iframe里面去，此时，document的mousemove就会失效，只有当我把鼠标移出iframe才会继续main的移动。于是乎，谷歌了一下，然后看了下别人是怎么解决的。
于是我在main里面加了一个透明遮罩，默认是隐藏，按住titlebar时，显示遮罩，放开titlebar时，隐藏遮罩。于是问题就迎刃而解了。

```html
  <div class="main">
    <div class="titlebar"></div>
    <div class="content">
      <iframe></iframe>
    </div>
    <div class="iframeCover" style="display:none;"></div>
  </div>
```