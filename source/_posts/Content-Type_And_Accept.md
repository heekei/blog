---
title: '简述Http协议中的Content-Type和Accept的关系'
categories:
  - web开发
date: 2018-05-02 21:48:08
tags:
---

首先要先介绍一下Http协议中的两个“头”，一个是`Request Header`，另一个是`Response Header`。

*详情请参见：* [HTTP Headers](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers)

## Request Header

客户端发起请求时，用来附加一些信息给服务器，称之为`请求头`。

## Response Header

服务器响应请求时，用来附加一些信息给客户端，称之为`响应头`。

## Accept 和 Content-Type

- `Accept` 是请求头中的一个字段
- `Content-Type`是响应头中的一个字段。

    在特定条件和浏览器版本中，`Accept`会被赋予默认值，具体值请参见：[Accept默认值](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Content_negotiation/Accept_%E9%BB%98%E8%AE%A4%E5%80%BC)。

    而`Content-Type`由后端服务器设置它的值，不设置的话通常默认为`text/plain`。在此不做详述。

在一般情况下，服务器会根据`Accept`的值，来决定返回的数据的类型，并设置`Content-Type`，通常`Content-Type`的值和`Accept`的第一个值相等。

## AngularJS 1.5中的例子

在AngularJS 1.5中，$http默认的`Accept`被设置为`application/json, text/plain, */*`，
见Angular源码第10288行：
```js
headers: {
  common: {
    'Accept': 'application/json, text/plain, */*'
  },
  post:   shallowCopy(CONTENT_TYPE_APPLICATION_JSON),
  put:    shallowCopy(CONTENT_TYPE_APPLICATION_JSON),
  patch:  shallowCopy(CONTENT_TYPE_APPLICATION_JSON)
},
```

在$http请求结束后，angular会判断服务器返回的data值是否为JSON形式的字符串，如果是的话，将会进行一次JSON格式化。从而确保即使后端服务器返回的`Content-Type`值为`text/html`，也能将其正常解析为JSON对象。
见Angular源码第10125行：
```js
function defaultHttpResponseTransform(data, headers) {
  if (isString(data)) {
    // Strip json vulnerability protection prefix and trim whitespace
    var tempData = data.replace(JSON_PROTECTION_PREFIX, '').trim();

    if (tempData) {
      var contentType = headers('Content-Type');
      if ((contentType && (contentType.indexOf(APPLICATION_JSON) === 0)) || isJsonLike(tempData)) {
        data = fromJson(tempData);
      }
    }
  }

  return data;
}
```