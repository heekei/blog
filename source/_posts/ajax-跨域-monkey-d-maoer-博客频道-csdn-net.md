---
title: ajax 跨域
tags:
  - ajax
id: .nan
categories:
  - web开发
date: 2014-05-06 14:23:09
---

解决浏览器的跨域问题，不要想完全通过在浏览器中使用脚本绕过浏览器的相同来源策略，那是做不到的，除非你所使用的浏览器版本刚好存在严重的bug，否则做浏览器的那帮人都不必混了。

解决跨域问题，要从浏览器之外想办法，据我所知有两种方法：

**方法1**：在服务器端实现一个服务器端的代理，接收跨域的请求，通过HTTP得到请求的数据后转发给客户端的浏览器。
但是你在客户端也要做一些事情，因为XMLHttpRequest对象默认情况下是不接受发到其他域的URL的。方法是使用Decorator模式给XMLHttpRequest加上一层封装，将发到其他域的URL改写为发给服务器端代理的URL。

《Ajax模式与最佳实践》最后一章：“基于REST的MVC模式”给出了一种非常详细的解决方案，这种方案也是属于方法1的。<!--more-->

**方法2**：在客户端实现一个客户端的HTTP代理，配置浏览器使用这个HTTP代理。通过HTTP代理请求跨域的数据（通过一个绑定在本域的特殊的URL，例如：/selenium-server/RemoteRunner.html?sessionId=260113）这是在Selenium RC中所使用的方式。他们为了绕过浏览器的相同来源策略，以便可以对于任意的Web网站执行客户端的自动测试，使用了一个Java实现的客户端的HTTP代理。
详情请看Selenium RC的文档：
[http://www.openqa.org/selenium-rc/tutorial.html](http://www.openqa.org/selenium-rc/tutorial.html)
在方法2中，同样要使用Decorator模式给XMLHttpRequest加上一层封装，将发到其他域的URL改写为能够触发客户端代理相应操作的URL。

有人会争论说，通过动态加载脚本（就是动态创建&lt;script&gt;元素）的方式可以实现跨域，那不是真正的跨域，因为数据的交互是单向的。那样你最多只能调用其他域的功能，你取回的数据还是无法与本域的数据（HTML DOM树）进行交互。

----------------------------------------------------------------------------------------------------------------------------------
ajax 跨域实现 前两天xz问我知不知道ajax怎么实现跨域调用，因为没听过这个概念，所以也知道怎么实现。xz说ajax跨域调用有几种方式，一种是iframe的方式，通过设置document.domain来实现，一种则是通过设置jsonp来实现。这两天查了一下资料，也写了几个demo，下面备忘一下。

我在本地建了三个站点,并设置了host文件模拟跨子域和跨全域

coolkissbh.com

blog.coolkissbh.com

coolkiss.com

一. ajax 跨域调用会有什么问题

coolkissbh.com下页面使用jquery的$.get调用blog.coolkissbh.com页面

跨域请求，IE 7和8下报 access denied错误
IE 6.0 则弹出 this page is accessing information that is not under its control. this poses a security risk.do you want to continue?提示框

firefox 没报错，但是不会做出请求

二. ajax跨域实现方法

1. 跨子域实现ajax

  **要求**：实现coolkissbh.com的页面 异步请求 blog.coolkissbh.com下的页面
  
  **实现方法**：借助iframe，通过设置iframe的src属性，嵌入blog.coolkissbh.com下的一个页面，比如AjaxProxy.aspx,然后由该页面去请求Ajax
  AjaxProxy请求完毕后，通过parent对象把返回的数据回传给coolkissbh.com的主页面。因此，真正的异步请求还是发生
  在blog.coolkissbh.com的域名下

  **注意**：通过这种方法实现的跨子域ajax请求，需要在coolkissbh.com的请求页面以及AjaxProxy.aspx页面中设置同样的域名，也就是
  document.domain = "coolkissbh.com";

  **注意**：关于设置domain的问题，如果是跨全域，使用上面方法时候，firefox下会提示Illegal document.domain value" code: "1009的错误，因此跨全域只能使用第二种方法

  **处理返回的数据**：

  AjaxProxy.aspx将ajax返回的数据保存到一个全局变量中，coolkissbh.com通过setInterval定时去判断iframe的页面是否加载完成
  如果加载完成，则获取AjaxProxy.aspx的全局变量值。然后再做其它处理。

  这里有个问题：我原来是打算在AjaxProxy.aspx的ajax请求完成后，调用parent的方法，同时将数据返回，但是在IE下，点击第一次时候
  就会出现“permission denied”的错误,再次点击就正常了。在firefox下就没有问题，不知道是什么原因。

2. 跨全域实现ajax

    **要求**：实现coolkissbh.com的页面异步请求coolkiss.com下的页面
    
    **实现方法**：上面提到跨全域不能通过设置domain方法来实现。但是可以使用script标签来实现，通过设置script标签的src属性为coolkiss.com域名下的一个页面，同时将callback函数传到该页面中，例如：
    ```js
    RequestAjax_CrossSite = function() {
      var obj = $("#crossSitePage");
      obj.attr("src", "[http://coolkiss.com/CrossSite.aspx?callback=handleData3](http://coolkiss.com/CrossSite.aspx?callback=handleData3)");
    }
    handleData3 = function(data) {
      $("#ResponseData").html(data);
    }
    ```
    CrossSite.aspx返回一个字符串，将返回的数据回传给callback，执行回调函数，实现ajax，例如：

    ```cs
    Response.Clear();
    Response.Write(string.Format("{0}('{1}')", Request["callback"], responseData));
    Response.End();
    ```

    **注意**：这种方法同样可以用于处理跨子域ajax的问题，但是就无法像jquery那样获取ajax调用的各个状态

3. 通过jquery的jsonp实现跨域ajax，其实原理跟第二种方法是一样的，支持跨全域和子域

    jquery 1.2 后添加了对跨域ajax的调用，也就是$.getJSON 函数
    调用方法如下:
    
    下面是coolkissbh.com下的页面
    ```js
    //使用jsonp实现跨全域
    RequestAjax_JSONP = function() {
    var obj = $("#crossSitePage");
    $.getJSON("[http://coolkiss.com/CrossSite.aspx?callback=?&amp;t](http://coolkiss.com/CrossSite.aspx?callback=?&amp;t)=" + Math.random(), function(data) {
    //alert(data);
    $("#ResponseData").html(data.content);
    });
    }
    ```

    coolkiss.com后台处理代码，将一个json对象传递给callback：

    ```cs
    public partial class CrossSite : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e){
            if (!IsPostBack)
            {
            LoadData_JSONP();
            }
        }
        protected void LoadData_JSONP()
        {
            string responseData = "{content:/"hello world from coolkiss.com/"}";
            if (Request["callback"] != null &amp;&amp; !string.IsNullOrEmpty(Request["callback"])){
                Response.Clear();
                Response.Write(string.Format("{0}({1})", Request["callback"], responseData));
                Response.End();
            }
        }
    }
    ```
    callback=? 其中？会自动替换为function(data)函数。
----------------------------------------------------------------------------------------------------------------------------------
JavaScript是一种在Web开发中经常使用的前端动态脚本技术。在JavaScript中，有一个很重要的安全性限制，被称为“Same-Origin Policy”（同源策略）。这一策略对于JavaScript代码能够访问的页面内容做了很重要的限制，即JavaScript只能访问与包含它的文档在同一域下的内容。

JavaScript这个安全策略在进行多iframe或多窗口编程、以及Ajax编程时显得尤为重要。根据这个策略，在baidu.com下的页面中包含的JavaScript代码，不能访问在google.com域名下的页面内容；甚至不同的子域名之间的页面也不能通过JavaScript代码互相访问。对于Ajax的影响在于，通过XMLHttpRequest实现的Ajax请求，不能向不同的域提交请求，例如，在abc.example.com下的页面，不能向def.example.com提交Ajax请求，等等。

然而，当进行一些比较深入的前端编程的时候，不可避免地需要进行跨域操作，这时候“同源策略”就显得过于苛刻。本文就这个问题，概括了跨域所需要的一些技术。

下面我们分两种情况讨论跨域技术：首先讨论不同子域的跨域技术，然后讨论完全不同域的跨域技术。

（一）不同子域的跨域技术。

我们分两个问题来分别讨论：第一个问题是如何跨不同子域进行JavaScript调用；第二个问题是如何向不同子域提交Ajax请求。

先来解决第一个问题，假设example.com域下有两个不同子域：abc.example.com和def.example.com。现在假设在def.example.com下面有一个页面，里面定义了一个JavaScript函数：
```js
function funcInDef() {

//.....

}
```
我们想在abc.example.com下的某个页面里调用上面的函数。再假设我们要讨论的abc.example.com下面的这个页面是以iframe形式嵌入在def.example.com下面那个页面里的，这样我们可能试图在iframe里做如下调用：
```js
window.top.funcInDef();
```
好，我们注意到，这个调用是被前面讲到的“同源策略”所禁止的，JavaScript引擎会直接抛出一个异常。

为了实现上述调用，我们可以通过修改两个页面的domain属性的方法做到。例如，我们可以将上面在abc.example.com和def.example.com下的两个页面的顶端都加上如下的JavaScript代码片段：

```html
<script type="text/javascript">

document.domain = "example.com";

</script>
```

这样，两个页面就变为同域了，前面的调用也可以正常执行了。

这里需要注意的一点是，一个页面的document.domain属性只能设置成一个更顶级的域名（除了一级域名），但不能设置成比当前域名更深层的子域名。例如，abc.example.com的页面只能将它的domain设置成example.com，不能设置成sub.abc.example.com，当然也不能设置成一级域名com。

上面的例子讨论的是两个页面属于iframe嵌套关系的情况，当两个页面是打开与被打开的关系时，原理也完全一样。

下面我们来解决第二个问题：如何向不同子域提交Ajax请求。

通常情况下，我们会用与下面类似的代码来创建一个XMLHttpRequest对象：


```js
factories = [

function() { return new XMLHttpRequest(); },

function() { return new ActiveXObject("Msxml2.XMLHTTP"); },

function() { return new ActiveXObject("Microsoft.XMLHTTP"); }

];

function newRequest() {

for(var i = 0; i &lt; factories.length; i++) {

try{

var factory = factories[i];

return factory();

} catch(e) {}

}

return null;

}
```


上面的代码中引用ActiveXObject，是为了兼容IE6系列浏览器。每次我们调用newRequest函数，就获得了一个刚刚创建的Ajax对象，然后用这个Ajax对象来发送HTTP请求。例如，下面的代码向abc.example.com发送了一个GET请求：


```js
var request = newRequest();

request.open("GET", "[http://abc.example.com](http://abc.example.com/)" );

request.send(null);
```


假设上面的代码包含在一个abc.example.com域名下的页面里，则这个GET请求可以正常发送成功，没有任何问题。然而，如果现在要向def.example.com发送请求，则出现跨域问题，JavaScript引擎抛出异常。

解决的办法是，在def.example.com域下放置一个跨域文件，假设叫crossdomain.html；然后将前面的newRequest函数的定义移到这个跨域文件中；最后像之前修改document.domain值的做法一样，在crossdomain.html文件和abc.example.com域下调用Ajax的页面顶端，都加上：


```html
<script type="text/javascript">

document.domain = "example.com";

</script>
```


为了使用跨域文件，我们在abc.example.com域下调用Ajax的页面中嵌入一个隐藏的指向跨域文件的iframe，例如：


```html
<iframe name="xd_iframe" style="display:none" src="[http://def.example.com/crossdomain.html"></iframe>
```


这时abc.example.com域下的页面和跨域文件crossdomain.html都在同一个域（example.com）下，我们可以在abc.example.com域下的页面中去调用crossdomain.html中的newRequest函数：


```js
var request = window.frames["xd_iframe"].newRequest();
```


这样获得的request对象，就可以向[http://def.example.com](http://def.example.com/)发送HTTP请求了。

（二）完全不同域的跨域技术。

如果顶级域名都不相同，例如example1.com和example2.com之间想通过JavaScript在前端通信，则所需要的技术更复杂些。

在讲解不同域的跨域技术之前，我们首先明确一点，下面要讲的技术也同样适用于前面跨不同子域的情况，因为跨不同子域只是跨域问题的一个特例。当然，在恰当的情况下使用恰当的技术，能够保证更优的效率和更高的稳定性。

简言之，根据不同的跨域需求，跨域技术可以归为下面几类：

JSONP跨域GET请求
通过iframe实现跨域
flash跨域HTTP请求
window.postMessage
下面详细介绍各种技术。

1\. JSONP。

利用在页面中创建&lt;script&gt;节点的方法向不同域提交HTTP请求的方法称为JSONP，这项技术可以解决跨域提交Ajax请求的问题。JSONP的工作原理如下所述：

假设在[http://example1.com/index.php](http://example1.com/index.php)这个页面中向[http://example2.com/getinfo.php](http://example2.com/getinfo.php)提交GET请求，我们可以将下面的JavaScript代码放在[http://example1.com/index.php](http://example1.com/index.php)这个页面中来实现：


```js
var eleScript= document.createElement("script");

eleScript.type = "text/javascript";

eleScript.src = "http://example2.com/getinfo.php";

document.getElementsByTagName("HEAD")[0].appendChild(eleScript);
```


当GET请求从[http://example2.com/getinfo.php](http://example2.com/getinfo.php)返回时，可以返回一段JavaScript代码，这段代码会自动执行，可以用来负责调用[http://example1.com/index.php](http://example1.com/index.php)页面中的一个callback函数。

JSONP的优点是：它不像XMLHttpRequest对象实现的Ajax请求那样受到同源策略的限制；它的兼容性更好，在更加古老的浏览器中都可以运行，不需要XMLHttpRequest或ActiveX的支持；并且在请求完毕后可以通过调用callback的方式回传结果。

JSONP的缺点则是：它只支持GET请求而不支持POST等其它类型的HTTP请求；它只支持跨域HTTP请求这种情况，不能解决不同域的两个页面之间如何进行JavaScript调用的问题。

2\. 通过iframe实现跨域。

iframe跨域的方式，功能强于JSONP，它不仅能用来跨域完成HTTP请求，还能在前端跨域实现JavaScript调用。因此，完全不同域的跨域问题，通常采用iframe的方式来解决。

与JSONP技术通过创建&lt;script&gt;节点向不同的域提交GET请求的工作方式类似，我们也可以通过在[http://example1.com/index.php](http://example1.com/index.php)页面中创建指向[http://example2.com/getinfo.php](http://example2.com/getinfo.php)的iframe节点跨域提交GET请求。然而，请求返回的结果无法回调[http://example1.com/index.php](http://example1.com/index.php)页面中的callback函数，因为受到“同源策略”的影响。

为了解决这个问题，我们需要在example1.com下放置一个跨域文件，比如路径是[http://example1.com/crossdomain.html](http://example1.com/crossdomain.html)。

当[http://example2.com/getinfo.php](http://example2.com/getinfo.php)这个请求返回结果的时候，它大体上有两个选择。

第一个选择是，它可以在iframe中做一个302跳转，跳转到跨域文件[http://example1.com/crossdomain.html](http://example1.com/crossdomain.html)，同时将返回结果经过URL编码之后作为参数缀在跨域文件URL后面，例如[http://example1.com/crossdomain.html?result=&lt;URL-Encoding-Content](http://example1.com/crossdomain.html?result=%3CURL-Encoding-Content)&gt;。

另一个选择是，它可以在返回的页面中再嵌入一个iframe，指向跨域文件，同时也是将返回结果经过URL编码之后作为参数缀在跨域文件URL后面。

在跨域文件中，包含一段JavaScript代码，这段代码完成的功能，是从URL中提取结果参数，经过一定处理后调用原来的[http://example1.com/index.php](http://example1.com/index.php)页面中的一个预先约定好的callback函数，同时将结果参数传给这个函数。[http://example1.com/index.php](http://example1.com/index.php)页面和跨域文件是在同一个域下的，因此这个函数调用可以通过。跨域文件所在iframe和原来的[http://example1.com/index.php](http://example1.com/index.php)页面的关系，在前述第一种选择下，后者是前者的父窗口，在第二种选择下，后者是前者的父窗口的父窗口。

根据前面的叙述，有了跨域文件之后，我们就可以实现通过iframe方式在不同域之间进行JavaScript调用。这个调用过程可以完全跟HTTP请求无关，例如有些站点可以支持动态地调整在页面中嵌入的第三方iframe的高度，这其实是通过在第三方iframe里面检测自己页面的高度变化，然后通过跨域方式的函数调用将这个变化告知父窗口来完成的。

既然利用iframe可以实现跨域JavaScript调用，那么跨域提交POST请求等其它类型的HTTP请求就不是难事。例如我们可以跨域调用目标域的JavaScript代码在目标域下提交Ajax请求（GET/POST/etc.），然后将返回的结果再跨域传原来的域。

使用iframe跨域，优点是功能强大，支持各种浏览器，几乎可以完成任何跨域想做的事情；缺点是实现复杂，要处理很多浏览器兼容问题，并且传输的数据不宜过大，过大了可能会超过浏览器对URL长度的限制，要考虑对数据进行分段传输等。

3\. 利用flash实现跨域HTTP请求

据称，flash在浏览器中的普及率高达90%以上。

flash代码和JavaScript代码之间可以互相调用，并且flash的“安全沙箱”机制与JavaScript的安全机制并不尽相同，因此，我们可以利用flash来实现跨域提交HTTP请求（支持GET/POST等）。

例如，我们用浏览器访问[http://example1.com/index.php](http://example1.com/index.php)这个页面，在这个页面中引用了[http://example2.com/flash.swf](http://example2.com/flash.swf)这个flash文件，然后在flash代码中向[http://example3.com/webservice.php](http://example3.com/webservice.php)发送HTTP请求。

这个请求能否被成功发送，取决于在example3.com的根路径下是否放置了一个crossdomain.xml以及这个crossdomain.xml的配置如何。flash的“安全沙箱”会保证：仅当example3.com服务器在根路径下确实放置了crossdomain.xml文件并且在这个文件中配置了允许接受来自example2.com的flash的请求时，这个请求才能真正成功。下面是一个crossdomain.xml文件内容的例子：


```xml
<?xml version="1.0"?>

<cross-domain-policy>

<allow-access-from domain="example2.com" />

</cross-domain-policy>
```


4\. window.postMessage

window.postMessage是HTML标准的下一个版本HTML5支持的一个新特性。受当前互联网技术突飞猛进的影响，浏览器跨域通信的需求越来越强烈，HTML标准终于把跨域通信考虑进去了。但目前HTML5仍然只是一个draft。

window.postMessage是一个安全的实现直接跨域通信的方法。但是目前并不是所有浏览器都能支持，只有Firefox 3、Safari 4和IE8可以支持这个调用。

使用它向其它窗口发送消息的调用方式大概如下:


```js
otherWindow.postMessage(message, targetOrigin);
```


在接收的窗口，需要设置一个事件处理函数来接收发过来的消息：


```js
window.addEventListener("message", receiveMessage, false);function receiveMessage(event){ if (event.origin!== "http://example.org:8080") return;}
```


消息包含三个属性：data、origin（携带发送窗口所在域的真实信息）和source（代表发送窗口的handle）。

安全性考虑：使用window.postMessage，必需要使用消息的origin和source属性来验证发送者的身份，否则会造成XSS漏洞。

window.postMessage在功能上同iframe实现的跨域功能同样强大，并且使用简单，效率更高，但缺点是它目前在浏览器兼容方面有待提高。

总结完所有的跨域方式之后，我们要时刻铭记，虽然跨域技术能给你带来更多的功能，催生出更灵活和更加平台化的产品，但是功能的放开也总是意味着安全的风险。在实现跨域技术的每个步骤和细节，都要时刻在头脑中考虑到对安全带来的影响，避免成为XSS攻击的漏洞。

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
javascript出于安全方面的考虑，是不允许跨域调用其他页面的对象的。但在安全限制的同时也给注入iframe或是ajax应用上带来了不少麻烦。没有记错的话前三届D2论坛上每次都有人提这个东西，这里把涉及到跨域的一些问题简单地整理一下：

首先什么是跨域，简单地理解就是因为javascript同源策略的限制，a.com 域名下的js无法操作b.com或是c.a.com域名下的对象。
遵循：``同协议，同域名，同端口``。

特别注意两点：

第一，如果是协议和端口造成的跨域问题“前台”是无能为力的，

第二：在跨域问题上，域仅仅是通过URL的首部来识别而不会去尝试判断相同的ip地址对应着两个域或两个域是否在同一个ip上。

接下来简单地总结一下在“前台”一般处理跨域的办法，后台proxy这种方案牵涉到后台的配置，这里就不阐述了,有兴趣的可以看看YAHOO的这篇文章： ?
JavaScript: Use a Web Proxy for Cross-Domain XMLHttpRequest Calls。

1、document.domain+iframe的设置
对于主域相同而子域不同的例子，可以通过设置document.domain的办法来解决。具体的做法是可以在[http://www.f2e.me/a.html](http://www.f2e.me/a.html)和[http://script.f2e.me/b.html](http://script.f2e.me/b.html)两个文件中分别加上document.domain = ‘f2e.me’；然后通过a.html文件中创建一个iframe,去控制iframe的contentDocument,这样两个js文件之间就可以“交互”了。当然这种办法只能解决主域相同而二级域名不同的情况，如果你异想天开的把script.f2e.me的domian设为alibaba.com那显然是会报错地！代码如下：

www.f2e.me上的a.html
```js
document.domain = 'f2e.me';
var ifr = document.createElement('iframe');
ifr.src = 'http://script.f2e.me/b.html';
ifr.style.display = 'none';
document.body.appendChild(ifr);
ifr.onload = function(){
var x = ifr.contentDocument;
alert(x.getElementsByTagName("h1")[0].childNodes[0].nodeValue);
}
```

script.f2e.me上的b.html
```js
document.domain = 'f2e.me';
```

2、动态创建script
虽然浏览器默认禁止了跨域访问，但并不禁止在页面中引用其他域的JS文件，并可以自由执行引入的JS文件中的function，根据这一点，可以方便地通过创建script节点的方法来实现完全跨域的通信。具体的做法可以参考yui的 Get Utility

这里判断script节点加载完毕还是蛮有意思的：ie只能通过script的readystatechange属性,Safari 3.x以上支持的是script的load事件，而firefox和oprea则要通过onload来解决。另外这种办法只能传递js类型的数据，不是很方便。以下是部分判断script加载完毕的方法。



```js
// ie支持script的readystatechange属性
// IE supports the readystatechange event for script and css nodes
if (ua.ie) {
    n.onreadystatechange = function() {
        var rs = this.readyState;
        if ("loaded" === rs || "complete" === rs) {
            n.onreadystatechange = null;
            f(id, url);
        }
    };
    //……
    
    // // Safari 3.x supports the load event for script nodes (DOM2)
    
    //……

    n.addEventListener("load", function() {
        f(id, url);
    });

    // FireFox and Opera support onload (but not DOM2 in FF) handlers for
    // script nodes.  Opera, but not FF, supports the onload event for link
    // nodes.
} else {
    n.onload = function() {
        f(id, url);
    };
}
```
3、利用iframe和location.hash
这个办法比较绕，但是可以解决完全跨域情况下的脚步置换问题。原理是利用location.hash来进行传值。在url：[http://f2e.me#helloword](http://f2e.me/#helloword)中的‘#helloworld’就是location.hash,改变hash并不会导致页面刷新，所以可以利用hash值来进行数据传递，当然数据容量是有限的。假设域名f2e.me下的文件cs1.html要和space007.com域名下的cs2.html传递信息，cs1.html首先创建自动创建一个隐藏的iframe，iframe的src指向space007.com域名下的cs2.html页面，这时的hash值可以做参数传递用。cs2.html响应请求后再将通过修改cs1.html的hash值来传递数据。（因为ie不允许修改parent.location.hash的值，所以要借助于f2e.me域名下的一个代理iframe）。同时在cs1.html上加一个定时器，隔一段时间来判断location.hash的值有没有变化，一点有变化则获取获取hash值。代码如下：
先是f2e.me下的文件cs1.html文件：


```js
function startRequest() {
    var ifr = document.createElement('iframe');
    ifr.style.display = 'none';
    ifr.src = 'http://www.space007.com/lab/cscript/cs2.html#paramdo';
    document.body.appendChild(ifr);
}
 
function checkHash() {
    try {
        var data = location.hash ? location.hash.substring(1) : '';
        if (console.log) {
            console.log('Now the data is ' + data);
        }
    } catch (e) {};
}
setInterval(checkHash, 2000);
```



space007.com域名下的cs2.html:

```js
(function () {
    //模拟一个简单的参数处理操作
    switch (location.hash) {
    case '#paramdo':
        callBack();
        break;
    case '#paramset':
        //do something……
        break;
    }
 
    function callBack() {
        try {
            parent.location.hash = 'somedata';
        } catch (e) {
            //ie的安全机制无法修改parent.location.hash,所以要利用一个中间在space007域下的代理iframe
            var ifrproxy = document.createElement('iframe');
            ifrproxy.style.display = 'none';
            ifrproxy.src = 'http://f2e.me/test/cscript/cs3.html#somedata';
            document.body.appendChild(ifrproxy);
        }
    }
})();
```

f2e.me下的域名cs3.html
```js
//因为parent.parent和自身属于同一个域，所以ie下可以改变其location.hash的值
parent.parent.location.hash = self.location.hash.substring(1);
```

当然这样做也存在很多缺点，诸如数据直接暴露在了url中，数据容量和类型都有限等……

4、利用flash

这是从YUI3的IO组件中看到的办法,具体可见：[http://developer.yahoo.com/yui/3/io/](http://developer.yahoo.com/yui/3/io/)

flash这个方案不是很明白，各位自己慢慢琢磨了，呵呵。你可以看在Adobe Developer Connection看到更多的跨域代理文件规范：
ross-Domain Policy File Specifications.
HTTP Headers Blacklist.
----------------------------------------------------------------------------------------------------------------------------------------------
跨域的安全限制都是指浏览器端来说的.服务器端是不存在跨域安全限制的,
所以通过本机服务器端通过类似httpclient方式完成“跨域访问”的工作，然后在浏览器端用AJAX获取本机服务器端“跨域访问”对应的url.来间接完成跨域访问也是可以的.但很显然开发量比较大,但限制也最少,很多widget开放平台server端(如sohu博客开放平台)其实就么搞的.不在本次讨论范围.

        要讨论的是浏览器端的真正跨域访问,推荐的是目前jQuery $.ajax()支持get方式的跨域,这其实是采用jsonp的方式来完成的.

真实案例:

```js
var qsData = {
    'searchWord': $("#searchWord").attr("value"),
    'currentUserId': $("#currentUserId").attr("value"),
    'conditionBean.pageSize': $("#pageSize").attr("value")
};
 
$.ajax({
    async: false,
    url: http: //跨域的dns/document!searchJSONResult.action,
    type: "GET",
    dataType: 'jsonp',
    jsonp: 'jsoncallback',
    data: qsData,
    timeout: 5000,
    beforeSend: function () {
        //jsonp 方式此方法不被触发.原因可能是dataType如果指定为jsonp的话,就已经不是ajax事件了
    },
    success: function (json) { //客户端jquery预先定义好的callback函数,成功获取跨域服务器上的json数据后,会动态执行这个callback函数
        if (json.actionErrors.length != 0) {
            alert(json.actionErrors);
        }
        genDynamicContent(qsData, type, json);
    },
    complete: function (XMLHttpRequest, textStatus) {
        $.unblockUI({
            fadeOut: 10
        });
    },
    error: function (xhr) {
        //jsonp 方式此方法不被触发.原因可能是dataType如果指定为jsonp的话,就已经不是ajax事件了
        //请求出错处理
        alert("请求出错(请检查相关度网络状况.)");
    }
});
```

注意:

```js
$.getJSON("http://跨域的dns/document!searchJSONResult.action?name1=" + value1 + "&jsoncallback=?", function (json) {
    if (json.属性名 == 值) {
        // 执行代码
    }
});
```

这种方式其实是上例$.ajax({..}) api的一种高级封装,有些$.ajax api底层的参数就被封装而不可见了.

这样,jquery就会拼装成如下的url get请求

``http://跨域的dns/document!searchJSONResult.action?&amp;jsoncallback=jsonp1236827957501&amp;_=1236828192549&amp;searchWord=%E7%94%A8%E4%BE%8B&amp;currentUserId=5351&amp;conditionBean.pageSize=15``


在响应端(http://跨域的dns/document!searchJSONResult.action),
通过 jsoncallback = request.getParameter("jsoncallback") 得到jquery端随后要回调的js function name:jsonp1236827957501
然后 response的内容为一个Script Tags:"jsonp1236827957501("+按请求参数生成的json数组+")";
jquery就会通过回调方法动态加载调用这个js tag:jsonp1236827957501(json数组);
这样就达到了跨域数据交换的目的.

jsonp的最基本的原理是:动态添加一个`<script>`标签，而script标签的src属性是没有跨域的限制的。这样说来,这种跨域方式其实与ajax XmlHttpRequest协议无关了.
这样其实"jQuery AJAX跨域问题"就成了个伪命题了,jquery $.ajax方法名有误导人之嫌.
如果设为dataType: 'jsonp', 这个$.ajax方法就和ajax XmlHttpRequest没什么关系了,取而代之的则是JSONP协议.
JSONP是一个非官方的协议，它允许在服务器端集成Script tags返回至客户端，通过javascript callback的形式实现跨域访问
JSONP即JSON with Padding。由于同源策略的限制，XmlHttpRequest只允许请求当前源（域名、协议、端口）的资源。如果要进行跨域请求，
我们可以通过使用html的script标记来进行跨域请求，并在响应中返回要执行的script代码，其中可以直接使用JSON传递javascript对象。
这种跨域的通讯方式称为JSONP。

jsonCallback 函数jsonp1236827957501(....): 是浏览器客户端注册的，获取跨域服务器上的json数据后，回调的函数

Jsonp原理：

首先在客户端注册一个callback (如:'jsoncallback'), 然后把callback的名字(如:jsonp1236827957501)传给服务器。注意：服务端得到callback的数值后，要用jsonp1236827957501(......)把将要输出的json内容包括起来，此时，服务器生成 json 数据才能被客户端正确接收。

然后以 javascript 语法的方式，生成一个function , function 名字就是传递上来的参数 'jsoncallback'的值 jsonp1236827957501 .

最后将 json 数据直接以入参的方式，放置到 function 中，这样就生成了一段 js 语法的文档，返回给客户端。

客户端浏览器，解析script标签，并执行返回的 javascript 文档，此时javascript文档数据,作为参数，
传入到了客户端预先定义好的 callback 函数(如上例中jquery $.ajax()方法封装的的success: function (json))里.（动态执行回调函数）

可以说jsonp的方式原理上和`<script src="http://跨域/...xx.js"></script>`是一致的(qq空间就是大量采用这种方式来实现跨域数据交换的) .JSONP是一种脚本注入(Script Injection)行为,所以也有一定的安全隐患.

注意,jquey是不支持post方式跨域的.

为什么呢?

虽然采用post +动态生成iframe是可以达到post跨域的目的(有位js牛人就是这样把jquery1.2.5 打patch的),但这样做是一个比较极端的方式,不建议采用.
也可以说get方式的跨域是合法的,post方式从安全角度上,被认为是不合法的, 万不得已还是不要剑走偏锋..

client端跨域访问的需求看来也引起w3c的注意了,看资料说html5 WebSocket标准支持跨域的数据交换,应该也是一个将来可选的跨域数据交换的解决方案,

----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Ajax的应用中，由于安全的问题，浏览器默认是不支持跨域调用的。传统解决的方法，包括：（参考[http://bob.pythonmac.org/archives/2005/12/05/remote-json-jsonp/](http://bob.pythonmac.org/archives/2005/12/05/remote-json-jsonp/)）

Local proxy:
Needs infrastructure (can't run a serverless client) and you get double-taxed on bandwidth and latency (remote - proxy - client).
Flash:
Remote host needs to deploy a crossdomain.xml file, Flash is relatively proprietary and opaque to use, requires learning a one-off moving target programming langage.
Script tag:
Difficult to know when the content is available, no standard methodology, can be considered a "security risk".
以上方法都各有缺陷，都不是很好多解决方案。后来出现了一种叫JSON with Padding 的技术，简称 JSONP .（原理参考[http://bob.pythonmac.org/archives/2005/12/05/remote-json-jsonp/](http://bob.pythonmac.org/archives/2005/12/05/remote-json-jsonp/)），应用JSONP可以实现JSON数据的跨域调用。非常的幸运，JQuery1.2以后支持JSONP的应用。下面侧重说明在JQuery中，Json的跨域调用。

应用JSONP实现Json数据跨域调用，需要服务器端与客户端的合作完成。引用Jquery官方的例子，客户端掉用如下：

```js
$.getJSON(
    "[http://api.flickr.com/services/feeds/photos_public.gne?tags=cat&tagmode=any&format=json&jsoncallback](http://api.flickr.com/services/feeds/photos_public.gne?tags=cat&tagmode=any&format=json&jsoncallback)=?", function (
    data) {
    $.each(data.items, function (i, item) {
        $("<img/>").attr("src", item.media.m).appendTo("#images");
        if (i == 3) return false;
    });
});
```

 注意这里调用的地址中jsoncallback=?是关键的所在！其中，符号会被Query自动替换成其他的回调方法的名称，具体过程和原理我们这里不理会。我们关心的是jsoncallback=?起什么作用了？原来jsoncallback=?被替换后，会把方法名称传给服务器。我们在服务器端要做什么工作呢？服务器要接受参数jsoncallback，然后把jsoncallback的值作为JSON数据方法名称返回，比如服务器是JSP,我们会这样做：


```java
 String jsoncallback=request.getParameter("jsoncallback");
 out.print(jsoncallback+"({/"account/":/"XX/",/"passed/":/"true/",/"error/":/"null/"})");
```
Jquery取得的数据可能如下：

```json
JQUET0988788({"account":"XX","passed":"true","error":"null"})
```

总结，用JSONP要做两件事：

 1. 请求地址加参数：jsoncallback=?

 2. 服务器段把jsoncallback的值作为方法名传回来，如JQUET098788(...)

转载自：[ajax 跨域 - Monkey D MaoEr - 博客频道 - CSDN.NET](http://blog.csdn.net/coldy456/article/details/5982712).