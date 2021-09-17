# Hyperjump API

## Adapted from Pancake API

The PancakeSwap API is a set of endpoints used by market aggregators (e.g. coinmarketcap.com) to surface PancakeSwap liquidity
and volume information. All information is fetched from the underlying subgraphs.

## Development

### Install requirements

```shell
yarn global add vercel
```

### Build

```shell
# Install dependencies
yarn

# Build project
vercel dev
```

Endpoints are based on filename inside the `api/` folder.

```shell
# api/pairs.ts
curl -X GET 'localhost:3000/api/pairs'

# ...
```

## Production

### Deploy

Deployments to production are triggered by a webhook when a commit, or a pull-request is merged to `master`.

If you need to force a deployment, use the following command:

```shell
vercel --prod
```

# Documentation

All PancakeSwap pairs consist of two different tokens. BNB is not a native currency in PancakeSwap, and is represented only by WBNB in the pairs.

The canonical WBNB address used by the PancakeSwap interface is `0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c`.

Results are cached for 5 minutes (or 300 seconds).

## [`/summary`](https://bsc-api.hyperjump.app/api/summary)

Returns data for the top ~1000 PancakeSwap pairs, sorted by reserves.

### Request

`GET https://bsc-api.hyperjump.app/api/summary`

### Response

```json5
{
  updated_at: 1234567, // UNIX timestamp
  data: {
    "0x..._0x...": {
      // BEP20 token addresses, joined by an underscore
      price: "...", // price denominated in token1/token0
      base_volume: "...", // last 24h volume denominated in token0
      quote_volume: "...", // last 24h volume denominated in token1
      liquidity: "...", // liquidity denominated in USD
      liquidity_BNB: "...", // liquidity denominated in BNB
    },
    // ...
  },
}
```

## [`/tokens`](https://bsc-api.hyperjump.app/api/tokens)

Returns the tokens in the top ~1000 pairs on PancakeSwap, sorted by reserves.

### Request

`GET https://bsc-api.hyperjump.app/api/tokens`

### Response

```json5
{
  updated_at: 1234567, // UNIX timestamp
  data: {
    "0x...": {
      // the address of the BEP20 token
      name: "...", // not necessarily included for BEP20 tokens
      symbol: "...", // not necessarily included for BEP20 tokens
      price: "...", // price denominated in USD
      price_BNB: "...", // price denominated in BNB
    },
    // ...
  },
}
```

## [`/tokens/0x...`](https://bsc-api.hyperjump.app/api/tokens/0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82)

Returns the token information, based on address.

### Request

`GET https://bsc-api.hyperjump.app/api/tokens/0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82`

### Response

```json5
{
  updated_at: 1234567, // UNIX timestamp
  data: {
    name: "...", // not necessarily included for BEP20 tokens
    symbol: "...", // not necessarily included for BEP20 tokens
    price: "...", // price denominated in USD
    price_BNB: "...", // price denominated in BNB
  },
}
```

## [`/pairs`](https://bsc-api.hyperjump.app/api/pairs)

Returns data for the top ~1000 PancakeSwap pairs, sorted by reserves.

### Request

`GET https://bsc-api.hyperjump.app/api/pairs`

### Response

```json5
{
  updated_at: 1234567, // UNIX timestamp
  data: {
    "0x..._0x...": {
      // the asset ids of BNB and BEP20 tokens, joined by an underscore
      pair_address: "0x...", // pair address
      base_name: "...", // token0 name
      base_symbol: "...", // token0 symbol
      base_address: "0x...", // token0 address
      quote_name: "...", // token1 name
      quote_symbol: "...", // token1 symbol
      quote_address: "0x...", // token1 address
      price: "...", // price denominated in token1/token0
      base_volume: "...", // volume denominated in token0
      quote_volume: "...", // volume denominated in token1
      liquidity: "...", // liquidity denominated in USD
      liquidity_BNB: "...", // liquidity denominated in BNB
    },
    // ...
  },
}
```
