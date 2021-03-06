---
weight: 4
layout: default
category: openmaster
language: cn
title: 协议及请求方式
---

# {{ page.title }}

本文讲述了组成官方 AdMaster API 的资源信息。本 API 处于测试状态。可能会有一些重大更改。

* TOC
{:toc}

## 架构

所有 API 的访问都是通过 HTTP 执行的，所有被发送和接收的数据都是 JSON。

    $ curl -i https://track.admasterapi.com

{:.prettyprint}
    HTTP/1.1 302 Found
    Server: nginx/1.0.12
    Date: Mon, 20 Feb 2012 11:15:49 GMT
    Content-Type: text/html;charset=utf-8
    Connection: keep-alive
    Status: 302 Found
    X-RateLimit-Limit: 5000
    ETag: "d41d8cd98f00b204e9800998ecf8427e"
    Location: http://developer.admaster.com.cn
    X-RateLimit-Remaining: 4999
    Content-Length: 0


空白区域作为 null 而不是被忽略。

所有的时间戳返回格式是ISO 8601：`YYYY-MM-DDTHH:MM:SSZ`

##  客户端错误

调用 API 客户端的错误有三种可能类型，它们收到的请求主体是：

1.发送无效 JSON 将导致 400 错误请求响应。

    HTTP/1.1 400 Bad Request
    Content-Length: 35

{:.prettyprint}
    {"message":"Problems parsing JSON"}


2.发送错误类型的 JSON 值将导致 400 错误请求响应

    HTTP/1.1 400 Bad Request
    Content-Length: 40

{:.prettyprint}
    {"message":"Body should be a JSON Hash"}


3.发送无效文件将导致 422 无法处理的实体响应。

    HTTP/1.1 422 Unprocessable Entity
    Content-Length: 149

{:.prettyprint}
    {
      "message": "Validation Failed",
      "errors": [
        {
          "resource": "Issue",
          "field": "title",
          "code": "missing_field",
          "value":"Resource Value"
        }
      ]
    }


所有的错误对象都有资源和字段属性，您的客户端可以说明问题所在。还有一个错误代码让您知道什么是错误区域，下面这些都是可能的验证错误代码：

* `missing` 这意味着资源不存在。
* `missing_field` 这意味着对资源所需的领域尚未确定。
* `invalid` 这意味着领域格式不合法。资源文档应该给您提供更专业的信息。
* `already_exists` 这意味着已经存在和该领域同样值的资源了。这就要求要有独立的key（如：标签名称）

如果资源有自定义的验证错误，他们将用资源来记录。

##  HTTP 谓词

对于每一个动作，API 尽可能的驱使您使用 HTTP 谓词。

`HEAD` 只获取 HTTP 的头信息，不管其他资源信息

`GET` 用于检索资源

`POST` 用于创建资源、执行自定义动作（如合并，推送）

`PATCH` 利用部分 JSON 数据更新资源，例如，一个资源有 title 和 body 属性。一个 PATCH 需求可能是通过接收一个或多个属性进行资源更新。PATCH 是一个相对较新的和不常见的 HTTP 谓词，所以资源端点也接收 POST 需求。

`PUT` 用于收集或代替资源。PUT 需求没有 body 属性，所以要确定设置内容长度为 0。

`DELETE` 用于删除资源。

## 认证


OAuth2 Token (头部发送):

    $ curl -H "Authorization: token OAUTH-TOKEN" http://track.admasterapi.com


OAuth2 Token (作为参数发送):

    $ curl http://track.admasterapi.com/advertisers?access_token=OAUTH-TOKEN


在某些地方，对认证的需求将返回 404，而不是 403。这是为了防止意外泄漏给未经授权的用户的私人资料。

## 分页

默认情况下，多项目请求将会被分页成 30 项。您可以利用参数`?page`指定进一步页，您也可以利用参数`?per_page`自定义页大小到 100。

    $ curl http://track.admasterapi.com/advertisers?page=2&amp;per_page=100


分页信息包含在 the link header：

{:.prettyprint}
    Link: <http://track.admasterapi.com/advertisers?page=3&per_page=100>; rel=”next”,
      <http://track.admasterapi.com/advertisers?page=50&per_page=100>; rel=”last”

换行仅做显示用。

`rel` 的可能值有：

* `next` 立即显示下页结果的URL。
* `last` 显示上页结果的URL。
* `first` 显示第一页结果的URL。
* `prev` 立即显示前页结果的URL。


## 速度限制

我们以每小时 5000 限制的 API 的请求。这是记录了您的登录，您的 OAuth 的令牌，或请求的 IP。
您可以检查返回的 API 请求的 HTTP 头，查看您当前的状态：

{:.prettyprint}
    $ curl -i https://track.admasterapi.com/users/whatever

    HTTP/1.1 200 OK
    Status: 200 OK
    X-RateLimit-Limit: 5000
    X-RateLimit-Remaining: 4966

你可以联系我们要求进行您应用程序白名单访问，我们更推荐为用户设置 OAuth 应用。

## 跨域资源共享

对于 AJAX 需求的，API 支持跨域资源共享。你可以从 HTML5 安全指南中阅读 CORS W3C working draft, 或者 this intro。


下面是一个发自浏览器的示例需求`http://some-site.com`：

    $ curl -i https://track.admasterapi.com -H "Origin: http://some-site.com"
    HTTP/1.1 302 Found

任何一个被接受的域名都是作为 OAUTH 应用程序注册的。下面是一个示例需求，用于浏览器触及 Calendar About Nothing:

    $ curl -i https://track.admasterapi.com -H "Origin: http://calendaraboutnothing.com"

{:.prettyprint}
    HTTP/1.1 302 Found
    Access-Control-Allow-Origin: http://calendaraboutnothing.com
    Access-Control-Expose-Headers: Link, X-RateLimit-Limit, X-RateLimit-Remaining, X-OAuth-Scopes, X-Accepted-OAuth-Scopes
    Access-Control-Allow-Credentials: true


下面是 CORS 系统预检请求：

    $ curl -i https://track.admasterapi.com -H "Origin: http://calendaraboutnothing.com" -X OPTIONS

{:.prettyprint}
    HTTP/1.1 204 No Content
    Access-Control-Allow-Origin: http://calendaraboutnothing.com
    Access-Control-Allow-Headers: Authorization, X-Requested-With
    Access-Control-Allow-Methods: GET, POST, PATCH, PUT, DELETE
    Access-Control-Expose-Headers: Link, X-RateLimit-Limit, X-RateLimit-Remaining, X-OAuth-Scopes, X-Accepted-OAuth-Scopes
    Access-Control-Max-Age: 86400
    Access-Control-Allow-Credentials: true


## JSON-P 回调(暂未支持)

你可以发送参数`?callback`到任何GET回调函数以获得 JSON 函数的结果包裹。浏览器通过跨域方式利用网页嵌入 AdMaster 内容就是一种典型的应用。
响应包括定期 API 相同的数据输出，加上相关的 HTTP 头信息。

    $ curl https://track.admasterapi.com/advertisers?callback=foo

{:.prettyprint}
    foo({
      "meta": {
        "status": 200,
        "X-RateLimit-Limit": "5000",
        "X-RateLimit-Remaining": "4966",
        "Link": [ // pagination headers and other links
          ["https://track.admasterapi.com/advertisers?page=2", {"rel": "next"}]
        ]
      },
      "data": {
        // the data
      }
    })


你可以写一个 javascript 处理程序来处理这样的回调：

    $ curl https://track.admasterapi.com/advertisers?callback=foo

{:.prettyprint}
    foo({
      "meta": {
        "status": 200,
        "X-RateLimit-Limit": "5000",
        "X-RateLimit-Remaining": "4966",
        "Link": [ // pagination headers and other links
          ["https://track.admasterapi.com/advertisers?page=2", {"rel": "next"}]
        ]
      },
      "data": {
        // the data
      }
    })



头都是相同的字符串值作为 HTTP 头，有一个例外：link。通过数组`[url, options]`阵列，link 头部预先为您解析。

所谓的 link，如下：

{:.prettyprint}
    Link: <url1>; rel=”next”, <url2>; rel=”foo”; bar=”baz”


...在回调输出中是这样的：

{:.prettyprint}
    {
      "Link": [
        [
          "url1",
          {
            "rel": "next"
          }
        ],
        [
          "url2",
          {
            "rel": "foo",
            "bar": "baz"
          }
        ]
      ]
    }
