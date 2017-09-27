---
title: '【转】C#中正则表达式的使用'
tags:
  - 'C#'
id: .nan
categories:
  - 'C#'
  - 程序设计
date: 2016-01-24 13:45:00
---

<div id="cnblogs_post_body">

<span style="font-family: 宋体;">目前为止，许多编程语言和工具都包含对正则表达式的支持，C#也不例外，C#基础类库中包含有一个命名空间（<span style="color: red;">System.Text.RegularExpressions</span>）和一系列可以充分发挥规则表达式威力的类(<span style="color: red;">Regex</span>、<span style="color: red;">Match</span>、<span style="color: red;">Group</span>等)。那么，什么是正则表达式，怎么定义正则表达式呢？</span>

<!--more-->
**&nbsp;**
**<span style="font-size: 12pt; font-family: 宋体;">一、正则表达式基础</span>**

<span style="font-family: Wingdings;">l<span style="font: 7pt 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></span>**<span style="font-family: 宋体;">什么是正则表达式</span>**

<span style="font-family: 宋体;">&nbsp;&nbsp;&nbsp;</span><span style="font-family: 宋体;">在编写字符串的处理程序时，</span><span style="font-family: 宋体;">经常会有查找符合某些复杂规则的字符串的需要。<span style="color: #993300;">**正则表达式**</span>就是用于描述这些规则的工具。换句话说，**<span style="color: #993300;">正则表达式</span>**就是记录文本规则的代码。</span>

<span style="font-family: 宋体;">&nbsp;&nbsp;&nbsp; </span><span style="font-family: 宋体;">通常，我们在使用WINDOWS查找文件时，会使用通配符（*和?）。如果你想查找某个目录下的所有Word文档时，你就可以使用*.doc进行查找，在这里，*就被解释为任意字符串。</span><span style="font-family: 宋体;">和通配符类似，正则表达式也是用来进行文本匹配的工具，只不过比起通配符，它能更精确地描述你的需求——当然，代价就是更复杂</span><span style="font-family: 宋体;">。</span>

<span style="font-family: Wingdings;">l<span style="font: 7pt 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></span>**<span style="font-family: 宋体;">一个简单的例子——验证电话号码 </span>**

<span style="font-family: 宋体;">学习正则表达式的最好方法是从例子开始</span><span style="font-family: 宋体;">，下面我们从验证电话号码开始，一步一步的了解正则表达式。</span>

<span style="font-family: 宋体;">在我们国家，电话号码（如：0379-65624150）通常包含3到4为以0开头的区号和一个7或8为的号码，中间通常以连字符’-’隔开。在这个例子中，首先我们要介绍一个元字符<span style="color: red;">\d</span>，它用来匹配一个0到9的数字。这个正则表达式可以写成：<span style="color: red;">^0\d{2,3}-\d{7,8}$</span></span>

<span style="font-family: 宋体;">我们来对他进行分析，<span style="color: red;">0</span>匹配数字“0”，<span style="color: red;">\d</span>匹配一个数字，<span style="color: red;">{2,3}</span>表示重复2到3次，<span style="color: red;">-</span>只匹配”-”自身，接下来的<span style="color: red;">\d</span>同样匹配一个数字，而 <span style="color: red;">{7,8}</span>则表示重复7到8次。当然，电话号码还可以写成 (0379)65624150，这里就交给读者完成。</span>

<span style="font-family: Wingdings;">l<span style="font: 7pt 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></span>**<span style="font-family: 宋体;">元字符</span>**

<span style="font-family: 宋体;">在上面的例子中，我们接触到了一个元字符<span style="color: red;">\d</span>，正如你所想的，正则表达式还有很多像<span style="color: red;">\d</span>一样的元字符，下表列出了一些常用的元字符：</span>

&nbsp;

<table style="margin: auto auto auto 32.4pt; border-collapse: collapse;" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 95.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="127">
**<span style="font-family: 宋体;">元字符</span>**
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 261pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="348">
**<span style="font-family: 宋体;">说明</span>**
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 95.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="127">
<span style="color: red; font-family: 宋体;">.</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 261pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="348">
<span style="font-family: 宋体;">匹配除换行符以外的任意字符</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 95.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="127">
<span style="color: red; font-family: 宋体;">\b</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 261pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="348">
<span style="font-family: 宋体;">匹配单词的开始或结束</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 95.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="127">
<span style="color: red; font-family: 宋体;">\d</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 261pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="348">
<span style="font-family: 宋体;">匹配数字</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 95.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="127">
<span style="color: red; font-family: 宋体;">\s</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 261pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="348">
<span style="font-family: 宋体;">匹配任意的空白符</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 95.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="127">
<span style="color: red; font-family: 宋体;">\w</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 261pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="348">
<span style="font-family: 宋体;">匹配字母或数字或下划线或汉字</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 95.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="127">
<span style="color: red; font-family: 宋体;">^</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 261pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="348">
<span style="font-family: 宋体;">匹配字符串的开始</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 95.4pt; padding-top: 0cm; border-bottom: #d4d0c8;" valign="top" width="127">
<span style="color: red; font-family: 宋体;">$</span>
</td>
<td style="padding-right: 5.4pt; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; width: 261pt; padding-top: 0cm; border: #d4d0c8;" valign="top" width="348">
<span style="font-family: 宋体;">匹配字符串的结束</span>
</td>
</tr>
</tbody>
</table>

**<span style="font-family: 宋体;">表1、常用的元字符</span>**

<span style="font-family: Wingdings;">l<span style="font: 7pt 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></span>**<span style="font-family: 宋体;">转义字符</span>**

<span style="font-family: 宋体;">&nbsp;&nbsp; </span><span style="font-family: 宋体;">如果你想查找元字符本身的话，比如你查找.,或者*,就出现了问题：你没办法指定它们，因为它们会被解释成别的意思。这时你就得使用\来取消这些字符的特殊意义。因此，你应该使用<span style="color: red;">\.</span>和<span style="color: red;">\*</span>。当然，要查找\本身，你也得用<span style="color: red;">\\</span>.</span>

<span style="font-family: 宋体;">例如：<span style="color: red;">unibetter\.com</span>匹配unibetter.com，<span style="color: red;">C:\\Windows</span>匹配C:\Windows。</span>

<span style="font-family: Wingdings;">l<span style="font: 7pt 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></span>**<span style="font-family: 宋体;">限定符</span>**

<span style="font-family: 宋体;">限定符又叫重复描述字符，表示一个字符要出现的次数。比如我们在匹配电话号码时使用的{3,4}就表示出现3到4次。常用的限定符有：</span>

&nbsp;

<div align="center">
<table style="margin: auto auto auto 32.4pt; border-collapse: collapse;" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 86.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="115">
**<span style="font-family: 宋体;">限定符</span>**
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 153pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="204">
**<span style="font-family: 宋体;">说明</span>**
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 86.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="115">
<span style="color: red; font-family: 宋体;">*</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 153pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="204">
<span style="font-family: 宋体;">重复零次或更多次</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 86.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="115">
<span style="color: red; font-family: 宋体;">+</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 153pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="204">
<span style="font-family: 宋体;">重复一次或更多次</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 86.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="115">
<span style="color: red; font-family: 宋体;">?</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 153pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="204">
<span style="font-family: 宋体;">重复零次或一次</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 86.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="115">
<span style="color: red; font-family: 宋体;">{n}</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 153pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="204">
<span style="font-family: 宋体;">重复n次</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 86.4pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="115">
<span style="color: red; font-family: 宋体;">{n,}</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 153pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="204">
<span style="font-family: 宋体;">重复n次或更多次</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 86.4pt; padding-top: 0cm; border-bottom: #d4d0c8;" valign="top" width="115">
<span style="color: red; font-family: 宋体;">{n,m}</span>
</td>
<td style="padding-right: 5.4pt; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; width: 153pt; padding-top: 0cm; border: #d4d0c8;" valign="top" width="204">
<span style="font-family: 宋体;">重复n到m次</span>
</td>
</tr>
</tbody>
</table>
</div>

**<span style="font-family: 宋体;">表2、常用的限定符</span>**

**<span style="font-size: 12pt; font-family: 宋体;">二、.NET中正则表达式的支持</span>**
<span style="font-family: 宋体;">&nbsp;&nbsp;&nbsp; System.Text.RegularExpressions </span><span style="font-family: 宋体;">命名空间包含一些类，这些类提供对 .NET Framework 正则表达式引擎的访问。该命名空间提供正则表达式功能，可以从运行在 Microsoft .NET Framework 内的任何平台或语言中使用该功能。 </span>
&nbsp;
<span style="font-family: 宋体;">&nbsp;&nbsp;&nbsp; **1**</span>**<span style="font-family: 宋体;">、在C#中使用正则表达式</span>**

<span style="font-family: 宋体;">在了解了C#中支持正则表达式的类后，我们一起来将上面提到的验证电话号码的正则表达式写入C#代码中，实现电话号码的验证。</span>

<span style="font-family: 宋体;">第一步，建立一个名为SimpleCheckPhoneNumber的Windows项目。</span>

<span style="font-family: 宋体;">第二步，引入System.Text.RegularExpressions命名空间。</span>

<span style="font-family: 宋体;">第三步，写出正则表达式。这里的正则表达式就是上面的验证号码的字符串。由于上面的字符串只能验证用连字符连接区号和号码的方式的电话号码，所以我们做了一些修改：</span><span style="color: red; font-family: 宋体;">0\d{2,3}-\d{7,8}|\(0\d{2,3}\)\d{7,8}</span><span style="font-family: 宋体;">。在这个表达式中，<span style="color: red;">| </span>号面的一部分是我们上面提到过的，后面一部分是用来验证(0379)65624150这种电话号码写法的。由于 <span style="color: red;">( </span>&nbsp;和 <span style="color: red;">&nbsp;) </span>也是元字符，所以要用转义字符。<span style="color: red;">| </span>表示分支匹配，要么匹配前面的一部分，要么匹配后面的一部分。</span>

<span style="font-family: 宋体;">第四步，正则表达式构造一个Regex类。</span>

<span style="font-family: 宋体;">第五步，使用Regex类的IsMatch方法验证匹配。Regex类的IsMatch()方法返回一个bool值，如果有匹配项，返回true，否则返回false。</span>

&nbsp;

**<span style="font-size: 12pt; font-family: 宋体;">三、正则表达式进阶</span>**

<span style="font-family: Wingdings;">l<span style="font: 7pt 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></span>**<span style="font-family: 宋体;">分组</span>**

<span style="font-family: 宋体;">在匹配电话号码的时候，我们已经用到过重复单个字符。下面我们来了解如何使用分组来匹配一个IP地址。</span>

<span style="font-family: 宋体;">众所周知，IP地址是四段点分十进制的字符串表示的。所以，我们可以通过地址的分组，来进行匹配。首先，我们来匹配第一段：<span style="color: red;">2[0-4]\d|25[0-5]|[01]?\d\d? </span>这段正则表达式可以匹配IP地址的一段数字。<span style="color: red;">2[0-4]\d</span> 匹配以2开头，十位为0到4，个位为任何数字的三位字段，<span style="color: red;">25[0-5] </span>匹配以25 开头，个位为0到5 的三位字段，<span style="color: red;">[01]?\d\d?</span> 匹配任何以1者0头，个位和十位为任何数子的字段。<span style="color: red;">?</span> 表示出现零次或一次。所以， <span style="color: red;">[01]</span> 和 最后一个 <span style="color: red;">\d</span> 都可以不出现，如果我们再向这个字符串后面添加一个<span style="color: red;"> \.</span> 来匹配 . 就可以划分一个段了。现在，我们把 <span style="color: red;">2[0-4]\d|25[0-5]|[01]?\d\d?\. </span>当做一个分组，就可以写成 <span style="color: red;">(2[0-4]\d|25[0-5]|[01]?\d\d?\.)</span> 。接下来我们就来使用这个分组。将这个分组重复两次，然后，再使用 <span style="color: red;">2[0-4]\d|25[0-5]|[01]?\d\d?</span> 就可以了。完整的正则表达式为： <span style="color: red;">(2[0-4]\d|25[0-5]|[01]?\d\d?\.){3}2[0-4]\d|25[0-5]|[01]?\d\d?</span> </span>

&nbsp;

<span style="font-family: Wingdings;">l<span style="font: 7pt 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></span>**<span style="font-family: 宋体;">后向引用</span>**

<span style="font-family: 宋体;">在我们了解分组以后，我们就可以使用后向引用了。所谓后向引用，就是使用前面捕获的结果，对后面的字符进行匹配。多用于匹配重复字符。比如匹配 go go 这样的重复字符。我们就可以使用 (go) \1来进行匹配。</span>

<span style="font-family: 宋体;">默认情况下，每个分组会自动拥有一个组号，规则是：从左向右，以分组的左括号为标志，第一个出现的分组的组号为1，第二个为2，以此类推。当然，你也可以自己指定子表达式的组名。要指定一个子表达式的组名，请使用这样的语法：<span style="color: red;">(?&lt;Word&gt;\w+)</span>(或者把尖括号换成'也行：<span style="color: red;">(?'Word'\w+))</span>,这样就把<span style="color: red;">\w+</span>的组名指定为Word了。要反向引用这个分组捕获的内容，你可以使用<span style="color: red;">\k&lt;Word&gt;</span>,所以上一个例子也可以写成这样：<span style="color: red;">\b(?&lt;Word&gt;\w+)\b\s+\k&lt;Word&gt;\b</span>。</span>

<span style="font-family: 宋体;">自定义组名还有另外一个好处，在我们的C#程序中，如果需要得到分组的值，我们就可以很明确的使用我们定义的分组的名字来得到，而不必使用下标。</span>

<span style="font-family: 宋体;">当我们并不想使用后向引用时，是不需要捕获组记忆任何东西的，这种情况下就可以利用<span style="color: red;">(?:nocapture)</span>语法来主动地告诉正则表达式引擎，不要把圆括号的内容当作捕获组，以便提高效率。</span>

<span style="font-family: Wingdings;">l<span style="font: 7pt 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></span>**<span style="font-family: 宋体;">零宽断言</span>**

<span style="font-family: 宋体;">在前面的元字符介绍中，我们已经知道了有这样一类字符，可以匹配一句话的开始、结束（<span style="color: red;">^ $</span>）或者匹配一个单词的开始、结束（<span style="color: red;">\b</span>）。这些元字符只匹配一个位置，指定这个位置满足一定的条件，而不是匹配某些字符，因此，它们被成为 **<span style="color: #993300;">零宽断言</span>**。所谓零宽，指的是它们不与任何字符相匹配，而匹配一个位置；所谓断言，指的是一个判断。正则表达式中只有当断言为真时才会继续进行匹配。</span>

<span style="font-family: 宋体;">在有些时候，我们精确的匹配一个位置，而不仅仅是句子或者单词，这就需要我们自己写出断言来进行匹配。下面是断言的语法：</span>

&nbsp;

<table style="margin: auto auto auto 32.4pt; border-collapse: collapse;" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 108pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="144">
**<span style="font-family: 宋体;">断言语法</span>**
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 285.7pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="381">
**<span style="font-family: 宋体;">说明</span>**
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 108pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="144">
<span style="color: red; font-family: 宋体;">(?=pattern)</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 285.7pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="381">
<span style="font-family: 宋体;">前向肯定断言，匹配pattern前面的位置</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 108pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="144">
<span style="color: red; font-family: 宋体;">(?!pattern)</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 285.7pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="381">
<span style="font-family: 宋体;">前向否定断言，匹配后面不是pattern的位置</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 108pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="144">
<span style="color: red; font-family: 宋体;">(?&lt;=pattern)</span>
</td>
<td style="border-right: #d4d0c8; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #f2f2f2; padding-bottom: 0cm; border-left: #d4d0c8; width: 285.7pt; padding-top: 0cm; border-bottom: white 2.25pt solid;" valign="top" width="381">
<span style="font-family: 宋体;">后向肯定断言，匹配pattern后面的位置</span>
</td>
</tr>
<tr>
<td style="border-right: white 2.25pt solid; padding-right: 5.4pt; border-top: #d4d0c8; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; border-left: #d4d0c8; width: 108pt; padding-top: 0cm; border-bottom: #d4d0c8;" valign="top" width="144">
<span style="color: red; font-family: 宋体;">(?&lt;!pattern)</span>
</td>
<td style="padding-right: 5.4pt; padding-left: 5.4pt; background: #cccccc; padding-bottom: 0cm; width: 285.7pt; padding-top: 0cm; border: #d4d0c8;" valign="top" width="381">
<span style="font-family: 宋体;">后向否定断言，匹配前面不是pattern的位置</span>
</td>
</tr>
</tbody>
</table>

**<span style="font-family: 宋体;">表3、断言的语法及说明</span>**

<span style="font-family: 宋体;">很难理解吗？我们来看一个例子。</span>

<span style="font-family: 宋体;">有一个标签：<span style="color: red;">&lt;book&gt;</span>，我们想要得到标签<span style="color: red;">&lt;book&gt;</span>的标签名（<span style="color: red;">book</span>），这个时候，我们就可以使用断言来处理。看下面这个表达式：</span><span style="font-size: 10pt; color: red; font-family: 宋体;">(?&lt;=\&lt;)(?&lt;tag&gt;\w*)(?=\&gt;)</span><span style="font-size: 10pt; font-family: 宋体;"> ，使用这个表达式，可以匹配<span style="color: red;">&lt;</span> 和 <span style="color: red;">&gt;</span>之间的字符，也就是这里的<span style="color: red;">book</span>。使用断言还还可以写出更加复杂的表达式，这里就不再举例了。</span>

<span style="font-family: 宋体;">还有一点非常重要，就是断言语法所使用的圆括号并不作为捕获组，所以不能使用编号或命名来对它进行引用。</span>

&nbsp;

<span style="font-family: Wingdings;">l&nbsp;</span>**<span style="font-family: 宋体;">贪婪与懒惰</span>**

<span style="font-family: 宋体;">当正则表达式中包含能接受重复的限定符时，通常的行为是（在使整个表达式能得到匹配的前提下）匹配尽可能多的字符。来看一下这个表达式：<span style="color: red;">a\w*b</span> ，用它来匹配字符串 aabab 时，得到的匹配结果是 <span style="color: red;">aabab</span> 。这种匹配被称为**<span style="color: #993300;">贪婪匹配</span>**。</span>

<span style="font-family: 宋体;">有些时候，我们希望让它尽可能的少重复，即用上面的例子得到的匹配结果是 <span style="color: red;">aab</span>，这时我们就要使用**<span style="color: #993300;">懒惰匹配</span>**。**<span style="color: #993300;">懒惰匹配</span>**需要在<span style="color: red;">重复限定符</span>的后面添加一个 <span style="color: red;">?</span> 符号，上面的表达式就可以写成：<span style="color: red;">a\w*?b</span> 我们再来匹配字符串 aabab时，得到的匹配结果是 <span style="color: red;">aab</span> 和 <span style="color: red;">ab</span> 。</span>

<span style="font-family: 宋体;">也许这个时候你要问，ab 比aab重复次数更少，为什么不先匹配ab呢？其实在正则表达式中还有比**<span style="color: #993300;">贪婪/懒惰</span>**优先级更高的规则：**<span style="color: #993300;">最先开始的匹配拥有最高的优先权——The match that begins earliest wins。</span>**</span>

<span style="font-family: Wingdings;">l&nbsp;</span>**<span style="font-family: 宋体;">注释</span>**

<span style="font-family: 宋体;">语法：<span style="color: red;">(?#comment)</span></span>

<span style="font-family: 宋体;">&nbsp;&nbsp; </span><span style="font-family: 宋体;">例如：</span><span style="font-family: 宋体;">2[0-4]\d(?#200-249)|25[0-5](?#250-255)|[01]?\d\d?(?#0-199)</span>

<span style="font-family: 宋体;">&nbsp;&nbsp; </span><span style="font-family: 宋体;">注意：如果使用注释，则需要格外注意不要在注释的小括号前面出现空格、换行符等一些字符，如果可以忽略这些字符，则最好使用“忽略模式里的空白符”选项，即C#中**<span style="color: #993300;">RegexOptions</span><span style="color: #993300;">枚举</span>**的IgnorePatternWhitespace选项（C#中的**<span style="color: #993300;">RegexOptions</span><span style="color: #993300;">枚举</span>**下面将会提到）。</span>

&nbsp;

<span style="font-family: Wingdings;">l&nbsp;</span>**<span style="font-family: 宋体;">C#</span>****<span style="font-family: 宋体;">中的处理选项</span>**

<span style="font-family: 宋体;">在C#中，可以使用**<span style="color: #993300;">RegexOptions </span><span style="color: #993300;">枚举</span>**来选择C#对正则表达式的处理方式。下面是MSDN中**<span style="color: #993300;">RegexOptions </span><span style="color: #993300;">枚举</span>**的成员介绍：</span>

&nbsp;

&nbsp;

&nbsp;

<span style="font-family: Wingdings;">l&nbsp;</span>**<span style="font-family: 宋体;">C#</span>****<span style="font-family: 宋体;">中Capture类、Group类、Match类</span>**

**<span style="color: #993300; font-family: 宋体;">Capture</span>****<span style="color: #993300; font-family: 宋体;">类</span>**<span style="font-family: 宋体;">：表示单个子表达式捕获中的结果。Capture类表示单个成功捕获中的一个子字符串。该类没有公共构造函数，可以从Group类或者Match类中得到一个Capture类的对象集合。Capture类有三个常用属性，分别是Index、Length和Value。Index表示捕获的子字符串的第一个字符的位置。Length表示捕获的子字符串的长度，Value表示捕获的子字符串。</span>

**<span style="color: #993300; font-family: 宋体;">Group</span>****<span style="color: #993300; font-family: 宋体;">类</span>**<span style="font-family: 宋体;">：表示正则表达式中分组的信息。该类提供了对分组匹配的正则表达式的支持。该类没有公共构造函数。可以从Match类中得到一个Group类的集合。如果正则表达式中的分组已命名，则可以使用名字对其进行访问，如果没有命名，则可以采用下标访问。注意：每一个Match的Groups集合中的第0个元素（Groups[0]）都是这个Match捕获的字符串，也是Capture的Value。</span>

**<span style="color: #993300; font-family: 宋体;">Match</span>****<span style="color: #993300; font-family: 宋体;">类</span>**<span style="font-family: 宋体;">：表示单个正则表达式匹配的结果。该类同样没有公共构造函数，可以从Regex类的Match()方法得到该类的一个实例，也可以使用Regex类的Matches()方法得到给类的一个集合。</span>

<span style="font-family: 宋体;">这三个类都能表示单个正则表达式匹配的结果，但Match类得到的更为详细，包含捕获和分组信息。所以，Match类在这个三个类中是最常用的。</span>
</div>

<div class="content">
<div>作者：[齐飞](http://youring2.cnblogs.com/)</div>
<div>来源：[http://youring2.cnblogs.com/](http://youring2.cnblogs.com/)</div>
</div>