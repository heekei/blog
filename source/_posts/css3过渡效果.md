---
title: CSS3过渡效果
id: .nan
categories:
  - web开发
date: 2015-01-11 09:28:30
tags:
---

    &lt;!doctype html&gt;
    &lt;html&gt;
    &lt;head&gt;
    &lt;style&gt;
    #aa{
    position:absolute;
    width:50px;
    height:50px;
    border-radius:50px;
    background-color:red;
    -webkit-animation:myfirst 3s alternate infinite;
    }
    @-webkit-keyframes myfirst
    {
    0%{background:red;top:0px;left:0px;}
    25% {background:yellow;top:200px;left:50px;}
    50%{background:lightblue;top:0px;left:100px;}
    75% {background:lightgreen;top:200px;left:150px;}
    100%{background:black;top:0px;left:200px;}
    }

    &lt;/style&gt;
    &lt;/head&gt;
    &lt;body&gt;

    &lt;a id="aa"&gt;&lt;/a&gt;
    &lt;/body&gt;
    &lt;/html&gt;
    