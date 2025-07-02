---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  # - shell
  # - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the CoinDCX API
---
# Terms and Conditions

These API License Terms and Conditions (“Terms”) shall govern the use of any ‘Market Data’ and Application Programming Interface (API) of CoinDCX by you, either an individual, association of persons, company, or any legal entity and its respective affiliates (hereinafter referred to as “User”). 
For the purpose of these Terms, ‘Market Data’ shall mean and include all data related to the trading activity on any website, applications or platform owned and operated by Primestack Pte. Limited and/or Neblio Technologies Private Limited or any of their parent company, subsidiaries, or affiliates (collectively referred to as “CoinDCX”) including any data, information made available to the User through the application programming interface of CoinDCX (“CoinDCX API”). The Market Data may include, without limitation, the prices and quantities of orders and transactions executed on any platform/ application of CoinDCX.
 
The User hereby agrees and acknowledges that upon accessing any CoinDCX API (defined hereinafter), Market Data and/or any other information, service, feature governed by terms contained herein, the User shall be bound by these Terms, as updated, modified, and/or replaced from time to time. The User is required to check for any such amendment, replacement or changes to the terms contained herein and any future use or access to any services covered herein.

### 1. DEFINITIONS

### 2. LICENSE GRANT AND AUTHORISED USE

### 3. FEES AND CHARGES


### 4. OWNERSHIP AND RIGHTS


### 5.  TERM AND TERMINATION


### 6.  REPRESENTATIONS AND WARRANTIES OF THE USER


### 7.  DISCLAIMER

### 8. SECURITY

### 9.  LIMITATION OF LIABILITY

### 10. INDEMNITY

### 11. ARBITRATION


### 12.	GOVERNING LAW AND JURISDICTION
 
### 13.	MISCELLANEOUS

# Introduction

This document is aimed to cover the API definitions for the new platform being developed by Norges idrettsforbund og olympiske og paralympiske komité, herewith referred to as NIF for short. This new API product is a set of RESTful services. The Integration Partners, herewith mentioned as IP for short, need to switch to the new APIs to sync data with NIF.

This document is written for developers, programmers and IPs who seek to sync data with  NIF. Any club that is interested in syncing their data with NIF needs to go through a registered IP to connect to these APIs.


> The python version used for API samples is 2.7

# Terminology

### Common

<!-- * `target currency` refers to the asset that is the `quantity` of a symbol.
* `base currency` refers to the asset that is the `price` of a symbol.
* `pair` uniquely identifies the market along with its exchange, and is available in market details api.
* `ecode` is used to specify the exchange for the given market. Valid values for ecode include:
  - `B`: Binance
  - `I`: CoinDCX
  - `HB`: HitBTC
  - `H`: Huobi
  - `BM`: BitMEX
  - `OE`: OkEx -->

### Orders

<!-- * `status`: used to denote the current status of the given order. Valid values for status include:
  - `init`: order is just created, but not placed in the orderbook (eg: stop-limit orders whose stop/trigger price hasn't reached)
  - `open`: order is successfully placed in the orderbook
  - `partially_filled`: order is partially filled
  - `filled`: order is completely filled
  - `partially_cancelled`: order is partially filled, but cancelled, thus inactive
  - `cancelled`: order is completely or partially cancelled
  - `rejected`: order is rejected (not placed on the exchange)

Among these, the open-equivalent status' includes:
<br/>`init, open, partially_filled`
<br/>Orders having any these status can undergo further change (like when they get filled or cancelled).

And settled or closed-equivalent status' includes:
<br/>`filled, partially_cancelled, cancelled, rejected`
<br/> Orders having any of these status could not undergo any change. -->

### Margin Orders

<!-- * `status`: used to denote the current status of the given margin order. Valid values for status include:
  - `init`: order is just created, but not placed in the orderbook
  - `open`: order is successfully placed in the orderbook
  - `partial_entry`: internal entry order is partially filled
  - `partial_close`: internal target order is partially closed
  - `cancelled`: order is completely cancelled
  - `rejected`: order is rejected (not placed on the exchange)
  - `close`: order is completely filled
  - `triggered`: stop variant order triggered at specified stop price

<br/>

* `order_type`: used to denote the type of order to be placed. Valid values for order_type includes:
  - `market_order`: in this order type we don't specify price; it is executed on the market price
  - `limit_order`: in this order type we specify the price on which order is to be executed
  - `stop_limit`: it is a type of limit order whether we specify stop price and a price, once price reaches stop_price, order is placed on the given price
  - `take_profit`: it is a type of limit order whether we specify stop price and a price, once price reaches stop_price, order is placed on the given price

<br/>

* Other Terms:
  - `target_price`: The price at which the trader plans to buy/sell or close the order position is called the Target Price. When the Target price is hit, the trade is closed and the trader’s funds are settled according to the P&L incurred. Target price feature is available if the trader checks the Bracket order checkbox.
  - `sl_price`: The price at which the trader wishes to Stop Loss is the SL Price.
  - `stop_price`: It is used in the Stop Variant order, to specify stop price -->

### Margin Internal Orders

<!-- * `status` for internal orders: used to denote the type of internal orders. Valid values for order_type includes:
  - `initial`: order is just created
  - `open`: order is successfully placed in the orderbook
  - `partially_filled`: order is partially filled
  - `filled`: order is completely filled
  - `cancelled`: order is completely cancelled
  - `rejected`: order is rejected (not placed on the exchange)
  - `partially_cancelled`: order is partially cancelled
  - `untriggered`: stop variant order was not triggered
 -->

# Futures
## Get Trade Fee (Single Symbol)

> Response :

```json
{
  "symbol": "BTCUSDT",
  "makerFee": 0.001,
  "takerFee": 0.002
}
```

### HTTP Request
Retrieves trading fee details (maker/taker) for a specific trading symbol.

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `GET`                       |
| **Endpoint Path**| `/api/v1/exchange/tradefee` |
| **Auth Required**| No                          |
| **Query Params** | `symbol` (string, required) |
| **Request Body** | N/A                         |

### Success Response
| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](../../data-models.md#apiresponse) structure. The `data` field contains:

**`data`** (object):
<br>
- **`symbol`** (`string`): The trading symbol queried.
<br>
- **`makerFee`** (`number`): The maker fee rate.
<br>
- **`takerFee`** (`number`): The taker fee rate.

## Get Trade Fees (All Symbols)

> Response :

```json
[
  {
    "symbol": "BTCUSDT",
    "makerFee": 0.001,
    "takerFee": 0.002
  },
  {
    "symbol": "ETHUSDT",
    "makerFee": 0.0012,
    "takerFee": 0.0022
  }
]
```

### HTTP Request 
| Attribute       | Value                        |
| :-------------- | :--------------------------- |
| **HTTP Method** | `GET`                        |
| **Endpoint Path**| `/api/v1/exchange/tradefees` |
| **Auth Required**| No                           |
| **Query Params** | None                         |
| **Request Body** | N/A                          |

### Success Response
| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](../../data-models.md#apiresponse) structure. The `data` field contains:

**`data`** (object):
<br>
- **`symbol`** (`string`): The trading symbol queried.
<br>
- **`makerFee`** (`number`): The maker fee rate.
<br>
- **`takerFee`** (`number`): The taker fee rate.

## Get Exchange Info

> Response : 

```json
{
  "pairs": [
    {
      "name": "Bitcoin",
      "pair": "BTCUSDT",
      "orderTypes": ["MARKET", "LIMIT"],
      "filters": [ { "filterType": "LIMIT_QTY_SIZE", "maxQty": "100", "minQty": "0.0001" } ],
      "makerFee": 0.001,
      "takerFee": 0.002,
      "maxLeverage": 100,
      "pricePrecision": "2",
      "quantityPrecision": "4",
      "baseAsset": "BTC",
      "quoteAsset": "USDT",
      "marginAssetsSupported": ["USDT"]
      // ... other pair details
    }
    // ... other pairs
  ],
  "quoteCurrencies": [ { "code": "USDT", "name": "Tether" /* ... */ } ],
  "categories": ["Crypto", "Major"],
  "conversionRates": { /* ... conversion rates ... */ }
}
```

Retrieves comprehensive exchange configuration information, including trading rules, filters, limits, precision settings, supported assets, etc.

### Request
| Attribute       | Value                           |
| :-------------- | :------------------------------ |
| **HTTP Method** | `GET`                           |
| **Endpoint Path**| `/api/v1/exchange/exchangeInfo` |
| **Auth Required**| No                              |
| **Query Params** | None                            |
| **Request Body** | N/A                             |

### Success Response
| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](../../data-models.md#apiresponse) structure. The `data` field contains:

**`data`** ([ExchangeInfo](../../data-models.md#exchangeinfo) object) :
- Detailed configuration including available pairs, filters, fees, leverage options, supported currencies, conversion rates, etc. Refer to the Data Models section for the full structure.

## Get Pairs Info

Retrieves information about all available trading pairs, including status, assets, and icons.

> Response :

```json
{
  "pairs": [
    {
      "name": "Bitcoin",
      "pair": "BTCUSDT",
      "isActive": true,
      "iconURL": "[https://example.com/btc.png](https://www.google.com/search?q=https://example.com/btc.png)",
      "baseAsset": "BTC",
      "quoteAsset": "USDT",
      "marginAsset": "USDT"
    }
    // ... other pairs
  ],
  "types": ["FUNDING_FEE", "COMMISSION"],
  "quoteCurrencies": [ { "code": "USDT", "name": "Tether" /* ... */ } ],
  "categories": ["Crypto", "Major"],
  "conversionRates": { /* ... conversion rates ... */ }
}
```

### Request

| Attribute       | Value                      |
| :-------------- | :------------------------- |
| **HTTP Method** | `GET`                      |
| **Endpoint Path**| `/api/v1/exchange/pairs`  |
| **Auth Required**| No                         |
| **Query Params** | None                       |
| **Request Body** | N/A                        |

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](../../data-models.md#apiresponse) structure. The `data` field contains:

**`data`** ([PairsInfo](../../data-models.md#pairsinfo) object):
- Includes list of pairs with basic info (name, symbol, status, icon, assets), transaction types, quote currencies, categories, and conversion rates. Refer to the Data Models section for the full structure.

## Fetch Markets

Retrieves details about all available trading symbols (markets), including their status, precision, fees, and leverage limits.

> Response :

```json
{
  "timezone": "UTC",
  "serverTime": 1744364554387,
  "rateLimits": [],
  "exchangeFilters": [],
  "symbols": [
    {
      "symbol": "1000PEPEINR",
      "status": "Open",
      "maintMarginPercent": "15",
      "requiredMarginPercent": "0",
      "baseAsset": "1000PEPE",
      "quoteAsset": "INR",
      "pricePrecision": 5,
      "quantityPrecision": 0,
      "baseAssetPrecision": 0,
      "quotePrecision": 0,
      "orderTypes": [ "LIMIT", "MARKET" ],
      "timeInForce": [ "GTC" ],
      "makerFee": 0.05,
      "takerFee": 0.1,
      "minLeverage": 1,
      "maxLeverage": 10
    },
    {
      "symbol": "XRPINR",
      "status": "Open",
      "maintMarginPercent": "15",
      "requiredMarginPercent": "0",
      "baseAsset": "XRP",
      "quoteAsset": "INR",
      "pricePrecision": 2,
      "quantityPrecision": 1,
      // ... other fields ...
      "makerFee": 0.05,
      "takerFee": 0.1,
      "minLeverage": 1,
      "maxLeverage": 20
    }
    // ... other symbols
  ]
}
```

### Request

| Attribute         | Value                      |
|-------------------|----------------------------|
| **HTTP Method** | `GET`                      |
| **Endpoint Path** | `/api/v1/market/markets`  |
| **Auth Required** | No                         |
| **Query Params** | None                       |
| **Request Body** | N/A                        |

### Success Response

| Status Code | Description        |
|-------------|--------------------|
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](../../data-models.md#apiresponse) structure. The `data` field contains:

**`data`** ([MarketsData](../data-models.md#marketsdata) object):
- Includes timezone, server time, rate limits, and a list of symbol details.


## Get Order Book

Retrieves the current order book (bids and asks) for a specific trading symbol.

> Response :

```json
{
  "symbol": "BTCUSDT",
  "bids": [
    [65000.50, 0.5],
    [65000.10, 1.2],
    [64999.80, 0.8]
  ],
  "asks": [
    [65001.00, 0.3],
    [65001.50, 1.0],
    [65002.00, 0.7]
  ],
  "timestamp": 1712345678901,
  "datetime": "2025-04-05T11:59:38.901Z",
  "nonce": 123456789
}
```

### Request

| Attribute         | Value                        |
|-------------------|------------------------------|
| **HTTP Method**   | `GET`                        |
| **Endpoint Path** | `/api/v1/market/orderBook`   |
| **Auth Required** | No                           |
| **Query Params**  | `symbol` (string, required)  |
| **Request Body**  | N/A                          |

### Success Response

| Status Code | Description        |
|-------------|--------------------|
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](../../data-models.md#apiresponse) structure. The `data` field contains:

**`data`** ([OrderBook](../../data-models.md#orderbook) object):
- Includes arrays of bids and asks with price/quantity pairs.

## Get 24hr Ticker

Retrieves price change statistics for a specific trading symbol over the last 24 hours.

> Response :

```json
{
  "symbol": "BTCUSDT",
  "info": {
    "eventTimestamp": 1712345678905,
    "symbol": "BTCUSDT",
    "priceChange": "150.50",
    "priceChangePercentage": "0.23",
    "weightedAveragePrice": "65100.00",
    "lastPrice": "65150.50",
    "lastQuantityTraded": "0.01",
    "openPrice": "65000.00",
    "highestPrice": "65500.00",
    "lowestPrice": "64800.00",
    "totalTradedVolume": "1500.50",
    "totalTradedQuoteAssetVolume": "97682775.00",
    "startTime": 1712259278905,
    "endTime": 1712345678905,
    "firstTradeId": 987000,
    "lastTradeId": 989500,
    "numberOfTrades": 2501
  },
  "timestamp": 1712345678905,
  "datetime": "2025-04-05T11:59:38.905Z",
  "high": 65500.00,
  "low": 64800.00,
  "vwap": 65100.00,
  "open": 65000.00,
  "close": 65150.50,
  "last": 65150.50,
  "change": 150.50,
  "percentage": 0.23,
  "average": 65075.25,
  "baseVolume": 1500.50,
  "quoteVolume": 97682775.00
}
```

### Request

| Attribute         | Value                          |
|-------------------|---------------------------------|
| **HTTP Method**   | `GET`                          |
| **Endpoint Path** | `/api/v1/market/ticker24Hr`    |
| **Auth Required** | No                             |
| **Query Params**  | `symbol` (string, required)    |
| **Request Body**  | N/A                            |

### Success Response

| Status Code | Description        |
|-------------|--------------------|
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](../../data-models.md#apiresponse) structure. The `data` field contains:

**`data`** ([Ticker](../../data-models.md#ticker) object):
24-hour statistics including price change, high/low, volume, etc.

## Get Market Info

Retrieves high-level market information, potentially including metrics for multiple symbols.

> Response : 

```json
{
  "BTCUSDT": {
    "lastPrice": "65150.50",
    "marketPrice": "65150.00",
    "priceChangePercent": "0.23",
    "baseAssetVolume": "1500.50"
  },
  "ETHUSDT": {
    "lastPrice": "3300.10",
    "marketPrice": "3300.00",
    "priceChangePercent": "1.50",
    "baseAssetVolume": "25000.75"
  }
}
```

### Request

| Attribute         | Value                          |
|-------------------|---------------------------------|
| **HTTP Method**   | `GET`                          |
| **Endpoint Path** | `/api/v1/market/marketInfo`    |
| **Auth Required** | No                             |
| **Query Params**  | None                           |
| **Request Body**  | N/A                            |

### Success Response

| Status Code | Description        |
|-------------|--------------------|
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](../../data-models.md#apiresponse) structure. The `data` field contains:

**`data`** (object):
A mapping of symbols to their respective [MarketInfo](../../data-models.md#marketinfo) objects.

## Get Aggregate Trades

Retrieves recent aggregate trades for a specific trading symbol.

> Response :

```json
[
  {
    "aggregateTradeId": 543210,
    "symbol": "BTCINR",
    "price": "5500000.00",
    "quantity": "0.005",
    "firstTradeId": 98765,
    "lastTradeId": 98767,
    "tradeTime": 1712345000123,
    "isBuyerMarketMaker": false
  }
]
```

### Request

| Attribute         | Value                          |
|-------------------|---------------------------------|
| **HTTP Method**   | `GET`                          |
| **Endpoint Path** | `/api/v1/market/aggTrade`      |
| **Auth Required** | No                             |
| **Query Params**  | `symbol` (string, required)    |
| **Request Body**  | N/A                            |

### Success Response

| Status Code | Description        |
|-------------|--------------------|
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](../../data-models.md#apiresponse) structure. The `data` field contains:

**`data`** (Array<[AggregateTrade](../../data-models.md#aggregatetrade)> objects):
A list of recent aggregate trades.

## Get System Time

Retrieves the current API server time. Useful for synchronizing clocks or validating timestamps in client applications.

> Response :

```json
{
  "timestamp": 1712345678000
}
```

### Request

| Attribute         | Value              |
|-------------------|--------------------|
| **HTTP Method**   | `GET`              |
| **Endpoint Path** | `/api/v1/system/time` |
| **Auth Required** | No                 |
| **Query Params**  | None               |
| **Request Body**  | N/A                |

### Success Response

| Status Code | Description        |
|-------------|--------------------|
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](../../data-models.md#apiresponse) structure. The `data` field contains:

**`data`** (object):
- **`timestamp`** (`number`): Current server time in Unix timestamp (ms).

## Get System Status

Checks the operational status of the API system.

> Response :

```json
{
  "status": "ok"
}
```

### Request

| Attribute         | Value              |
|-------------------|--------------------|
| **HTTP Method**   | `GET`              |
| **Endpoint Path** | `/api/v1/system/status` |
| **Auth Required** | No                 |
| **Query Params**  | None               |
| **Request Body**  | N/A                |

### Success Response

| Status Code | Description        |
|-------------|--------------------|
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](../../data-models.md#apiresponse) structure. The `data` field contains:

**`data`** (object):
- **`status`** (`string`): Current system status. Possible values include `"ok"`, `"error"`.







