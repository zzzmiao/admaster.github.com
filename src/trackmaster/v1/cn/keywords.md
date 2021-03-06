---
weight: 5
layout: default
category: trackmaster
subcategory: agency
language: cn
title: 关键字
---

# 关键字

* TOC
{:toc}

## 获取广告位下的关键字列表

    GET /networks/advertisers/campaigns/placements/:placement_id/keywords

**参数**     

`page`
: _可选_ **integer** - 显示页码  
    
	默认显示页码为 ‘1’，起始页为 ‘1’ 而不是 ‘0’。`page`和`per_page`一起使用，例如当返回的数据超过 30 条时，可以通过设定`page`显示 30 条之后的数据。    

`per_page`
: _可选_ **integer** - 分页数量，默认每页 30 条        

	`per_page`和`page`一起使用显示一系列数据或者单独使用限制返回数据的数目。当不指定`per_page` 时，默认最大返回 30 条数据。


**响应**

    Status: 200 OK
    X-RateLimit-Limit: 5000
    X-RateLimit-Remaining: 4999

{:.prettyprint}
        {
            //广告位ID
            "placement_id":1
            //关键字ID
            "order_num":1
            //关键字名称
            "name": Example
            //关键字点击目标地址
            "target_url": http://www.admaster.com.cn
            //创建时间
            "created_at": "2012-12-27T15:45:27Z"
        }
   


## 获取指定广告位下的关键字详情

    GET /networks/advertisers/campaigns/placements/:placement_id/keywords/:keyword_id

**响应**

    Status: 200 OK
    X-RateLimit-Limit: 5000
    X-RateLimit-Remaining: 4999

{:.prettyprint}
        {
            //广告位ID
            "placement_id":1
            //关键字ID
            "order_num":1
            //关键字名称
            "name": Example
            //关键字点击目标地址
            "target_url": http://www.admaster.com.cn
            //创建时间
            "created_at": "2012-12-27T15:45:27Z"
        }


## 添加关键字在指定的广告位下

    POST /networks/advertisers/campaigns/placements/:placement_id/keywords

注意：同一个广告位下创建关键字,关键字 URL 不能超过 100 个。 

**参数**

`name`
: _必选_ **string** - 关键字位置名称,字符串，长度 3-100 个字符

`target_url`
: _可选_ **string** - 关键字默认点击目标地址，例如: http://www.admaster.com.cn/


**请求**

{:.prettyprint}
    	{
        	"name": "keyword name"
        	"target_url": "http://www.admaster.com.cn"
    	}

**响应**

    Status: 201 Created
    Location: http://{{site.track_api_host}}/networks/advertisers/campaigns/placements/keywords/1
    X-RateLimit-Limit: 5000
    X-RateLimit-Remaining: 4999

{:.prettyprint}
	 {
        //广告位 ID
        "placement_id":1
        //关键字 ID
        "order_num":1
        //关键字名称
        "name": Example
        //关键字点击目标地址
        "target_url": http://www.admaster.com.cn
        //创建时间
        "created_at": "2012-12-27T15:45:27Z"
    }


## 修改指定广告位下的关键字

    PATCH /networks/advertisers/campaigns/placements/:placement_id/keywords/:keyword_id

**参数**

`name`
: _必选_ **string** - 关键字位置名称,字符串，长度 3-100 个字符

`target_url`
: _可选_ **string** - 关键字默认点击目标地址，例如: http://www.admaster.com.cn/

**请求**

{:.prettyprint}
    	{
        	"name": "keyword name"
        	"target_url": "http://www.admaster.com.cn"
    	}

**响应**

    Status: 204 No Content
    X-RateLimit-Limit: 5000
    X-RateLimit-Remaining: 4999



## 删除指定广告位下的关键字

    DELETE /networks/advertisers/campaigns/placements/:placement_id/keywords/:keyword_id


**响应**

    Status: 204 No Content
    Location: http://{{site.track_api_host}}/networks/advertisers/campaigns/placements/1/keywords
    X-RateLimit-Limit: 5000
    X-RateLimit-Remaining: 4996



