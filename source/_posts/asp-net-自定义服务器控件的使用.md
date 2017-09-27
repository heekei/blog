---
title: ASP.NET 自定义服务器控件的使用
tags:
  - .NET
  - asp.net
id: .nan
categories:
  - web开发
date: 2016-01-13 09:10:56
---

最近在做一个网站后台，想要用

    &lt;xxx:xxx /&gt;
    `</pre>

    这种形式来直接读取数据库的数据，于是研究了一下自定义服务器控件。
    首先，先新建一个WebCustomControl1类（WEB自定义服务器控件）
    VS默认生成的代码如下：
    <!--more-->

    <pre>`using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    namespace UniversalWebSiteBg 
    {
        //是设置控件默认的属性的。这里是其Text属性。就是当你选择这个控件的时候，在属性窗口中自动被选中的是Text属性。
        [DefaultProperty("Text")] 
        //这行表示，当你在工具箱拖动自定义服务器控件时，为它生成的默认标记
        [ToolboxData("&lt;{0}:WebCustomControl1 runat=server&gt;&lt;/{0}:WebCustomControl1&gt;")]
        public class WebCustomControl1 : WebControl
        {
            // "Text"是这个服务器控件的一个属性
            [Bindable(true)] //指定是否通常用于绑定
            [Category("Appearance")] //说明该属性的分类是“外观”
            [DefaultValue("")]//默认值为空
            [Localizable(true)]//指定是否应本地化该属性
            public string Text
            {
                get
                {
                    String s = (String)ViewState["Text"];
                    return ((s == null) ? String.Empty : s);
                }

                set
                {
                    ViewState["Text"] = value;
                }
            }

            //返回输出服务器控件在网页上所显示的值。
            //默认的输出结构是：&lt;span&gt;这里是输出的值&lt;/span&gt;
            //到这里，可能和我一样的初学者会问了，为什么默认输出的是span标记？为什么不能是a标记，为什么不能是单纯的文本？
            //这里就说到了控件的渲染方法RenderContents了，VS默认是使用RenderContents方法,这样的话输出的是span标记也是固定了
            //如果想输出单纯文本的话，那么就应该是使用Render，而不是RenderContents了。
            protected override void RenderContents(HtmlTextWriter output)
            {
                output.Write(Text);
            }
        }
    }

    `</pre>

    于是，我们将改动一下默认的渲染方法：

    <pre>`        protected override void Render(HtmlTextWriter output)
            {
                output.Write(Text);
            }
    `</pre>

    这样的话，控件输出的内容就不包含标记了。
    那么，如何输出其他的标记呢？

    <pre>`        protected override void Render(HtmlTextWriter output)
            {
                output.WriteBeginTag("a");//输出标记的开头
                output.Write("这是一个标记");//输出标记的内容
                output.WriteEndTag("a");//输出标记的结束符
            }
    `</pre>

    这样运行的话，你会发现，html源码如下：

    <pre>`&lt;a&gt;这是一个标记&lt;/a&gt;
    `</pre>

    到这里，控件的构建就完成了，但是，你要在ASPX页面中去使用它，还需要在前台页面注册它，表明它的命名空间，标记前缀等信息。
    回到开头那里的

    <pre>`[ToolboxData("&lt;{0}:WebCustomControl1 runat=server&gt;&lt;/{0}:WebCustomControl1&gt;")]
    `</pre>

    {0}这个就是标记前缀字段

    <pre>`//在前台页面重要写出：
    &lt;%@ Register Assembly="UniversalWebSiteBg" Namespace="UniversalWebSiteBg" TagPrefix="UniBg" %&gt;
    `</pre>

    在这里，我写的标记前缀是  UniBg
    于是，我在工具箱中拖动控件到页面中后，
    会生成标记：

    <pre>`&lt;UniBg:WebCustomControl1 Id="WebCustomControl1" Text="" runat="server" /&gt;

于是这个简单的服务器控件就完成了。