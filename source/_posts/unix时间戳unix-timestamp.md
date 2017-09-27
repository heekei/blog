---
title: Unix时间戳(Unix timestamp)
tags:
  - 'C#'
  - JAVA
  - PHP
  - ASP
  - .NET
  - JavaScript
  - MySQL
  - Perl
  - PostgreSQL
  - Python
  - Ruby
  - SQL Server
  - Unix
  - Linux
  - VBScript
id: .nan
categories:
  - 'C#'
  - web开发
date: 2014-05-31 12:12:24
---

<div class="utddiv" style="font-weight: bold; color: #006aad;">如何在不同编程语言中获取现在的Unix时间戳(Unix timestamp)</div>
<!--more-->
<table style="color: #777777;">
<tbody>
<tr>
<td class="uttd" style="font-weight: bold;">Java</td>
<td>time</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">JavaScript</td>
<td>Math.round(new Date().getTime()/1000)
getTime()返回数值的单位是毫秒</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Microsoft .NET / C#</td>
<td>epoch = (DateTime.Now.ToUniversalTime().Ticks - 621355968000000000) / 10000000</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">MySQL</td>
<td>SELECT unix_timestamp(now())</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Perl</td>
<td>time</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">PHP</td>
<td>time()</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">PostgreSQL</td>
<td>SELECT extract(epoch FROM now())</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Python</td>
<td>先 import time 然后 time.time()</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Ruby</td>
<td>获取Unix时间戳：Time.now 或 Time.new
显示Unix时间戳：Time.now.to_i</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">SQL Server</td>
<td>SELECT DATEDIFF(s, '1970-01-01 00:00:00', GETUTCDATE())</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Unix / Linux</td>
<td>date +%s</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">VBScript / ASP</td>
<td>DateDiff("s", "01/01/1970 00:00:00", Now())</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">其他操作系统
(如果Perl被安装在系统中)</td>
<td>命令行状态：perl -e "print time"</td>
</tr>
</tbody>
</table>
<div class="utddiv" style="font-weight: bold; color: #006aad;">如何在不同编程语言中实现Unix时间戳(_Unix timestamp_) → 普通时间？</div>
<table class="getcurrentunixtimetable" style="color: #777777;">
<tbody>
<tr>
<td class="uttd" style="font-weight: bold;">Java</td>
<td>String date = new java.text.SimpleDateFormat("dd/MM/yyyy HH:mm:ss").format(new java.util.Date(<span style="text-decoration: underline;">Unix timestamp</span> * 1000))</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">JavaScript</td>
<td>先 var unixTimestamp = new Date(<span style="text-decoration: underline;">Unix timestamp</span> * 1000) 然后commonTime = unixTimestamp.toLocaleString()</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Linux</td>
<td>date -d @<span style="text-decoration: underline;">Unix timestamp</span></td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">MySQL</td>
<td>from_unixtime(<span style="text-decoration: underline;">Unix timestamp</span>)</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Perl</td>
<td>先 my $time = <span style="text-decoration: underline;">Unix timestamp</span> 然后 my ($sec, $min, $hour, $day, $month, $year) = (localtime($time))[0,1,2,3,4,5,6]</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">PHP</td>
<td>date('r', <span style="text-decoration: underline;">Unix timestamp</span>)</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">PostgreSQL</td>
<td>SELECT TIMESTAMP WITH TIME ZONE 'epoch' + <span style="text-decoration: underline;">Unix timestamp</span>) * INTERVAL '1 second';</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Python</td>
<td>先 import time 然后 time.gmtime(<span style="text-decoration: underline;">Unix timestamp</span>)</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Ruby</td>
<td>Time.at(<span style="text-decoration: underline;">Unix timestamp</span>)</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">SQL Server</td>
<td>DATEADD(s, <span style="text-decoration: underline;">Unix timestamp</span>, '1970-01-01 00:00:00')</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">VBScript / ASP</td>
<td>DateAdd("s", <span style="text-decoration: underline;">Unix timestamp</span>, "01/01/1970 00:00:00")</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">其他操作系统
(如果Perl被安装在系统中)</td>
<td>命令行状态：perl -e "print scalar(localtime(<span style="text-decoration: underline;">Unix timestamp</span>))"</td>
</tr>
</tbody>
</table>
<div class="utddiv" style="font-weight: bold; color: #006aad;">如何在不同编程语言中实现普通时间 → Unix时间戳(_Unix timestamp_)？</div>
<table style="color: #777777;">
<tbody>
<tr>
<td class="uttd" style="font-weight: bold;">Java</td>
<td>long epoch = new java.text.SimpleDateFormat("<span style="text-decoration: underline;">dd/MM/yyyy HH:mm:ss</span>").parse("01/01/1970 01:00:00");</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">JavaScript</td>
<td>var commonTime = new Date(Date.UTC(<span style="text-decoration: underline;">year</span>, <span style="text-decoration: underline;">month</span> - 1, <span style="text-decoration: underline;">day</span>, <span style="text-decoration: underline;">hour</span>,<span style="text-decoration: underline;">minute</span>, <span style="text-decoration: underline;">second</span>))</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">MySQL</td>
<td>SELECT unix_timestamp(<span style="text-decoration: underline;">time</span>)
时间格式: YYYY-MM-DD HH:MM:SS 或 YYMMDD 或 YYYYMMDD</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Perl</td>
<td>先 use Time::Local 然后 my $time = timelocal($sec, $min, $hour, $day, $month, $year);</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">PHP</td>
<td>mktime(<span style="text-decoration: underline;">hour</span>, <span style="text-decoration: underline;">minute</span>, <span style="text-decoration: underline;">second</span>, <span style="text-decoration: underline;">day</span>, <span style="text-decoration: underline;">month</span>, <span style="text-decoration: underline;">year</span>)</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">PostgreSQL</td>
<td>SELECT extract(epoch FROM date('<span style="text-decoration: underline;">YYYY-MM-DD HH:MM:SS</span>'));</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Python</td>
<td>先 import time 然后 int(time.mktime(time.strptime('<span style="text-decoration: underline;">YYYY-MM-DD HH:MM:SS</span>', '%Y-%m-%d %H:%M:%S')))</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Ruby</td>
<td>Time.local(<span style="text-decoration: underline;">year</span>, <span style="text-decoration: underline;">month</span>, <span style="text-decoration: underline;">day</span>, <span style="text-decoration: underline;">hour</span>, <span style="text-decoration: underline;">minute</span>, <span style="text-decoration: underline;">second</span>)</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">SQL Server</td>
<td>SELECT DATEDIFF(s, '1970-01-01 00:00:00', <span style="text-decoration: underline;">time</span>)</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">Unix / Linux</td>
<td>date +%s -d"Jan 1, 1970 00:00:01"</td>
</tr>
<tr>
<td class="uttd" style="font-weight: bold;">VBScript / ASP</td>
<td>DateDiff("s", "01/01/1970 00:00:00", <span style="text-decoration: underline;">time</span>)</td>
</tr>
</tbody>
</table>
转载自：[http://tool.chinaz.com/Tools/unixtime.aspx](http://tool.chinaz.com/Tools/unixtime.aspx)