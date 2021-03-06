---
weight: 12
layout: default
category: trackmaster
subcategory: agency
language: cn
title: 项目报告
---

# 项目报告

##### 通知：获取地域数据原维度geo改为province，文档中已更改，原有geo会并行一段时间，请开发者及时更改。 #####


* TOC
{:toc}

## 获取指定项目下有操作权限的项目报告列表

    GET /campaigns/:campaign_id/reports

**参数**

`time`
: _可选_ **string** - 数据时间类型,与参数 `start_time` 和 `end_time` 共同使用。


  * `hourly` 获取小时数据
  * `daily` 获取日数据——默认

`dims`
: _可选_ **string** - 数据聚合维度,多个选项之间用`,`分开。

  * `time` 按时间维度聚合、结果会显示具体的时间列
  * `media` 按媒体维度聚合
  * `placement` 按广告位维度聚合
  * `keyword` 按关键字维度聚合
  * `creative` 按创意维度聚合
  * `province` 按省级地域维度聚合

`network_media_id`
: _可选_ **integer** - 网络媒体 ID

`placement_id`
: _可选_ **integer** - 广告位 ID

`keyword_id`
: _可选_ **integer** - 关键字 ID

`creative_id`
: _可选_ **integer** - 创意 ID

`geo_id`
: _可选_ **integer** - 地域 ID

`start_time`
: _可选_ **hour** - 报告开始时间，与参数`time`一起使用。
采用国际标准 ISO 8601 的日期和时间显示格式。

  * 当参数 `time` 选择 `小时`，时间格式 YYYY-MM-DDThh:mm:ss+08:00。例 2012-11-06T01:00:00+08:00。仅支持北京时间的时区表示，输出格式同样为 YYYY-MM-DDThh:mm:ss+08:00。

  * 当参数 `time` 选择 `天`，时间格式 YYYY-MM-DD。例 2012-11-06。



`end_time`
: _可选_ **hour** - 报告结束时间，与参数`time`一起使用。
采用国际标准 ISO 8601 的日期和时间显示格式。

  * 当参数 `time` 选择 `小时`，时间格式 YYYY-MM-DDThh:mm:ss+08:00。例 2012-11-06T01:00:00+08:00。仅支持北京时间的时区表示，输出格式同样为 YYYY-MM-DDThh:mm:ss+08:00。

  * 当参数 `time` 选择 `天`，时间格式 YYYY-MM-DD。例 2012-11-06。


`sort`
: _可选_ **string** - 列表排序以什么排序，结合参数`direction`使用。

  * `time` - 按照时间排序
  * `imp` - 按照曝光排序
  * `clk` - 按照点击排序
  * `uimp` - 按照独立曝光排序
  * `uclk` - 按照独立点击排序
  * `network_media_id` - 按照网络媒体 ID 排序
  * `placement_id` - 按照广告位 ID 排序

`direction`
: _可选_ **string** - 排序方式，结合参数`sort`使用。

  * `asc` 升序 (_默认_)
  * `desc` 降序

`page`
: _可选_ **integer** - 显示页码

	默认显示页码为 ‘1’，起始页为 ‘1’ 而不是 ‘0’。`page` 和 `per_page`一起使用，例如当返回的数据超过 30 条时，可以通过设定 `page`显示 30 条之后的数据。

`per_page`
: _可选_ **integer** - 分页数量，默认每页 30 条

	返回数据的数目。当不指定`per_page` 时，默认最大返回 30 条数据。`per_page` 和 `page` 一起使用显示一系列数据或者单独使用限制

**响应**

    Status: 200 OK
    Link: <http://{{site.track_api_host}}/campaigns/12/reports?page=2>; rel="next",
          <http://{{site.track_api_host}}/campaigns/12/reports?page=10>; rel="last"

{:.prettyprint}
    [
      {
        "campaign_id": 10185,//项目 ID
        "network_media_id": 1484,//媒体网络 ID
        "time": "2012-08-03",//此处 `time` 格式与参数 `start_time`、`end_time` 一致，例如当参数`time`为 daily，则此处时间为从 `start_time` 到 `end_time` 分天时间.
        "imp": 90,//曝光数
        "uimp": 60,//独立曝光数
        "clk": 30,//点击数
        "uclk": 23,//独立点击数
      }
    ]


**字段说明**

返回值字段 | 字段类型     | 字段说明
imp      | integer     | 曝光
uimp     | integer     | 独立曝光
clk      | integer     | 点击
uclk     | integer     | 独立点击

**获取数据组合说明**

不是所有的属性都可以搭配获取数据，只有特定的组合才可以获取到相应数据。当选择了 dims=time 时，显示内容按时间分组聚合。

组合|说明
time=daily  |粒度为天，项目数据
time=daily&dims=creative|粒度为天，项目分创意数据
time=daily&dims=province|粒度为天，项目分省级地域数据
time=daily&dims=media|粒度为天，项目分媒体数据
time=daily&dims=media,province|粒度为天，项目分媒体分省级地域数据
time=daily&dims=media,creative |粒度为天，项目分媒体分创意数据
time=daily&dims=media,placement|粒度为天，项目分媒体分广告位数据
time=daily&dims=media,placement,creative |粒度为天，项目分媒体分广告位分创意数据
time=daily&dims=media,placement,province|粒度为天，项目分媒体分广告位分省级地域数据
time=daily&dims=media,placement,keyword|粒度为天，项目分媒体分广告位分关键字数据
time=hourly|粒度为小时，项目数据
time=hourly&dims=creative|粒度为小时，项目分创意数据
time=hourly&dims=province |粒度为小时，项目分省级地域数据
time=hourly&dims=media |粒度为小时，项目分媒体数据
time=hourly&dims=media,creative |粒度为小时，项目分媒体分创意数据
time=hourly&dims=media,province|粒度为小时，项目分媒体分省级地域数据
time=hourly&dims=media,placement|粒度为小时，项目分媒体分广告位数据
time=hourly&dims=media,placement,creative |粒度为小时，项目分媒体分广告位分创意数据
time=hourly&dims=media,placement,province|粒度为小时，项目分媒体分广告位分省级地域数据



**示例**

维度参数包括 time，显示内容按时间分组聚合。

{:.prettyprint}
    [
        {
            "campaign_id": 10116,
            "time": "2012-11-01",
            "imp": 3,
            "uimp": 3,
            "clk": 7,
            "uclk": 7,
        },
        {
            "campaign_id": 10116,
            "time": "2012-11-02",
        …
    ]

维度参数不选择 time

{:.prettyprint}
    [
        {
            "campaign_id": 10116,
            "imp": 28,
            "uimp": 27,
            "clk": 72,
            "uclk": 39,
        }
    ]

