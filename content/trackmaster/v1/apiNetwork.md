---
title: API - 工作网络
---
#API - 工作网络

<h2 id="p1">获取当前授权用户有权操作的所有工作网络</h2>

    GET /networks

###响应
<pre class="headers">
<code>Status: 200 OK
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 4999
</code></pre>
<pre class="highlight">
<code class="language-javascript">
[
   {
    "id": 1,
    "url": "https://api.trackmaster.com.cn/networks/1",
    "name": "测试工作网络", //别名
    "created_at": "2012-09-06T20:39:23Z" //创建时间
   }
]
</code></pre>

关于错误返回值与错误代码，参见[错误代码说明][apiCommon]。


###适用版本
[v1.0][version]


<h2 id="p2">获取指定 ID 工作网络信息</h2>

    GET /networks/:id

###响应
<pre class="headers">
<code>Status: 200 OK
Link: <http://api.trackmaster.com.cn/networks/1/advs>; rel="advs"
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 4999
</code></pre>
<pre class="highlight">
<code class="language-javascript">
{
  "id": 1,
  "url": "https://api.trackmaster.com.cn/networks/1",
  "name": "测试工作网络",//别名
  "created_at": "2012-09-06T20:39:23Z" //创建时间
}
</code></pre>

关于错误返回值与错误代码，参见[错误代码说明][apiCommon]。


###适用版本

[v1.0][version]


[version]: /trackmaster/v1/apiVersion/
[apiCommon]:/trackmaster/v1/apiCommon/#p5