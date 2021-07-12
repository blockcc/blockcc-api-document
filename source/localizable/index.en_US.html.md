---
title: Block.cc APIv3 Developer Documentation

language_tabs: # must be one of https://git.io/vQNgJ

- shell


toc_footers:

- <a target="_blank" href='https://data.block.cc/login'>Login data.block.cc</a>
- <br>
- <a class="locale-button" href='../zh_CN'><img src="/images/flags/zh_CN.svg" alt="简体中文"/></a> <a class="locale-button" href='../en_US'><img src="/images/flags/en_US.svg" alt="English"/></a>

search: true
code_clipboard: true
---

# Overview

## Introduction

[Block.cc](https://block.cc)  is a professional cryptocurrency information service platform that provides one-stop cryptocurrency services such as cryptocurrency quotes, data, and information. Blockchain enthusiasts and cryptocurrency investors provide the most authoritative and smooth products and services.

Third-party application developers can use this service to quickly build a stable and efficient cryptocurrency market system to provide technical support for real-time business needs and product operations.

## Access URLs

You can compare the latency of using the data.block.cc and data.mifengcha.com domain names by yourself, and choose the one with the lowest latency.

Among them, the data.mifengcha.com domain name has made certain link delay optimizations for users in mainland China.

#### REST API

https://data.block.cc/api/v3

https://data.mifengcha.com/api/v3

#### Websocket Feed

wss://data.block.cc/ws/v3

wss://data.mifengcha.com/ws/v3

## Current Version

<aside class="success">
Version: v3.0.0
</aside>

## Authenticate

### Get API KEY

All Block.cc API requests must be authenticated using an API key. If you don't have an API key yet, please [click to register](https://data.block.cc/register).

### Use your API KEY

> example:

```shell
curl -X GET \
   -H 'X-API-KEY: [YOUR_API_KEY]' \
  'https://data.block.cc/api/v3/markets'
  
curl -X GET \
  'https://data.block.cc/api/v3/markets?api_key=[YOUR_API_KEY]'
  
wscat -c 'wss://data.block.cc/ws/v3?api_key=[YOUR_API_KEY]'

```


You can provide an API key in a REST API call in the following ways:

1. Preferred method: Via a custom request header named `X-API-KEY`

2. Convenient method: pass a query string parameter named `api_key`

The API key can only be provided in the Websocket API via a query string parameter named `api_key`


## Rate Limited

```json
{
  "code": 429,
  "message": "Rate Limited"
}
```

In order to provide higher quality of service, different restrictions are made according to different packages. See [Pricing](https://data.block.cc/pricing) for details
> You can check your remaining times through the response header:

```
X-RateLimit-Limit：10000 
X-RateLimit-Remaining：9999 
```

## Error Code

> example:

```json
{
  "code": 10030,
  "message": "Param Required"
}
```

HTTP Status Code | Error Code | Error Message
--------- | -----------| -----------
400 | 10010| Duration Limited
400 | 10030| Param Required
400 | 10050| Empty Resource
400 | 10070| Unavailable Resource
429 | 10040| Rate Limited
414 | 10090| Url Too Long
500 | 10020| Internal Server Error
404 | 404| Not Found

## Pagination

```shell
curl -X GET \
  'https://data.block.cc/api/v3/markets?page=0&size=100'
```



By default, requests that return multiple entries will be paged by 20 entries per page. You can specify other pages using the `page` parameter. You can also use the `size` parameter to set a custom page size up to 100. Please note that for technical reasons, not all APIs conform to `size`, see the description of API parameters.

Note that the page number starts at 0, and the default page parameter will return 0 on the first page.

### Link Header

The paging information in response header：

```
Link →</api/v3/tickers?page=1&size=20>; rel="next",
</api/v3/tickers?page=1185&size=20>; rel="last",
</api/v3/tickers?page=0&size=20>; rel="first"

X-Total-Count →10000
```

The specific paging information is displayed in the link tag of the response header, so that the caller can directly obtain the url address of the content such as the next page, the last page, the previous page, etc., instead of manually calculating and splicing.

Possible `rel` values are:

Name | Description
--- | ---
next | The next page link
last | The last page link
first | The first page link
prev | The previous page link

# Changelog

### 2021-05-26

- Add `platforms` field to [Symbols](#symbols)

### 2020-10-16

- Add request param `interval` to [HistoricalPrice](#historicalprice)

### 2020-10-01

- APIv1 is not available for free users. APIv3 has been stable for users for more than a year. It is recommended to
  update to APIv3 for a better experience.

### 2020-05-25

Add SocialMedia API [SocialMedia](#socialmedia)

### 2020-04-16

Add kline 1min interval [Kline](#kline)

### 2020-04-09

Add fields to [Price](#price)

- Highest daily price
- Lowest daily price
- Weekly price change
- Weekly high price
- Weekly lowest price
- Monthly price change
- Highest monthly price
- Lowest monthly price
- Highest price in history
- Lowest price in history

# REST API

<aside class="success">
URL: https://data.block.cc/api/v3
</aside>


## Meta Data

Metadata is the basic data and is generally used as a parameter for requesting market data.

### Markets

> return:

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

Get a list of all supported markets

#### Request URL

`GET https://data.block.cc/api/v3/markets`

`GET https://data.block.cc/api/v3/markets/{slug}`

#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
slug | URL Path|No| Get single market,e.g. `/api/v3/markets/gate-io`
page |QueryString|No| Current page, default 0, (>=0)
size |QueryString|No| Per page size, default is 20 (100>=size>=1)


#### Response Parameter
Parameter | Description
--------- | -----------
slug | Market slug (ID)
fullname | Full Market Name
websiteUrl | Exchange's official website link
volume | (old) Transaction volume (USD) counted by human intervention
reportedVolume | Exchange reported trading volume (USD)
expectedVolume | Estimated Trading Volume (USD)
status | Status: [enable, disable]. disable is to stop updating data
kline | Is access K-line data
spot | Is spot market
futures | Is futures market

### Symbols

```shell
curl -X GET \
  'https://data.block.cc/api/v3/symbols'
```

```shell
curl -X GET \
  'https://data.block.cc/api/v3/symbols/ethereum'
```

> Response:

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

Get a list of all supported currencies

#### Request URL

`GET https://data.block.cc/api/v3/symbols`

`GET https://data.block.cc/api/v3/symbols/{slug}`

#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
slug | URL Path|No| for getting single currency
details |QueryString|No| Is getting the details of the currency, the value is 1(yes), 0(no), the default is 0
page |QueryString|No| Current page, default 0, (>=0).
size |QueryString|No| Per page size, default is 20 (100>=size>=1).



#### Response Parameter
Parameter | Description
--------- | -----------
slug | Slug（ID）
symbol | Symbol
fullname | Fullname
logoUrl| Logo Url
volumeUsd | Transaction volume (USD) through manual intervention
status | Status: [enable, disable]. Disable is to stop updating data
marketCapUsd | Currency Market Cap
availableSupply | Available Supply
totalSupply | Total Supply
maxSupply | Maximum Supply
website | Official website link
explorerUrls | Block Explorer Link
whitePaperUrls | White Paper Link
githubId| Github
twitterId | Twitter
facebookId | FaceBook
telegramId| Telegram
algorithm | Core Algorithm
proof | Proof
platforms | platform information
issueDate | Time to issue
details | Currency introduction, not returned by default

## Market Data

### ExchangeRate


```shell
curl -X GET \
  'https://data.block.cc/api/v3/exchange_rate'
```

> Response:

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

Get the exchange rate, the exchange rate of this interface is based on `USD`

<aside class="notice">
Updated: crypto currency update time of 60 seconds, fiat currency update time was 4 hours.
</aside>

<aside class="notice">
Data Source: The crypto currency exchange rate is obtained from the currency price calculated by weighted average.
</aside>

#### Request URL

`GET https://data.block.cc/api/v3/exchange_rate`

#### Request Parameter

None

#### Response Parameter

Parameter | Description
--------- | -----------
c | Target Currency
r | Exchange Rate

### Price

```shell
curl -X GET \
  'https://data.block.cc/api/v3/price?slug=bitcoin,filecoin'
```

> Response:

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

Get currency price

<aside class="notice">
Update Time: 5 seconds to 60 seconds, according to the volume size classification, the most traded currency prices updated every five seconds.
</aside>

#### Request URL

`GET https://data.block.cc/api/v3/price`

#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
slug |QueryString|No| Currency slug, comma-separated
page |QueryString|No| Current page, default 0, (>=0)
size |QueryString|No| Per page size, default is 20 (100>=size>=1)

#### Response Parameter

Parameter | Description
--------- | -----------
s | Slug
S | Symbol
u | Price(USD)
b | Price(BTC)
T | 13-bit Unix Timestamp
a | Amount(unit: baseCurrency)
v | Volume(unit: USD)
ra | ReportedAmount(unit: baseCurrency)
rv | ReportedVolume(unit: USD)
m | MarketCap(unit: USD)
c | 24h price change
h | 24h highest price
l | 24h lowest price
cw | One week price change
hw | One week highest price
lw | One week lowest price
cm | One month price change
hm | One month highest price
lm | One month lowest price
ha | Historical highest price
la | Historical lowest price

### HistoricalPrice

```shell
curl -X GET \
  'https://data.block.cc/api/v3/price/history?slug=bitcoin'
```

> Response:

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

Get currency historical price

<aside class="notice">
Data Source: Snapshot of current price and trading volume every 5 minutes.
</aside>

#### Request URL

`GET https://data.block.cc/api/v3/price/history?slug=bitcoin`

#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
slug |QueryString|Yes| Symbol Slug.
start |QueryString|No| Start Time，unit: mills, default min timestamp
end |QueryString|No| End Time，unit: mills，default current timestamp
interval |QueryString|否| Interval[5m,15m,30m,1h,2h,6h,12h,1d,2d], default calculated based on `start`, `end`

- The data size limit per request is about 1000
- When `interval` is not blank, it needs to satisfy `(end-start) / interval <= 1000`, if more than 1000 data points are requested, 400 `10010 Duration Limited` will be responded.
- When `interval` is blank, it will downsample the responded data, select the optimal `interval` to ensure the coverage of data, in principle `interval` ≈ `end`-`start` / 1000.

#### Response Parameter

Parameter | Description
--------- | -----------
T | 13-bit Unix Timestamp
u | Price(USD)
b | Price(BTC)
v | Volume(USD)
a | Amount(unit: baseCurrency)
m | MarketCap(unit: USD)



### Tickers

```shell
curl -X GET \
  'https://data.block.cc/api/v3/tickers?market=bitfinex'
```

> Response:

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

Get Tickers

<aside class="notice">
Updated: 1 second to 30 seconds, the impact factor update frequency include: the exchange supports batch API, Websocket supported and network environment.
</aside>

<aside class="notice">
Data Source: From Exchange
</aside>

<aside class="warning">
Note: The data center will intercept abnormal data. Please check the time stamp when connecting.
</aside>

#### Request URL

`GET https://data.block.cc/api/v3/tickers`

#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
market |QueryString|No| Market Slug, comma-separated
symbol |QueryString|No| BaseCurrency Symbol, comma-separated
slug |QueryString|No|  BaseCurrency Slug, comma-separated
currency |QueryString|No|  QuoteCurrency Symbol, comma-separated
market_pair |QueryString|No| MarketPairDesc, comma-separated
page |QueryString|No| Current page, default 0, (>=0)
size |QueryString|No| Per page size, default 20 (>=1)

* Data are sorted in reverse order of USD transaction volume
* market, symbol, slug, currency are the screening conditions for trading pairs.
* When there are more than two parameters of market, symbol, slug, and currency, only transaction pairs that meet the conditions are returned.
* Ignore market, symbol, slug, currency when market_pair exists.

e.g.

* Get bitfinex's BTC Tickers `market=bitfinex&symbol=BTC`
* Get bitfinex and binance's BTC and ETH Tickers `market=bitfinex,binance&symbol=BTC,ETH`
* Get gate-io_BTC_USDT and binance_ETH_BTC Tickers `market_pair=gate-io_BTC_USDT,binance_ETH_BTC`

#### Response Parameter

Parameter | Description
--------- | -----------
T | 13-bit Unix Timestamp
c | Last Price, Close Price(unit：quoteCurrency)
b | Best Bid Price(unit：quoteCurrency)
a | Best Ask Price(unit：quoteCurrency)
o | 24H Open Price(unit：quoteCurrency)
h | 24H High Price(unit：quoteCurrency)
l | 24H Low Price(unit：quoteCurrency)
bv | 24H Base Volume(unit：baseCurrency)
qv | 24H Quote Volume(unit：quoteCurrency)
C | 24H Price Change Rate
m | MarketPairDesc
p | Purity
r | To Usd Rate
s | Spread

* The data update time is generally the timestamp returned by the exchange API. If the exchange interface does not return a timestamp, it is the timestamp before the request.
* If the increase is 1%, the `C` return value is 0.01
* r is the exchange rate from base currency to USD.
* Get latest USD price as: latest price (c) * quote currency to USD exchange rate (r)
* Get 24-hour USD transaction volume: 24 hour quote volume (qv) * quote currency to USD exchange rate (r)

### Orderbook

```shell
curl -X GET \
  'https://data.block.cc/api/v3/orderbook?desc=gate-io_BTC_USDT'
```

> Response:

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

Get OrderBook

<aside class="notice">
Updated: 0.1-40 seconds, the influence update frequency factor comprising: a support Websocket and network environment.
</aside>

<aside class="notice">
Data Source: From Exchange
</aside>

<aside class="warning">
Note: The data center will intercept abnormal data. Please check the timestamp when accessing.
</aside>

#### Request URL

`GET https://data.block.cc/api/v3/orderbook`

#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
desc |QueryString|Yes| MarketPair Desc e.g. gate-io_BTC_USDT
limit |QueryString|No| Limit,default 25.


#### Response Parameter

Parameter | Description
--------- | -----------
T|13-bit Unix Timestamp
m|MarketPairDesc
b|Bids
a|Asks

b/a | Description
--------- | -----------
0|Price
1|Amount

* The data update time is generally the timestamp returned by the exchange API, if the exchange API does not return a
  timestamp, it is the timestamp before request

### Trades

```shell
curl -X GET \
  'https://data.block.cc/api/v3/trades?desc=gate-io_BTC_USDT'
```

> Response:

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

Get Trades

<aside class="notice">
Updated: 5-40 seconds, the influence update frequency factor comprising: a support Websocket and network environment.
</aside>

<aside class="notice">
Data Source: From Exchange
</aside>

#### Request URL

`GET https://data.block.cc/api/v3/trades`

#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
desc |QueryString|Yes| Market Pair Desc. e.g.：gate-io_BTC_USDT
limit |QueryString|No| Data Size, 0<limit<=50

#### Response Parameter

Parameter | Description
--------- | -----------
T|13-bit Unix Timestamp
p|Price
v|Base Volume
s|Taker Order Side[buy,sell,none]
m|Market Pair Desc

### Kline

```shell
curl -X GET \
  'https://data.block.cc/api/v3/kline?desc=gate-io_BTC_USDT&interval=15m'
```

> Response:

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

Get Kline Data(OHLCV)

<aside class="notice">
Data Source: From Exchange
</aside>

#### Request URL

`GET https://data.block.cc/api/v3/kline`

#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
desc |QueryString|Yes| MarketPair Desc e.g. gate-io_BTC_USDT
interval |QueryString|No| interval [1m,5m,15m,30m,1h,6h,1d], default: 5m
end |QueryString|No| End Time，unit: mills，default current time
start |QueryString|No| Start Time，unit: mills，default `end - (1000 * interval)`

- maximum of 2000 pieces of data for a single request, `(end-start)/interval <= 2000`, if the request exceeds 2000 data points, the response will be return 400 `10010 Duration Limited`

#### Response Parameter

Parameter | Description
--------- | -----------
T|Open Time 13-bit Unix Timestamp
o|Open Price(unit: quoteCurrency)
c|Close Price(unit: quoteCurrency)
l|Low Price(unit: quoteCurrency)
h|High Price(unit: quoteCurrency)
v|Volume (unit: baseCurrency)

## News data


### Brief

```shell
curl -X GET \
  'https://data.block.cc/api/v3/briefs?locale=zh_CN'
```

> Response:

```json
 [
  {
    "images": [],
    "datetime": 1576477950000,
    "importance": false,
    "source": "金色财经",
    "title": "亿利集团董事长王文彪：要将区块链治沙模式推广到全世界",
    "content": "12月16日,IFIC全球金融科技创新峰会·东京站正式开幕.亿利资源集团董事长,库布齐沙漠治理的带头人王文彪在IFIC TOKYO开场致辞中表示,亿利集团在30余年的治理了库布其沙漠,我们的治理模式已经可以在全中国推广.区块链技术的集成应用在新的技术革新和产业变革中起着重要作用,我们想用区块链把沙漠治理模式带到全世界,向世界推广,为全人类做些贡献,做区块链公益.在此,希望日本的朋友到沙漠考察访问,将公益基金和区块链相连接,所投入的资金用来促进环境的发展,让全世界人知道怎样运用区块链使沙漠变成良田,达到可持续发展.",
    "url": "https://m.block.cc/news/5df724fe07a014563b8269c0"
  },
  {
    "images": [],
    "datetime": 1576477582000,
    "importance": false,
    "source": "小葱",
    "title": "工信部将从推动云计算与区块链等技术融合创新等入手推动云计算产业快速发展",
    "content": "工信部信软司副司长董大健表示,下一步,工信部将从五方面入手推动云计算产业快速发展.一是持续优化发展环境,规范云计算市场,培育龙头骨干企业；二是加快突破核心技术,加快云计算在自主基础软硬件平台上的适配迁移,推动云计算以5G、工业互联网、大数据、人工智能、区块链等技术融合创新；三是深入推动企业上云应用云；四是完善云计算的标准体系；五是打造安全保障体系.（c114）",
    "url": "https://m.block.cc/news/5df7238e07a014563b8269b4"
  }
]

```

Get Briefs

#### Request URL

`GET https://data.block.cc/api/v3/briefs?locale=zh_CN`

#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
locale |QueryString|Yes| Language, support zh_CN (Chinese), en_US (English), ko_KR (Korean)
page |QueryString|No| Current page, default 0, (>=0)
size |QueryString|No| Per page size, default 20 (>=1)


#### Response Parameter

Parameter | Description
--------- | -----------
title|Title
content|Content
timestamp|13-bit Unix Timestamp
importance|Is Importance
url|Block.cc Link
source|Source
images|Cover Image Link


### Announcements

```shell
curl -X GET \
  'https://data.block.cc/api/v3/announcements?locale=zh_CN'
```

> Response:

```json
 [
  {
    "title" : "关于杠杆交易上线LTC/USDT、EOS/USDT、TRX/USDT的公告（1220）",
    "content" : "尊敬的用户： \n\n\n BiKi平台将于2019年12月20日18:00开放LTC/USDT、EOS/USDT、TRX/USDT杠杆交易. \n\n  \n\n提示：\n 1、合理操作杠杆交易,可以提高收益.但同时考虑到加密货币市场目前的波动性,它也会为交易者带来更大的风险.\n 2、杠杆交易受市场风险、波动性和复杂性的影响较大,请您务必充分了解杠杆交易的风险后审慎使用,BiKi平台不对因杠杆交易造成的损失负责.\n  \n 感谢您对BiKi平台的支持,BiKi团队期待您的宝贵意见.\n BiKi团队\n 2019-12-20",
    "timestamp" : 1576811020489,
    "importance" : false,
    "url" : "https://m.block.cc/news/5dfc3a0cc3af0a649f3f4563",
    "market" : "bikicoin",
    "sourceUrl" : "https://www.biki.com/noticeInfo/2007",
    "images": []
  },
  {
    "title" : "Binance战略投资FTX并上市FTX Token（FTT）",
    "content" : "亲爱的用户： \n\nBinance已完成对FTX的战略投资（投资详情）,现已上线FTX Token（FTT）,开通FTT/BNB、FTT/BTC、FTT/USDT 交易市场,邀您体验！FTT充值通道现已开放,立即充值. \n\nFTT的上币费用为0 BNB. \n\n声明：Binance已从对FTX的投资中获得了一部分FTT代币. 其中绝大多数FTT锁定期为两年. 我们致力于帮助FTT和FTX生态系统实现长期,可持续的发展. \n\n规则说明： \n\n- 关于FTX Token (FTT)\n- 费率说明\n- 交易规则 \n\n风险提示：数字货币是一种高风险的投资方式,请投资者谨慎购买,并注意投资风险.Binance会遴选优质币种,但不对投资行为承担担保、赔偿等责任. \n\n  \n\n感谢您对Binance的支持！ \n\n  \n\nBinance团队 \n\n2019年12月20日 \n\n  \n\nBinance社群 \n\n微博： https://weibo.com/u/7336825507  \n\nTelegram： https://t.me/BinanceChinese \n\nFacebook： https://www.facebook.com/BinanceChinese \n\nTwitter： https://twitter.com/binance",
    "timestamp" : 1576810511000,
    "importance" : false,
    "url" : "https://m.block.cc/news/5dfc3a03c3af0a649f3f4416",
    "market" : "binance",
    "sourceUrl" : "https://binance.zendesk.com/hc/zh-cn/articles/360038147271-Binance%E6%88%98%E7%95%A5%E6%8A%95%E8%B5%84FTX%E5%B9%B6%E4%B8%8A%E5%B8%82FTX-Token-FTT-",
    "images" : [ ]
  }
]

```

Get Exchange Announcement List

<aside class="notice">
Data Source: From Exchange
</aside>

#### Request URL

`GET https://data.block.cc/api/v3/announcements?locale=zh_CN`

#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
locale |QueryString|Yes| Language, support zh_CN (Chinese), en_US (English), ko_KR (Korean)
page |QueryString|No| Current page, default 0, (>=0)
size |QueryString|No| Per page size, default 20 (>=1)
market |QueryString|No| Market slug


#### Response Parameter

Parameter | Description
--------- | -----------
title|Title
content|HTML Content
timestamp|13-bit Unix Timestamp
importance|Is Importance
sourceUrl|Source Url
market|Market
url|Block.cc Link
images|Cover Image Link


### Articles

```shell
curl -X GET \
  'https://data.block.cc/api/v3/articles?locale=zh_CN'
```

> Response:

```json
[
  {
    "id": "5e1d96a5aeae8770b7ff34ac",
    "timestamp": 1578997412000,
    "title": "主流币种普涨势头将迎考验,平台币示弱DASH迫近中线强阻",
    "description": "白盘主流币种继续扩大涨势,DASH冲高至近三个半月新高,不过这个短期“风向标”币种已经走到了一个至关重要的多空分界线附近.",
    "author": "小葱区块链",
    "categories": "资讯",
    "images": [],
    "url": "https://m.block.cc/news/5e1d96a5aeae8770b7ff34ac",
    "source": "火星财经",
    "sourceUrl": "https://news.huoxing24.com/20200114182213991739.html",
    "keywords": "dash,币种,中线,势头,平台,阻力,ht,行情,btc,指标,区域,圆弧底,bsv,bnb,过程,延续性,均线,关键,观点,涨幅,涨势,整数,结构,效应,形态,小时,双顶,阶段,市场,二度,全数,仔细观察,撰文,风向标,风向,横盘,标记,时段,时间,分水岭,士气,情况,力图,点位,所处,盲目"
    "content": "<html value>"
  }
  "..."
]

```

Get Article List

#### Request URL

`GET https://data.block.cc/api/v3/articles?locale=zh_CN`

#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
locale |QueryString|Yes| Language, support zh_CN (Chinese), en_US (English), ko_KR (Korean)
page |QueryString|No| Current page, default 0, (>=0)
size |QueryString|No| Per page size, default 20 (>=1, <=20)


#### Response Parameter

Parameter | Description
--------- | -----------
id| Article ID
timestamp|13-bit Unix Timestamp
title|Title
description|Description
author|Author
categories|Categories, comma-separated
images|Cover Image Link
url|Block.cc Link
source|Source


### Article

```shell
curl -X GET \
  'https://data.block.cc/api/v3/articles/5e1d96a5aeae8770b7ff34ac'
```

> Response:

```json
  {
  "id": "5e1d96a5aeae8770b7ff34ac",
  "timestamp": 1578997412000,
  "title": "主流币种普涨势头将迎考验,平台币示弱DASH迫近中线强阻",
  "description": "白盘主流币种继续扩大涨势,DASH冲高至近三个半月新高,不过这个短期“风向标”币种已经走到了一个至关重要的多空分界线附近.",
  "author": "小葱区块链",
  "categories": "资讯",
  "images": [],
  "url": "https://m.block.cc/news/5e1d96a5aeae8770b7ff34ac",
  "source": "火星财经",
  "sourceUrl": "https://news.huoxing24.com/20200114182213991739.html",
  "keywords": "dash,币种,中线,势头,平台,阻力,ht,行情,btc,指标,区域,圆弧底,bsv,bnb,过程,延续性,均线,关键,观点,涨幅,涨势,整数,结构,效应,形态,小时,双顶,阶段,市场,二度,全数,仔细观察,撰文,风向标,风向,横盘,标记,时段,时间,分水岭,士气,情况,力图,点位,所处,盲目"
  "content": "<html value>",
  "btcPrice": 8465.6615
}
```

Get One Article

#### Request URL

`GET https://data.block.cc/api/v3/articles/{id}`

#### Request Parameter

Parameter | Description
--------- | -----------
id| Article ID


#### Response Parameter

Same as the article list format, with the addition of the `btcPrice` field

Parameter | Description
--------- | -----------
btcPrice|Bitcoin price at launch


### SocialMedia

```shell
curl https://data.block.cc/api/v3/social_media?source=TWITTER
```

> Response:

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

Get SocialMedia List

<aside class="notice">
Data Source：Twitter，Weibo
</aside>

#### Request URL

`GET https://data.block.cc/api/v3/social_media`


#### Request Parameter

Parameter | Position | Required | Description
--------- |---------|--------- | -----------
source |QueryString|Yes| Source: [WEIBO, TWITTER], default WEIBO
page |QueryString|No| Current page, default 0, (>=0)
size |QueryString|No| Per page size, default is 20 (100>=size>=1)



#### Response Parameter

Parameter | Description
-------- | :----------
id | SocialMedia ID
source | SocialMedia Platform
content |Content
images | Images Link
userId | User Id
avatar | User Avatar
screenName | User Name (ScreenName)
timestamp | 13-bit Unix Timestamp
retweeted | Is Retweeted
retweet | Retweeted Content
retweet.content | Retweeted Content
retweet.images | Retweeted Images Link
retweet.userId | Retweeted User Id
retweet.avatar | Retweeted User Avatar
retweet.screenName | Retweeted User Name (ScreenName)




# WebSocket API


<aside class="success">
URL: wss://data.block.cc/ws/v3
</aside>

* The validity period of each link does not exceed 24 hours, please properly handle the disconnection and reconnection.
* The server will send a ping frame every 30 seconds, and the client should reply to the pong frame within 30 seconds, otherwise the server will actively disconnect the link.
* API_KEY needs to be passed in as a parameter when connecting, `wss://data.block.cc/ws/v3?api_key=[YOUR_API_KEY]`.
* One link can subscribe to up to 200 topics.
* The maximum number of connections for an account is based on its price package, see [Pricing](https://data.block.cc/pricing) for details.
* The server will not verify the correctness of the topic. If the subscription is invalid, the topic will not respond and occupy the topic subscription quota.
* The unchanged data will not be pushed within 90 seconds


## Subscribe
```shell
wscat -c 'wss://data.block.cc/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "subscribe", "args": ["price:bitcoin"]}
```

> Response:

```json
{"code":0,"message":"Subscribed"}
```

#### Request Parameter

`{"op": "subscribe", "args": ["<topic1>","<topic2>", ...]}`

#### Response Parameter

Parameter | Description
--------- | -----------
code| Message Code
message| Message




## Unsubscribe
```shell
wscat -c 'wss://data.block.cc/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "unsubscribe", "args": ["price:bitcoin"]}
```

> Response:

```json
{"code":0,"message":"Unsubscribed"}
```

#### Request Parameter

`{"op": "unsubscribe", "args": ["<topic1>","<topic2>", ...]}`

#### Response Parameter

Parameter | Description
--------- | -----------
code| Message Code
message| Message

## List Subscription

```shell
wscat -c 'wss://data.block.cc/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "subscribe", "args": ["price:bitcoin"]}
> {"op": "topics"}
```

> Response:

```json
{"code":0,"message":"Success","topics":["price:bitcoin"]}
```

#### Request Parameter

`{"op": "topics"}`

#### Response Parameter

Parameter | Description
--------- | -----------
code| Message Code
message| Message
topics| Subscribed Topic List

## Topic: Price

```shell
wscat -c 'wss://data.block.cc/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "subscribe", "args": ["price:bitcoin"]}
```
> Update:

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

#### Topic Format

`price:` + [Symbol](#symbols) slug

#### Response Parameter

Parameter | Description
--------- | -----------
code| Message Code
message| Message
data| [Price](#price)


## Topic: Ticker

```shell
wscat -c 'wss://data.block.cc/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "subscribe", "args": ["ticker:gate-io_BTC_USDT"]}
```
> Update:

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

#### Topic Format

`ticker:` + MarketPair Desc(`binance_BNB_USDT`, `huobipro_HT_USDT`)

#### Response Parameter

Parameter | Description
--------- | -----------
code| Message Code
message| Message
data| [Ticker](#tickers)



## Topic: Orderbook

```shell
wscat -c 'wss://data.block.cc/ws/v3?api_key=[YOUR_API_KEY]'
> {"op": "subscribe", "args": ["orderbook:gate-io_BTC_USDT"]}
```
> Update:

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

#### Topic Format

`orderbook:` + MarketPair Desc(`binance_BNB_USDT`, `huobipro_HT_USDT`)

#### Response Parameter

Parameter | Description
--------- | -----------
code| Message Code
message| Message
data| [Orderbook](#orderbook)




# Exchanges Included

API recommendations

1. Use QueryString or URLParams to pass parameters.
2. Please provide an interface to obtain tickers in batches so that prices and trading pairs can be updated in batches.
3. If you cannot provide an interface to obtain tickers in batches, please provide an interface to obtain all trading pairs. (Not recommended, high request frequency, slow update speed)
4. Please use symbol splitting or fixed length for trading pairs as much as possible.
5. Under normal circumstances, the number of requests per minute is about 10 times.
6. The interface data is guaranteed to be stable and true.

[Click Here](http://cn.mikecrm.com/TNFyHPJ) Submit the exchange acceptance form
