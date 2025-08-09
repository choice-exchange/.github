# Choice.Exchange API Documentation for CoinGecko

This document provides a detailed overview of the Choice.Exchange public API endpoints for integration with CoinGecko.

**Base URL**: `https://api.choice.exchange/api/coingecko`

## General Information

* All endpoints return data in `JSON` format.
* All decimal values are returned as **strings** to preserve precision.
* Endpoints are rate-limited to ensure high availability. Exceeding the rate limit will result in a `429 Too Many Requests` error.

---

## Endpoint 1: `/tickers`

The `/tickers` endpoint provides 24-hour pricing and volume information for all market pairs available on the exchange.

**URL**: `https://api.choice.exchange/api/coingecko/tickers`

The response is a JSON array of ticker objects. This endpoint also supports pagination; to navigate, you can append the `?page=<number>` query parameter. When pagination is used, the response will be wrapped in a standard pagination object (`{"count": ..., "results": [...]}`).

### Response Object

| Field              | Data Type | Category           | Description                                                                                             |
| ------------------ | --------- | ------------------ | ------------------------------------------------------------------------------------------------------- |
| `ticker_id`        | string    | **Mandatory**      | A unique identifier for the trading pair, the pool contract address. Eg. `inj1d5xuqkrj8kwqvwyxss4q5w0dkduywqq3uucv87`.   |
| `base_currency`    | string    | **Mandatory**      | The contract address of the base token.                                                                 |
| `target_currency`  | string    | **Mandatory**      | The contract address of the target token.                                                               |
| `last_price`       | decimal   | **Mandatory**      | The latest price of the base token in terms of the target token, derived from the pool's asset reserves. |
| `base_volume`      | decimal   | **Mandatory**      | The total trading volume in the last 24 hours, denominated in the base token.                           |
| `target_volume`    | decimal   | **Mandatory**      | The total trading volume in the last 24 hours, denominated in the target token.                         |
| `pool_id`          | string    | **Mandatory** (DEX)| The contract address of the liquidity pool.                                                             |
| `liquidity_in_usd` | decimal   | **Mandatory** (DEX)| The total value of liquidity in the pool, denominated in USD.                                           |
| `high`             | decimal   | Recommended        | The highest transaction price in the last 24 hours. If no trades, defaults to `last_price`.             |
| `low`              | decimal   | Recommended        | The lowest transaction price in the last 24 hours. If no trades, defaults to `last_price`.              |
| `bid`              | decimal   | Not Applicable     | Field included for compatibility. Returns `null`.                                                       |
| `ask`              | decimal   | Not Applicable     | Field included for compatibility. Returns `null`.                                                       |

### Example Request & Response

**Request**:

```bash
GET https://api.choice.exchange/api/coingecko/tickers
```

**Response**:

```json
{
    "count": 22,
    "next": null,
    "previous": null,
    "results": [
        {
            "ticker_id": "inj1d5xuqkrj8kwqvwyxss4q5w0dkduywqq3uucv87",
            "base_currency": "inj",
            "target_currency": "peggy0xdAC17F958D2ee523a2206206994597C13D831ec7",
            "last_price": "14.596332356186970660",
            "base_volume": "0.000000000000000000",
            "target_volume": "0.000000000000000000",
            "pool_id": "inj1d5xuqkrj8kwqvwyxss4q5w0dkduywqq3uucv87",
            "liquidity_in_usd": "25648.717354177875048333",
            "high": "14.596332356186970660",
            "low": "14.596332356186970660",
            "bid": null,
            "ask": null
        }
    ]
}
```

---

## Endpoint 2: `/historical_trades`

The `/historical_trades` endpoint provides recently completed trades for a given market pair.

**URL**: `https://api.choice.exchange/api/coingecko/historical_trades`

### Request Parameters

| Parameter      | Data Type | Category     | Description                                                                                                                                |
| -------------- | --------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `ticker_id`    | string    | **Mandatory**| The contract address for the pair (e.g., `inj1d5xuqkrj8kwqvwyxss4q5w0dkduywqq3uucv87`).                                                                  |
| `type`         | string    | Optional     | Filters trades by type. Can be `buy` or `sell`. If omitted, both types are returned.                                                       |
| `page`        | integer   | Optional     | The page number.
| `start_time`   | integer   | Optional     | A Unix timestamp (seconds) to specify the start of the time range for the query.                                                           |
| `end_time`     | integer   | Optional     | A Unix timestamp (seconds) to specify the end of the time range for the query.                                                             |

### Response Object

The response is a JSON object containing two keys: `"buy"` and `"sell"`. Each key holds an array of trade objects.

| Field             | Data Type | Category    | Description                                                                                                                            |
| ----------------- | --------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `trade_id`        | integer   | **Mandatory** | A globally unique, stable integer ID for the trade, derived from `(block_number * 10000) + event_index`.                                 |
| `price`           | decimal   | **Mandatory** | The transaction price of the base asset in terms of the target asset.                                                                  |
| `base_volume`     | decimal   | **Mandatory** | The volume of the transaction, denominated in the base token.                                                                          |
| `target_volume`   | decimal   | **Mandatory** | The volume of the transaction, denominated in the target token.                                                                        |
| `trade_timestamp` | integer   | **Mandatory** | The Unix timestamp in **milliseconds** for when the trade occurred.                                                                    |
| `type`            | string    | **Mandatory** | The type of trade. `buy` indicates the base asset was bought; `sell` indicates the base asset was sold.                                  |

### Example Request & Response

**Request**:
```
GET https://api.choice.exchange/api/coingecko/historical_trades?ticker_id=inj1d5xuqkrj8kwqvwyxss4q5w0dkduywqq3uucv87&start_time=1752978890&end_time=1752982350
```

**Response**:
```json
{
    "count": 2,
    "next": null,
    "previous": null,
    "results": [
        {
            "trade_id": 1258016010267,
            "price": "14.196770938718858475",
            "base_volume": "0.598421502796090238",
            "target_volume": "8.495653000000000000",
            "trade_timestamp": 1752982321453,
            "type": "buy"
        },
        {
            "trade_id": 1257965180167,
            "price": "14.168236630284272883",
            "base_volume": "0.890412994164318932",
            "target_volume": "12.615582000000000000",
            "trade_timestamp": 1752978890668,
            "type": "buy"
        }
    ]
}
```
