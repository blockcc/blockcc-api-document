---
title: Block.cc APIv3 开发者文档

language_tabs: # must be one of https://git.io/vQNgJ
- shell

toc_footers:

- <a target="_blank" href='https://data.mifengcha.com/register?lang=zh_CN'>创建API KEY</a>
- <a href='../v1/'>查看v1文档</a>
- <a href="http://cn.mikecrm.com/TNFyHPJ">交易所收录</a>
- <a href="http://mifengcha.mikecrm.com/Zha7vqZ">币种收录</a>
- <a href='https://data.mifengcha.com/contactus'>问题反馈</a>
- <br>
- <a class="locale-button" href='../zh_CN'><img src="/images/flags/zh_CN.svg" alt="简体中文"/></a> <a class="locale-button" href='../en_US'><img src="/images/flags/en_US.svg" alt="English"/></a>

search: false
code_clipboard: true
---

# 更新日志
<table>
	<tr>
		<th width="110px">生效时间(UTC+8)</th>
		<th width="50px">变化</th>
		<th>更新内容</th>
	</tr>
<tr>
		<td>2021.05.26</td>
		<td>新增</td>
		<td>
			币种新增 platforms 字段
		</td>
	</tr>
	<tr>
		<td>2021.01.21</td>
		<td>新增</td>
		<td>
			新增JAVA SDK
		</td>
	</tr>
	<tr>
		<td>2020.10.16</td>
		<td>新增</td>
		<td>历史价格接口新增时间间隔请求参数 </td>
	</tr>
	<tr>
		<td>2020.10.01</td>
		<td>优化</td>
		<td>API v1不再为免费用户提供, APIv3已稳定为用户服务了1年多的时间, 建议更新到APIv3获得更好的体验.</td>
	</tr>
	<tr>
		<td>2020.05.25</td>
		<td>新增</td>
		<td>新增社交媒体API</td>
	</tr>
	<tr>
		<td>2020.04.16</td>
		<td>新增</td>
		<td>K线新增1分钟类型</td>
	</tr>
	<tr>
		<td>2020.04.09</td>
		<td>新增</td>
		<td>价格新增字段：24小时最高价、24小时最低价、1周涨跌幅、1周最高价、1周最低价、1月涨跌幅、1月最高价、1月最低价、历史最高价、历史最低价</td>
	</tr>
</table>


# 简介
欢迎您选择蜜蜂查API服务！

此文档是蜜蜂查的唯一官方文档，蜜蜂查API提供的功能和服务会在此文档持续更新。

蜜蜂查提供了丰富的API接口供开发者使用，接入简单，使用方便。第三方应用开发者可以借助该服务，快速构建稳定高效的数字货币行情系统，为实时业务和产品运营提供技术支持。
<br>
<br>



**如何阅读本文档**

文档左侧是目录 ，中间是正文，右侧是请求参数和响应结果的示例。



以下是蜜蜂查API文档各章节主要内容
<br>
<br>

第一部分是概要介绍：

- **快速入门：** 该章节对蜜蜂查API做了简单且全方位的介绍，适合第一次使用蜜蜂查API的用户。

- **联系我们：** 该章节介绍遇到问题，如何联系我们。
  <br>
  <br>


第二部分是每个接口类的详细介绍，每个接口类一个章节，每个章节分为如下内容：

- **简介：** 对该接口类进行简单介绍，包括一些注意事项和说明。

- **具体接口：** 介绍每个接口的用途、限频、请求、参数、返回等详细信息。

- **常见问题：** 介绍该接口类下常见问题和解答。


# 快速入门
## 接入准备
如需使用蜜蜂查API，请先登录账号，完成API Key的申请，再根据本文档详情进行开发。
如果您还没有API密钥，请[点击注册](https://data.mifengcha.com/register  
)。

您可以通过以下方式在REST API调用中提供API密钥：

1. 首选方法：通过名为`X-API-KEY`的自定义请求头
2. 便捷方法：通过名为`api_key` 的查询字符串参数

在Websocket API中只能通过名为api_key 的查询字符串参数提供API密钥

<h2 id="SDK">SDK</h2>
蜜蜂查API SDK：<a href="https://github.com/blockcc/blockcc-api-client-java" target="_blank">JAVA</a>

## 接口类型
蜜蜂查API为用户提供两种接口，您可根据自己的使用场景和偏好来选择适合的方式进行行情查询。

##### REST API
REST，即Representational State Transfer的缩写，是目前较为流行的基于HTTP的一种通信机制，每一个URL代表一种资源。

##### WebSocket API
WebSocket是HTML5一种新的协议（Protocol）。它实现了客户端与服务器全双工通信，通过一次简单的握手就可以建立客户端和服务器连接，服务器可以根据业务规则主动推送信息给客户端。

## 接入URLS
您可以自行比较使用data.block.cc和data.mifengcha.com两个域名的延迟情况，选择延迟低的进行使用。其中，data.mifengcha.com域名对中国大陆的用户做了一定的链路延迟优化。

##### REST API
https://data.block.cc/api

https://data.mifengcha.com/api

##### Websocket Feed
wss://data.block.cc/ws

wss://data.mifengcha.com/ws

# 接入说明
## 接口概览


<table>
    <tr>
        <td><strong>接口数据类型</strong></td>
        <td><strong>描述</strong></td>
        <td><strong>接口数据类型</strong></td>
    </tr>
    <tr>
        <td align="center" rowspan="4" style="vertical-align: middle;">基础数据</td>
        <td>获取所有支持的交易所列表</td>
        <td>/v3/markets</td>
    </tr>
    <tr>
        <td>获取指定交易所信息</td>
        <td>/v3/markets/{slug}</td>
    </tr>
    <tr>
        <td>获取所有支持的币种列表</td>
        <td>/v3/symbols</td>
    </tr>
    <tr>
        <td>获取单个币种信息</td>
        <td>/v3/symbols/{slug}</td>
    </tr>
    <tr>
        <td align="center" rowspan="7" style="vertical-align: middle;">行情数据</td>
        <td>获取汇率</td>
        <td>/v3/exchange_rate</td>
    </tr>
    <tr>
        <td>获取币种价格</td>
        <td>/v3/price</td>
    </tr>
    <tr>
        <td>获取币种历史价格</td>
        <td>/v3/price/history?slug=bitcoin</td>
    </tr>
    <tr>
        <td>批量获取交易对Tickers</td>
        <td>/v3/tickers</td>
    </tr>
    <tr>
        <td>获取交易对深度</td>
        <td>/v3/orderbook</td>
    </tr>
    <tr>
        <td>获取交易对成交记录</td>
        <td>/v3/trades</td>
    </tr>
    <tr>
        <td>获取交易对K线数据</td>
        <td>/v3/kline</td>
    </tr>
    <tr>
        <td align="center" rowspan="5" style="vertical-align: middle;">资讯数据</td>
        <td>获取快讯数据</td>
        <td>/v3/briefs?locale=zh_CN</td>
    </tr>
    <tr>
        <td>获取交易所公告数据</td>
        <td>/v3/announcements?locale=zh_CN</td>
    </tr>
    <tr>
        <td>获取资讯文章列表</td>
        <td>/v3/articles?locale=zh_CN</td>
    </tr>
    <tr>
        <td>获取单篇资讯文章内容</td>
        <td>/v3/articles/{id}</td>
    </tr>
    <tr>
        <td>获取社交媒体内容</td>
        <td>/v3/social_media</td>
    </tr>
</table>
## 请求限制
为了提供更高服务质量，根据不同的套餐作出不同的限制。详情查看 [Pricing](https://data.mifengcha.com/pricing)

版本 |  调用次数(次/月)| 每秒查询率（次/秒) | WebSocket连接数 | Webhook Supper[Beta]
---|:---:|:---:|:---:|:---:
体验版 | 10,000 | 1 | × | ×
基础版 | 300,000 | 2 |1 | ×
专业版 | 900,000 | 3 | 3 | ×
企业版 | 4,000,000 | 5 | 15 | √
旗舰版 | 8,000,000 | 8 |45 | √

## 请求格式
所有的API请求都是restful，目前只有一种方法：GET

## 返回格式
所有的接口都是JSON格式。

## 数据类型
本文档对JSON格式中数据类型的描述做如下约定：

- `string`: 字符串类型，用双引号（"）引用
- `int`: 32位整数，主要涉及到状态码、大小、次数等
- `long`: 64位整数，主要涉及到Id和时间戳
- `double`: 浮点数，主要涉及到金额和价格，建议程序中使用高精度浮点型
- `object`: 对象，包含一个子对象{}
- `array`: 数组，包含多个对象

## 最佳实践
##### 公共类
- 不建议在中国大陆境内使用临时域名以及代理的方式访问蜜蜂查API，此类方式访问API连接的稳定性很难保证。
- 建议使用香港云服务器进行访问。

## 错误码
> 错误码响应内容示例:

```json
{
  "code": 10030,
  "message": "Param Required"
}
```

HTTP 状态码|错误码 | 错误信息
--------- | -----------| -----------
400 | 10010| Duration Limited, 请求获取的数据区间太大 请求获取的数据区间太大
400 | 10030| Param Required, 请求参数缺失
400 | 10050| Empty Resource, 空资源
400 | 10070| Unavailable Resource, 该资源已停用
429 | 10040| Rate Limited, 请求并发过高或者请求次已达上限
414 | 10090| Url Too Long, url太长
500 | 10020| Internal Server Error, 服务器错误 请联系客服反馈
404 | 404| Not Found

## 分页
```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/markets?page=0&size=100'
```
默认情况下，返回多个条目的请求将被按照每页20条进行分页。您可以使用`?page`参数指定其他页面。还可以使用`?size`参数设置最大100的自定义页面大小。请注意，出于技术原因，并非所有API都遵守`?size`，请参阅对于API参数说明。
请注意，页码是从0开始，缺省page参数将返回第一页 0。
##### 响应头
该响应头包括分页信息：

```
Link →</api/v3/tickers?page=1&size=20>; rel="next",
</api/v3/tickers?page=1185&size=20>; rel="last",
</api/v3/tickers?page=0&size=20>; rel="first"

X-Total-Count →10000
```

具体分页信息显示在响应头的link标签，这样调用者可以直接得到诸如下一页、最后一页、上一页等内容的 url 地址，而不是自己手动去计算和拼接。

可能的rel值为:

名字 | 描述
--- | ---
next | 下一页的链接
last | 最后一页的链接
first | 第一页的链接
prev | 上一页的链接

# 联系我们
使用过程中如有问题或者建议，您可选择以下任一方式联系我们：

- 点击[这里](https://data.mifengcha.com/contactus)提交表单。
- 发邮件至[support@mifengcha.com](mailto:support@mifengcha.com)联系工作人员。


如您遇到API错误，请按照以下模板向我们反馈问题

1. 问题描述
2. 问题发生的用户账号（邮箱）
3. 完整的URL请求
4. 完整的返回结果
5. 问题出现时间和频率（如何时开始出现，是否可以重现）

# REST API
## 基础数据
### 简介
基础数据一般作为请求行情数据的参数。

### 获取所有支持的交易所列表
```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/markets'
```
> 将会返回以下内容:

```json
 [
    {
      "slug": "binance",
      "fullname": "Binance",
      "websiteUrl": "https://www.binance.com/",
      "volume": 2490122366.2343,
      "reportedVolume":2490122366.2343,
      "expectedVolume": 2490122366.2343,
      "monthlyVisits":19386372.159024935,
      "status": "enable",
      "kline": true,
      "spot":true,
      "futures":false
    },
    {
      "slug": "okex",
      "fullname": "OKEX",
      "websiteUrl": "https://www.okex.com",
      "volume": 2490122366.2343,
      "reportedVolume":2490122366.2343,
      "expectedVolume": 2490122366.2343,
      "monthlyVisits":19386372.159024935,
      "status": "enable",
      "kline": true,
      "spot":true,
      "futures":false
    }
  ]
```


##### 请求URL

`GET https://data.mifengcha.com/api/v3/markets`

##### 请求参数

参数|必选|说明
--------- |--------- | -----------
page |否| 当前页数，默认 0, (>=0)。注意：只有slug不存在时，该值才有效
size |否| 每页数据量，默认 20 (100>=size>=1)。 注意：只有slug不存在时，该值才有效



##### 返回参数说明
参数 | 说明
--------- | -----------
slug | 交易所名称(ID)
fullname | 交易所全称
websiteUrl | 交易所官网链接
volume | (旧)通过人工干预统计的交易量(USD)
reportedVolume | 交易所上报交易量(USD)
expectedVolume | 推测交易量(USD)
status | 状态: [enable, disable]. disable为停止更新数据
kline | 是否接入K线数据
spot | 是否支持现货
futures | 是否支持期货

### 获取指定交易所信息
```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/markets/binance'
```
> 将会返回以下内容:

```json
    {
      "slug": "binance",
      "fullname": "Binance",
      "websiteUrl": "https://www.binance.com/",
      "volume": 2490122366.2343,
      "reportedVolume":2490122366.2343,
      "expectedVolume": 2490122366.2343,
      "monthlyVisits":19386372.159024935,
      "status": "enable",
      "kline": true,
      "spot":true,
      "futures":false
    }
```


##### 请求URL

`GET https://data.mifengcha.com/api/v3/markets/{slug}`

##### 请求参数
<table>
	<tr>
        <td>参数</td>
        <td>说明</td>
    </tr>
    <tr>
        <td>slug</td>
        <td>交易所名称(ID)</td>
    </tr>
</table>


##### 返回参数说明
参数 | 说明
--------- | -----------
slug | 交易所名称(ID)
fullname | 交易所全称
websiteUrl | 交易所官网链接
volume | (旧)通过人工干预统计的交易量(USD)
reportedVolume | 交易所上报交易量(USD)
expectedVolume | 推测交易量(USD)
status | 状态: [enable, disable]. disable为停止更新数据
kline | 是否接入K线数据
spot | 是否支持现货
futures | 是否支持期货

<h3 id="获取所有支持的币种列表">获取所有支持的币种列表</h3>
```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/symbols'
```


> 将会返回以下内容:

```json
[
  {
    "slug": "ethereum",
    "symbol": "ETH",
    "fullname": "Ethereum",
    "logoUrl": "https://mifengcha.oss-cn-beijing.aliyuncs.com/static/coinInfo/ethereum.png",
    "volumeUsd": 48535287645.18,
    "status": "enable",
    "marketCapUsd": 330065744723.0739,
    "availableSupply": 116017188.1865,
    "totalSupply": 116017188.1865,
    "maxSupply": 116017188.1865,
    "websiteUrl": "https://www.ethereum.org/",
    "explorerUrls": "https://eth.tokenview.com/,https://ethplorer.io/,https://blockchair.com/ethereum,https://hecoinfo.com/token/0x64ff637fb478863b7468bc97d30a5bf3a428a1fd,https://etherscan.io/",
    "whitePaperUrls": null,
    "githubId": "ethereum",
    "twitterId": "ethereum",
    "facebookId": "ethereumproject",
    "telegramId": null,
    "redditId": "ethereum",
    "algorithm": null,
    "proof": null,
    "platforms": [
      {
        "contractAddress": "0x2170ed0880ac9a755fd29b2688956bd959f933f8",
        "platform": "BSC",
        "explorer": "https://bscscan.com/token/0x2170ed0880ac9a755fd29b2688956bd959f933f8"
      },
      {
        "contractAddress": "0x64ff637fb478863b7468bc97d30a5bf3a428a1fd",
        "platform": "HECO",
        "explorer": "https://hecoinfo.com/token/0x64ff637fb478863b7468bc97d30a5bf3a428a1fd"
      }
    ],
    "issueDate": "2015-07-29T16:00:00Z",
    "details": null
  }
]

```

##### 请求URL

`GET https://data.mifengcha.com/api/v3/symbols`


##### 请求参数

参数|传输方式|必选|说明
--------- |---------|--------- | -----------
details |QueryString|否| 是否获取币种详情介绍，取值为1(是)，0(否)，默认为0
page |QueryString|否| 当前页数，默认 0, (>=0)。注意：只有slug不存在时，该值才有效
size |QueryString|否| 每页数据量，默认 20 (100>=size>=1)。 注意：只有slug不存在时，该值才有效



##### 返回参数说明
参数 | 说明
--------- | -----------
slug | 币种名称（ID）
symbol | 币种符号
fullname | 币种全称
logoUrl| 图标链接
volumeUsd | 通过人工干预统计的交易量(USD)
status | 状态: [enable, disable]. disable为停止更新数据
marketCapUsd |币种市值
availableSupply |流通量
totalSupply |发行总量
maxSupply |最大发行量
websiteUrl |官网链接
explorerUrls |区块浏览器链接
whitePaperUrls |白皮书
githubId |Github
twitterId |Twitter
facebookId |FaceBook
telegramId |Telegram
algorithm |核心算法
proof |激励机制
platforms | 平台信息
issueDate |上市时间
details |币种介绍, 默认不返回

### 获取单个币种信息
```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/symbols/ethereum'
```

> 将会返回以下内容:

```json
{
  "slug": "ethereum",
  "symbol": "ETH",
  "fullname": "Ethereum",
  "logoUrl": "https://mifengcha.oss-cn-beijing.aliyuncs.com/static/coinInfo/ethereum.png",
  "volumeUsd": 48535287645.18,
  "status": "enable",
  "marketCapUsd": 330065744723.0739,
  "availableSupply": 116017188.1865,
  "totalSupply": 116017188.1865,
  "maxSupply": 116017188.1865,
  "websiteUrl": "https://www.ethereum.org/",
  "explorerUrls": "https://eth.tokenview.com/,https://ethplorer.io/,https://blockchair.com/ethereum,https://hecoinfo.com/token/0x64ff637fb478863b7468bc97d30a5bf3a428a1fd,https://etherscan.io/",
  "whitePaperUrls": null,
  "githubId": "ethereum",
  "twitterId": "ethereum",
  "facebookId": "ethereumproject",
  "telegramId": null,
  "redditId": "ethereum",
  "algorithm": null,
  "proof": null,
  "platforms": [
    {
      "contractAddress": "0x2170ed0880ac9a755fd29b2688956bd959f933f8",
      "platform": "BSC",
      "explorer": "https://bscscan.com/token/0x2170ed0880ac9a755fd29b2688956bd959f933f8"
    },
    {
      "contractAddress": "0x64ff637fb478863b7468bc97d30a5bf3a428a1fd",
      "platform": "HECO",
      "explorer": "https://hecoinfo.com/token/0x64ff637fb478863b7468bc97d30a5bf3a428a1fd"
    }
  ],
  "issueDate": "2015-07-29T16:00:00Z",
  "details": null
}
```

##### 请求URL

`GET https://data.mifengcha.com/api/v3/symbols/{slug}`

##### 请求参数

参数|传输方式|必选|说明
--------- |---------|--------- | -----------
slug | URL Path|否| 获取单个币种信息时使用
details |QueryString|否| 是否获取币种详情介绍，取值为1(是)，0(否)，默认为0



##### 返回参数说明
参数 | 说明
--------- | -----------
slug | 币种名称（ID）
symbol | 币种符号
fullname | 币种全称
logoUrl| 图标链接
volumeUsd | 通过人工干预统计的交易量(USD)
status | 状态: [enable, disable]. disable为停止更新数据
marketCapUsd |币种市值
availableSupply |流通量
totalSupply |发行总量
maxSupply |最大发行量
websiteUrl |官网链接
explorerUrls |区块浏览器链接
whitePaperUrls |白皮书
githubId |Github
twitterId |Twitter
facebookId |FaceBook
telegramId |Telegram
algorithm |核心算法
proof |激励机制
platforms | 平台信息
issueDate |上市时间
details |币种介绍, 默认不返回



## 行情数据
### 获取汇率
该接口的汇率都是以`USD`为基础兑换货币

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/exchange_rate'
```

> 将会返回以下内容:

```json
 [
   {
    "c": "USD",
    "r": 1
    },
    {
    "c": "AED",
    "r": 3.6729
    },
    {
    "c": "AFN",
    "r": 78.31899
    },
    {
    "c": "ALL",
    "r": 111.601841
    },
    {
    "c": "AMD",
    "r": 477.853793
    }
  ]

```
<aside class="notice">
更新时间：数字货币更新时间为60秒，法币更新时间为4小时。
</aside>

<aside class="notice">
数据来源：数字货币汇率从加权平均计算的币种价格获取， 法币汇率由外汇交易所以及各大银行牌价结合。 
</aside>

##### 请求URL

`GET https://data.mifengcha.com/api/v3/exchange_rate`

##### 请求参数

此接口不接受任何参数。

##### 返回参数说明

参数 | 说明
--------- | -----------
c | 目标兑换货币
r | 目标兑换汇率,如基础货币为USD，CNY下的数字为USDCNY的汇率。

<h3 id="获取币种价格">获取币种价格</h3>

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/price?slug=bitcoin,filecoin'
```

> 将会返回以下内容:

```json
   [
      {
        "s": "bitcoin",
        "S": "BTC",
        "T": 1564201016247,
        "u": 10254.613,
        "b": 1,
        "a": 66180.407,
        "v": 663551832.77,
        "ra": 68260.277,
        "rv": 684890110,
        "m": 182193710000,
        "c": 0.0111,
        "h": 10254,
        "l": 10254,
        "cw": 0.0111,
        "hw": 10254,
        "lw": 10254,
        "cm": 0.0111,
        "hm": 10254,
        "lm": 10254,
        "ha": 10254,
        "la": 10254
      }
  ]
```

<aside class="notice">
更新时间：5秒-60秒，按照交易量大小分级，交易量最大的币种5秒更新一次价格。
</aside>

##### 请求URL

`GET https://data.mifengcha.com/api/v3/price`

##### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
slug |QueryString|否| 币种名称,可传多个币种,逗号分割。
page |QueryString|否| 当前页数，默认 0, (>=0)。注意：只有slug和symbol两者皆不存在，该值才有效
size |QueryString|否| 每页数据量，默认 20 (100>=size>=1)。 注意：只有slug和symbol两者皆不存在，该值才有效

注意：若slug和symbol两者皆存在，优先级 slug > symbol， 按照交易量大小降序返回。

##### 返回参数说明

参数 | 说明
--------- | -----------
s | 币种名称
S | 币种符号
u | 价格(USD)
b | 价格(BTC)
v | 交易量(USD)
T | 时间戳(毫秒)
a | 交易量(单位为当前币种)
ra | 报告交易量(单位为当前币种)
rv | 报告交易量(USD)
m | 市值(USD)
c | 24小时涨跌幅
h | 24小时最高价
l | 24小时最低价
cw | 1周涨跌幅
hw | 1周最高价
lw | 1周最低价
cm | 1月涨跌幅
hm | 1月最高价
lm | 1月最低价
ha | 历史最高价
la | 历史最低价


<h3 id="获取币种历史价格">获取币种历史价格</h3>

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/price/history?slug=bitcoin'
```

> 将会返回以下内容:

```json

   [
      {
        "T" : 1577758200000,
        "u" : 7260.6657,
        "b" : 1.0,
        "a" : 1501274.4,
        "v" : 1.087066438855E10,
        "m" : 1.087066438855E10
      }, 
      {
        "T" : 1577691000000,
        "u" : 7397.4515,
        "b" : 1.0,
        "a" : 1495078.8,
        "v" : 1.09867723397E10,
        "m" : 1.132314561458E10
      }, 
      {  
        "T" : 1577724600000,
        "u" : 7253.9142,
        "b" : 1.0,
        "a" : 1559626.1,
        "v" : 1.132314561458E10,
        "m" : 1.132314561458E10
      }
  ]
```
<aside class="notice">
 数据来源：每5分钟快照一次当前价格，交易量。
</aside>


##### 请求URL

`GET https://data.mifengcha.com/api/v3/price/history?slug=bitcoin`

##### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
slug |QueryString|是| 币种名称。
start |QueryString|否| 起始时间，单位：毫秒, 默认该币种最早记录的时间
end |QueryString|否| 截止时间，单位：毫秒，默认当前时间
interval |QueryString|否| 数据点间隔[5m,15m,30m,1h,2h,6h,12h,1d,2d], 默认情况下根据 `start`,`end` 计算

- 每次请求的数据量限制大约为1000条
- `interval`不为空的情况下, 需要满足 `(end - start) / interval <= 1000`, 如果请求超过1000个数据点则会响应400 `10010 Duration Limited`.
- `interval`为空的情况下, 该接口会对返回数据进行下采样(downsampled), 选择最优的`interval`, 确保数据点的覆盖, 原则上`interval` ≈ `end` - `start` / 1000。

##### 返回参数说明

参数 | 说明
--------- | -----------
T | 时间戳(毫秒)
u | 价格(单位: USD)
b | 价格(单位: BTC)
v | 交易量(单位: USD)
a | 交易量(单位: 当前币种)
m | 市值(unit: USD)

### 批量获取交易对Tickers

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/tickers?market=bitfinex'
```

> 将会返回以下内容:

```json

    [
      {
        "T": 1566546506486,
        "m": "gate-io_BTC_USDT",
        "o": 9991.19,
        "c": 10206,
        "l": 9860,
        "h": 10250,
        "a": 10206,
        "A": 0,
        "b": 10205.5,
        "B": 0,
        "C": 0.0215,
        "bv": 252242,
        "qv": 2574378787,
        "r": 1,
        "p": 0,
        "s": 0
      }
    ]


```

<aside class="notice">
更新时间：1秒-30秒，影响更新频率的因数包括: 交易所是否支持批量接口,是否支持Websocket以及网络环境。
</aside>

<aside class="notice">
数据来源：通过交易所API获取
</aside>

<aside class="warning">
注意: 数据中心会对异常数据进行拦截。请接入时检查时间戳。
</aside>

##### 请求URL

`GET https://data.mifengcha.com/api/v3/tickers`

##### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
market |QueryString|否| 交易所名称，可传多个，逗号分割
symbol |QueryString|否| 币种符号，可传多个，逗号分割
slug |QueryString|否| 币种名称，可传多个，逗号分割
currency |QueryString|否| 基础货币，可传多个，逗号分割
market_pair |QueryString|否| 交易所的交易对名称，可传多个，逗号分割
page |QueryString|否| 当前页数，默认 0, (>=0)
size |QueryString|否| 每页数据量，默认 20 (>=1)

* 数据按照美元交易量倒序排序
* market,symbol,slug,currency为交易对筛选条件。
* market,symbol,slug,currency存在两种以上的参数时，只返回条件都符合的交易对。
* 当market_pair存在时忽略market,symbol,slug,currency。

例如：

* 获取bitfinex的BTC交易对Ticker `market=bitfinex&symbol=BTC`
* 获取bitfinex与binance的BTC和ETH交易对Ticker `market=bitfinex,binance&symbol=BTC,ETH`
* 获取gate-io_BTC_USDT与binance_ETH_BTC的Ticker `market_pair=gate-io_BTC_USDT,binance_ETH_BTC`

##### 返回参数说明

参数 | 说明
--------- | -----------
T | 数据更新时间
c | 最新价格(单位：基础货币)
b | 买一价(单位：基础货币)
a | 卖一价(单位：基础货币)
o | 开盘价(单位：基础货币)
h | 24小时最高价(单位：基础货币)
l | 24小时最低价(单位：基础货币)
bv | 24小时交易货币交易量
qv | 24小时基础货币交易量
C | 24小时涨幅
m | 交易所的交易对名称
p | 纯度
r | 基础转货币转美元的汇率
s | 点差

* 数据更新时间一般为交易所接口返回的时间戳，如果交易所接口未返回时间戳则为发出请求前的时间戳
* 如涨幅为1%，则返回值为0.01
* r为基础货币转换到美元的汇率.
* 获取最新美元价格为： 最新价(c) * 基础转货币转美元的汇率(r)
* 获取24小时美元交易量为： 24小时基础货币交易量(qv) * 基础转货币转美元的汇率(r)


<h3 id="获取交易对深度">获取交易对深度</h3>

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/orderbook?desc=gate-io_BTC_USD'
```

> 将会返回以下内容:

```json
 {
    "T": 1526706175469,
    "m": "gate-io_BTC_USDT",
    "b": [
      [
        8221.6,
        4.99792
      ],
      [
        8221.2,
        1.212
      ],
      [
        8220.6,
        0.04144752
      ]
    ],
    "a": [
      [
        8221.7,
        1.10182386
      ],
      [
        8222.1,
        2.78535112
      ],
      [
        8223.2,
        4.21445703
      ]
    ]
  }

```



<aside class="notice">
更新时间：1秒-40秒，影响更新频率的因数包括: 交易所是否支持批量接口,是否支持Websocket以及网络环境。
</aside>

<aside class="notice">
数据来源：通过交易所API获取
</aside>

<aside class="warning">
注意: 数据中心会对异常数据进行拦截。请接入时检查时间戳。
</aside>

##### 请求URL

`GET https://data.mifengcha.com/api/v3/orderbook`

##### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
desc |QueryString|是| 交易所的某个交易对。例如：gate-io_BTC_USDT
limit |QueryString|否| 深度档位，默认25。


##### 返回参数说明

参数 | 说明
--------- | -----------
T|更新时间戳
m|交易所的交易对名称
b|买单列表
a|卖单列表

b/a | 说明
--------- | -----------
0|价格
1|挂单量

* 数据更新时间一般为交易所接口返回的时间戳，如果交易所接口未返回时间戳则为发出请求前的时间戳

### 获取交易对成交记录

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/trades?desc=gate-io_BTC_USDT'
```

> 将会返回以下内容:

```json
 [
  {
    "T": 1573721951113,
    "p": 8634,
    "v": 5,
    "s": "buy",
    "m": "gate-io_BTC_USDT"
  },
  {
    "T": 1573721944964,
    "p": 8634,
    "v": 0.001519,
    "s": "sell",
    "m": "gate-io_BTC_USDT"
  }
]

```

<aside class="notice">
更新时间：5秒-60秒，影响更新频率的因数包括: 交易所是否支持批量接口,是否支持Websocket以及网络环境。
</aside>

<aside class="notice">
数据来源：通过交易所API获取
</aside>

##### 请求URL

`GET https://data.mifengcha.com/api/v3/trades`

##### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
desc |QueryString|是| 交易所的某个交易对。例如：gate-io_BTC_USDT
limit |QueryString|否| 返回数据量，0<limit<=50

##### 返回参数说明

参数 | 说明
--------- | -----------
T|交易成交时间戳
p|成交价格
v|成交量
s|成交类型[buy,sell,none]，为taker操作方向
m|交易对信息

<h3 id="获取交易对K线数据">获取交易对K线数据</h3>

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/kline?desc=gate-io_BTC_USDT&type=15m&start=1573637497000'
```

> 将会返回以下内容:

```json
 [
  {
    "T": 1573680600000,
    "o": 8789,
    "h": 8795.3,
    "l": 8778.4,
    "c": 8791.2,
    "v": 14.16106481
  },
  {
    "T": 1573676100000,
    "o": 8806.3,
    "h": 8818.6,
    "l": 8802.8,
    "c": 8817.7,
    "v": 11.33948342
  }
]

```


<aside class="notice">
数据来源：通过交易所API获取
</aside>

##### 请求URL

`GET https://data.mifengcha.com/api/v3/kline`

##### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
desc |QueryString|是| 交易所的某个交易对。例如：gate-io_BTC_USDT
interval |QueryString|否| K线类型 (数据点间隔)[1m,5m,15m,30m,1h,6h,1d],默认5m
end |QueryString|否| 截止时间，单位：毫秒，默认当前时间
start |QueryString|否| 起始时间，单位：毫秒，默认`end - (1000 * interval)`, 即表示 `end` 之前1000条数据

- 单次请求最大可获取2000条数据, 传入参数需满足 `(end - start) / interval <= 2000`, 如果请求超过2000个数据点则会响应400 `10010 Duration Limited`

##### 返回参数说明

参数 | 说明
--------- | -----------
T|时间戳
o|开盘价
c|收盘价
l|最低价
h|最高价
v|交易量


## 资讯数据
### 获取快讯数据
```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/briefs?locale=zh_CN'
```

> 将会返回以下内容:

```json
 [
  {
    "images": [],
    "datetime": 1576477950000,
    "importance": false,
    "source": "金色财经",
    "title": "亿利集团董事长王文彪：要将区块链治沙模式推广到全世界",
    "content": "12月16日，IFIC全球金融科技创新峰会·东京站正式开幕。亿利资源集团董事长，库布齐沙漠治理的带头人王文彪在IFIC TOKYO开场致辞中表示，亿利集团在30余年的治理了库布其沙漠，我们的治理模式已经可以在全中国推广。区块链技术的集成应用在新的技术革新和产业变革中起着重要作用，我们想用区块链把沙漠治理模式带到全世界，向世界推广，为全人类做些贡献，做区块链公益。在此，希望日本的朋友到沙漠考察访问，将公益基金和区块链相连接，所投入的资金用来促进环境的发展，让全世界人知道怎样运用区块链使沙漠变成良田，达到可持续发展。",
    "url": "https://m.mifengcha.com/news/5df724fe07a014563b8269c0"
  },
  {
    "images": [],
    "datetime": 1576477582000,
    "importance": false,
    "source": "小葱",
    "title": "工信部将从推动云计算与区块链等技术融合创新等入手推动云计算产业快速发展",
    "content": "工信部信软司副司长董大健表示，下一步，工信部将从五方面入手推动云计算产业快速发展。一是持续优化发展环境，规范云计算市场，培育龙头骨干企业；二是加快突破核心技术，加快云计算在自主基础软硬件平台上的适配迁移，推动云计算以5G、工业互联网、大数据、人工智能、区块链等技术融合创新；三是深入推动企业上云应用云；四是完善云计算的标准体系；五是打造安全保障体系。（c114）",
    "url": "https://m.mifengcha.com/news/5df7238e07a014563b8269b4"
  }
]

```


##### 请求URL

`GET https://data.mifengcha.com/api/v3/briefs?locale=zh_CN`

##### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
locale |QueryString|是| 语言，支持zh_CN(中文),en_US(英文),ko_KR(韩文)
page |QueryString|否| 当前页数，默认 0, (>=0)
size |QueryString|否| 每页数据量，默认 20 (>=1)

##### 返回参数说明

参数 | 说明
--------- | -----------
title|标题
content|内容
timestamp|时间戳
importance|是否是要闻
url|mifengcha.com原文链接
source|来源
images|图片链接


### 获取交易所公告数据

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/announcements?locale=zh_CN'
```

> 将会返回以下内容:

```json
 [
  {
    "title" : "关于杠杆交易上线LTC/USDT、EOS/USDT、TRX/USDT的公告（1220）",
    "content" : "尊敬的用户： \n\n\n BiKi平台将于2019年12月20日18:00开放LTC/USDT、EOS/USDT、TRX/USDT杠杆交易。 \n\n  \n\n提示：\n 1、合理操作杠杆交易，可以提高收益。但同时考虑到加密货币市场目前的波动性，它也会为交易者带来更大的风险。\n 2、杠杆交易受市场风险、波动性和复杂性的影响较大，请您务必充分了解杠杆交易的风险后审慎使用，BiKi平台不对因杠杆交易造成的损失负责。\n  \n 感谢您对BiKi平台的支持，BiKi团队期待您的宝贵意见。\n BiKi团队\n 2019-12-20",
    "timestamp" : 1576811020489,
    "importance" : false,
    "url" : "https://m.mifengcha.com/news/5dfc3a0cc3af0a649f3f4563",
    "market" : "bikicoin",
    "sourceUrl" : "https://www.biki.com/noticeInfo/2007",
    "images": []
  },
  {
    "title" : "Binance战略投资FTX并上市FTX Token（FTT）",
    "content" : "亲爱的用户： \n\nBinance已完成对FTX的战略投资（投资详情），现已上线FTX Token（FTT），开通FTT/BNB、FTT/BTC、FTT/USDT 交易市场，邀您体验！FTT充值通道现已开放，立即充值。 \n\nFTT的上币费用为0 BNB。 \n\n声明：Binance已从对FTX的投资中获得了一部分FTT代币。 其中绝大多数FTT锁定期为两年。 我们致力于帮助FTT和FTX生态系统实现长期，可持续的发展。 \n\n规则说明： \n\n- 关于FTX Token (FTT)\n- 费率说明\n- 交易规则 \n\n风险提示：数字货币是一种高风险的投资方式，请投资者谨慎购买，并注意投资风险。Binance会遴选优质币种，但不对投资行为承担担保、赔偿等责任。 \n\n  \n\n感谢您对Binance的支持！ \n\n  \n\nBinance团队 \n\n2019年12月20日 \n\n  \n\nBinance社群 \n\n微博： https://weibo.com/u/7336825507  \n\nTelegram： https://t.me/BinanceChinese \n\nFacebook： https://www.facebook.com/BinanceChinese \n\nTwitter： https://twitter.com/binance",
    "timestamp" : 1576810511000,
    "importance" : false,
    "url" : "https://m.mifengcha.com/news/5dfc3a03c3af0a649f3f4416",
    "market" : "binance",
    "sourceUrl" : "https://binance.zendesk.com/hc/zh-cn/articles/360038147271-Binance%E6%88%98%E7%95%A5%E6%8A%95%E8%B5%84FTX%E5%B9%B6%E4%B8%8A%E5%B8%82FTX-Token-FTT-",
    "images" : [ ]
  }
]

```

<aside class="notice">
数据来源：通过交易所获取
</aside>
##### 请求URL

`GET https://data.mifengcha.com/api/v3/announcements?locale=zh_CN`

##### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
locale |QueryString|是| 语言，支持zh_CN(中文),en_US(英文),ko_KR(韩文)
page |QueryString|否| 当前页数，默认 0, (>=0)
size |QueryString|否| 每页数据量，默认 20 (>=1)
market |QueryString|否| 获取指定的交易所公告


##### 返回参数说明

参数 | 说明
--------- | -----------
title|标题
content|内容
timestamp|时间戳
importance|是否是要闻
sourceUrl|原文链接
market|来源
url|mifengcha.com原文链接
images|图片链接


### 获取资讯文章列表

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/articles?locale=zh_CN'
```

> 将会返回以下内容:

```json
[
  {
    "id": "5e1d96a5aeae8770b7ff34ac",
    "timestamp": 1578997412000,
    "title": "主流币种普涨势头将迎考验，平台币示弱DASH迫近中线强阻",
    "description": "白盘主流币种继续扩大涨势，DASH冲高至近三个半月新高，不过这个短期“风向标”币种已经走到了一个至关重要的多空分界线附近。",
    "author": "小葱区块链",
    "categories": "资讯",
    "images": [],
    "url": "https://m.mifengcha.com/news/5e1d96a5aeae8770b7ff34ac",
    "source": "火星财经",
    "sourceUrl": "https://news.huoxing24.com/20200114182213991739.html",
    "keywords": "dash,币种,中线,势头,平台,阻力,ht,行情,btc,指标,区域,圆弧底,bsv,bnb,过程,延续性,均线,关键,观点,涨幅,涨势,整数,结构,效应,形态,小时,双顶,阶段,市场,二度,全数,仔细观察,撰文,风向标,风向,横盘,标记,时段,时间,分水岭,士气,情况,力图,点位,所处,盲目"
    "content": "<html value>"
  }
  "..."
]

```


##### 请求URL

`GET https://data.mifengcha.com/api/v3/articles?locale=zh_CN`

##### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
locale |QueryString|是| 语言，支持zh_CN(中文),en_US(英文),ko_KR(韩文)
page |QueryString|否| 当前页数，默认 0, (>=0)
size |QueryString|否| 每页数据量，默认 20 (>=1, <=20)


##### 返回参数说明

参数 | 说明
--------- | -----------
id| 文章id
timestamp|发布时间
title|标题
description|文章描述
author|文章作者
categories|分类, 逗号分隔
images|图片链接
url|mifengcha.com原文链接
source|来源


### 获取单篇资讯文章内容

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/articles/5e1d96a5aeae8770b7ff34ac'
```

> 将会返回以下内容:

```json
  {
  "id": "5e1d96a5aeae8770b7ff34ac",
  "timestamp": 1578997412000,
  "title": "主流币种普涨势头将迎考验，平台币示弱DASH迫近中线强阻",
  "description": "白盘主流币种继续扩大涨势，DASH冲高至近三个半月新高，不过这个短期“风向标”币种已经走到了一个至关重要的多空分界线附近。",
  "author": "小葱区块链",
  "categories": "资讯",
  "images": [],
  "url": "https://m.mifengcha.com/news/5e1d96a5aeae8770b7ff34ac",
  "source": "火星财经",
  "sourceUrl": "https://news.huoxing24.com/20200114182213991739.html",
  "keywords": "dash,币种,中线,势头,平台,阻力,ht,行情,btc,指标,区域,圆弧底,bsv,bnb,过程,延续性,均线,关键,观点,涨幅,涨势,整数,结构,效应,形态,小时,双顶,阶段,市场,二度,全数,仔细观察,撰文,风向标,风向,横盘,标记,时段,时间,分水岭,士气,情况,力图,点位,所处,盲目"
  "content": "<html value>",
  "btcPrice": 8465.6615
}
```


##### 请求URL

`GET https://data.mifengcha.com/api/v3/articles/{id}`

##### 请求参数

参数 | 说明
--------- | -----------
id| 文章id


##### 返回参数说明

与文章格式与列表相同，多出`btcPrice`字段

参数 | 说明
--------- | -----------
btcPrice|发文时比特币价格


<h3 id="获取社交媒体内容">获取社交媒体内容</h3>

```shell
curl -X GET \
 'https://data.mifengcha.com/api/v3/social_media?source=TWITTER'
```

> 将会返回以下内容:

```json
[
  {
    "id": "1264806231472046080",
    "source": "TWITTER",
    "content": "The ability to recognize humor is a defining characteristic of intelligence.\n\nI use it constantly to cull my followers by placing the burden of culling upon themselves.",
    "images": [],
    "userId": "961445378",
    "avatar": "https://mifengcha.oss-cn-beijing.aliyuncs.com/static/twitter/screen_name/officialmcafee.jpg",
    "screenName": "officialmcafee",
    "timestamp": 1590388279000,
    "retweeted": false,
    "retweet": null
  },
  {
    "id": "1264805947861536771",
    "source": "TWITTER",
    "content": "⚡ @cartesiproject Listing + Contest ⚡\n\n$CTSI/USDT trading starts today at 5 PM IST on #WazirX.\n\nWe're giving away 378 #CTSI worth ₹1,000 each to 5 lucky people who:\n\n1. Retweet this tweet\n2. Reply to this tweet &amp; mention 3 friends in the reply\n\nValid for 24 hrs! https://t.co/MIhccc43AI",
    "images": [
      "https://mifengcha.oss-cn-beijing.aliyuncs.com/static/twitter/media/bdc36e0a6a2ad8f6be9bcabf3dead476.jpg"
    ],
    "userId": "955744720092835841",
    "avatar": "https://mifengcha.oss-cn-beijing.aliyuncs.com/static/twitter/screen_name/WazirXIndia.jpg",
    "screenName": "WazirXIndia",
    "timestamp": 1590388211000,
    "retweeted": false,
    "retweet": null
  }
]
```


<aside class="notice">
数据来源：Twitter，微博
</aside>

##### 请求URL

`GET https://data.mifengcha.com/api/v3/social_media`


##### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
source |QueryString|否| 来源: [WEIBO, TWITTER], 默认 WEIBO
page |QueryString|否| 当前页数，默认 0, (>=0)。
size |QueryString|否| 每页数据量，默认 20 (100>=size>=1)。



##### 返回参数说明

参数 | 说明
-------- | :----------
id | 推文ID
source | 社交媒体平台
content |推文内容
images | 推文图片
userId |用户ID
avatar |用户头像
screenName | 用户名
timestamp | 发布时间
retweeted | 是否是转推
retweet | 被转发推文
retweet.content | 被转发推文内容
retweet.images | 被转发推文图片
retweet.userId | 被转发用户ID
retweet.avatar | 被转发用户头像
retweet.screenName | 被转发用户名

# WebSocket API

##### 请求URL

<aside class="success">
URL: wss://data.mifengcha.com/ws/v3
</aside>

* 每个链接有效期不超过24小时，请妥善处理断线重连。
* 每30秒服务端会发送ping帧，客户端应当在30秒内回复pong帧，否则服务端会主动断开链接。
* 连接时需要将用户对应的API_KEY作为参数传入，`wss://data.mifengcha.com/ws/v3?api_key=[YOUR_API_KEY]`。
* 每个链接最多可订阅200个Topic。
* 每个账户的最大连接数根据账户套餐设定, 详情查看 [Pricing](https://data.mifengcha.com/pricing)。
* 服务端不会校验Topic的正确性，若订阅无效Topic不会有响应，并且占用Topic订阅额度。
* 90秒内不会推送没有变化的数据

## 订阅
```shell
wscat -c 'wss://data.mifengcha.com/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "subscribe", "args": ["price:bitcoin"]}
```

> 订阅成功将会返回以下内容:

```json
{"code":0,"message":"Subscribed"}
```

##### 消息格式

`{"op": "subscribe", "args": ["<topic1>","<topic2>", ...]}`

##### 返回参数说明

参数 | 说明
--------- | -----------
code| Message Code
message| Message




## 取消订阅
```shell
wscat -c 'wss://data.mifengcha.com/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "unsubscribe", "args": ["price:bitcoin"]}
```

> 取消订阅成功将会返回以下内容:

```json
{"code":0,"message":"Unsubscribed"}
```

##### 消息格式

`{"op": "unsubscribe", "args": ["<topic1>","<topic2>", ...]}`

##### 返回参数说明

参数 | 说明
--------- | -----------
code| Message Code
message| Message

## 列出已订阅列表

```shell
wscat -c 'wss://data.mifengcha.com/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "subscribe", "args": ["price:bitcoin"]}
> {"op": "topics"}
```

> 将会返回以下内容:

```json
{"code":0,"message":"Success","topics":["price:bitcoin"]}
```

##### 消息格式

`{"op": "topics"}`

##### 返回参数说明

参数 | 说明
--------- | -----------
code| Message Code
message| Message
topics| Subscribed Topic List

## Topic: Price

```shell
wscat -c 'wss://data.mifengcha.com/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "subscribe", "args": ["price:bitcoin"]}
```
> 更新时推送以下内容:

```json
{
  "code": 0,
  "message": "Success",
  "topic": "price:bitcoin",
  "data": {
    "s": "bitcoin",
    "S": "BTC",
    "T": 1564201016247,
    "u": 10254.613,
    "b": 1,
    "a": 66180.407,
    "v": 663551832.77,
    "ra": 68260.277,
    "rv": 684890110,
    "m": 182193710000,
    "c": 0.0111
  }
}
```

##### Topic格式

`price:` + [币种](#获取所有支持的币种列表) slug

##### 返回参数说明

参数 | 说明
--------- | -----------
code| Message Code
message| Message
data| [Price](#获取币种价格)


## Topic: Ticker

```shell
wscat -c 'wss://data.mifengcha.com/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "subscribe", "args": ["ticker:gate-io_BTC_USDT"]}
```
> 更新时推送以下内容:

```json
{
  "code": 0,
  "message": "Success",
  "topic": "gate-io_BTC_USDT",
  "data": {
    "T": 1566546506486,
    "m": "gate-io_BTC_USDT",
    "o": 9991.19,
    "c": 10206,
    "l": 9860,
    "h": 10250,
    "a": 10206,
    "A": 0,
    "b": 10205.5,
    "B": 0,
    "C": 0.0215,
    "bv": 252242,
    "qv": 2574378787,
    "r": 1,
    "p": 0,
    "s": 0
  }
}
```

##### Topic格式

`ticker:` + 交易对(`binance_BNB_USDT`, `huobipro_HT_USDT`)

##### 返回参数说明

参数 | 说明
--------- | -----------
code| Message Code
message| Message
data| [Ticker](#tickers)



## Topic: Orderbook

```shell
wscat -c 'wss://data.mifengcha.com/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "subscribe", "args": ["orderbook:gate-io_BTC_USDT"]}
```
> 更新时推送以下内容:

```json
{
  "code": 0,
  "message": "Success",
  "topic": "gate-io_BTC_USDT",
  "data": {
    "T": 1526706175469,
    "m": "gate-io_BTC_USDT",
    "b": [
      [
        8221.6,
        4.99792
      ],
      [
        8221.2,
        1.212
      ]
    ],
    "a": [
      [
        8221.7,
        1.10182386
      ],
      [
        8222.1,
        2.78535112
      ]
    ]
  }
}
```

##### Topic格式

`orderbook:` + 交易对(`binance_BNB_USDT`, `huobipro_HT_USDT`)

##### 返回参数说明

参数 | 说明
--------- | -----------
code| Message Code
message| Message
data| [Orderbook](#获取交易对深度)
 



