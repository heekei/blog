---
title: Asp.Net alert弹出提示信息的5种方法
tags:
  - 弹窗
  - alert
  - asp.net
id: .nan
categories:
  - web开发
date: 2014-08-28 08:59:20
---

1.ClientScript.RegisterStartupScript(GetType(),"message","&lt;script&gt;alert('第一种方式,无白屏！');&lt;/script&gt;");
2.HttpContext.Current.Response.Write("&lt;script&gt;alert('第二种方式,有白屏！')&lt;/script&gt;");
3.public static void Show(System.Web.UI.Page page, string msg)
{
page.ClientScript.RegisterStartupScript(page.GetType(), "message", "&lt;script language='javascript' defer&gt;alert('" +  msg.ToString() + "');&lt;/script&gt;");
}
Show(this, "第三种方式，无白屏！");
4.Response.Write("&lt;script&gt;alert('第四种方式，有白屏！')&lt;/script&gt;");

5.window.showModalDialog('XXX.aspx', '', 'dialogWidth:429px;dialogHeight:200px;location:no,menubar:no,toolbar:no,status:no');