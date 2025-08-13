# Choice Exchange API for CoinMarketCap Integration

This document provides the necessary technical API documentation for CoinMarketCap to integrate with the Choice Exchange AMM DEX. All endpoints adhere to the specified standards and conventions for DEXes.

## General Information

*   **API Base URL**: `https://api.choice.exchange/api/coinmarketcap/`
*   **Authentication**: No authentication is required for any of the public market data endpoints.
*   **Data Format**: All endpoints return data in JSON format.
*   **Pagination**: Endpoints do not use pagination and return the complete dataset in a single response, as required.
*   **Data Freshness**: All 24-hour rolling window statistics are calculated in real-time. Endpoints are cached for 60-120 seconds to ensure high performance and availability.

## Factory Contract Address

To identify pools belonging to Choice Exchange, please use the following factory contract address:

*   **Factory Address**: `inj1k9lcqtn3y92h4t3tdsu7z8qx292mhxhgsssmxg`
*   **Router Address**: `inj1ne2durmsx2jurvy4wgnhegv3xt6789up8xgum3`

---

## API Endpoints

### 1. Summary

Provides a high-level overview of market data for all trading pairs on the exchange.

*   **Endpoint**: `/summary`
*   **Method**: `GET`
*   **Sample URL**: `https://api.choice.exchange/api/coinmarketcap/summary`

#### Response Object

The endpoint returns a JSON array of market pair objects.

```json
[
  {
    "trading_pairs": "inj_peggy0xdAC17F958D2ee523a2206206994597C13D831ec7",
    "base_currency": "inj",
    "quote_currency": "peggy0xdAC17F958D2ee523a2206206994597C13D831ec7",
    "last_price": "25.5412894561",
    "lowest_ask": "25.5412894561",
    "highest_bid": "25.5412894561",
    "base_volume": "10542.12",
    "quote_volume": "269264.55",
    "price_change_percent_24h": "1.52",
    "highest_price_24h": "26.1045123",
    "lowest_price_24h": "25.0198744"
  },
  {
    "trading_pairs": "factory/inj17gkuet8f643f82p36gmlexsj3ef3g768c3u3q2/ninja_inj",
    "base_currency": "factory/inj17gkuet8f643f82p36gmlexsj3ef3g768c3u3q2/ninja",
    "quote_currency": "inj",
    "last_price": "0.0001258",
    ...
  }
]
```

#### Response Field Descriptions

| Name                     | Type    | Status    | Description                                                                                              |
| ------------------------ | ------- | --------- | -------------------------------------------------------------------------------------------------------- |
| `trading_pairs`          | string  | Mandatory | Identifier of a ticker, constructed as `BASE_ADDRESS_QUOTE_ADDRESS`.                                     |
| `base_currency`          | string  | Mandatory | The unique on-chain contract address of the base currency.                                               |
| `quote_currency`         | string  | Mandatory | The unique on-chain contract address of the quote currency.                                              |
| `last_price`             | decimal | Mandatory | Last transacted price of the base currency, quoted in the quote currency.                                |
| `lowest_ask`             | decimal | Mandatory | For our AMM, this is the current instantaneous `last_price`.                                             |
| `highest_bid`            | decimal | Mandatory | For our AMM, this is the current instantaneous `last_price`.                                             |
| `base_volume`            | decimal | Mandatory | 24-hour trading volume denoted in the **base** currency.                                                 |
| `quote_volume`           | decimal | Mandatory | 24-hour trading volume denoted in the **quote** currency.                                                |
| `price_change_percent_24h`| decimal | Mandatory | 24-hour percentage price change of the market pair.                                                      |
| `highest_price_24h`      | decimal | Mandatory | Highest transacted price in the last 24 hours.                                                           |
| `lowest_price_24h`       | decimal | Mandatory | Lowest transacted price in the last 24 hours.                                                            |                                                         |

### 2. Assets

Provides a detailed summary for each currency available for trading on the exchange.

*   **Endpoint**: `/assets`
*   **Method**: `GET`
*   **Sample URL**: `https://api.choice.exchange/api/coinmarketcap/assets`

#### Response Object

The endpoint returns a JSON object where each key is the asset's contract address.

```json
{
    "factory/inj127l5a2wmkyvucxdlupqyac3y0v6wqfhq03ka64/qunt": {
        "name": "Injective Quants",
        "symbol": "QUNT",
        "contractAddress": "factory/inj127l5a2wmkyvucxdlupqyac3y0v6wqfhq03ka64/qunt",
        "contractAddressUrl": "https://injscan.com/asset/factory%2Finj127l5a2wmkyvucxdlupqyac3y0v6wqfhq03ka64%2Fqunt"
    },
    "factory/inj16dd5xzszud3u5wqphr3tq8eaz00gjdn3d4mvj8/agent": {
        "name": "First Injective AI token",
        "symbol": "AGENT",
        "contractAddress": "factory/inj16dd5xzszud3u5wqphr3tq8eaz00gjdn3d4mvj8/agent",
        "contractAddressUrl": "https://injscan.com/asset/factory%2Finj16dd5xzszud3u5wqphr3tq8eaz00gjdn3d4mvj8%2Fagent"
    },
}
```

#### Response Field Descriptions

| Name                 | Type   | Status    | Description                                                               |
| -------------------- | ------ | --------- | ------------------------------------------------------------------------- |
| `name`               | string | Mandatory | Full name of the cryptocurrency.                                          |
| `symbol`             | string | Mandatory | Symbol/ticker of the cryptocurrency, e.g., `INJ`.                         |
| `contractAddress`    | string | Mandatory | The unique on-chain address of the asset.                                 |
| `contractAddressUrl` | string | Mandatory | A direct, URL-encoded link to the asset's page on the block explorer.     |

### 3. Ticker

Provides a 24-hour pricing and volume summary for all market pairs, formatted according to the DEX (Uniswap Sample C1) specification.

*   **Endpoint**: `/ticker`
*   **Method**: `GET`
*   **Sample URL**: `https://api.choice.exchange/api/coinmarketcap/ticker`

#### Response Object

The endpoint returns a JSON object where each key is the `base_id` and `quote_id` (contract addresses) separated by an underscore.

```json
{
    "inj_peggy0xdAC17F958D2ee523a2206206994597C13D831ec7": {
        "base_id": "inj",
        "base_name": "Injective",
        "base_symbol": "INJ",
        "quote_id": "peggy0xdAC17F958D2ee523a2206206994597C13D831ec7",
        "quote_name": "Tether",
        "quote_symbol": "USDT",
        "last_price": "14.65177068263648744895952765",
        "base_volume": "0",
        "quote_volume": "0"
    },
  ...
}
```

#### Response Field Descriptions

| Name            | Type    | Status    | Description                                                       |
| --------------- | ------- | --------- | ----------------------------------------------------------------- |
| `base_id`       | string  | Mandatory | The unique on-chain contract address of the base asset.           |
| `base_name`     | string  | Mandatory | The full name of the base asset.                                  |
| `base_symbol`   | string  | Mandatory | The symbol of the base asset, e.g., `INJ`.                        |
| `quote_id`      | string  | Mandatory | The unique on-chain contract address of the quote asset.          |
| `quote_name`    | string  | Mandatory | The full name of the quote asset.                                 |
| `quote_symbol`  | string  | Mandatory | The symbol of the quote asset, e.g., `USDT`.                      |
| `last_price`    | decimal | Mandatory | Last transacted price of the base asset based on the quote asset.   |
| `base_volume`   | decimal | Mandatory | 24-hour trading volume denoted in the **base** asset.             |
| `quote_volume`  | decimal | Mandatory | 24-hour trading volume denoted in the **quote** asset.            |


### 4. Trades

Returns recently completed trades for a given market pair.

*   **Endpoint**: `/trades/<market_pair>`
*   **Method**: `GET`
*   **Note**: The `market_pair` parameter must be constructed using the contract addresses of the two assets, separated by a single underscore. e.g., `BASE_ADDRESS_QUOTE_ADDRESS`.

*   **Sample URL**: `https://api.choice.exchange/api/coinmarketcap/trades/inj_peggy0xdAC17F958D2ee523a2206206994597C13D831ec7`

#### Response Object

The endpoint returns a JSON array of trade objects.

```json
[
  {
    "trade_id": 7854120001,
    "price": "25.5412",
    "base_volume": "10.5",
    "quote_volume": "268.1826",
    "timestamp": 1678886400000,
    "type": "buy"
  },
  {
    "trade_id": 7854110005,
    "price": "25.5399",
    "base_volume": "5.2",
    "quote_volume": "132.80748",
    "timestamp": 1678886340000,
    "type": "sell"
  }
]
```

#### Response Field Descriptions

| Name            | Type      | Status    | Description                                                                   |
| --------------- | --------- | --------- | ----------------------------------------------------------------------------- |
| `trade_id`      | integer   | Mandatory | A unique ID for the trade transaction.                                        |
| `price`         | decimal   | Mandatory | The transacted price of the base asset, quoted in the quote asset.            |
| `base_volume`   | decimal   | Mandatory | The transaction amount in the **base** currency.                              |
| `quote_volume`  | decimal   | Mandatory | The transaction amount in the **quote** currency.                             |
| `timestamp`     | integer   | Mandatory | Unix timestamp in milliseconds for when the transaction occurred.             |
| `type`          | string    | Mandatory | The type of trade, either `buy` or `sell`.                                    |
