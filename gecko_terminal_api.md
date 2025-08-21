# GeckoTerminal API Endpoints

This document provides an overview of the GeckoTerminal integration API endpoints. These endpoints are designed to expose blockchain data in a format compatible with GeckoTerminal's charting and data aggregation services.

**Base URL**: `https://api.choice.exchange/api/coingecko`

## API Reference

The following endpoints are available:

### 1. Latest Block

Provides the latest block number and timestamp that is guaranteed to be fully processed and available in the `/events` endpoint. This is a crucial endpoint for determining the safe upper bound for querying event data.

-   **Endpoint:** `/latest-block`
-   **Method:** `GET`
-   **Query Parameters:** None

**Success Response (200 OK):**

```json
{
    "block": {
        "blockNumber": 1234567,
        "blockTimestamp": 1678886400
    }
}
```

**Error Responses:**

-   **404 Not Found:** Returned if no blocks are found or if the indexer is still processing the initial blocks.
-   **500 Internal Server Error:** Indicates a database error or an issue fetching the block timestamp.
-   **503 Service Unavailable:** Returned if the connection to the blockchain's RPC/LCD endpoint fails.

### 2. Asset Information

Retrieves detailed information about a specific token (asset).

-   **Endpoint:** `/asset`
-   **Method:** `GET`
-   **Query Parameters:**
    -   `id` (string, required): The contract address of the token.

**Success Response (200 OK):**

```json
{
    "asset": {
        "id": "token_contract_address",
        "name": "My Token",
        "symbol": "MTK",
        "decimals": 6,
        "totalSupply": "1000000.000000",
        "circulatingSupply": "500000.000000",
        "coinGeckoId": "my-token"
    }
}
```

**Error Responses:**

-   **400 Bad Request:** Returned if the `id` query parameter is missing.
-   **404 Not Found:** Returned if an asset with the specified `id` does not exist.

### 3. Pair Information

Fetches details for a specific liquidity pool (pair).

-   **Endpoint:** `/pair`
-   **Method:** `GET`
-   **Query Parameters:**
    -   `id` (string, required): The contract address of the liquidity pool.

**Success Response (200 OK):**

```json
{
    "pair": {
        "id": "pair_contract_address",
        "dexKey": "choiceexchange",
        "asset0Id": "token1_contract_address",
        "asset1Id": "token2_contract_address",
        "createdAtBlockNumber": 1234500,
        "createdAtBlockTimestamp": 1678886000,
        "createdAtTxnId": "transaction_hash_of_creation",
        "feeBps": 30
    }
}
```

**Error Responses:**

-   **400 Bad Request:** Returned if the `id` query parameter is missing.
-   **404 Not Found:** Returned if a pair with the specified `id` does not exist.

### 4. Events

Returns a chronological list of events (swaps, joins, exits) that occurred within a specified block range.

-   **Endpoint:** `/events`
-   **Method:** `GET`
-   **Query Parameters:**
    -   `fromBlock` (integer, required): The starting block number for the event query.
    -   `toBlock` (integer, required): The ending block number for the event query.

**Success Response (200 OK):**

```json
{
    "events": [
        {
            "block": {
                "blockNumber": 1234567,
                "blockTimestamp": 1678886400
            },
            "eventType": "swap",
            "txnId": "0xabc...",
            "txnIndex": 0,
            "eventIndex": 1,
            "maker": "user_wallet_address",
            "pairId": "pair_contract_address",
            "asset0In": "100.000000",
            "asset1In": "0",
            "asset0Out": "0",
            "asset1Out": "49.850000",
            "priceNative": "0.4985",
            "reserves": {
                "asset0": "10100.000000",
                "asset1": "4950.150000"
            }
        },
        {
            "block": {
                "blockNumber": 1234568,
                "blockTimestamp": 1678886412
            },
            "eventType": "join",
            "txnId": "0xdef...",
            "txnIndex": 2,
            "eventIndex": 0,
            "maker": "another_user_address",
            "pairId": "pair_contract_address",
            "amount0": "50.000000",
            "amount1": "25.000000",
            "reserves": {
                "asset0": "10150.000000",
                "asset1": "4975.150000"
            }
        }
    ]
}
```

**Event Types:**
- `swap`: Represents a trade between the two assets in the pair.
- `join`: Represents a user adding liquidity to the pool.
- `exit`: Represents a user removing liquidity from the pool.

**Error Responses:**
- **400 Bad Request:** Returned if `fromBlock` or `toBlock` are missing or are not valid integers.
