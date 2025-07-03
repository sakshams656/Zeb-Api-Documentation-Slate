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
    content: Documentation for the ZebPay API
---
# Terms and Conditions

These Terms govern your use of the Market Data and Application Programming Interface (API) provided by Zebpay. By accessing any Zebpay API or using Market Data or any related information, service, or feature governed by these Terms, you agree to be bound by them. These Terms apply to you, whether you are an individual, association, company, or any legal entity, and include your affiliates.
You acknowledge that these Terms may be updated, modified, or replaced periodically. It is your responsibility to review and comply with any changes to these Terms. Continued use of Zebpay APIs or Market Data constitutes acceptance of these Terms as updated.


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

# Terminology

# Data Models

## ApiResponse

This is the standard wrapper structure for most successful API responses. The actual data payload specific to the endpoint is contained within the `data` field.

> Response (Generic Structure) :

```json
{
  "statusDescription": "Success",
  "data": {
    // Specific data structure depends on the endpoint called
    // See examples for OrderBook, Ticker, etc., below
  },
  "statusCode": 200,
  "customMessage": ["OK"]
}
```

### Fields

| Field Name         | Type             | Description                                                                 |
|--------------------|------------------|-----------------------------------------------------------------------------|
| `statusDescription`| `string`         | Human-readable status description (e.g., "Success").                        |
| `data`             | `T`              | The core response data payload. Its structure varies (see specific models below). |
| `statusCode`       | `number`         | The HTTP status code returned by the API (e.g., 200, 201).                  |
| `customMessage`    | `Array<string>`  | Additional informational messages from the API.                           |

## Order

Represents a trading order, returned by `GET /api/v1/trade/order` and within lists from `GET /api/v1/trade/order/open-orders` and `GET /api/v1/trade/order/history`.

### Fields:

| Field Name      | Type                  | Description                                                       |
|-----------------|-----------------------|-------------------------------------------------------------------|
| `clientOrderId` | `string`              | Client-generated unique order identifier.                         |
| `datetime`      | `string`              | ISO8601 formatted datetime string of order creation.              |
| `timestamp`     | `number`              | Unix timestamp (ms) of order creation.                            |
| `symbol`        | `string`              | Trading pair symbol.                                              |
| `type`          | `string`              | Order type (e.g., 'MARKET', 'LIMIT').                               |
| `timeInForce`   | `string`              | Time in force policy (e.g., 'GTC', 'IOC', 'FOK').                   |
| `side`          | `string`              | Order side ('BUY' or 'SELL').                                       |
| `price`         | `number`              | Order price (can be 0 for market orders).                           |
| `amount`        | `number`              | Order amount in the base asset.                                    |
| `filled`        | `number`              | Amount of the order that has been filled.                           |
| `remaining`     | `number`              | Amount of the order remaining to be filled.                        |
| `status`        | `string` \| `undefined` | Order status (e.g., 'new', 'filled', 'canceled').                   |
| `reduceOnly`    | `boolean`             | Whether the order is reduce-only.                                  |
| `postOnly`      | `boolean`             | Whether the order is post-only.                                    |
| `average`       | `number` \| `undefined` | Average fill price if partially/fully filled.                       |
| `trades`        | `Array<object>` \| `undefined` | List of trade objects associated with filling this order.       |
| `info`          | `object` \| `undefined` | Raw order data object from the exchange.                           |

<!-- FUTURES - PUBLIC -->
# Futures - Public

## Base URL

The base URL for the Futures API is:

`https://futuresbe.zebpay.com`

## Get Trade Fee (Single Symbol)

Retrieves trading fee details (maker/taker) for a specific trading symbol.

> Response :

```json
{
  "symbol": "BTCUSDT",
  "makerFee": 0.001,
  "takerFee": 0.002
}
```

### HTTP Request

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

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

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

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

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

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `ExchangeInfo`

Represents detailed exchange configuration, returned within the `data` field of `GET /api/v1/exchange/exchangeInfo`. This is a complex object containing arrays and nested objects for trading rules, filters, leverage, fee tiers, and more.

### Top-Level Fields

| Field Name        | Type            | Description                                                                                                  |
|-------------------|-----------------|--------------------------------------------------------------------------------------------------------------|
| `pairs`           | `Array<object>` | List of objects detailing rules for each trading pair (filters, fees, leverage, precision, assets, etc.).      |
| `quoteCurrencies` | `Array<object>` | List of objects describing available quote currencies.                                                     |
| `categories`      | `Array<string>` | List of asset categories (e.g., "AI", "DeFi").                                                                 |
| `conversionRates` | `object`        | Object containing various currency conversion rates used by the exchange.         

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

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `PairsInfo`

Represents high-level metadata about all supported trading pairs, returned within the `data` field of `GET /api/v1/exchange/pairs`. Ideal for quickly listing active pairs, quote assets, and transaction types.

### Top-Level Fields

| Field Name        | Type            | Description                                                                       |
|-------------------|-----------------|-----------------------------------------------------------------------------------|
| `pairs`           | `Array<object>` | List of objects with basic info for each pair (name, symbol, status, icon, assets). |
| `types`           | `Array<string>` | List of available transaction types (e.g., "FUNDING_FEE", "COMMISSION").         |
| `quoteCurrencies` | `Array<object>` | List of objects describing available quote currencies.                          |
| `categories`      | `Array<string>` | List of asset categories.                                                         |
| `conversionRates` | `object`        | Object containing various currency conversion rates.                            |

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

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `MarketsData`

Represents the structure of the `data` field returned by `GET /api/v1/market/markets`. It contains exchange metadata and a list of symbols.

### Fields:

| Field Name        | Type                   | Description                                                  |
|-------------------|------------------------|--------------------------------------------------------------|
| `timezone`        | `string`               | Exchange timezone (e.g., "UTC").                             |
| `serverTime`      | `number`               | Current server time in Unix timestamp (ms).                  |
| `rateLimits`      | `Array<object>`        | List of rate limit rules applied by the exchange.            |
| `exchangeFilters` | `Array<object>`        | List of global exchange filters.                             |
| `symbols`         | `Array<MarketSymbol>` | List of available trading symbols and their details. See [`MarketSymbol`](#marketsymbol)      |

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

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `OrderBook`

Represents the market depth for a trading pair, returned within the `data` field of `GET /api/v1/market/orderBook`.

### Fields:

| Field Name | Type                      | Description                                                      |
|------------|---------------------------|------------------------------------------------------------------|
| `symbol`   | `string`                  | Trading pair symbol (e.g., "BTCUSDT").                           |
| `bids`     | `Array<[number, number]>` | Array of buy orders `[price, amount]`, sorted by price descending.|
| `asks`     | `Array<[number, number]>` | Array of sell orders `[price, amount]`, sorted by price ascending.|
| `timestamp`| `number` \| `null`        | Unix timestamp (ms) when the order book was generated.           |
| `datetime` | `string` \| `null`        | ISO8601 formatted datetime string.                               |
| `nonce`    | `number` \| `null`        | Exchange-provided sequence number, if available.                 |

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

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `Ticker`

Represents 24-hour market statistics for a trading pair, returned within the `data` field of `GET /api/v1/market/ticker24Hr`.

### Fields:

| Field Name   | Type     | Description                                                     |
|--------------|----------|-----------------------------------------------------------------|
| `symbol`     | `string` | Trading pair symbol.                                              |
| `info`       | `object` | Raw ticker data object from the exchange (see description below). |
| `timestamp`  | `number` | Unix timestamp in milliseconds.                                  |
| `datetime`   | `string` | ISO8601 formatted datetime string.                                |
| `high`       | `number` | Highest price in the 24-hour period.                              |
| `low`        | `number` | Lowest price in the 24-hour period.                               |
| `vwap`       | `number` | Volume-weighted average price.                                    |
| `open`       | `number` | Opening price.                                                    |
| `close`      | `number` | Closing price (last price).                                       |
| `last`       | `number` | Last traded price.                                                |
| `change`     | `number` | Price change in the period.                                       |
| `percentage` | `number` | Price change percentage.                                          |
| `average`    | `number` | Average price.                                                    |
| `baseVolume` | `number` | Trading volume in the base asset.                                 |
| `quoteVolume`| `number` | Trading volume in the quote asset.                                |

### `info` Object Fields (Common Examples): 

| Field Name                    | Type     | Description                                      |
|-------------------------------|----------|--------------------------------------------------|
| `eventTimestamp`              | `number` | Event timestamp in milliseconds.                 |
| `priceChange`                 | `string` | Price change as a string.                        |
| `priceChangePercentage`       | `string` | Price change percentage as a string.             |
| `weightedAveragePrice`        | `string` | Volume-weighted average price as a string.       |
| `lastPrice`                   | `string` | Last traded price as a string.                   |
| `lastQuantityTraded`          | `string` | Last traded quantity as a string.                |
| `openPrice`                   | `string` | Opening price as a string.                       |
| `highestPrice`                | `string` | Highest price in 24h as a string.                |
| `lowestPrice`                 | `string` | Lowest price in 24h as a string.                 |
| `totalTradedVolume`           | `string` | Total volume (base asset) as a string.           |
| `totalTradedQuoteAssetVolume` | `string` | Total volume (quote asset) as a string.          |
| `startTime`                   | `number` | Period start time (ms).                          |
| `endTime`                     | `number` | Period end time (ms).                            |
| `firstTradeId`                | `number` | First trade ID in the period.                    |
| `lastTradeId`                 | `number` | Last trade ID in the period.                     |
| `numberOfTrades`              | `number` | Total number of trades in the period.            |

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

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `MarketInfo`

Represents high-level market information for a trading pair, potentially returned by `GET /api/v1/market/marketInfo`. The `data` field often contains an object mapping symbols to these structures.

### Fields:

| Field Name           | Type     | Description                     |
|----------------------|----------|---------------------------------|
| `lastPrice`          | `string` | Last traded price.              |
| `marketPrice`        | `string` | Current market price.           |
| `priceChangePercent` | `string` | Price change percentage.        |
| `baseAssetVolume`    | `string` | Trading volume in base asset.   |

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

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `AggregateTrade`

Represents combined trades at the same price level, returned as an array within the `data` field of `GET /api/v1/market/aggTrade`.

### Fields:

| Field Name           | Type      | Description                                                     |
|----------------------|-----------|-----------------------------------------------------------------|
| `aggregateTradeId`   | `number`  | Unique identifier for the aggregate trade.                      |
| `symbol`             | `string`  | Trading pair symbol.                                            |
| `price`              | `string`  | Trade price.                                                    |
| `quantity`           | `string`  | Trade quantity.                                                 |
| `firstTradeId`       | `number`  | ID of the first trade included in the aggregate.                |
| `lastTradeId`        | `number`  | ID of the last trade included in the aggregate.                 |
| `tradeTime`          | `number`  | Timestamp of the trade in milliseconds.                         |
| `isBuyerMarketMaker` | `boolean` | Whether the buyer was the market maker.                         |

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

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

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

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

**`data`** (object):
- **`status`** (`string`): Current system status. Possible values include `"ok"`, `"error"`.

<!-- SPOT - PUBLIC -->
# Spot - Public

## Base URL

The base URL for the Spot API is:

`https://api.zebpay.com`

## Get All Tickers
Get 24-hour price and volume statistics for all trading pairs.

> Response : 

```json
[
  {
    "symbol": "BTC-INR",
    "bestBid": "7172080.69",
    "bestBidQty": "0.15471",
    "bestAsk": "7198376.05",
    "bestAskQty": "0.00161248",
    "priceChange": "-97725.008776",
    "priceChangePercent": "-1.36",
    "high": "7300000",
    "low": "6965026.32",
    "vol": "0.17943873",
    "volValue": "1289432.45",
    "last": "7185662.41"
  },
]
```

### Endpoint
`GET /api/v2/market/allTickers`

## Get Ticker
Get 24-hour price and volume statistics for a specific trading pair.

> Response :

```json
{
  "symbol": "BTC-INR",
  "bestBid": "7172080.69",
  "bestBidQty": "0.15471",
  "bestAsk": "7198376.05",
  "bestAskQty": "0.00161248",
  "priceChange": "-97725.008776",
  "priceChangePercent": "-1.36",
  "high": "7300000",
  "low": "6965026.32",
  "vol": "0.17943873",
  "volValue": "1289432.45",
  "last": "7185662.41"
}
```

### Endpoint
`GET /api/v2/market/ticker`

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| symbol | string | Yes | Trading pair symbol (e.g., BTC-INR) |

## Get Order Book
Get the current order book for a trading pair.

> Response :

```json
{
  "bids": [
    ["5499000", "0.5"],
    ["5498000", "1.2"]
  ],
  "asks": [
    ["5501000", "0.3"],
    ["5502000", "0.8"]
  ]
}
```

### Endpoint 
`GET /api/v2/market/orderbook`

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| symbol | string | Yes | Trading pair symbol |
| limit | number | No | Number of orders to return (default: 15) |

## Get Order Book Ticker
Get the best bid/ask price and quantity for a trading pair.

> Response :

```json
{
  "symbol": "BTC-INR",
  "bidPrice": "5499000",
  "bidQty": "0.5",
  "askPrice": "5501000",
  "askQty": "0.3"
}
```

### Endpoint
`GET /api/v2/market/orderbook/ticker`

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| symbol | string | Yes | Trading pair symbol |

## Get Recent Trades
Get recent trades for a trading pair.

> Response :

```json
[
  {
    "id": "123456",
    "price": "5500000",
    "quantity": "0.001",
    "time": "1612345678",
    "isBuyerMaker": true
  }
]
```

### Endpoint 
`GET /api/v2/market/trades`

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| symbol | string | Yes | Trading pair symbol |
| limit | number | No | Number of trades to return (default: 200) |
| page | number | No | Page number (default: 1) |

## Get Klines/Candlesticks
Get historical klines/candlesticks for a trading pair.

> Response : 

```json
[
  [
    1612345678000,  // Open time
    "5500000",      // Open
    "5600000",      // High
    "5400000",      // Low
    "5550000",      // Close
    "10.5",         // Volume
    1612345738000,  // Close time
    "57750000",     // Quote volume
    100,            // Number of trades
    "5.5",          // Taker buy base volume
    "30250000"      // Taker buy quote volume
  ]
]
```

### Endpoint
`GET /api/v2/market/klines`

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| symbol | string | Yes | Trading pair symbol |
| interval | string | Yes | Kline interval (1m, 5m, 15m, 1h, 4h, 1d) |
| startTime | number | No | Start time in milliseconds |
| endTime | number | No | End time in milliseconds |

## Get Service Status
Get the current system status.

> Response :

```json
{
    "remarks": "",
    "status": "open"
}
```

### Endpoint 
`GET /api/v2/status`

## Get Server Time
Get the current server time.

> Response :

```json
{
  "time": 1744361888858
}
```

### Endpoint
`GET /api/v2/time`

## Get Trading Pairs
Get information about all available trading pairs.

> Response :

```json
{
  "data": {
    "tradePairs": [
      {
        "symbol": "BTC-INR",
        "name": "BTC-INR",
        "baseCurrency": "BTC",
        "quoteCurrency": "INR",
        "feeCurrency": "INR",
        "baseMinSize": "",
        "quoteMinSize": "100",
        "baseMaxSize": "",
        "quoteMaxSize": "10000000",
        "baseIncrement": "0.00000001",
        "quoteIncrement": "0.01",
        "priceIncrement": 0.01,
        "enableTrading": true
      }
    ]
  },
  "statusCode": 200,
  "statusDescription": "Success",
}
```

### Endpoint 
`GET /api/v2/ex/tradepairs`

## Get Coin Settings
Get information about all available currencies.

> Response : 

```json
{
  "data": [
    {
      "currency": "BTC",
      "name": "BTC",
      "fullName": "Bitcoin",
      "precision": "8",
      "type": "crypto",
      "isDebitEnabled": false,
      "chains": [
          {
              "chainName": "Bitcoin",
              "withdrawalMinSize": "0.000482",
              "depositMinSize": "0.00000001",
              "withdrawalFee": "0.00040000",
              "isWithdrawEnabled": "true",
              "isDepositEnabled": "true",
              "contractAddress": "",
              "withdrawPrecision": "8",
              "maxWithdraw": "2.45060379000000",
              "maxDeposit": "100.00000000",
              "needTag": "false",
              "chainId": "bitcoin",
              "AddressRegex": "^(bc1([ac-hj-np-z02-9]{25,62}|[ac-hj-np-z02-9]{59})|1[a-km-zA-HJ-NP-Z1-9]{25,34}|3[a-km-zA-HJ-NP-Z1-9]{25,34})$"
          }
      ]
    }
  ],
  "statusCode": 200,
  "statusDescription": "Success",
}
``` 

### Endpoint
`GET /api/v2/ex/currencies`

# Authentication

To access private endpoints—such as those related to account balances, order management, or trading—you must authenticate your requests. Public endpoints (like order book, tickers, and exchange info) do **not** require authentication.

This API supports two authentication methods:

1. **JWT (JSON Web Token)**
2. **API Key + Secret Key**

You may authenticate requests using either method, depending on your application setup.

## 1. JWT Authentication

JWTs are self-contained tokens that securely represent a user or session.

```http
Authorization: Bearer <your_jwt_token>
```

> Example cURL Request :

```bash
curl -X GET 'https://api.zebapi.com/api/v1/wallet/balance' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...' \
  -H 'Accept: application/json'
```

> ✅ **Tip:** Use JWT when you're authenticating user sessions in web or mobile applications.

### How It Works

1. Obtain a valid JWT token (usually via login or an authentication flow).
2. For every request to a private endpoint, attach the token to the `Authorization` header:



## 2. API Key + Secret Key Authentication

This method is ideal for programmatic access or server-to-server integrations.

### How It Works

Each request to a private endpoint must be signed with an `HMAC-SHA256` signature using your `Secret Key`. The signature is validated server-side to ensure integrity and authenticity.

### Required Headers

| Header              | Value                             |
|---------------------|------------------------------------|
| `x-auth-apikey`     | Your API Key                       |
| `x-auth-signature`  | HMAC-SHA256 signature              |
| `Content-Type`      | `application/json`                 |
| `Accept`            | `application/json`                 |

## Manual HTTP Authentication

This section is for developers not using the sample client libraries.

> 💡 **Recommendation:** Use the [Node.js Client](https://github.com/zebpay/zebpay-api-references/tree/main/futures/clients/rest-http/node/README.md) or [Python Client](https://github.com/zebpay/zebpay-api-references/tree/main/futures/clients/rest-http/python/README.md) to avoid handling these steps manually.

### Manual JWT Authentication

1. **Obtain JWT Token**
2. **Add Header**

```http
Authorization: Bearer <your_jwt_token>
```

That’s it. This header must be included in all authenticated requests.

### Manual API Key + Secret Authentication

Here’s a step-by-step guide:

### Step 1: Retrieve your credentials
- `API Key`
- `Secret Key`

> 🔒 Keep your `Secret Key` safe! Never expose it publicly.

### Step 2: Generate `timestamp`

Use the current Unix timestamp **in milliseconds**.

- JavaScript: ``Date.now()``
- Python: ``int(time.time() * 1000)``

Let’s call this value `timestamp`.

> 🕒 Ensure your system clock is reasonably accurate. Small clock drifts may cause signature errors or request rejections.

### Step 3: Construct `dataToSign`

- For **GET** requests:
    - Append `timestamp` to your query parameters
    - Example:
      ```
      symbol=BTCUSDT&timestamp=1712345678901
      ```
    - This full query string becomes your `dataToSign`.

- For **POST/PUT/DELETE** requests:
    - Add `timestamp` to the root level of your JSON body.
    - Serialize the full object into a **compact** JSON string (no extra whitespace).
    - This JSON string becomes your `dataToSign`.

### Step 4: Generate HMAC Signature

Use `HMAC-SHA256` with your `Secret Key` and the `dataToSign` from Step 3.

- Output should be a **lowercase hexadecimal** string.
- Let’s call this the `signature`.

Example (Node.js):

```javascript
const crypto = require('crypto');

function generateSignature(secret, dataToSign) {
  return crypto.createHmac('sha256', secret)
    .update(dataToSign)
    .digest('hex');
}
```

### Step 5: Add headers to your HTTP request

```http
x-auth-apikey: <your_api_key>
x-auth-signature: <signature>
Content-Type: application/json
Accept: application/json
```

> Example (Signed GET Request) :

```bash
curl -X GET "https://api.zebapi.com/api/v1/trade/history?symbol=BTCUSDT&timestamp=1712345678901" \
  -H "x-auth-apikey: YOUR_API_KEY" \
  -H "x-auth-signature: abcdef1234567890deadbeef..." \
  -H "Accept: application/json"
```

> Example (Signed POST Request) :

```bash
curl -X POST "https://api.zebapi.com/api/v1/trade/order" \
  -H "x-auth-apikey: YOUR_API_KEY" \
  -H "x-auth-signature: abcdef1234567890deadbeef..." \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
        "symbol": "BTCUSDT",
        "type": "LIMIT",
        "side": "BUY",
        "price": 65000,
        "amount": 0.01,
        "timestamp": 1712345678901
      }'
```

## Security Best Practices

- ✅ **Use environment variables** to store API keys and secrets.
- ❌ **Never hardcode secrets** in source files or commit them to Git.
- 🔐 **Restrict API Key permissions** to only the scopes you need.
- 🔄 **Rotate keys regularly** and revoke any that may be compromised.

# Futures - Private

These endpoints allow users to manage orders, positions, leverage, and view trade-related history. **Authentication (JWT or API Key/Secret) is required.**

## Create Order

Places a new trading order.

> Response :

```json
{
  "clientOrderId": "myNewOrder123",
  "datetime": "2025-04-05T13:10:00.123Z",
  "timestamp": 1712346600123,
  "symbol": "BTCUSDT",
  "type": "MARKET",
  "timeInForce": "GTC",
  "side": "BUY",
  "price": 0, // Market order price is determined at execution
  "amount": 0.001,
  "filled": 0.001, // Example if filled immediately
  "remaining": 0,
  "reduceOnly": false,
  "postOnly": false,
  "status": "filled" // Example status
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `POST`                      |
| **Endpoint Path**| `/api/v1/trade/order`       |
| **Auth Required**| Yes                         |
| **Query Params** | None                        |
| **Request Body** | (object, required) See below |

**Request Body Parameters:**

-   **`symbol`** (`string`, required): Trading symbol (e.g., "BTCUSDT") .
-   **`amount`** (`number`, required): Order quantity in base asset .
-   **`side`** (`string`, required): `"BUY"` or `"SELL"` .
-   **`type`** (`string`, required): `"MARKET"` or `"LIMIT"` .
-   **`marginAsset`** (`string`, required): Asset used for margin (e.g., "USDT") .
-   `price` (`number`, optional): Required if `type` is `"LIMIT"`. Must be positive .
-   `stopLossPrice` (`number`, optional): Trigger price for stop-loss .
-   `takeProfitPrice` (`number`, optional): Trigger price for take-profit .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. | *(Note: Some APIs might return 201 Created)* |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `CreateOrderResponseData`

Response from `POST /api/v1/trade/order`. This structure is also used as the base for responses when adding TP/SL or closing positions.

* **Structure:** Generally inherits fields from the [Order](#order) model, representing the details of the newly created order. Key fields typically include `clientOrderId`, `timestamp`, `symbol`, `type`, `side`, `price`, `amount`, `status`, etc.

## Cancel Order

> Response :

```json
{
  "clientOrderId": "myLimitOrder456",
  "status": "canceled",
  "symbol": "BTCUSDT",
  "info": {
    "clientOrderId": "myLimitOrder456",
    "orderId": "987654321",
    "status": "CANCELED",
    "success": true
  }
}
```

Cancels an existing open order.

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `DELETE`                    |
| **Endpoint Path**| `/api/v1/trade/order`       |
| **Auth Required**| Yes                         |
| **Query Params** | None                        |
| **Request Body** | (object, required) See below |

**Request Body Parameters:**

-   **`clientOrderId`** (`string`, required): The client-generated ID of the order to cancel .
-   `symbol` (`string`, optional): Trading symbol .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `CancelOrderResponseData`

Response from `DELETE /api/v1/trade/order`. This has a unique structure.

### Fields:

| Field Name      | Type     | Description                                                    |
|-----------------|----------|----------------------------------------------------------------|
| `clientOrderId` | `string` | The ID of the order requested for cancellation.              |
| `status`        | `string` | Status confirming cancellation (e.g., "canceled").             |
| `symbol`        | `string` | Trading pair symbol.                                           |
| `info`          | `object` | Raw cancellation response data from the exchange (may include `orderId`, `success`). |

## Get Order Details

Fetches details of a specific order using its client order ID.

> Response :

```json
{
  "clientOrderId": "myLimitOrder456",
  "datetime": "2025-04-05T13:05:00.000Z",
  "timestamp": 1712346300000,
  "symbol": "BTCUSDT",
  "type": "LIMIT",
  "timeInForce": "GTC",
  "side": "SELL",
  "price": 66000.00,
  "amount": 0.1,
  "filled": 0,
  "remaining": 0.1,
  "status": "canceled", // Status updated after cancellation
  "reduceOnly": false,
  "postOnly": false
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `GET`                       |
| **Endpoint Path**| `/api/v1/trade/order`       |
| **Auth Required**| Yes                         |
| **Query Params** | `id` (string, required)     |
| **Request Body** | N/A                         |

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

**`data`** ([Order](#order) object) :
- Detailed information about the requested order.

## Add TP/SL to Position

Adds Take Profit (TP) and/or Stop Loss (SL) orders to an existing position.

> Response :

```json
{
  "clientOrderId": "tpOrder789",
  "datetime": "2025-04-05T13:15:00.987Z",
  "timestamp": 1712346900987,
  "symbol": "BTCUSDT",
  "type": "TAKE_PROFIT_MARKET", // Example type
  "timeInForce": "GTC",
  "side": "SELL", // Assuming added to a long position
  "price": 67000.00, // The trigger price
  "amount": 0.05, // Amount to close
  "filled": 0,
  "remaining": 0.05,
  "reduceOnly": true, // TP/SL orders are typically reduce-only
  "postOnly": false,
  "status": "new"
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `POST`                      |
| **Endpoint Path**| `/api/v1/trade/order/addTPSL`  |
| **Auth Required**| Yes                         |
| **Query Params** | None                        |
| **Request Body** | (object, required) See below |

**Request Body Parameters:**

-   **`positionId`** (`string`, required): Identifier of the position .
-   **`amount`** (`number`, required): Order amount .
-   **`side`** (`string`, required): Order side (`"BUY"` or `"SELL"`) .
-   `symbol` (`string`, optional): Trading symbol .
-   `stopLossPrice` (`number`, optional): Trigger price for stop-loss. At least one of `stopLossPrice` or `takeProfitPrice` is required .
-   `takeProfitPrice` (`number`, optional): Trigger price for take-profit. At least one of `stopLossPrice` or `takeProfitPrice` is required .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `AddTPSLResponseData`

Response from `POST /api/v1/trade/order/addTPSL`.

* **Structure:** This is an alias for [CreateOrderResponseData](#createorderresponsedata). Refer to the `CreateOrderResponseData` definition for fields and examples.

## Add Margin to Position

Adds margin to an existing isolated margin position.

> Response :

```json
{
  "info": {
    "lockedBalance": 150.50, // Example updated balance
    "withdrawableBalance": 8350.00, // Example updated balance
    "asset": "USDT",
    "message": "Margin added successfully"
  },
  "type": "add",
  "amount": 100.0,
  "code": "USDT",
  "symbol": "BTCUSDT",
  "status": "ok" // Example status
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `POST`                      |
| **Endpoint Path**| `/api/v1/trade/addMargin`    |
| **Auth Required**| Yes                         |
| **Query Params** | None                        |
| **Request Body** | (object, required) See below |

**Request Body Parameters:**

-   **`positionId`** (`string`, required): Identifier of the position .
-   **`amount`** (`number`, required): Amount of margin to add .
-   `symbol` (`string`, optional): Trading symbol .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `MarginResponse`

Response from `POST /api/v1/trade/addMargin` and `POST /api/v1/trade/reduceMargin`.

### Fields:

| Field Name | Type     | Description                                                      |
|------------|----------|------------------------------------------------------------------|
| `info`     | `object` | Raw margin operation data (may include `lockedBalance`, `withdrawableBalance`, `asset`, `message`). |
| `type`     | `string` | Type of operation ('add' or 'reduce').                           |
| `amount`   | `number` | Amount of margin added/reduced.                                  |
| `code`     | `string` | Asset/currency code.                                             |
| `symbol`   | `string` | Trading pair symbol.                                             |
| `status`   | `string` | Status of the margin operation (e.g., "ok").                     |


## Reduce Margin from Position

Reduces margin from an existing isolated margin position.

> Response : 

```json
{
  "info": {
    "lockedBalance": 100.50, // Example updated balance
    "withdrawableBalance": 8400.00, // Example updated balance
    "asset": "USDT",
    "message": "Margin reduced successfully"
  },
  "type": "reduce",
  "amount": 50.0,
  "code": "USDT",
  "symbol": "BTCUSDT",
  "status": "ok" // Example status
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `POST`                      |
| **Endpoint Path**| `/api/v1/trade/reduceMargin` |
| **Auth Required**| Yes                         |
| **Query Params** | None                        |
| **Request Body** | (object, required) See below |

**Request Body Parameters:**

-   **`positionId`** (`string`, required): Identifier of the position .
-   **`amount`** (`number`, required): Amount of margin to reduce .
-   `symbol` (`string`, optional): Trading symbol .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

**`data`** ([MarginResponse](#marginresponse) object) :
- Details confirming the margin reduction and potentially updated balances.

## Close Position

Closes an existing open position using a market order.

> Response :

```json
{
  "clientOrderId": "closePosMarket1122",
  "datetime": "2025-04-05T13:20:00.456Z",
  "timestamp": 1712347200456,
  "symbol": "BTCUSDT",
  "type": "MARKET",
  "timeInForce": "IOC", // Often Immediate-Or-Cancel for close orders
  "side": "SELL", // Assuming closing a long position
  "price": 0,
  "amount": 0.05, // Amount closed
  "filled": 0.05,
  "remaining": 0,
  "reduceOnly": true, // Close orders are inherently reduce-only
  "postOnly": false,
  "status": "filled"
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `POST`                      |
| **Endpoint Path**| `/api/v1/trade/position/close`  |
| **Auth Required**| Yes                         |
| **Query Params** | None                        |
| **Request Body** | (object, required) See below |

**Request Body Parameters:**

-   **`positionId`** (`string`, required): Identifier of the position to close .
-   `symbol` (`string`, optional): Trading symbol .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `ClosePositionResponseData`

Response from `POST /api/v1/trade/position/close`.

* **Structure:** This is an alias for [CreateOrderResponseData](#createorderresponsedata), as closing a position effectively creates a market order. Refer to the `CreateOrderResponseData` definition for fields and examples.

## Get Open Orders

Retrieves a list of the user's currently open orders, optionally filtered by symbol.

> Response

```json
{
  "data": [
    {
      "clientOrderId": "myOpenLimitOrder789",
      "datetime": "2025-04-05T13:18:00.000Z",
      "timestamp": 1712347080000,
      "symbol": "BTCUSDT",
      "type": "LIMIT",
      "timeInForce": "GTC",
      "side": "BUY",
      "price": 64000.00,
      "amount": 0.02,
      "filled": 0,
      "remaining": 0.02,
      "status": "new",
      "reduceOnly": false,
      "postOnly": false
    }
    // ... potentially more open orders
  ],
  "totalCount": 1, // Example count
  "nextTimestamp": null // Example if no more pages
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `GET`                       |
| **Endpoint Path**| `/api/v1/trade/order/open-orders` |
| **Auth Required**| Yes                         |
| **Query Params** | See below                   |
| **Request Body** | N/A                         |

### Query Parameters:

-   **`symbol`** (`string`, required): Trading symbol .
-   `limit` (`number`, optional): Maximum number of orders to return .
-   `since` (`number`, optional): Fetch orders created after this Unix timestamp (ms) .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `OrdersListResponse`

Response from `GET /api/v1/trade/order/open-orders` and `GET /api/v1/trade/order/history`.

### Fields:

| Field Name      | Type             | Description                                                           |
|-----------------|------------------|-----------------------------------------------------------------------|
| `data`          | `Array<Order>`   | List of [Order](#order) objects.                                      |
| `totalCount`    | `number`         | Total number of records matching the query (may not always be present/accurate). |
| `nextTimestamp` | `number`\|`null`  | Timestamp for fetching the next page (pagination cursor).             |

- A list of open [Order](../../data-models.md#order) objects, potentially with pagination info.

## Get Positions

Retrieves a list of the user's current positions, optionally filtered by symbols or status.

> Response :

```json
[
  {
    "id": "pos-btc-long-123",
    "symbol": "BTCUSDT",
    "timestamp": 1712347500000,
    "datetime": "2025-04-05T13:25:00.000Z",
    "side": "long", // or "buy"
    "contracts": 0.05,
    "contractSize": 1, // Assuming contract size is 1 BTC
    "entryPrice": 65100.00,
    "notional": 3255.00,
    "leverage": 10,
    "initialMargin": 325.50,
    "liquidationPrice": 59000.00, // Example
    "marginMode": "isolated",
    "status": "OPEN"
  }
]
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `GET`                       |
| **Endpoint Path**| `/api/v1/trade/positions`    |
| **Auth Required**| Yes                         |
| **Query Params** | See below                   |
| **Request Body** | N/A                         |

### Query Parameters:

-   `symbols` (`Array<string>`, optional): List of trading symbols to filter by .
-   `status` (`string`, optional): Filter by status (`"OPEN"`, `"CLOSED"`, `"LIQUIDATED"`) .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `Position`

Represents a futures trading position, returned within arrays from `GET /api/v1/trade/positions`.

### Fields:

| Field Name         | Type                 | Description                                                      |
|--------------------|----------------------|------------------------------------------------------------------|
| `id`               | `string`             | Unique position identifier.                                      |
| `symbol`           | `string`             | Trading pair symbol.                                             |
| `timestamp`        | `number`             | Unix timestamp (ms) when the position data was generated.        |
| `datetime`         | `string`             | ISO8601 formatted datetime string.                               |
| `side`             | `string`             | Position side ('buy'/'long' or 'sell'/'short').                    |
| `contracts`        | `number`             | Size of the position in contracts.                               |
| `contractSize`     | `number`             | Size of one contract in the base asset.                          |
| `entryPrice`       | `number`             | Average entry price of the position.                             |
| `notional`         | `number`             | Notional value of the position.                                  |
| `leverage`         | `number`             | Leverage used for this position.                                 |
| `initialMargin`    | `number`             | Initial margin requirement.                                      |
| `liquidationPrice` | `number`             | Estimated liquidation price.                                     |
| `marginMode`       | `string`             | Margin mode ('isolated' or 'cross').                               |
| `status`           | `string` \| `undefined` | Position status (e.g., 'OPEN', 'CLOSED', 'LIQUIDATED').            |

## Get User Leverage (Single Symbol)

Retrieves the user's leverage setting for a specific trading symbol.

> Response :

```json
{
  "symbol": "BTCUSDT",
  "marginMode": "isolated",
  "longLeverage": 10,
  "shortLeverage": 10,
  "info": {
    "contractName": "BTCUSDT",
    "leverage": 10,
    "openPositionCount": 1
  }
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `GET`                       |
| **Endpoint Path**| `/api/v1/trade/userLeverage` |
| **Auth Required**| Yes                         |
| **Query Params** | `symbol` (string, required)  |
| **Request Body** | N/A                         |

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `Leverage`

Represents user leverage settings for a trading pair, returned by leverage-related endpoints.

### Fields:

| Field Name      | Type     | Description                                                      |
|-----------------|----------|------------------------------------------------------------------|
| `symbol`        | `string` | Trading pair symbol.                                             |
| `marginMode`    | `string` | Margin mode ('isolated' or 'cross').                             |
| `longLeverage`  | `number` | Leverage setting for long positions.                             |
| `shortLeverage` | `number` | Leverage setting for short positions.                            |
| `info`          | `object` | Raw leverage data from the exchange (may contain `leverage`, `updatedLeverage`, etc.). |

## Get All User Leverages

Retrieves the user's leverage settings for all symbols.

> Response :

```json
[
  {
    "symbol": "BTCUSDT",
    "marginMode": "isolated",
    "longLeverage": 10,
    "shortLeverage": 10,
    "info": { /* ... */ }
  },
  {
    "symbol": "ETHUSDT",
    "marginMode": "isolated",
    "longLeverage": 20,
    "shortLeverage": 20,
    "info": { /* ... */ }
  }
]
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `GET`                       |
| **Endpoint Path**| `/api/v1/trade/userLeverages` |
| **Auth Required**| Yes                         |
| **Query Params** | None                        |
| **Request Body** | N/A                         |

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

**`data`** (Array<[Leverage](#leverage)> object) :
- A list of leverage settings for all symbols configured by the user.

## Update User Leverage

Updates the user's leverage setting for a specific symbol.

> Response :

```json
{
  "symbol": "BTCUSDT",
  "marginMode": "isolated",
  "longLeverage": 25, // Newly updated value
  "shortLeverage": 25, // Newly updated value
  "info": {
    "contractName": "BTCUSDT",
    "updatedLeverage": 25, // Field name might vary in actual response
    "openPositionCount": 1
  }
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `POST`                      |
| **Endpoint Path**| `/api/v1/trade/update/userLeverage` |
| **Auth Required**| Yes                         |
| **Query Params** | None                        |
| **Request Body** | (object, required) See below |

**Request Body Parameters:**

-   **`symbol`** (`string`, required): Trading symbol .
-   **`leverage`** (`number`, required): The new desired leverage value .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

**`data`** ([Leverage](#leverage) object) :
- The updated leverage settings for the symbol.

## Get Order History

Retrieves the user's historical orders with pagination.

> Response :

```json
{
  "data": [
    {
      "clientOrderId": "myMarketOrder111",
      "datetime": "2025-04-04T10:00:00.000Z",
      "timestamp": 1712240400000,
      "symbol": "BTCUSDT",
      "type": "MARKET",
      // ... other order fields ...
      "status": "filled"
    },
     {
      "clientOrderId": "myLimitOrder222",
      "datetime": "2025-04-04T11:00:00.000Z",
      "timestamp": 1712244000000,
      "symbol": "ETHUSDT",
      "type": "LIMIT",
      // ... other order fields ...
      "status": "canceled"
    }
  ],
  "totalCount": 50, // Example total
  "nextTimestamp": 1712240399999 // Example pagination cursor
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `GET`                       |
| **Endpoint Path**| `/api/v1/trade/order/history`  |
| **Auth Required**| Yes                         |
| **Query Params** | See below                   |
| **Request Body** | N/A                         |

### Query Parameters:

-   `pageSize` (`number`, optional): Number of orders per page .
-   `timestamp` (`number`, optional): Fetch orders created before this Unix timestamp (ms) .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `OrdersListResponse`

Response from `GET /api/v1/trade/order/open-orders` and `GET /api/v1/trade/order/history`.

### Fields:

| Field Name      | Type             | Description                                                           |
|-----------------|------------------|-----------------------------------------------------------------------|
| `data`          | `Array<Order>`   | List of [Order](#order) objects.                                      |
| `totalCount`    | `number`         | Total number of records matching the query (may not always be present/accurate). |
| `nextTimestamp` | `number`\|`null`  | Timestamp for fetching the next page (pagination cursor).             |

## Get Trade History

Retrieves the user's historical trades with pagination.

> Response :

```json
{
  "data": [
    {
      "id": "trade123",
      "timestamp": 1712240400500,
      "datetime": "2025-04-04T10:00:00.500Z",
      "symbol": "BTCUSDT",
      "order": "orderLink111",
      "type": "market",
      "side": "buy",
      "takerOrMaker": "taker",
      "price": 65050.00,
      "amount": 0.01,
      "cost": 650.50,
      "fee": { "cost": 0.6505, "currency": "USDT" },
      "info": { /* ... raw exchange data ... */ }
    }
    // ... other trades
  ],
  "totalCount": 120, // Example total
  "nextTimestamp": 1712240400499 // Example pagination cursor
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `GET`                       |
| **Endpoint Path**| `/api/v1/trade/history`      |
| **Auth Required**| Yes                         |
| **Query Params** | See below                   |
| **Request Body** | N/A                         |

### Query Parameters:

-   `pageSize` (`number`, optional): Number of trades per page .
-   `timestamp` (`number`, optional): Fetch trades executed before this Unix timestamp (ms) .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `TradesListResponse`

Response from `GET /api/v1/trade/history`.

### Fields:

| Field Name      | Type             | Description                                              |
|-----------------|------------------|----------------------------------------------------------|
| `data`          | `Array<Trade>`   | List of [Trade](#trade) objects.                         |
| `totalCount`    | `number`         | Total number of records matching the query.            |
| `nextTimestamp` | `number`\|`null`  | Timestamp for fetching the next page.                  |

### `Trade`

Represents a completed trade execution, returned in lists from `GET /api/v1/trade/history`.

### Fields:

| Field Name     | Type     | Description                                                     |
|----------------|----------|-----------------------------------------------------------------|
| `id`           | `string` | Unique trade identifier.                                          |
| `timestamp`    | `number` | Unix timestamp in milliseconds.                                 |
| `datetime`     | `string` | ISO8601 formatted datetime string.                                |
| `symbol`       | `string` | Trading pair symbol.                                              |
| `order`        | `string` | Identifier of the order associated with this trade.               |
| `type`         | `string` | Order type that resulted in this trade (e.g., 'market').            |
| `side`         | `string` | Trade side ('buy' or 'sell').                                     |
| `takerOrMaker` | `string` | Whether the user was the taker or maker.                          |
| `price`        | `number` | Execution price of the trade.                                     |
| `amount`       | `number` | Amount traded in the base asset.                                  |
| `cost`         | `number` | Total cost of the trade in the quote asset (price * amount).        |
| `fee`          | `object` | Object detailing the fee paid.                                    |
| `  cost`       | `number` | Amount of the fee.                                                |
| `  currency`   | `string` | Currency the fee was paid in.                                     |
| `info`         | `object` | Raw trade data object from the exchange.                          |

## Get Transaction History

Retrieves the user's historical wallet transactions (fees, funding, etc.) with pagination.

> Response :

```json
{
  "data": [
    {
      "txid": "txn-fee-abc",
      "timestamp": 1712154000000,
      "datetime": "2025-04-03T12:00:00.000Z",
      "type": "COMMISSION",
      "amount": -0.6505,
      "currency": "USDT",
      "status": "completed",
      "fee": {}, // Fee might be implicit in amount for type=COMMISSION
      "info": { /* ... raw exchange data ... */ }
    },
    {
      "txid": "txn-funding-def",
      "timestamp": 1712067600000,
      "datetime": "2025-04-02T12:00:00.000Z",
      "type": "FUNDING_FEE",
      "amount": -1.2345,
      "currency": "USDT",
      "status": "completed",
      "fee": {},
      "info": { /* ... raw exchange data ... */ }
    }
  ],
  "totalCount": 35, // Example total
  "nextTimestamp": 1712067599999 // Example pagination cursor
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `GET`                       |
| **Endpoint Path**| `/api/v1/trade/transaction/history`  |
| **Auth Required**| Yes                         |
| **Query Params** | See below                   |
| **Request Body** | N/A                         |

### Query Parameters:

-   `pageSize` (`number`, optional): Number of transactions per page .
-   `timestamp` (`number`, optional): Fetch transactions before this Unix timestamp (ms) .

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `TransactionsListResponse`

Response from `GET /api/v1/trade/transaction/history`.

### Fields:

| Field Name      | Type                  | Description                                               |
|-----------------|-----------------------|-----------------------------------------------------------|
| `data`          | `Array<Transaction>`  | List of [Transaction](#transaction) objects.              |
| `totalCount`    | `number`              | Total number of records matching the query.             |
| `nextTimestamp` | `number`\|`null`       | Timestamp for fetching the next page.                     |

### `Transaction`

Represents a wallet transaction (e.g., fee, funding), returned in lists from `GET /api/v1/trade/transaction/history`.

### Fields:

| Field Name | Type     | Description                                                     |
|------------|----------|-----------------------------------------------------------------|
| `txid`     | `string` | Unique transaction identifier.                                   |
| `timestamp`| `number` | Unix timestamp in milliseconds.                                  |
| `datetime` | `string` | ISO8601 formatted datetime string.                                 |
| `type`     | `string` | Type of transaction (e.g., 'COMMISSION', 'FUNDING_FEE').           |
| `amount`   | `number` | Amount of the transaction (can be negative).                       |
| `currency` | `string` | Currency of the transaction.                                       |
| `status`   | `string` | Status of the transaction.                                         |
| `fee`      | `object` | Object detailing any fee associated (often implicit in amount).      |
| `info`     | `object` | Raw transaction data from the exchange.                            |

## Get Wallet Balance

Retrieves the user's balances for all assets in their wallet.

> Response :

```json
{
  "USDT": {
    "total": 10000.50,
    "free": 8500.25,
    "used": 1500.25
  },
  "BTC": {
    "total": 0.5,
    "free": 0.2,
    "used": 0.3
  }
}
```

### Request

| Attribute       | Value                       |
| :-------------- | :-------------------------- |
| **HTTP Method** | `GET`                       |
| **Endpoint Path**| `/api/v1/wallet/balance` [cite: node/utils/config.js]   |
| **Auth Required**| Yes                         |
| **Query Params** | None                        |
| **Request Body** | N/A                         |

### Success Response

| Status Code | Description      |
| :---------- | :--------------- |
| `200 OK`    | Request succeeded. |

The response follows the standard [ApiResponse](#apiresponse) structure. The `data` field contains:

### `WalletBalance`

Represents the user's wallet balances, returned within the `data` field of `GET /api/v1/wallet/balance`. It's typically an object mapping asset symbols to `AssetBalance` objects.

### Structure:

- Keys are asset symbols (`string`, e.g., "USDT", "BTC").
- Values are [AssetBalance](#assetbalance) objects (defined below).

### `AssetBalance`

Represents the balance for a single asset, used within the `WalletBalance` object.

### Fields:

| Field Name | Type     | Description                                                    |
|------------|----------|----------------------------------------------------------------|
| `total`    | `number` | Total balance of the asset.                                    |
| `free`     | `number` | Available balance for trading or withdrawals.                  |
| `used`     | `number` | Balance currently reserved for open orders or positions (frozen).|


# Spot - Private 

## Get Account Balance
Get the balance for specific assets or all assets.

> Response :

```json
{
  "data": [
    {
        "currency": "BTC",
        "balance": "1.14527466",
        "available": "1.14237653",
        "locked": "0.00289813",
        "lockedInExchange": "0",
        "lockedInQt": "0",
        "lockedInLending": "0.0021982",
        "lockedInTransfers": "0.00069993",
        "isFiat": false,
        "cryptoPackBalance": "0.06670147",
        "updatedAt": 1744362355956
    },
    {
        "currency": "INR",
        "balance": "7383376.46",
        "available": "7365026.44",
        "locked": "18350.02",
        "lockedInExchange": "0",
        "lockedInQt": "0",
        "lockedInLending": "0",
        "lockedInTransfers": "18350.02",
        "isFiat": true,
        "cryptoPackBalance": "0",
        "updatedAt": 1744362355956
    }
  ],
  "statusCode": 200,
  "statusDescription": "Success"
}
```

### Endpoint 

`GET /api/v2/account/balance`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token or API key authentication |
| X-AUTH-APIKEY | Yes | Required for API key authentication |
| X-AUTH-SIGNATURE | Yes | Required for API key authentication |

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| symbol | string | No | Trading pair symbol |
| currencies | string | No | Comma-separated list of currencies |

## Place New Order

Place a new order.

> Response :

```json
{
  "data": {
      "orderId": 6016691,
      "price": "5333400",
      "side": "BUY",
      "quantity": "0.0001",
      "status": "OPEN",
      "symbol": "BTC-INR",
      "type": "LIMIT",
      "createdAt": 1744362483471
  },
  "statusCode": 200,
  "statusDescription": "Success"
}
```

### Endpoint
 
`POST /api/v2/ex/orders`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token or API key authentication |
| X-AUTH-APIKEY | Yes | Required for API key authentication |
| X-AUTH-SIGNATURE | Yes | Required for API key authentication |

### Request Body

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| symbol | string | Yes | Trading pair symbol |
| side | string | Yes | Order side (BUY/SELL) |
| type | string | Yes | Order type (LIMIT/MARKET/STOP_LIMIT)|
| price | string | No | Order price |
| quantity | string | No | Order quantity |
| quoteOrderQty | string | No | Quote order quantity |
| stopPrice | string | No | Stop price |
| platform | string | No | Platform |

## Get Orders

Get orders with optional filtering.

> Response :

```json
{
  "data": {
    "currentPage": 1,
    "pageSize": 100,
    "totalNum": 5,
    "totalPage": 1,
    "items": [
        {
          "orderId": 6004295,
          "symbol": "ETH-INR",
          "side": "BUY",
          "type": "LIMIT",
          "origQty": "0.0001",
          "price": "5333400",
          "avgExecutedPrice": "0",
          "openQty": "0.0001",
          "filledQty": "0",
          "cancelledQty": "0",
          "status": "OPEN",
          "origQuoteOrderQty": "0",
          "stopPrice": "0",
          "feeCurrency": "INR",
          "fees": "0",
          "tax": "0",
          "tds": "0",
          "createdAt": 1744284283839,
          "updatedAt": 1744284284207
        }
      ]
    },
    "statusCode": 200,
    "statusDescription": "Success"
}
```

### Endpoint

`GET /api/v2/ex/orders`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token or API key authentication |
| X-AUTH-APIKEY | Yes | Required for API key authentication |
| X-AUTH-SIGNATURE | Yes | Required for API key authentication |

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| symbol | string | Yes | Trading pair symbol |
| status | string | No | Order status (ACTIVE, FILLED, CANCELLED) (default: ACTIVE)|
| currentPage | number | No | Current page number (default: 1) |
| pageSize | number | No | Number of records per page (default: 20) |
| startTime | number | No | Start time in milliseconds (default: 20) |
| endTime | number | No | End time in milliseconds (default: 20) |

## Get Order Details
Get details of a specific order.

> Response :

```json
{
    "data": {
        "orderId": 5784098,
        "symbol": "ETH-INR",
        "side": "SELL",
        "type": "LIMIT",
        "origQty": "0.11",
        "price": "172470.9",
        "avgExecutedPrice": "0",
        "openQty": "0",
        "filledQty": "0",
        "cancelledQty": "0.11",
        "status": "CANCELLED",
        "origQuoteOrderQty": "0",
        "stopPrice": "0",
        "feeCurrency": "INR",
        "fees": "0",
        "tax": "0",
        "tds": "0",
        "createdAt": 1742300104074,
        "updatedAt": 1742302555861
    },
    "statusCode": 200,
    "statusDescription": "Success"
}
```

### Endpoint

`GET /api/v2/ex/orders/{orderId}`

### Headers
| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token or API key authentication |
| X-AUTH-APIKEY | Yes | Required for API key authentication |
| X-AUTH-SIGNATURE | Yes | Required for API key authentication |


## Cancel Order
Cancel a specific order.

> Response :

```json
{
  "data": {
      "orderId": 6016691,
      "symbol": "BTC-INR"
  },
  "statusCode": 200,
  "statusDescription": "Success"
}
```

### Endpoint

`DELETE /api/v2/ex/orders/{orderId}`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token or API key authentication |
| X-AUTH-APIKEY | Yes | Required for API key authentication |
| X-AUTH-SIGNATURE | Yes | Required for API key authentication |


## Cancel All Orders for a symbol
Cancel all orders for a trading pair.

> Response :

```json
{
    "data": null,
    "statusCode": 200,
    "statusDescription": "Success"
}
```

### Endpoint 

`DELETE /api/v2/ex/orders`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token or API key authentication |
| X-AUTH-APIKEY | Yes | Required for API key authentication |
| X-AUTH-SIGNATURE | Yes | Required for API key authentication |

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| symbol | string | Yes | Trading pair symbol |

## Cancel All Orders

Cancel all orders

> Response :

```json
{
    "data": null,
    "statusCode": 200,
    "statusDescription": "Success"
}
```

### Endpoint

`DELETE /api/v2/ex/cancelAll`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token or API key authentication |
| X-AUTH-APIKEY | Yes | Required for API key authentication |
| X-AUTH-SIGNATURE | Yes | Required for API key authentication |

## Get Order Fills
Get fills for a specific order.

> Response:

```json
{
    "data": {
        "orderId": 660529,
        "symbol": "TRX-INR",
        "side": "BUY",
        "type": "LIMIT",
        "origQty": "5.744925",
        "price": "2.611",
        "avgExecutedPrice": "2.610999",
        "openQty": "0",
        "filledQty": "5.744925",
        "cancelledQty": "0",
        "status": "FILLED",
        "origQuoteOrderQty": "0",
        "stopPrice": "0",
        "feeCurrency": "INR",
        "fees": "0.02",
        "tax": "0",
        "tds": "0",
        "createdAt": 1601280571090,
        "updatedAt": 1610362215763,
        "fills": [
            {
                "quantity": "5.744925",
                "price": "2.611",
                "amount": "14.9999",
                "fees": "0.02",
                "intradayFees": "0",
                "tax": "0",
                "tds": "0",
                "totalFees": "0.02",
                "feeCurrency": "INR",
                "createdAt": 1610362215763,
                "isMaker": true
            }
        ]
    },
    "statusCode": 200,
    "statusDescription": "Success"
}
``` 

### Endpoint

`GET /api/v2/ex/orders/fills/{orderId}`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token or API key authentication |
| X-AUTH-APIKEY | Yes | Required for API key authentication |
| X-AUTH-SIGNATURE | Yes | Required for API key authentication |

## Get Exchange fee
Get Exchange fee details

> Response :

```json
{
    "data": {
        "symbol": "BTC-INR",
        "takerFeeRate": "0.5",
        "makerFeeRate": "0.45",
        "percentage": true,
        "gst": "18",
        "tds": "0"
    },
    "statusCode": 200,
    "statusDescription": "Success"
}
``` 

### Endpoint

`GET /api/v2/ex/fee/{symbol}`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token or API key authentication |
| X-AUTH-APIKEY | Yes | Required for API key authentication |
| X-AUTH-SIGNATURE | Yes | Required for API key authentication |

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| side | string | Yes | Order side (BUY,SELL) |

