# ONRAMP | OFFRAMP

OnRamp and OffRamp flows are used for external fiat-to-crypto and crypto-to-fiat operations.
OnRamp is used when the client pays fiat through a provider and receives crypto to an external wallet.
OffRamp is used when the client sends crypto and receives fiat through a provider.

> BASE_URL https://api.dev.wbdevel.net

### Headers
- `x-api-key: {{x-api-key}}`

### Step 1. Get available assets
Use this endpoint to retrieve available fiat and crypto assets for the merchant context. Use the response to build asset selectors and validate `code/network` pairs in downstream requests.

**POST** `/api/v2/exchange/merchant/assets`

### Headers
- `x-api-key: {{x-api-key}}`

**Response**
```json
{
  "fiatAssets": [
    { "id": "BYN", "code": "BYN" },
    { "id": "EUR", "code": "EUR" },
    { "id": "RUB", "code": "RUB" },
    { "id": "USD", "code": "USD" }
  ],
  "cryptoAssets": [
    { "id": "BNB", "code": "BNB", "network": "BNB" },
    { "id": "USDC_BNB", "code": "USDC", "network": "BNB", "protocol": "BEP-20" },
    { "id": "USDT_BNB", "code": "USDT", "network": "BNB", "protocol": "BEP-20" },
    { "id": "BTC", "code": "BTC", "network": "Bitcoin" },
    { "id": "AAVE", "code": "AAVE", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "ETH", "code": "ETH", "network": "Ethereum" },
    { "id": "LINK", "code": "LINK", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "PAXG", "code": "PAXG", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "UNI", "code": "UNI", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "USDC_ERC", "code": "USDC", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "USDT_ERC", "code": "USDT", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "XAUT", "code": "XAUT", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "TON", "code": "TON", "network": "Ton" },
    { "id": "USDT_TON", "code": "USDT", "network": "Ton", "protocol": "TON" },
    { "id": "TRX", "code": "TRX", "network": "Tron" },
    { "id": "USDT_TRC", "code": "USDT", "network": "Tron", "protocol": "TRC-20" }
  ]
}
```

### Headers

| Name | Type | Required | Description |
|---|---|---:|---|
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |

### Request

| Name | Type | Required | Description |
|---|---|---:|---|
| `destination` | `string` | No | Flow destination filter. Use it to scope available routes to the required integration context. Allowed values: `EXCHANGE`, `SDK_EXCHANGE`, `ACCOUNTING`, `SDK_ACCOUNTING`, `SDK_CROSS`. |

### Response

| Name | Type | Description |
|---|---|---|
| `fiatAssets` | `array of objects` | List of fiat assets that can be shown to the client as available payment or payout currencies for OnRamp/OffRamp flows. |
| `fiatAssets[].id` | `string` | Fiat asset identifier. |
| `fiatAssets[].code` | `string` | Fiat asset code used in exchange requests. |
| `cryptoAssets` | `array of objects` | List of crypto assets/networks that can be used in deposit, withdrawal, buy, sell, or conversion flows. |
| `cryptoAssets[].id` | `string` | Crypto asset identifier. |
| `cryptoAssets[].code` | `string` | Crypto asset code used in exchange requests. |
| `cryptoAssets[].network` | `string` | Network name used for chain-specific assets. |
| `cryptoAssets[].protocol` | `string/null` | Token protocol for tokenized assets, if applicable. |

### Errors

| Name | Code | Description |
|---|---|---|
| `400 CURRENCY_NOT_FOUND` | BUSINESS | Asset configuration contains unsupported or unknown asset mapping. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation. |
### Step 2. Get payment providers
Use this endpoint to retrieve available fiat payment providers for the selected client and direction. Use the response to choose a provider route and validate supported currency/direction combinations.

**POST** `/api/v2/exchange/merchant/payment/provider`

### Headers
- `x-api-key: {{x-api-key}}`

**Request**
```json
{
  "clientId": "{{clientId}}",
  "fiatAsset": "BYN",
  "orderType": "BUY"
}
```
**Response**
```json
[
  {
    "id": "ASSIST",
    "name": "ASSIST",
    "addPaymentMethod": true,
    "config": {
      "paymentSystems": [
        {
          "paymentSystem": "VISA",
          "type": "PSP",
          "directions": [
            {
              "direction": "BUY",
              "currencies": [
                {
                  "currency": "BYN",
                  "countries": ["Belarus", "Azerbaijan", "Armenia", "Kazakhstan", "Kyrgyzstan", "Moldova", "Tajikistan", "Turkmenistan", "Uzbekistan", "Georgia"]
                }
              ]
            }
          ]
        }
      ]
    },
    "commissions": [
      {
        "bank": "OTHER",
        "buyCommission": "3,5-4,9",
        "sellCommission": "3,0-4,9"
      }
    ]
  },
]
```

### Headers

| Name | Type | Required | Description |
|---|---|---:|---|
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| `externalClientId` | `string` | No | Optional external client identifier used for merchant/client mapping validation. |

### Request

| Name | Type | Required | Description |
|---|---|---:|---|
| `clientId` | `string` | No | Client identifier in WhiteBird system for merchant scope checks. |
| `fiatAsset` | `string` | No | Fiat code filter for provider routes. |
| `orderType` | `string` | No | Order direction. Allowed values: `BUY`, `SELL`, `SWAP`. |
| `destination` | `string` | No | Flow destination filter. Use it to scope available routes to the required integration context. Allowed values: `EXCHANGE`, `SDK_EXCHANGE`, `ACCOUNTING`, `SDK_ACCOUNTING`, `SDK_CROSS`. |

### Response

| Name | Type | Description |
|---|---|---|
| `[].id` | `string` | Provider identifier. |
| `[].name` | `string` | Provider display name. |
| `[].addPaymentMethod` | `boolean` | Defines whether provider supports adding payment methods. |
| `[].config` | `object` | Provider routing configuration. |
| `[].config.paymentSystems` | `array of objects` | Payment systems list for provider. |
| `[].config.paymentSystems[].paymentSystem` | `string` | Payment system name (for example `VISA`, `MIR`, `BelCard`). |
| `[].config.paymentSystems[].type` | `string` | Provider channel type. |
| `[].config.paymentSystems[].directions[].direction` | `string` | Direction for payment system route (`BUY`/`SELL`). |
| `[].config.paymentSystems[].directions[].currencies[].currency` | `string` | Fiat currency for this route. |
| `[].config.paymentSystems[].directions[].currencies[].countries` | `array<string>` | Optional country restrictions for this route. |
| `[].commissions` | `array of objects` | Commission settings for provider. |
| `[].commissions[].bank` | `string` | Bank group key for commission row. |
| `[].commissions[].buyCommission` | `string` | Commission value/range for buy direction. |
| `[].commissions[].sellCommission` | `string` | Commission value/range for sell direction. |

### Errors

| Name | Code | Description |
|---|---|---|
| `400 CLIENT_NOT_FOUND` | BUSINESS | Provided `clientId` is invalid or not linked to merchant. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation. |
### Step 3. Get payment methods
Use this endpoint to retrieve payment methods/tokens available for the selected provider context. Use the response to select `paymentMethodToken` with valid status and pass it to quote creation.

**POST** `/api/v2/exchange/merchant/payment/method`

> Take payment method `id` and pass it as `paymentMethodToken` in quote creation. Use records with `status=ENABLED` and `isRestricted=false`.

### Headers
- `x-api-key: {{x-api-key}}`

**Request**
```json
{
  "clientId": "{{clientId}}",
  "fiatAsset": "BYN",
  "orderType": "BUY"
}
```
**Response**
```json
[
  {
    "id": "8ff59a79-461a-4c6f-8f77-832cfa836c86",
    "number": "**** **** **** 1111",
    "brand": "VISA",
    "providerId": "ASSIST",
    "providerType": "ASSIST",
    "status": "ENABLED",
    "isRestricted": false,
    "isCrypto": false,
    "country": "Russia"
  },
]
```

### Headers

| Name | Type | Required | Description |
|---|---|---:|---|
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| `externalClientId` | `string` | No | Optional external client identifier used for merchant/client mapping validation. |

### Request

| Name | Type | Required | Description |
|---|---|---:|---|
| `clientId` | `string` | Yes | Client identifier used to scope available payment methods. |
| `fiatAsset` | `string` | No | Fiat code filter for payment methods. |
| `orderType` | `string` | No | Order direction. Allowed values: `BUY`, `SELL`, `SWAP`. |
| `destination` | `string` | No | Flow destination filter. Use it to scope available routes to the required integration context. Allowed values: `EXCHANGE`, `SDK_EXCHANGE`, `ACCOUNTING`, `SDK_ACCOUNTING`, `SDK_CROSS`. |
| `providers` | `array<string>` | No | Provider filter list (for current examples: `ASSIST`). |
| `isCrypto` | `boolean` | No | Filters crypto payment methods if supported by provider. |
| `countryGroup` | `array<string>` | No | Country group filter. Allowed values: `BELARUS`, `RUSSIA`, `FOREIGN`. |

### Response

| Name | Type | Description |
|---|---|---|
| `[].id` | `string` | Payment method token identifier used in quote request. |
| `[].number` | `string` | Masked card/account representation for UI display. |
| `[].brand` | `string` | Payment brand, if applicable. |
| `[].providerId` | `string` | Provider identifier. |
| `[].providerType` | `string` | Provider type value (for current examples: `ASSIST`). |
| `[].status` | `string` | Payment method status (use enabled methods in flow). |
| `[].isRestricted` | `boolean` | Restriction flag for method availability. |
| `[].isCrypto` | `boolean` | Indicates crypto payment method record. |
| `[].country` | `string` | Country associated with the payment method. |
| `[].currency` | `string` | Payment currency code, if returned. |
| `[].supportedCurrencies` | `array<string>` | Supported fiat currencies for this method, if returned. |

### Errors

| Name | Code | Description |
|---|---|---|
| `400 CLIENT_NOT_FOUND` | BUSINESS | Provided `clientId` is invalid or not linked to merchant. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation. |
### Step 4. Calculate limits (v2)
**POST** `/api/v2/exchange/merchant/limit`

Use this endpoint to calculate min/max available amount for the selected pair and payment method.
Use the response to validate user input before quote creation.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| clientId | string | No | WhiteBird client identifier. |
| fromAsset | object | Yes | Source asset object. |
| fromAsset.code | string | Yes | Source asset code. |
| fromAsset.network | string \| null | No | Source asset network for crypto assets. |
| toAsset | object | Yes | Target asset object. |
| toAsset.code | string | Yes | Target asset code. |
| toAsset.network | string \| null | No | Target asset network for crypto assets. |
| paymentMethod | string | Yes | Payment provider type used for the operation, for example `CA` or `ASSIST`. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| asset | object | Asset used for limit values. |
| asset.code | string | Asset code. |
| asset.network | string \| null | Asset network if applicable. |
| min | number | Minimum allowed amount. |
| max | number | Maximum allowed amount. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| CURRENCY_NOT_FOUND | 400 | Invalid `fromAsset`/`toAsset` pair. |
| CLIENT_NOT_FOUND | 400 | `clientId` is invalid for the merchant. |
| `400 Bad Request` | HTTP | Request validation failed for one or more fields. |

**Request**
```json
{
    "clientId": "{{clientId}}",
    "toAsset": {
        "code": "TRX",
        "network": "Tron"
    },
    "fromAsset": {
        "code": "RUB",
        "network": null
        
    },
    "paymentMethod": "CA"
}
```
**Response**
```json
{
    "asset": {
        "id": "RUB",
        "code": "RUB"
    },
    "min": 263.16,
    "max": 1182894.74
}
```
### Step 5. Create quote (v2)
**POST** `/api/v2/exchange/merchant/quote`

> From response take quoteId.
> [You can Check the request fields to understand how the calculation is performed.](./V2.md#quote-fields)

Use this endpoint to calculate executable quote values (amounts, rate, fees, expiration).
Use the response `quoteId` as an input for buy/sell order creation.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| clientId | string | No | WhiteBird client identifier. |
| fromAsset | object | Yes | Source asset object. |
| fromAsset.code | string | Yes | Source asset code. |
| fromAsset.network | string \| null | No | Source asset network for crypto assets. |
| fromAsset.amount | number | No | Source amount when quote is input-side based. |
| toAsset | object | Yes | Target asset object. |
| toAsset.code | string | Yes | Target asset code. |
| toAsset.network | string \| null | No | Target asset network for crypto assets. |
| toAsset.amount | number | No | Target amount when quote is output-side based. |
| paymentMethod | string | No | Payment provider type used for the operation, for example `CA` or `ASSIST`. |
| paymentMethodToken | string | No | Selected payment method token/id. |
| destinationCryptoAddress | string | No | Destination wallet for buy flow. |
| comment | string | No | Optional merchant comment. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| quoteId | string | Quote identifier used for order creation. |
| fromAsset | object | Resolved source asset and amount. |
| toAsset | object | Resolved target asset and amount. |
| rate | number | Final client rate. |
| plainRate | number | Market/base rate before final quote adjustments. |
| fee | object | Fee breakdown object. |
| fee.total | number | Total fee amount. |
| fee.service | number \| null | Service fee component. |
| fee.network | number \| null | Network/payment component. |
| fee.asset | string | Fee asset code. |
| expirationDate | string | Quote expiration timestamp in server date-time format. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| INVALID_QUOTE | 400 | Quote cannot be calculated for provided values. |
| CURRENCY_NOT_FOUND | 400 | Invalid asset or network. |
| CLIENT_NOT_FOUND | 400 | `clientId` is invalid for the merchant. |
| `400 Bad Request` | HTTP | Request validation failed for one or more fields. |

**Request**
```json
{
    "clientId": "{{clientId}}",
    "fromAsset": {
        "code": "RUB",
        "network": null,
        "amount": "150"
    },
    "toAsset": {
        "code": "TRX",
        "network": "Tron"
    },
    "paymentMethod": "CA",
    "paymentMethodToken": "{{payment_token}}"
}
```
**Response**
```json
{
    "quoteId": "8bed6c08-90bc-4521-81d7-68bb0176ebae",
    "fromAsset": {
        "code": "RUB",
        "amount": "150"
    },
    "toAsset": {
        "code": "TRX",
        "network": "Tron",
        "amount": "5.127711"
    },
    "rate": 29.2528,
    "plainRate": 27.13,
    "fee": {
        "total": 3.75,
        "service": null,
        "network": 0.263,
        "asset": "RUB"
    },
    "expirationDate": "2026-04-27T11:11:11+0000"
}
```
### Step 6. Buy Crypto (v2)
**GET** `/api/v2/exchange/merchant/buy?destinationCryptoAddress={{walletAddress}}&quoteId={{quoteId}}`

> From response take orderId.

Use this endpoint to create a fiat-to-crypto order from a previously created quote.
Use the response `id` as `orderId` for status polling and order details retrieval.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Params

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| quoteId | string (UUID) | Yes | Quote identifier returned by quote API. |
| destinationCryptoAddress | string | No | Destination wallet address for payout. |
| comment | string | No | Optional order comment. |
| returnUrl | string | No | URL for success return flow. |
| failUrl | string | No | URL for fail return flow. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| body | none | - | GET endpoint without request body. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| id | string | Order identifier. |
| type | string | Order type. Allowed values: `BUY`, `SELL`, `SWAP`. |
| status | string | Current order lifecycle state. Allowed values: `NEW`, `PROCESSING`, `COMPLETED`, `EXPIRED`, `ERROR`. |
| creationDate | string | Order creation timestamp in server date-time format. |
| modificationDate | string | Last order update timestamp in server date-time format. |
| fiatPaymentLink | string | Payment link/token for fiat step processing. |
| expiresAtDate | string \| null | Order expiration timestamp in server date-time format. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| `400 QUOTE_NOT_FOUND` | BUSINESS | Quote does not exist or already expired. |
| `400 INVALID_QUOTE` | BUSINESS | Quote cannot be calculated or cannot be used for order creation. |
| `400 AML_FRAUD_VALIDATION_ERROR` | BUSINESS | AML/fraud checks blocked order creation. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation or client scope. |

**Response**
```json
{
    "id": "4dffdc69-7821-4007-94ad-522ad6a46117",
    "type": "BUY",
    "status": "PROCESSING",
    "creationDate": "2026-04-27T11:13:26.237357",
    "modificationDate": "2026-04-27T11:13:27.727824",
    "fiatPaymentLink": "c76ca74d-7aea-4688-942e-8385c413826f"
}
```
### Step 7. Get order by ID
**GET** `/api/v2/exchange/merchant/order?orderId={{Order_ID}}`

Use this endpoint to fetch full order details by `orderId`.
Use the response to track all order phases (exchange, fiat transaction, crypto transaction) and status transitions.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Params

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| orderId | string (UUID) | Yes | Order identifier returned by buy/sell API. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| body | none | - | GET endpoint without request body. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| id | string | Order identifier. |
| type | string | Order type. Allowed values: `BUY`, `SELL`, `SWAP`. |
| status | string | Current order lifecycle state. Allowed values: `NEW`, `PROCESSING`, `COMPLETED`, `EXPIRED`, `ERROR`. |
| number | number | Internal order number. |
| exchangeOperation | object | Exchange side details (input/output, rates, fees). |
| cryptoTransaction | object | Crypto transfer details. |
| cryptoTransaction.status | string | Crypto transaction status. Allowed values: `NEW`, `PENDING_REVIEW`, `NOT_FOUND`, `REJECTED`, `TIMEOUT`, `INVALID_AMOUNT`, `ERROR`, `AML_ERROR`, `AML_BLOCKED`, `ARREST`, `SUBMITTING`, `SUBMITTED`, `PENDING`, `SELECTED`, `CONFIRMED`, `PENDING_RESOLVE`. |
| fiatTransaction | object | Fiat processing details. |
| fiatTransaction.status | string | Fiat transaction status. Allowed values: `NEW`, `PENDING_REVIEW`, `REJECTED`, `TIMEOUT`, `DECLINED`, `INVALID_AMOUNT`, `ERROR`, `AML_BLOCKED`, `PENDING`, `PROCESSING`, `APPROVED`. |
| operationType | string | Internal operation type, for example `FIAT_TO_CRYPTO` or `CRYPTO_TO_FIAT`. |
| exchangeType | string | Exchange direction as internal enum value. |
| fromSource / toSource | string | Source side enum values (`EXT`/`INT`). |
| serverDate | string | Server timestamp in server date-time format. |
| completionDate | string \| null | Completion timestamp in server date-time format. |
| resultMessage | string \| null | Processing message. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| ORDER_NOT_FOUND | 404 | Order not found or not accessible in merchant scope. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation or client scope. |

**Response**
```json
{
    "id": "4dffdc69-7821-4007-94ad-522ad6a46117",
    "type": "BUY",
    "status": "PROCESSING",
    "creationDate": "2026-04-27T11:13:26.237357",
    "modificationDate": "2026-04-27T11:13:27.727824",
    "number": 831000003994,
    "exchangeOperation": {
        "inputCurrency": "RUB",
        "inputAsset": 270,
        "outputCurrency": "TRX",
        "outputAsset": 9.440281,
        "exchangeFeeAssetInFiat": 6.75,
        "bonusOutputAsset": null,
        "plainRatio": 27.13,
        "ratio": 28.6008,
        "currencyPair": {
            "fromCurrency": "RUB",
            "toCurrency": "TRX"
        }
    },
    "cryptoTransaction": {
        "hash": null,
        "externalCryptoAddress": "TCT2pKJXo233hrKWQMeCptC8My1KGvtsU4",
        "internalCryptoAddress": null,
        "fromAddress": null,
        "toAddress": "TCT2pKJXo233hrKWQMeCptC8My1KGvtsU4",
        "status": "NEW",
        "currency": "TRX",
        "fee": null,
        "feePaymentEnabledByClient": false,
        "type": "AUTO",
        "comment": null
    },
    "fiatTransaction": {
        "status": "PROCESSING",
        "paymentToken": "59676b7a-da24-4916-8236-1efef6796009",
        "post": null,
        "brand": null,
        "internalToken": null,
        "orderIdentity": "c76ca74d-7aea-4688-942e-8385c413826f",
        "link": "c76ca74d-7aea-4688-942e-8385c413826f",
        "providerType": "CA",
        "paymentType": null,
        "processingBank": null,
        "resultMessage": null,
        "currency": "RUB",
        "processorTransactionNumber": null
    },
    "client": {
        "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562"
    },
    "serverDate": "2026-04-27T11:16:09+0000",
    "exchangeType": "SELL",
    "operationType": "FIAT_TO_CRYPTO",
    "orderType": "DEFAULT",
    "completionDate": null,
    "resultMessage": null,
    "submitByResident": null,
    "merchantName": "wb",
    "merchantBonus": null,
    "promoCodeDetails": null,
    "fromSource": "EXT",
    "toSource": "EXT",
    "expiresAtDate": null
}
```
### Step 8. Get current order (optional)
**GET** `/api/v2/exchange/merchant/order/current?clientId={{clientId}}`

Use this endpoint to fetch the current active order for a specific client.
Use the response for quick status restore when user returns to the flow.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Params

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| clientId | string | Yes | WhiteBird client identifier. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| body | none | - | GET endpoint without request body. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| id | string | Current active order id. |
| type | string | Order type. Allowed values: `BUY`, `SELL`, `SWAP`. |
| status | string | Current order lifecycle state. Allowed values: `NEW`, `PROCESSING`, `COMPLETED`, `EXPIRED`, `ERROR`. |
| creationDate | string | Order creation timestamp in server date-time format. |
| modificationDate | string | Last order update timestamp in server date-time format. |
| number | number | Internal order number. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| CLIENT_NOT_FOUND | 400 | `clientId` is invalid for the merchant. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation or client scope. |

**Response**
```json
{
  "id": "4dffdc69-7821-4007-94ad-522ad6a46117",
  "type": "BUY",
  "status": "PROCESSING",
  "creationDate": "2026-04-27T11:13:26.237357",
  "modificationDate": "2026-04-27T11:13:27.727824",
  "number": 831000003994
}
```
### Step 9. Get order history (optional)
**POST** `/api/v2/exchange/merchant/order/history`

Use this endpoint to fetch paged order history for a client.
Use the response to build transaction history screens and filtering/pagination UI.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Params

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| page | number | No | Page number (Spring pageable). |
| size | number | No | Page size. |
| sort | string | No | Sort format (default by `creationDate,DESC`). |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| clientId | string | Yes | WhiteBird client identifier. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| content | array of objects | Orders page content. |
| content[].id | string | Order identifier. |
| content[].status | string | Order status value. |
| totalElements | number | Total matched items. |
| totalPages | number | Total page count. |
| number | number | Current page number. |
| size | number | Current page size. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| CLIENT_NOT_FOUND | 400 | `clientId` is invalid for the merchant. |
| `400 Bad Request` | HTTP | Invalid pageable/filter request. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation or client scope. |

**Request**
```json
{
  "clientId": "{{clientId}}"
}
```
**Response**
```json
{
  "content": [
    {
      "id": "37a48ec9-150f-4c3f-88f8-7e8d48a2fd3b",
      "status": "PROCESSING"
    },
    {
      "id": "2cac7fd6-83e0-4d81-9532-85fb1bc14ca2",
      "status": "COMPLETED"
    },
    {
      "id": "7d8d3767-f403-4727-a9a0-9be71c563ac5",
      "status": "FAILED"
    }
  ],
  "totalElements": 33,
  "totalPages": 4,
  "number": 0,
  "size": 10
}
```

## 3. OffRamp (crypto -> fiat) — Merchant API

Below is a tested OffRamp flow with real request/response examples.

> All requests are merchant backend-to-backend using `x-api-key`.

### Headers
- `x-api-key: {{x-api-key}}`
---
### Step 1. Get available assets
**POST** `/api/v2/exchange/merchant/assets`

Use this endpoint to fetch available fiat and crypto assets for OffRamp flow in the current merchant context.
Use the response to validate selected source crypto and target fiat assets before requesting limits/quotes.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| destination | string | No | Flow destination filter. Use `SDK_EXCHANGE` for SDK OnRamp/OffRamp operations or `EXCHANGE` for direct merchant exchange operations, depending on merchant configuration. Allowed values: `EXCHANGE`, `SDK_EXCHANGE`, `ACCOUNTING`, `SDK_ACCOUNTING`, `SDK_CROSS`. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| fiatAssets | array of objects | List of fiat assets that can be shown to the client as available payment or payout currencies for OnRamp/OffRamp flows. |
| fiatAssets[].id | string | Asset identifier. |
| fiatAssets[].code | string | Fiat currency code. |
| cryptoAssets | array of objects | List of crypto assets/networks that can be used in deposit, withdrawal, buy, sell, or conversion flows. |
| cryptoAssets[].id | string | Asset identifier. |
| cryptoAssets[].code | string | Crypto currency code. |
| cryptoAssets[].network | string | Blockchain network name. |
| cryptoAssets[].protocol | string \| null | Token protocol (if tokenized asset). |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| CURRENCY_NOT_FOUND | 400 | Invalid/unknown asset mapping requested by merchant configuration. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation or client scope. |

**Response**
```json
{
  "fiatAssets": [
    { "id": "BYN", "code": "BYN" },
    { "id": "EUR", "code": "EUR" },
    { "id": "RUB", "code": "RUB" },
    { "id": "USD", "code": "USD" }
  ],
  "cryptoAssets": [
    { "id": "BNB", "code": "BNB", "network": "BNB" },
    { "id": "USDC_BNB", "code": "USDC", "network": "BNB", "protocol": "BEP-20" },
    { "id": "USDT_BNB", "code": "USDT", "network": "BNB", "protocol": "BEP-20" },
    { "id": "BTC", "code": "BTC", "network": "Bitcoin" },
    { "id": "AAVE", "code": "AAVE", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "ETH", "code": "ETH", "network": "Ethereum" },
    { "id": "LINK", "code": "LINK", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "PAXG", "code": "PAXG", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "UNI", "code": "UNI", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "USDC_ERC", "code": "USDC", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "USDT_ERC", "code": "USDT", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "XAUT", "code": "XAUT", "network": "Ethereum", "protocol": "ERC-20" },
    { "id": "TON", "code": "TON", "network": "Ton" },
    { "id": "USDT_TON", "code": "USDT", "network": "Ton", "protocol": "TON" },
    { "id": "TRX", "code": "TRX", "network": "Tron" },
    { "id": "USDT_TRC", "code": "USDT", "network": "Tron", "protocol": "TRC-20" }
  ]
}
```
### Step 2. Get payment providers
**POST** `/api/v2/exchange/merchant/payment/provider`

Use this endpoint to get payout providers available for the selected OffRamp context.
Use the response to select provider and payout corridor before requesting payment methods.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| clientId | string | No | WhiteBird client identifier in merchant context. |
| fiatAsset | string | No | Fiat asset code used for filtering providers. |
| orderType | string | No | Order direction. Allowed values: `BUY`, `SELL`, `SWAP`. |
| destination | string | No | Flow destination filter. Use `SDK_EXCHANGE` for SDK OnRamp/OffRamp operations or `EXCHANGE` for direct merchant exchange operations, depending on merchant configuration. Allowed values: `EXCHANGE`, `SDK_EXCHANGE`, `ACCOUNTING`, `SDK_ACCOUNTING`, `SDK_CROSS`. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| [].id | string | Provider identifier. |
| [].name | string | Provider display name. |
| [].addPaymentMethod | boolean | Indicates whether payment method binding/creation is available. |
| [].config.paymentSystems | array of objects | Supported payment systems/directions/currencies. |
| [].commissions | array of objects | Provider commission configuration. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| CLIENT_NOT_FOUND | 400 | `clientId` is invalid for the merchant. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation or client scope. |

**Request**
```json
{
  "clientId": "{{clientId}}",
  "fiatAsset": "RUB",
  "orderType": "SELL"
}
```
**Response**
```json
[
  {
    "id": "ASSIST",
    "name": "ASSIST",
    "addPaymentMethod": true,
    "config": {
      "paymentSystems": [
        {
          "paymentSystem": "VISA",
          "type": "PSP",
          "directions": [
            {
              "direction": "SELL",
              "currencies": [
                { "currency": "BYN" },
                { "currency": "USD" },
                { "currency": "EUR" },
                { "currency": "RUB" }
              ]
            },
            {
              "direction": "BUY",
              "currencies": [
                { "currency": "BYN" },
                { "currency": "USD" },
                { "currency": "EUR" },
                { "currency": "RUB" }
              ]
            }
          ]
        },
        {
          "paymentSystem": "BelCard",
          "type": "PSP",
          "directions": [
            {
              "direction": "SELL",
              "currencies": [{ "currency": "BYN", "countries": ["Belarus"] }]
            },
            {
              "direction": "BUY",
              "currencies": [{ "currency": "BYN", "countries": ["Belarus"] }]
            }
          ]
        }
      ]
    },
    "commissions": [
      { "bank": "OTHER", "buyCommission": "3,5-4,9", "sellCommission": "3,0-4,9" },
      { "bank": "BELARUSBANK", "buyCommission": "3,0", "sellCommission": "2,5" }
    ]
  },
  {
    "id": "MTS",
    "name": "MTS",
    "addPaymentMethod": true,
    "config": {
      "paymentSystems": [
        {
          "paymentSystem": "MIR",
          "type": "PSP",
          "directions": [
            {
              "direction": "SELL",
              "currencies": [{ "currency": "RUB", "countries": ["Russia"] }]
            }
          ]
        }
      ]
    },
    "commissions": [
      { "buyCommission": "2,5", "sellCommission": "2,0" },
      { "bank": "RF_CARDS", "destination": "EXCHANGE", "sellCommission": "2,0" }
    ]
  }
]
```
### Step 3. Get payment methods
**POST** `/api/v2/exchange/merchant/payment/method`

> Take payment method `id` and pass it as `paymentMethodToken` in quote creation. Use records with status=`ENABLED` and `isRestricted=false`.

Use this endpoint to get available payout methods for the selected OffRamp direction.
Use the response to select `paymentMethodToken` for quote and sell order creation.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| clientId | string | Yes | WhiteBird client identifier. |
| fiatAsset | string | No | Fiat asset filter. |
| orderType | string | No | Order direction. Allowed values: `BUY`, `SELL`, `SWAP`. |
| destination | string | No | Flow destination filter. Use `SDK_EXCHANGE` for SDK OnRamp/OffRamp operations or `EXCHANGE` for direct merchant exchange operations, depending on merchant configuration. Allowed values: `EXCHANGE`, `SDK_EXCHANGE`, `ACCOUNTING`, `SDK_ACCOUNTING`, `SDK_CROSS`. |
| providers | array<string> | No | Provider filter list. |
| isCrypto | boolean | No | Flag for crypto-only payment methods. |
| countryGroup | array of strings | No | Country group filter. Allowed values: `BELARUS`, `RUSSIA`, `FOREIGN`. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| [].id | string | Payment method token/id. |
| [].number | string | Masked card/account number. |
| [].brand | string | Payment brand. |
| [].providerId | string | Provider identifier. |
| [].providerType | string | Legacy provider type field. |
| [].status | string | Payment method status. |
| [].isRestricted | boolean | Restriction flag. |
| [].isCrypto | boolean | Indicates crypto payment method. |
| [].country | string | Country label. |
| [].currency | string | Payment currency code. |
| [].supportedCurrencies | array<string> | Supported fiat currencies for the method. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| CLIENT_NOT_FOUND | 400 | `clientId` is invalid for the merchant. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation or client scope. |

**Request**
```json
{
  "clientId": "{{clientId}}",
  "fiatAsset": "RUB",
  "orderType": "SELL"
}
```
**Response**
```json
[
    {
        "id": "8ff59a79-461a-4c6f-8f77-832cfa836c86",
        "number": "**** **** **** 1111",
        "brand": "VISA",
        "providerId": "ASSIST",
        "providerType": "ASSIST",
        "status": "ENABLED",
        "isRestricted": false,
        "isCrypto": false,
        "country": "Russia"
    },
    {
        "providerId": "VTB",
        "providerType": "VTB",
        "status": "ENABLED",
        "name": "VTB"
    },
    {
        "providerId": "BRAZINO",
        "providerType": "BRAZINO",
        "status": "ENABLED",
        "name": "BRAZINO"
    }
]
```
### Step 4. Calculate limits (v2)
**POST** `/api/v2/exchange/merchant/limit`

Use this endpoint to calculate min/max available amount for the selected OffRamp pair and payout method.
Use the response to validate the amount before quote creation.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| clientId | string | No | WhiteBird client identifier. |
| fromAsset | object | Yes | Source asset object. |
| fromAsset.code | string | Yes | Source asset code. |
| fromAsset.network | string \| null | No | Source asset network for crypto assets. |
| toAsset | object | Yes | Target asset object. |
| toAsset.code | string | Yes | Target asset code. |
| toAsset.network | string \| null | No | Target asset network for crypto assets. |
| paymentMethod | string | Yes | Payment provider type used for the operation, for example `CA` or `ASSIST`. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| asset | object | Asset used for limit values. |
| asset.code | string | Asset code. |
| asset.network | string \| null | Asset network if applicable. |
| min | number | Minimum allowed amount. |
| max | number | Maximum allowed amount. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| CURRENCY_NOT_FOUND | 400 | Invalid `fromAsset`/`toAsset` pair. |
| CLIENT_NOT_FOUND | 400 | `clientId` is invalid for the merchant. |
| `400 Bad Request` | HTTP | Request validation failed for one or more fields. |

**Request**
```json
{
    "clientId": "{{clientId}}",
    "toAsset": {
        "code": "RUB",
        "network": null
    },
    "fromAsset": {
        "code": "TRX",
        "network": "Tron"
        
    },
    "paymentMethod": "CA"
}
```
**Response**
```json
{
    "asset": {
        "id": "TRX",
        "code": "TRX",
        "network": "Tron"
    },
    "min": 9.985536,
    "max": 44884.20673
}
```
### Step 5. Create quote (v2)
**POST** `/api/v2/exchange/merchant/quote`

> From response take quoteId.
> [You can Check the request fields to understand how the calculation is performed.](./V2.md#quote-fields)

Use this endpoint to calculate executable OffRamp quote values for crypto-to-fiat exchange.
Use the response `quoteId` to create the sell order.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| clientId | string | No | WhiteBird client identifier. |
| fromAsset | object | Yes | Source asset object. |
| fromAsset.code | string | Yes | Source asset code. |
| fromAsset.network | string \| null | No | Source asset network for crypto assets. |
| fromAsset.amount | number | No | Source amount when quote is input-side based. |
| toAsset | object | Yes | Target asset object. |
| toAsset.code | string | Yes | Target asset code. |
| toAsset.network | string \| null | No | Target asset network for crypto assets. |
| toAsset.amount | number | No | Target amount when quote is output-side based. |
| paymentMethod | string | No | Payment provider type used for the operation, for example `CA` or `ASSIST`. |
| paymentMethodToken | string | No | Selected payment method token/id. |
| destinationCryptoAddress | string | No | Optional destination wallet context. |
| comment | string | No | Optional merchant comment. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| quoteId | string | Quote identifier used for order creation. |
| fromAsset | object | Resolved source asset and amount. |
| toAsset | object | Resolved target asset and amount. |
| rate | number | Final client rate. |
| plainRate | number | Market/base rate before final quote adjustments. |
| fee | object | Fee breakdown object. |
| fee.total | number | Total fee amount. |
| fee.service | number \| null | Service fee component. |
| fee.network | number \| null | Network/payment component. |
| fee.asset | string | Fee asset code. |
| expirationDate | string | Quote expiration timestamp in server date-time format. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| INVALID_QUOTE | 400 | Quote cannot be calculated for provided values. |
| CURRENCY_NOT_FOUND | 400 | Invalid asset or network. |
| CLIENT_NOT_FOUND | 400 | `clientId` is invalid for the merchant. |
| `400 Bad Request` | HTTP | Request validation failed for one or more fields. |

**Request**
```json
{
    "clientId": "{{clientId}}",
    "fromAsset": {
        "code": "TRX",
        "network": "Tron",
        "amount": "150"
    },
    "toAsset": {
        "code": "RUB",
        "network": null
    },

    "paymentMethod": "CA",
    "paymentMethodToken": "{{payment_token}}"
}
```
**Response**
```json
{
    "quoteId": "f8274e38-7f36-4d39-97ad-fabf10274b92",
    "fromAsset": {
        "code": "TRX",
        "network": "Tron",
        "amount": "150"
    },
    "toAsset": {
        "code": "RUB",
        "amount": "3955.59"
    },
    "rate": 26.3706,
    "plainRate": 26.7722,
    "fee": {
        "total": 60.24,
        "service": null,
        "network": 0,
        "asset": "RUB"
    },
    "expirationDate": "2026-04-27T12:15:27+0000"
}
```
### Step 6. Sell crypto (v2)
**GET** `/api/v2/exchange/merchant/sell?failureDepositAddress={{walletAddress}}&quoteId={{quoteId}}&sourceAddress={{walletAddress1}}`

> From response take orderId (id).

Use this endpoint to create a crypto-to-fiat order from an existing quote.
Use the response `id` as `orderId` for polling and status tracking.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Params

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| quoteId | string (UUID) | Yes | Quote identifier returned by quote API. |
| failureDepositAddress | string | No | Refund wallet if order fails before completion. |
| sourceAddress | string | No | Sender wallet address for compliance checks. |
| bankIdentifier | string | No | Optional bank identifier used by selected payment provider. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| body | none | - | GET endpoint without request body. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| id | string | Order identifier. |
| type | string | Order type. Allowed values: `BUY`, `SELL`, `SWAP`. |
| status | string | Current order lifecycle state. Allowed values: `NEW`, `PROCESSING`, `COMPLETED`, `EXPIRED`, `ERROR`. |
| creationDate | string | Order creation timestamp in server date-time format. |
| modificationDate | string | Last order update timestamp in server date-time format. |
| cryptoTransaction | object \| null | Crypto transaction summary (nullable at creation step). |
| expiresAtDate | string | Deposit/order expiration timestamp in server date-time format. |
| depositCryptoAddress | string | Address where user must send crypto for sell flow. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| QUOTE_NOT_FOUND | 400 | Quote does not exist or already expired. |
| INVALID_QUOTE | 400 | Quote is invalid for order creation. |
| AML_FRAUD_VALIDATION_ERROR | 400 | AML/fraud checks blocked order creation. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation or client scope. |

**Response**
```json
{
    "id": "da078a7a-b700-44f1-88f5-ec754baab3f3",
    "type": "SELL",
    "status": "PROCESSING",
    "creationDate": "2026-04-27T11:57:53.370917",
    "modificationDate": "2026-04-27T11:57:53.756792",
    "cryptoTransaction": null,
    "expiresAtDate": "2026-04-27T12:27:52.605110",
    "depositCryptoAddress": "TSaysSYUgBoKbtBgnXdxvNQPjkeXC1s9eM"
}
```
### Step 7. Get order by ID
**GET** `/api/v2/exchange/merchant/order?orderId={{Order_ID}}`

Use this endpoint to fetch complete OffRamp order details by `orderId`.
Use the response to track payout processing, crypto deposit state, and final status.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Params

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| orderId | string (UUID) | Yes | Order identifier returned by sell API. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| body | none | - | GET endpoint without request body. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| id | string | Order identifier. |
| type | string | Order type. Allowed values: `BUY`, `SELL`, `SWAP`. |
| status | string | Current order lifecycle state. Allowed values: `NEW`, `PROCESSING`, `COMPLETED`, `EXPIRED`, `ERROR`. |
| exchangeOperation | object | Exchange side details (input/output, rates, fees). |
| cryptoTransaction.status | string | Crypto transaction status. Allowed values: `NEW`, `PENDING_REVIEW`, `NOT_FOUND`, `REJECTED`, `TIMEOUT`, `INVALID_AMOUNT`, `ERROR`, `AML_ERROR`, `AML_BLOCKED`, `ARREST`, `SUBMITTING`, `SUBMITTED`, `PENDING`, `SELECTED`, `CONFIRMED`, `PENDING_RESOLVE`. |
| fiatTransaction.status | string | Fiat transaction status. Allowed values: `NEW`, `PENDING_REVIEW`, `REJECTED`, `TIMEOUT`, `DECLINED`, `INVALID_AMOUNT`, `ERROR`, `AML_BLOCKED`, `PENDING`, `PROCESSING`, `APPROVED`. |
| operationType | string | Internal operation type, for example `FIAT_TO_CRYPTO` or `CRYPTO_TO_FIAT`. |
| expiresAtDate | string \| null | Order expiration timestamp in server date-time format. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| ORDER_NOT_FOUND | 404 | Order not found or not accessible in merchant scope. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation or client scope. |

**Response**
```json
{
    "id": "da078a7a-b700-44f1-88f5-ec754baab3f3",
    "type": "SELL",
    "status": "PROCESSING",
    "creationDate": "2026-04-27T11:57:53.370917",
    "modificationDate": "2026-04-27T11:57:53.756792",
    "number": 551000004001,
    "exchangeOperation": {
        "inputCurrency": "TRX",
        "inputAsset": 150,
        "outputCurrency": "RUB",
        "outputAsset": 3956.8,
        "exchangeFeeAssetInFiat": 60.26,
        "bonusOutputAsset": null,
        "plainRatio": 26.7804,
        "ratio": 26.3787,
        "currencyPair": {
            "fromCurrency": "TRX",
            "toCurrency": "RUB"
        }
    },
    "cryptoTransaction": {
        "hash": null,
        "externalCryptoAddress": null,
        "internalCryptoAddress": "TSaysSYUgBoKbtBgnXdxvNQPjkeXC1s9eM",
        "fromAddress": null,
        "toAddress": "TSaysSYUgBoKbtBgnXdxvNQPjkeXC1s9eM",
        "status": "NEW",
        "currency": "TRX",
        "fee": null,
        "feePaymentEnabledByClient": false,
        "type": "AUTO",
        "comment": null
    },
    "fiatTransaction": {
        "status": "NEW",
        "paymentToken": "b6716f86-8d8c-45ff-8db0-2ae910d6f837",
        "post": null,
        "brand": null,
        "internalToken": null,
        "orderIdentity": "e7663e7f-f189-491a-877d-ec5fc0f6af4b",
        "link": null,
        "providerType": "CA",
        "paymentType": null,
        "processingBank": null,
        "resultMessage": null,
        "currency": "RUB",
        "processorTransactionNumber": null
    },
    "client": {
        "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562"
    },
    "serverDate": "2026-04-27T11:58:38+0000",
    "exchangeType": "BUY",
    "operationType": "CRYPTO_TO_FIAT",
    "orderType": "DEFAULT",
    "completionDate": null,
    "resultMessage": null,
    "submitByResident": null,
    "merchantName": "wb",
    "merchantBonus": null,
    "promoCodeDetails": null,
    "fromSource": "EXT",
    "toSource": "EXT",
    "expiresAtDate": "2026-04-27T12:27:52.605110"
}
```
### Step 8. Get current order (optional)
**GET** `/api/v2/exchange/merchant/order/current?clientId={{clientId}}`

Use this endpoint to fetch the current active OffRamp order for a client.
Use the response to restore flow state when user comes back to the session.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Params

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| clientId | string | Yes | WhiteBird client identifier. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| body | none | - | GET endpoint without request body. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| id | string | Current active order id. |
| status | string | Current order lifecycle state. Allowed values: `NEW`, `PROCESSING`, `COMPLETED`, `EXPIRED`, `ERROR`. |
| clientId | string | WhiteBird client identifier in the order payload. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| CLIENT_NOT_FOUND | 400 | `clientId` is invalid for the merchant. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation or client scope. |

**Response**
```json
{
  "id": "da078a7a-b700-44f1-88f5-ec754baab3f3",
  "status": "PROCESSING",
  "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562"
}
```
### Step 9. Get order history (optional)
**POST** `/api/v2/exchange/merchant/order/history`

Use this endpoint to fetch paged OffRamp order history for a client.
Use the response for history UI, status analytics, and reconciliation.

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment. |
| externalClientId | string | No | External client mapping identifier. |

### Params

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| page | number | No | Page number (Spring pageable). |
| size | number | No | Page size. |
| sort | string | No | Sort format (default by `creationDate,DESC`). |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| clientId | string | Yes | WhiteBird client identifier. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| content | array of objects | Orders page content. |
| content[].id | string | Order identifier. |
| content[].status | string | Order status value. |
| totalElements | number | Total matched items. |
| totalPages | number | Total page count. |
| number | number | Current page number. |
| size | number | Current page size. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| CLIENT_NOT_FOUND | 400 | `clientId` is invalid for the merchant. |
| `400 Bad Request` | HTTP | Invalid pageable/filter request. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this operation or client scope. |

**Request**
```json
{
  "clientId": "{{clientId}}"
}
```
**Response**
```json
{
  "content": [
    {
      "id": "8bdafd97-6e16-4146-89a5-750e09353584",
      "status": "COMPLETED"
    },
    {
      "id": "2cac7fd6-83e0-4d81-9532-85fb1bc14ca2",
      "status": "COMPLETED"
    },
    {
      "id": "7d8d3767-f403-4727-a9a0-9be71c563ac5",
      "status": "FAILED"
    }
  ],
  "totalElements": 33,
  "totalPages": 4,
  "number": 0,
  "size": 10
}
```


## Quote fields

If you see response:

```json
{
  "rate": 29.2528,
  "plainRate": 27.13,
  "fee": {
    "total": 3.75,
    "service": null,
    "network": 0.263,
    "asset": "RUB"
  }
}
```

### How these values are calculated:

- `plainRate` = base market rate (bid/ask) before final quote adjustments.
- `rate` (final client rate):
  - for SELL: `rate = toAmount / fromGrossAmount`
  - for BUY: `rate = fromGrossAmount / toAmount`
- percentage fee for any step: `fee = amount * percent / 100`
- reverse fee (when target amount is fixed): `fee = amount / (1 - percent) - amount`
- total fee by side:
  - `fromFeeAmount = fromPaymentFeeAmount + fromExchangeFeeAmount`
  - `toFeeAmount = toPaymentFeeAmount + toExchangeFeeAmount`
  - if input is FIAT: `fee.total = fromFeeAmount`
  - if input is CRYPTO: `fee.total = toFeeAmount`
- network fee in response:
  - if input is CRYPTO: `fee.network = fromPaymentFeeAmount`
  - if input is FIAT: `fee.network = toPaymentFeeAmount`
- `fee.service = null` (service part is not returned as separate value in this response format).

### Variables:

- `fromGrossAmount`: how much the client gives before deductions.
- `fromAmount`: amount left from input side after input-side fees.
- `toAmount`: final amount the client receives after output-side fees.
- `amount`: current amount used in fee formula on current calculation step.
- `percent`: fee percent configured for this fee rule (for example `2.5` means `2.5%`).
- `fromPaymentFeeAmount` / `toPaymentFeeAmount`: payment/transfer part of fee (card processor or blockchain/network).
- `fromExchangeFeeAmount` / `toExchangeFeeAmount`: exchange service part of fee.
