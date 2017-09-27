---
title: css笔记
id: .nan
categories:
  - web开发
date: 2016-10-17 09:06:00
tags:
---

## 1. 实现浮动元素的两端对齐

![](https://odbs277x0.bkt.clouddn.com/16-9-22/63994819.jpg)

**方法一**：将容器的宽度设置大一些，并且设置overflow:hidden;

如下图所示：
![](https://odbs277x0.bkt.clouddn.com/16-9-22/73211011.jpg)

**缺陷**：只支持固定尺寸,对于自适应的尺寸支持效果不好;


**方法二**：设置margin-left,并给first-child设置margin-left为0。

**缺陷**：适用于单行，多行需要使用js控制nth-child.


**完美解决方法**：

```html
    <div class="threeCol-pic">
        <img src="">
        <img src="">
        <img src="">
        <span class="justify_fix"></span>
    </div>
```
```css
    .threeCol-pic{
        text-align: justify;
    }
    .threeCol-pic img{
        display: inline-block;
        text-align: center;
    }
    .justify_fix{
        display:inline-block; 
        width:100%; 
        height:0; 
        overflow:hidden;
    }
```

## 2. 实现竖向文字

**方法**：通过增大文字父容器的padding-left 和padding-right，使content区域中的文字被迫换行，从而实现竖向文字。
**注**：writing-mode 的支持性不好，需要写hack ，因此我认为使用上述方式更为有效。

## 3. 关于CSS3 box-sizing属性

**content-box**：

padding和border**不被包含**在定义的width和height之内。对象的实际宽度等于设置的width值和border、padding之和，即 ( Element width = width + border + padding ) 此属性表现为标准模式下的盒模型。

**border-box**：

padding和border**被包含**在定义的width和height之内。对象的实际宽度就等于设置的width值，即使定义有border和padding也不会改变对象的实际宽度，即 ( Element width = width ) 此属性表现为怪异模式下的盒模型

注：**IE11下，input元素默认box-sizing为border-box**

## 4. margin:auto实现绝对定位元素的居中

首先，我们来看下CSS代码：
```css
    .element {
        width: 600px; height: 400px;
        position: absolute; left: 0; top: 0; right: 0; bottom: 0;
        margin: auto;    /* 有了这个就自动居中了 */
    }
```

代码两个关键点：

1. 上下左右均0位置定位；
2. margin: auto

于是，就居中了。上面代码的width: 600px height: 400px仅是示意，你修改为其他尺寸，或者不设置尺寸（需要是图片这种自身包含尺寸的元素），都是居中显示的。很有意思的~~

## 5. table里td标签的width属性在ie浏览器中无效解决方法

table里td设置width，firefox等浏览器里显示正常，ie里则无效。style="width:...px;"也是一样的效果。要让ie也有效，必须需要在table里再设置一下table-layout: fixed，如

```html
<table style="table-layout: fixed">
```


这样ie里td的width才生效。

## 6. 浮动元素高度自适应父元素高度

```css
.parent{
    height:100%;
    overflow:overflow;
}
.child{
    float:left;
    padding-bottom: 100%;
    margin-bottom: -100%;
}
```


## 7. writing-mode

在IE7 中， 文字的writing-mode设置为tb-lr（垂直方向从左到右 的文字排版）之后。
    对于**line-height**和**letter-spacing**的渲染依旧是按照横向文字排版的方式进行渲染。

## 8. IE7下对:first-child的识别
```css
    p:first-child{
        color:red;
    }
```
如果p元素有任何非p的兄弟元素，first-child不生效。

**注**：最坑的是，注释也算作兄弟元素，因为被解析为文本节点了。