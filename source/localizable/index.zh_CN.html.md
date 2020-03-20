---
title: 蜜蜂查开发者文档 v3

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a target="_blank" href='https://data.mifengcha.com/login'>登陆data.mifengcha.com</a>
  - <a href='../v1/'>查看v1文档</a>
  - <br>
  - <a class="locale-button" href='../zh_CN'><img src="/images/flags/zh_CN.svg" alt="简体中文"/></a> <a class="locale-button" href='../en_US'><img src="/images/flags/en_US.svg" alt="English"/></a>

search: false
---

# 概述

## 简介

蜜蜂查(中文站:mifengcha.com / 国际站:block.cc)是专业的区块链产业信息服务平台，提供数字货币行情、数据、资讯等一站式区块链产业服务，追求更及时、更全面、更专业、更准确的行情数据，致力为区块链爱好者和数字货币投资者提供最权威最流畅的产品和服务。

第三方应用开发者可以借助该服务，快速构建稳定高效的数字货币行情系统，为实时业务需求和产品运营提供技术支持。

## Access URLs

您可以自行比较使用data.block.cc和data.mifengcha.com两个域名的延迟情况，选择延迟低的进行使用。

其中，data.mifengcha.com域名对中国大陆的用户做了一定的链路延迟优化。

#### REST API

https://data.block.cc/api/v3

https://data.mifengcha.com/api/v3

#### Websocket Feed

wss://data.block.cc/ws/v3

wss://data.mifengcha.com/ws/v3

## 当前版本

<aside class="success">
Version: v3.0.0
</aside>

## 认证

### 获取API密钥

针对蜜蜂查 API发出的所有请求都必须使用API密钥进行验证。 如果您还没有API密钥，请[点击注册](https://data.mifengcha.com/register)。

### 使用您的密钥

> Api Key请求示例:

```shell
curl -X GET \
   -H 'X-API-KEY: [YOUR_API_KEY]' \
  'https://data.mifengcha.com/api/v3/markets'
  
curl -X GET \
  'https://data.mifengcha.com/api/v3/markets?api_key=[YOUR_API_KEY]'
  
wscat -c 'wss://data.mifengcha.com/ws/v3?api_key=[YOUR_API_KEY]'

```


您可以通过以下方式在REST API调用中提供API密钥：

1.首选方法：通过名为`X-API_KEY`的自定义请求头

2.便捷方法：通过名为`api_key` 的查询字符串参数

在Websocket API中只能通过名为`api_key` 的查询字符串参数提供API密钥


## 请求限制

```json
{
  "code": 429,
  "message": "Rate Limited"
}
```

为了提供更高服务质量，根据不同的套餐作出不同的限制。详情查看 [Pricing](https://data.mifengcha.com/pricing)

> 您随时可以通过响应头查看你的剩余次数:

```
X-RateLimit-Limit：10000 
X-RateLimit-Remaining：9999 
```

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
400 | 10010| Duration Limited, 请求获取的数据区间太大
400 | 10030| Param Required
400 | 10050| Empty Resource
400 | 10070| Unavailable Resource
429 | 10040| Rate Limited
414 | 10090| Url Too Long
500 | 10020| Internal Server Error
404 | 404| Not Found

## 分页

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/markets?page=0&size=100'
```



默认情况下，返回多个条目的请求将被按照每页20条进行分页。您可以使用`?page`参数指定其他页面。还可以使用`?size`参数设置最大100的自定义页面大小。请注意，出于技术原因，并非所有API都遵守`?size`，请参阅对于API参数说明。

请注意，页码是从0开始，缺省page参数将返回第一页 0。


### 响应头

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

# REST API 

<aside class="success">
URL: https://data.mifengcha.com/api/v3
</aside>


## 元数据

元数据为基础数据，一般作为请求行情数据的参数.

### Markets

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

获取所有支持的交易所列表

#### 请求URL

`GET https://data.mifengcha.com/api/v3/markets`

#### 请求参数

参数|传输方式|必选|说明
--------- |---------|--------- | -----------
slug | URL Path|否| 获取指定交易所信息，例如:/api/v3/markets/gate-io
page |QueryString|否| 当前页数，默认 0, (>=0)。注意：只有slug不存在时，该值才有效
size |QueryString|否| 每页数据量，默认 20 (100>=size>=1)。 注意：只有slug不存在时，该值才有效


#### 返回参数说明
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

### Symbols

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/symbols'
```

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/symbols/bitcoin'
```

> 将会返回以下内容:

```json
 [
    {
      "slug": "bitcoin",
      "symbol": "BTC",
      "fullname" : "Bitcoin",
      "logoUrl" : "https://mifengcha.oss-cn-beijing.aliyuncs.com/static/coinInfo/bitcoin.png",
      "volumeUsd": 4463819005.1846,
      "status": "enable",
      "marketCapUsd":157081834083.0375,
      "availableSupply":18039125,
      "totalSupply":18039125,
      "maxSupply":21000000,
      "website":"https://bitcoin.org/en/",
      "explorerUrls": "https://live.blockcypher.com/btc/,http://blockchain.info,https://blockchair.com/bitcoin/,https://explorer.viabtc.com/btc,https://blockexplorer.com/,https://btc.com/",
      "whitePaperUrls":"https://bitcoin.org/bitcoin.pdf",
      "githubId": "bitcoin",
      "twitterId": "btc",
      "facebookId": "bitcoins",
      "telegramId": "www_bitcoin_com",
      "redditId": "bitcoin",
      "algorithm": "SHA256",
      "proof": "POW",
      "issueDate": "2008-10-31T16:00:00Z",
      "details" : [ {
            "locale" : "zh_CN",
            "fullName" : "比特币",
            "description" : "比特币（BitCoin）的概念最初由中本聪在2008年提出，根据中本聪的思路设计发布的开源软件以及建构其上的P2P网络。比特币是一种P2P形式的数字货币。点对点的传输意味着一个去中心化的支付系统。与大多数货币不同，比特币不依靠特定货币机构发行，它依据特定算法，通过大量的计算产生，比特币经济使用整个p2p网络中众多节点构成的分布式数据库来确认并记录所有的交易行为，并使用密码学的设计来确保货币流通各个环节安全性。p2p的去中心化特性与算法本身可以确保无法通过大量制造比特币来人为操控币值。基于密码学的设计可以使比特币只能被真实的拥有者转移或支付。这同样确保了货币所有权与流通交易的匿名性。比特币与其他虚拟货币最大的不同，是其总数量非常有限，具有极强的稀缺性。该货币系统曾在4年内只有不超过1050万个，之后的总数量将被永久限制在2100万个。 比特，是一种计算机专业术语，是信息量单位，是由英文BIT音译而来。二进制数的一位所包含的信息就是一比特，如二进制数0100就是4比特。那么，比特这个概念和货币联系到一起，不难看出，比特币非现实货币，而是一种计算机电子虚拟货币，存储在你的电脑上。目前，这种崭新的虚拟货币不受任何政府、任何银行控制。因此，它还未被合法化。"
          }
      ]
    }
  ]

```

```json
{
  "slug": "bitcoin",
  "symbol": "BTC",
  "fullname" : "Bitcoin",
  "logoUrl" : "https://mifengcha.oss-cn-beijing.aliyuncs.com/static/coinInfo/bitcoin.png",
  "volumeUsd": 4463819005.1846,
  "status": "enable",
  "marketCapUsd":157081834083.0375,
  "availableSupply":18039125,
  "totalSupply":18039125,
  "maxSupply":21000000,
  "website":"https://bitcoin.org/en/",
  "explorerUrls": "https://live.blockcypher.com/btc/,http://blockchain.info,https://blockchair.com/bitcoin/,https://explorer.viabtc.com/btc,https://blockexplorer.com/,https://btc.com/",
  "whitePaperUrls":"https://bitcoin.org/bitcoin.pdf",
  "githubId": "bitcoin",
  "twitterId": "btc",
  "facebookId": "bitcoins",
  "telegramId": "www_bitcoin_com",
  "redditId": "bitcoin",
  "algorithm": "SHA256",
  "proof": "POW",
  "issueDate": "2008-10-31T16:00:00Z",
  "details" : [ {
        "locale" : "zh_CN",
        "fullName" : "比特币",
        "description" : "比特币（BitCoin）的概念最初由中本聪在2008年提出，根据中本聪的思路设计发布的开源软件以及建构其上的P2P网络。比特币是一种P2P形式的数字货币。点对点的传输意味着一个去中心化的支付系统。与大多数货币不同，比特币不依靠特定货币机构发行，它依据特定算法，通过大量的计算产生，比特币经济使用整个p2p网络中众多节点构成的分布式数据库来确认并记录所有的交易行为，并使用密码学的设计来确保货币流通各个环节安全性。p2p的去中心化特性与算法本身可以确保无法通过大量制造比特币来人为操控币值。基于密码学的设计可以使比特币只能被真实的拥有者转移或支付。这同样确保了货币所有权与流通交易的匿名性。比特币与其他虚拟货币最大的不同，是其总数量非常有限，具有极强的稀缺性。该货币系统曾在4年内只有不超过1050万个，之后的总数量将被永久限制在2100万个。 比特，是一种计算机专业术语，是信息量单位，是由英文BIT音译而来。二进制数的一位所包含的信息就是一比特，如二进制数0100就是4比特。那么，比特这个概念和货币联系到一起，不难看出，比特币非现实货币，而是一种计算机电子虚拟货币，存储在你的电脑上。目前，这种崭新的虚拟货币不受任何政府、任何银行控制。因此，它还未被合法化。"
      }
  ]
}
```

获取所有支持的币种列表

#### 请求URL

`GET https://data.mifengcha.com/api/v3/symbols`

`GET https://data.mifengcha.com/api/v3/symbols/{slug}`

#### 请求参数

参数|传输方式|必选|说明
--------- |---------|--------- | -----------
slug | URL Path|否| 获取单个币种信息时使用
details |QueryString|否| 是否获取币种详情介绍，取值为1(是)，0(否)，默认为0
page |QueryString|否| 当前页数，默认 0, (>=0)。注意：只有slug不存在时，该值才有效
size |QueryString|否| 每页数据量，默认 20 (100>=size>=1)。 注意：只有slug不存在时，该值才有效



#### 返回参数说明
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
website |官网链接
explorerUrls |区块浏览器链接
whitePaperUrls |白皮书
githubId |Github
twitterId |Twitter
facebookId |FaceBook
telegramId |Telegram
algorithm |核心算法
proof |激励机制
issueDate |上市时间
details |币种介绍, 默认不返回

## 行情数据

### ExchangeRate


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

获取汇率,该接口的汇率都是以`USD`为基础兑换货币

<aside class="notice">
更新时间：数字货币更新时间为60秒，法币更新时间为4小时。
</aside>

<aside class="notice">
数据来源：数字货币汇率从加权平均计算的币种价格获取， 法币汇率由外汇交易所以及各大银行牌价结合。 
</aside>

#### 请求URL

`GET https://data.mifengcha.com/api/v3/exchange_rate`

#### 请求参数

None

#### 返回参数说明

参数 | 说明
--------- | -----------
c | 目标兑换货币
r | 目标兑换汇率,如基础货币为USD，CNY下的数字为USDCNY的汇率。

### Price

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
        "c": 0.0111
      },
      {
        "s": "ethereum",
        "S": "ETH",
        "T": 1564201016249,
        "u": 224.72639,
        "b": 0.021914663,
        "a": 1123748.2,
        "v": 250398262.42,
        "ra": 1123748.2,
        "rv": 250398260,
        "m": 23944294000,
        "c": 0.0111
      }
  ]
```

获取币种价格

<aside class="notice">
更新时间：5秒-60秒，按照交易量大小分级，交易量最大的币种5秒更新一次价格。
</aside>

#### 请求URL

`GET https://data.mifengcha.com/api/v3/price`

#### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
slug |QueryString|否| 币种名称,可传多个币种,逗号分割。
page |QueryString|否| 当前页数，默认 0, (>=0)。注意：只有slug和symbol两者皆不存在，该值才有效
size |QueryString|否| 每页数据量，默认 20 (100>=size>=1)。 注意：只有slug和symbol两者皆不存在，该值才有效
注意：若slug和symbol两者皆存在，优先级 slug > symbol， 按照交易量大小降序返回。

#### 返回参数说明

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

### HistoricalPrice

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
        "v" : 1.087066438855E10
      }, 
      {
        "T" : 1577691000000,
        "u" : 7397.4515,
        "b" : 1.0,
        "a" : 1495078.8,
        "v" : 1.09867723397E10
      }, 
      {  
        "T" : 1577724600000,
        "u" : 7253.9142,
        "b" : 1.0,
        "a" : 1559626.1,
        "v" : 1.132314561458E10
      }
  ]
```

获取币种历史价格

<aside class="notice">
数据来源：每5分钟快照一次当前价格，交易量。
</aside>

#### 请求URL

`GET https://data.mifengcha.com/api/v3/price/history?slug=bitcoin`

#### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
slug |QueryString|是| 币种名称。
start |QueryString|否| 起始时间，单位：毫秒, 默认该币种最早记录的时间
end |QueryString|否| 截止时间，单位：毫秒，默认当前时间

- 该接口会对返回数据进行下采样(downsampled), 数据点的间隔(`interval`)根据传递的参数`end` - `start`设置, 原则上`interval` = `end` - `start` / 1000, 每次请求数据返回限制大约为1000条。

#### 返回参数说明

参数 | 说明
--------- | -----------
T | 时间戳(毫秒)
u | 价格(单位: USD)
b | 价格(单位: BTC)
v | 交易量(单位: USD)
a | 交易量(单位: 当前币种)



### Tickers

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/tickers?market=bitfinex'
```

> 将会返回以下内容:

```json

    [
      {
        "T": 1566546506486,
        "m": "gate-io_BTC_USD",
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

批量获取交易对Tickers

<aside class="notice">
更新时间：1秒-30秒，影响更新频率的因数包括: 交易所是否支持批量接口,是否支持Websocket以及网络环境。
</aside>

<aside class="notice">
数据来源：通过交易所API获取
</aside>

<aside class="warning">
注意: 数据中心会对异常数据进行拦截。请接入时检查时间戳。
</aside>

#### 请求URL

`GET https://data.mifengcha.com/api/v3/tickers`

#### 请求参数

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
* 获取gate-io_BTC_USD与binance_ETH_BTC的Ticker `market_pair=gate-io_BTC_USD,binance_ETH_BTC`

#### 返回参数说明

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


### Orderbook

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/orderbook?desc=gate-io_BTC_USD'
```

> 将会返回以下内容:

```json
 {
    "T": 1526706175469,
    "m": "gate-io_BTC_USD",
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

获取交易对深度

<aside class="notice">
更新时间：1秒-40秒，影响更新频率的因数包括: 交易所是否支持批量接口,是否支持Websocket以及网络环境。
</aside>

<aside class="notice">
数据来源：通过交易所API获取
</aside>

<aside class="warning">
注意: 数据中心会对异常数据进行拦截。请接入时检查时间戳。
</aside>

#### 请求URL

`GET https://data.mifengcha.com/api/v3/orderbook`

#### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
desc |QueryString|是| 交易所的某个交易对。例如：gate-io_BTC_USD
limit |QueryString|否| 深度档位，默认25。


#### 返回参数说明

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


### Trades

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/trades?desc=gate-io_BTC_USD'
```

> 将会返回以下内容:

```json
 [
    {
      "T": 1573721951113,
      "p": 8634,
      "v": 5,
      "s": "buy",
      "m": "gate-io_BTC_USD"

    },
    {
      "T": 1573721944964,
      "p": 8634,
      "v": 0.001519,
      "s": "sell",
      "m": "gate-io_BTC_USD"
    }
  ]

```

获取交易对成交记录

<aside class="notice">
更新时间：5秒-60秒，影响更新频率的因数包括: 交易所是否支持批量接口,是否支持Websocket以及网络环境。
</aside>

<aside class="notice">
数据来源：通过交易所API获取
</aside>

#### 请求URL

`GET https://data.mifengcha.com/api/v3/trades`

#### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
desc |QueryString|是| 交易所的某个交易对。例如：gate-io_BTC_USD
limit |QueryString|否| 返回数据量，默认50


#### 返回参数说明

参数 | 说明
--------- | -----------
T|交易成交时间戳
p|成交价格
v|成交量
s|成交类型[buy,sell,none]，为taker操作方向
m|交易对信息

* 数据按照交易成交时间戳倒序排序


### Kline

```shell
curl -X GET \
  'https://data.mifengcha.com/api/v3/kline?desc=gate-io_BTC_USD&type=15m&start=1573637497000'
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

获取交易对K线数据(OHLCV)

<aside class="notice">
数据来源：通过交易所API获取
</aside>

#### 请求URL

`GET https://data.mifengcha.com/api/v3/kline`

#### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
desc |QueryString|是| 交易所的某个交易对。例如：gate-io_BTC_USD
interval |QueryString|否| K线类型 (数据点间隔)[5m,15m,30m,1h,6h,1d,7d],默认5m
end |QueryString|否| 截止时间，单位：毫秒，默认当前时间
start |QueryString|否| 起始时间，单位：毫秒，默认`end - (1000 * interval)`, 即表示 `end` 之前1000条数据

- 单次请求最大可获取2000条数据，`(end - start) / interval <= 2000`, 如果请求超过2000个数据点则会响应400 `10010 Duration Limited`

#### 返回参数说明

参数 | 说明
--------- | -----------
T|时间戳
o|开盘价
c|收盘价
l|最低价
h|最高价
v|交易量

## 资讯数据


### Brief

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

获取快讯数据

#### 请求URL

`GET https://data.mifengcha.com/api/v3/briefs?locale=zh_CN`

#### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
locale |QueryString|是| 语言，支持zh_CN(中文),en_US(英文),ko_KR(韩文)
page |QueryString|否| 当前页数，默认 0, (>=0)
size |QueryString|否| 每页数据量，默认 20 (>=1)


#### 返回参数说明

参数 | 说明
--------- | -----------
title|标题
content|内容
timestamp|时间戳
importance|是否是要闻
url|mifengcha.com原文链接
source|来源
images|图片链接


### Announcements

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
    "images" : [ ]
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

获取交易所公告数据
<aside class="notice">
数据来源：通过交易所获取
</aside>
#### 请求URL

`GET https://data.mifengcha.com/api/v3/announcements?locale=zh_CN`

#### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
locale |QueryString|是| 语言，支持zh_CN(中文),en_US(英文),ko_KR(韩文)
page |QueryString|否| 当前页数，默认 0, (>=0)
size |QueryString|否| 每页数据量，默认 20 (>=1)
market |QueryString|否| 获取指定的交易所公告


#### 返回参数说明

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


### Articles

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

获取资讯文章列表

#### 请求URL

`GET https://data.mifengcha.com/api/v3/articles?locale=zh_CN`

#### 请求参数

参数名称|传输方式|必选|说明
--------- |---------|--------- | -----------
locale |QueryString|是| 语言，支持zh_CN(中文),en_US(英文),ko_KR(韩文)
page |QueryString|否| 当前页数，默认 0, (>=0)
size |QueryString|否| 每页数据量，默认 20 (>=1, <=20)


#### 返回参数说明

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


### Article

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

获取单篇资讯文章内容

#### 请求URL

`GET https://data.mifengcha.com/api/v3/articles/{id}`

#### 请求参数

参数 | 说明
--------- | -----------
id| 文章id


#### 返回参数说明

与文章格式与列表相同，多出`btcPrice`字段

参数 | 说明
--------- | -----------
btcPrice|发文时比特币价格



# WebSocket API


<aside class="success">
URL: wss://data.mifengcha.com/ws/v3
</aside>

* 每个链接有效期不超过24小时，请妥善处理断线重连。
* 每30秒服务端会发送ping帧，客户端应当在30秒内回复pong帧，否则服务端会主动断开链接。
* 连接时需要将用户对应的API_KEY作为参数传入，`wss://data.mifengcha.com/ws/v3?api_key=[YOUR_API_KEY]`。
* 每个链接最多可订阅200个Topic。
* 每个账户可以建立的链接数根据账户套餐设定, 详情查看 [Pricing](https://data.mifengcha.com/pricing)。
* 服务端不会校验Topic的正确性，若订阅无效Topic不会有响应，并且占用Topic额度。
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

#### 消息格式

`{"op": "subscribe", "args": ["<topic1>","<topic2>", ...]}`

#### 返回参数说明

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

#### 消息格式

`{"op": "unsubscribe", "args": ["<topic1>","<topic2>", ...]}`

#### 返回参数说明

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

#### 消息格式

`{"op": "topics"}`

#### 返回参数说明

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

#### Topic格式

`price:` + [币种](#symbols) slug

#### 返回参数说明

参数 | 说明
--------- | -----------
code| Message Code
message| Message
data| [Price](#price)


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
    "m": "gate-io_BTC_USD",
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

#### Topic格式

`ticker:` + 交易对(`binance_BNB_USDT`, `huobipro_HT_USDT`)

#### 返回参数说明

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

#### Topic格式

`orderbook:` + 交易对(`binance_BNB_USDT`, `huobipro_HT_USDT`)

#### 返回参数说明

参数 | 说明
--------- | -----------
code| Message Code
message| Message
data| [Orderbook](#orderbook)




# 交易所收录

API 建议

1. 请使用QueryString或者URLParams方式传参。
2. 请提供批量获取ticker的接口，以便批量更新价格和交易对。
3. 如果暂时不能提供批量获取ticker的接口, 请提供获取所有交易对的接口。(不推荐,请求频率高,更新速度慢)
4. 交易对请尽量使用符号分割或者固定长度。
5. 正常情况下每分钟请求次数为10次左右。
6. 接口数据保证稳定真实。
  
[点此连接](http://cn.mikecrm.com/TNFyHPJ) 进入交易所收录表单
