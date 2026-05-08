# OnRAMP | OffRAMP

OnRamp and OffRamp flows are used for external fiat-to-crypto and crypto-to-fiat operations.
OnRamp is used when the client pays fiat through a provider and receives crypto to an external wallet.
OffRamp is used when the client sends crypto and receives fiat through a provider.

> BASE_URL https://api.dev.wbdevel.net

## 1. OnRamp (fiat -> crypto)

OnRamp flow is used when the client pays fiat through a provider and receives crypto to an external wallet. The flow is quote first, then order creation.

### Step 1. Get available assets
Use this endpoint to retrieve available fiat and crypto assets for the merchant context. Use the response to build asset selectors and validate `code/network` pairs in downstream requests.

**POST** `/api/v2/exchange/merchant/assets`

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**
```json
{
  "fiatAssets": [
        {
            "id": "BYN",
            "code": "BYN"
        },
        {
            "id": "EUR",
            "code": "EUR"
        },
        {
            "id": "RUB",
            "code": "RUB"
        },
        {
            "id": "USD",
            "code": "USD"
        }
  ],
  "cryptoAssets": [
        {
            "id": "BTC",
            "code": "BTC",
            "network": "Bitcoin"
        },
        {
            "id": "AAVE",
            "code": "AAVE",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "ETH",
            "code": "ETH",
            "network": "Ethereum"
        },
        {
            "id": "LINK",
            "code": "LINK",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "PAXG",
            "code": "PAXG",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "UNI",
            "code": "UNI",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "USDC_ERC",
            "code": "USDC",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "USDT_ERC",
            "code": "USDT",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "XAUT",
            "code": "XAUT",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "TON",
            "code": "TON",
            "network": "Ton"
        },
        {
            "id": "USDT_TON",
            "code": "USDT",
            "network": "Ton",
            "protocol": "TON"
        },
        {
            "id": "TRX",
            "code": "TRX",
            "network": "Tron"
        },
        {
            "id": "USDT_TRC",
            "code": "USDT",
            "network": "Tron",
            "protocol": "TRC-20"
        },
        {
            "id": "BNB",
            "code": "BNB",
            "network": "BNB"
        },
        {
            "id": "USDT_BNB",
            "code": "USDT",
            "network": "BNB",
            "protocol": "BEP-20"
        },
        {
            "id": "USDC_BNB",
            "code": "USDC",
            "network": "BNB",
            "protocol": "BEP-20"
        },
        {
            "id": "SOL",
            "code": "SOL",
            "network": "SOLANA"
        },
        {
            "id": "USDT_SPL",
            "code": "USDT",
            "network": "SOLANA",
            "protocol": "SPL"
        },
        {
            "id": "USDC_SPL",
            "code": "USDC",
            "network": "SOLANA",
            "protocol": "SPL"
        }
  ]
}
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">destination</td>
      <td>string</td>
      <td>No</td>
      <td>Optional flow destination filter. Recommended value: EXCHANGE.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAssets</td>
      <td>array of objects</td>
      <td>List of fiat assets that can be shown to the client as available payment or payout currencies for OnRamp/OffRamp flows.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAssets[].id</td>
      <td>string</td>
      <td>Internal fiat asset identifier used in API requests and routing logic.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAssets[].code</td>
      <td>string</td>
      <td>Currency code that can be displayed to the client in UI.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAssets</td>
      <td>array of objects</td>
      <td>List of crypto assets/networks that can be used in deposit, withdrawal, buy, sell, or conversion flows.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAssets[].id</td>
      <td>string</td>
      <td>Internal crypto asset identifier used in API requests; may include network-specific suffixes such as USDT_TRC.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAssets[].code</td>
      <td>string</td>
      <td>Asset ticker displayed to the client; can differ from id when asset is network-specific.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAssets[].network</td>
      <td>string</td>
      <td>Blockchain network that must be used for sending or receiving this crypto asset.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAssets[].protocol</td>
      <td>string</td>
      <td>Token protocol shown to prevent sending funds through the wrong network.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CURRENCY_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Asset configuration contains unsupported or unknown asset mapping.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation.</td>
    </tr>
  </tbody>
</table>

### Step 2. Get payment providers
Use this endpoint to retrieve available fiat payment providers for the selected client and direction. Use the response to choose a provider route and validate supported currency/direction combinations.

**POST** `/api/v2/exchange/merchant/payment/provider`

**Headers**
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
  }
]
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>Optional external client identifier used for merchant/client mapping validation.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>No</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAsset</td>
      <td>string</td>
      <td>No</td>
      <td>Fiat code filter for provider routes.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderType</td>
      <td>string</td>
      <td>No</td>
      <td>Order direction. Allowed values: BUY, SELL, SWAP.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">destination</td>
      <td>string</td>
      <td>No</td>
      <td>Optional flow destination filter. Recommended value: EXCHANGE.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providers</td>
      <td>array of string</td>
      <td>No</td>
      <td>Optional list of allowed fiat providers.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">isCrypto</td>
      <td>boolean</td>
      <td>No</td>
      <td>Crypto-method filter.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">countryGroup</td>
      <td>array of string</td>
      <td>No</td>
      <td>Optional country group filter.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Provider identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">name</td>
      <td>string</td>
      <td>Provider display name.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">addPaymentMethod</td>
      <td>boolean</td>
      <td>Defines whether provider supports adding payment methods.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config</td>
      <td>object</td>
      <td>Provider routing configuration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems</td>
      <td>array of objects</td>
      <td>Payment systems list for provider.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].paymentSystem</td>
      <td>string</td>
      <td>Payment system name (for example VISA, MIR, BelCard).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].type</td>
      <td>string</td>
      <td>Provider channel type.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions</td>
      <td>array of objects</td>
      <td>Supported operation directions for this payment system.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].direction</td>
      <td>string</td>
      <td>Direction for payment system route (BUY/SELL).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies</td>
      <td>array of objects</td>
      <td>Supported currencies for selected direction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies[].currency</td>
      <td>string</td>
      <td>Fiat currency for this route.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies[].banks</td>
      <td>array of string</td>
      <td>Optional bank restrictions for this route.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies[].countries</td>
      <td>array of string</td>
      <td>Optional country restrictions for this route.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions</td>
      <td>array of objects</td>
      <td>Commission settings for provider.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions[].bank</td>
      <td>string</td>
      <td>Bank group key for commission row.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions[].buyCommission</td>
      <td>string</td>
      <td>Commission value/range for buy direction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions[].sellCommission</td>
      <td>string</td>
      <td>Commission value/range for sell direction.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Provided clientId is invalid or not linked to merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation.</td>
    </tr>
  </tbody>
</table>

### Step 3. Get payment methods
Use this endpoint to retrieve payment methods/tokens available for the selected provider context. Use the response to select `paymentMethodToken` with valid status and pass it to quote creation.

**POST** `/api/v2/exchange/merchant/payment/method`

> Take payment method `id` and pass it as `paymentMethodToken` in quote creation. Use records with `status=ENABLED` and `isRestricted=false`.

**Headers**
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
  }
]
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>Optional external client identifier used for merchant/client mapping validation.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>Yes</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAsset</td>
      <td>string</td>
      <td>No</td>
      <td>Fiat code filter for payment methods.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderType</td>
      <td>string</td>
      <td>No</td>
      <td>Order direction. Allowed values: BUY, SELL, SWAP.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">destination</td>
      <td>string</td>
      <td>No</td>
      <td>Optional flow destination filter. Recommended value: EXCHANGE.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providers</td>
      <td>array of string</td>
      <td>No</td>
      <td>Optional list of allowed fiat providers.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">isCrypto</td>
      <td>boolean</td>
      <td>No</td>
      <td>Filters crypto payment methods if supported by provider.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">countryGroup</td>
      <td>array of string</td>
      <td>No</td>
      <td>Optional country group filter.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Payment method token. Pass this value as paymentMethodToken in OnRamp/OffRamp quote requests.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">number</td>
      <td>string</td>
      <td>Masked payment method number shown to client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">brand</td>
      <td>string</td>
      <td>Payment brand, if applicable.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providerId</td>
      <td>string</td>
      <td>Payment provider identifier used in integrations and filters (for example ASSIST, CA, MTS).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providerType</td>
      <td>string</td>
      <td>Provider category/type returned by provider integration. Usually matches providerId for standard routes.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">name</td>
      <td>string</td>
      <td>Deprecated provider display field that may be returned by some integrations.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>Payment method status. Allowed values: ENABLED, DIRECTION_DISABLED, CURRENCY_DISABLED, UNKNOWN.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">isRestricted</td>
      <td>boolean</td>
      <td>Shows whether this payment method is restricted. Use only methods with isRestricted=false.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">isCrypto</td>
      <td>boolean</td>
      <td>Indicates crypto payment method record.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">country</td>
      <td>string</td>
      <td>Country associated with the payment method.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">currency</td>
      <td>string</td>
      <td>Payment currency code, if returned.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">supportedCurrencies</td>
      <td>array of string</td>
      <td>Supported fiat currencies for this method, if returned.</td>
    </tr>
  </tbody>
</table>

#**Payment method status values**

<table width="100%">
  <thead>
    <tr>
      <th width="220" style="word-break: break-word; white-space: normal;">Status</th>
      <th width="680">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">ENABLED</td>
      <td>Payment method can be used for the selected flow, direction, and currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">DIRECTION_DISABLED</td>
      <td>Payment method exists, but is not available for selected orderType.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">CURRENCY_DISABLED</td>
      <td>Payment method exists, but does not support selected fiatAsset.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">UNKNOWN</td>
      <td>Status cannot be resolved because required filters were not provided.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Provided clientId is invalid or not linked to merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation.</td>
    </tr>
  </tbody>
</table>

### Step 4. Calculate limits 
Use this endpoint to calculate min/max available amount for the selected pair and payment method.
Use the response to validate user input before quote creation.

**POST** `/api/v2/exchange/merchant/limit`

**Headers**
- `x-api-key: {{x-api-key}}`

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

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>No</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset</td>
      <td>object</td>
      <td>Yes</td>
      <td>Source asset object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.code</td>
      <td>string</td>
      <td>Yes</td>
      <td>Source asset code.</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.network</td>
      <td>string | null</td>
      <td>No</td>
      <td>Source asset network for crypto assets.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset</td>
      <td>object</td>
      <td>Yes</td>
      <td>Target asset object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.code</td>
      <td>string</td>
      <td>Yes</td>
      <td>Target asset code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.network</td>
      <td>string | null</td>
      <td>No</td>
      <td>Target asset network for crypto assets.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">paymentMethod</td>
      <td>string</td>
      <td>Yes</td>
      <td>Payment provider type used for the operation, for example CA or ASSIST.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">asset</td>
      <td>object</td>
      <td>Asset used for limit values.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">asset.id</td>
      <td>string</td>
      <td>Internal asset identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">asset.code</td>
      <td>string</td>
      <td>Asset code.</td>
    </tr>
  
    <tr>
      <td style="word-break: break-word; white-space: normal;">asset.network</td>
      <td>string | null</td>
      <td>Asset network if applicable.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">min</td>
      <td>number</td>
      <td>Minimum allowed amount.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">max</td>
      <td>number</td>
      <td>Maximum allowed amount.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CURRENCY_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Invalid fromAsset/toAsset pair.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Client id is invalid or not linked to the merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 Bad Request</td>
      <td>HTTP</td>
      <td>Request validation failed for one or more fields.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 5. Create quote 
Use this endpoint to calculate executable quote values (amounts, rate, fees, expiration).
Use the response `quoteId` as an input for buy/sell order creation.

**POST** `/api/v2/exchange/merchant/quote`

> From response take quoteId.

**Headers**
- `x-api-key: {{x-api-key}}`

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

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>No</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset</td>
      <td>object</td>
      <td>Yes</td>
      <td>Source asset object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.code</td>
      <td>string</td>
      <td>Yes</td>
      <td>Source asset code.</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.network</td>
      <td>string | null</td>
      <td>No</td>
      <td>Source asset network for crypto assets.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.amount</td>
      <td>number</td>
      <td>No</td>
      <td>Source amount when quote is input-side based.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset</td>
      <td>object</td>
      <td>Yes</td>
      <td>Target asset object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.code</td>
      <td>string</td>
      <td>Yes</td>
      <td>Target asset code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.network</td>
      <td>string | null</td>
      <td>No</td>
      <td>Target asset network for crypto assets.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.amount</td>
      <td>number</td>
      <td>No</td>
      <td>Target amount when quote is output-side based.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">paymentMethod</td>
      <td>string</td>
      <td>No</td>
      <td>Payment provider type used for the operation, for example CA or ASSIST.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">paymentMethodToken</td>
      <td>string</td>
      <td>No</td>
      <td>Selected payment method token/id.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">destinationCryptoAddress</td>
      <td>string</td>
      <td>No</td>
      <td>Destination wallet address for crypto-out flows (used when output.type is CRYPTO_TRANSFER).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">comment</td>
      <td>string</td>
      <td>No</td>
      <td>Used only for the TON network as a transfer memo for the recipient. For other networks the value is ignored.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">quoteId</td>
      <td>string</td>
      <td>Quote identifier used for order creation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset</td>
      <td>Quote identifier returned by quote creation; required to create an order before quote expiration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.code</td>
      <td>string</td>
      <td>Source asset code.</td>
    </tr>
  
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.network</td>
      <td>string | null</td>
      <td>Source asset network if returned.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.amount</td>
      <td>string</td>
      <td>Source amount used in quote calculation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset</td>
      <td>object</td>
      <td>Resolved target asset and amount.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.code</td>
      <td>string</td>
      <td>Target asset code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.network</td>
      <td>string | null</td>
      <td>Target asset network if returned.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.amount</td>
      <td>string</td>
      <td>Target amount resolved by quote calculation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">rate</td>
      <td>number</td>
      <td>Final client-facing rate applied to the quote/order. Show this value to the client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">plainRate</td>
      <td>Rate pair for the operation. Display this value to the client as the final pair label.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fee</td>
      <td>object</td>
      <td>Fee breakdown object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fee.total</td>
      <td>number</td>
      <td>Total fee amount in fee.asset currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fee.service</td>
      <td>number | null</td>
      <td>Service fee component in fee.asset currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fee.network</td>
      <td>number | null</td>
      <td>Network/payment component in fee.asset currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fee.asset</td>
      <td>string</td>
      <td>Asset code in which fee.total, fee.service, and fee.network are expressed.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">expirationDate</td>
      <td>string</td>
      <td>Quote expiration timestamp in server date-time format.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 INVALID_QUOTE</td>
      <td>BUSINESS</td>
      <td>Quote cannot be calculated for provided values.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CURRENCY_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Invalid asset or network.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Client id is invalid or not linked to the merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 Bad Request</td>
      <td>HTTP</td>
      <td>Request validation failed for one or more fields.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 6. Buy Crypto 
Use this endpoint to create a fiat-to-crypto order from a previously created quote.
Use the response `id` as `orderId` for status polling and order details retrieval.

**GET** `/api/v2/exchange/merchant/buy?destinationCryptoAddress={{walletAddress}}&quoteId={{quoteId}}`

> From response take orderId.

**Headers**
- `x-api-key: {{x-api-key}}`

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

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Params**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">quoteId</td>
      <td>string (UUID)</td>
      <td>Yes</td>
      <td>Quote identifier returned by quote creation; required to create an order before quote expiration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">destinationCryptoAddress</td>
      <td>string</td>
      <td>No</td>
      <td>Destination wallet address for crypto-out flows (used when output.type is CRYPTO_TRANSFER).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">comment</td>
      <td>string</td>
      <td>No</td>
      <td>Used only for the TON network as a transfer memo for the recipient. For other networks the value is ignored.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">returnUrl</td>
      <td>string</td>
      <td>No</td>
      <td>URL the client should be redirected to on successful payment flow.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">failUrl</td>
      <td>string</td>
      <td>No</td>
      <td>URL the client should be redirected to on failed payment flow.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">body</td>
      <td>none</td>
      <td>-</td>
      <td>GET endpoint without request body.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Order identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">type</td>
      <td>string</td>
      <td>Order type. Allowed values: BUY, SELL, SWAP.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>Current order lifecycle state. Allowed values: NEW, PROCESSING, COMPLETED, EXPIRED, ERROR.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">creationDate</td>
      <td>string</td>
      <td>Order creation timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">modificationDate</td>
      <td>string</td>
      <td>Last order update timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatPaymentLink</td>
      <td>string</td>
      <td>Payment link/token for fiat step processing.</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">expiresAtDate</td>
      <td>string | null</td>
      <td>Order expiration timestamp in server date-time format.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 QUOTE_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Quote does not exist or already expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 INVALID_QUOTE</td>
      <td>BUSINESS</td>
      <td>Quote cannot be calculated or cannot be used for order creation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 AML_FRAUD_VALIDATION_ERROR</td>
      <td>BUSINESS</td>
      <td>AML/fraud checks blocked order creation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 7. Get order by ID
Use this endpoint to fetch full order details by `orderId`.
Use the response to track all order phases (exchange, fiat transaction, crypto transaction) and status transitions.

**GET** `/api/v2/exchange/merchant/order?orderId={{Order_ID}}`

**Headers**
- `x-api-key: {{x-api-key}}`

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

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Params**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderId</td>
      <td>string (UUID)</td>
      <td>Yes</td>
      <td>Order identifier returned by buy/sell API.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">body</td>
      <td>none</td>
      <td>-</td>
      <td>GET endpoint without request body.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Order identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">type</td>
      <td>string</td>
      <td>Order type. Allowed values: BUY, SELL, SWAP.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>Current order lifecycle state. Allowed values: NEW, PROCESSING, COMPLETED, EXPIRED, ERROR.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">creationDate</td>
      <td>string</td>
      <td>Order creation timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">modificationDate</td>
      <td>string</td>
      <td>Last order update timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">number</td>
      <td>number</td>
      <td>Internal order number.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation</td>
      <td>object</td>
      <td>Exchange side details (input/output, rates, fees).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.inputCurrency</td>
      <td>string</td>
      <td>Source asset/currency code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.inputAsset</td>
      <td>number</td>
      <td>Source amount.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.outputCurrency</td>
      <td>string</td>
      <td>Destination asset/currency code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.outputAsset</td>
      <td>number</td>
      <td>Destination amount.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.exchangeFeeAssetInFiat</td>
      <td>number</td>
      <td>Exchange fee represented in fiat asset currency (exchangeOperation.inputCurrency / fiat leg currency).</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.bonusOutputAsset</td>
      <td>number | null</td>
      <td>Bonus amount, if promo bonus is applied.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.plainRatio</td>
      <td>number</td>
      <td>Base system rate at the moment of quote calculation. Used as a reference value.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.ratio</td>
      <td>number</td>
      <td>Final client-facing rate applied to the quote/order. Show this value to the client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.currencyPair</td>
      <td>object</td>
      <td>Currency pair metadata.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.currencyPair.fromCurrency</td>
      <td>string</td>
      <td>Pair source currency code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.currencyPair.toCurrency</td>
      <td>string</td>
      <td>Pair destination currency code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction</td>
      <td>object</td>
      <td>Crypto transfer details.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.hash</td>
      <td>string | null</td>
      <td>Blockchain transaction hash when available.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.externalCryptoAddress</td>
      <td>string | null</td>
      <td>External wallet address used in operation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.internalCryptoAddress</td>
      <td>string | null</td>
      <td>Internal wallet/deposit address used in operation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.fromAddress</td>
      <td>string | null</td>
      <td>Source blockchain address.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.toAddress</td>
      <td>string | null</td>
      <td>Destination blockchain address.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.status</td>
      <td>string</td>
      <td>Crypto transaction status. Allowed values: NEW, PENDING_REVIEW, NOT_FOUND, REJECTED, TIMEOUT, INVALID_AMOUNT, ERROR, AML_ERROR, AML_BLOCKED, ARREST, SUBMITTING, SUBMITTED, PENDING, SELECTED, CONFIRMED, PENDING_RESOLVE.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.currency</td>
      <td>string</td>
      <td>Crypto asset code used in transaction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.fee</td>
      <td>number | null</td>
      <td>Blockchain/network fee amount in cryptoTransaction.currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.feePaymentEnabledByClient</td>
      <td>boolean</td>
      <td>Whether client-paid network fee mode is enabled.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.type</td>
      <td>string</td>
      <td>Crypto transfer processing type.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.comment</td>
      <td>string | null</td>
      <td>Transaction comment, if provided.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction</td>
      <td>object</td>
      <td>Fiat processing details.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.status</td>
      <td>string</td>
      <td>Fiat transaction status. Allowed values: NEW, PENDING_REVIEW, REJECTED, TIMEOUT, DECLINED, INVALID_AMOUNT, ERROR, AML_BLOCKED, PENDING, PROCESSING, APPROVED.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.paymentToken</td>
      <td>string | null</td>
      <td>Payment method token used by provider leg.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.post</td>
      <td>string | null</td>
      <td>Provider postback payload/reference, if returned.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.brand</td>
      <td>string | null</td>
      <td>Card/payment brand returned by provider.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.internalToken</td>
      <td>string | null</td>
      <td>Internal provider token/reference.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.orderIdentity</td>
      <td>string | null</td>
      <td>Provider-side order reference used for reconciliation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.link</td>
      <td>string | null</td>
      <td>Provider payment link/reference.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.providerType</td>
      <td>string | null</td>
      <td>Fiat provider type.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.paymentType</td>
      <td>string | null</td>
      <td>Provider payment channel/type.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.processingBank</td>
      <td>string | null</td>
      <td>Processing bank, if returned by provider.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.resultMessage</td>
      <td>string | null</td>
      <td>Provider processing message.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.currency</td>
      <td>string | null</td>
      <td>Fiat currency used by provider transaction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.processorTransactionNumber</td>
      <td>string | null</td>
      <td>Provider processor transaction identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">client</td>
      <td>object</td>
      <td>Client scope details.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">client.clientId</td>
      <td>string</td>
      <td>Client identifier used for request scoping.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">operationType</td>
      <td>string</td>
      <td>Internal operation type, for example FIAT_TO_CRYPTO or CRYPTO_TO_FIAT.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeType</td>
      <td>string</td>
      <td>Exchange direction as internal enum value.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderType</td>
      <td>string</td>
      <td>Internal order subtype.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">submitByResident</td>
      <td>boolean | null</td>
      <td>Resident submission flag, if applicable.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">merchantName</td>
      <td>string</td>
      <td>Merchant name associated with order.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">merchantBonus</td>
      <td>number | null</td>
      <td>Merchant bonus amount, if applicable.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">promoCodeDetails</td>
      <td>string | null</td>
      <td>Promo code details payload, if applied.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromSource</td>
      <td>string</td>
      <td>Source side enum value (EXT/INT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toSource</td>
      <td>string</td>
      <td>Destination side enum value (EXT/INT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">serverDate</td>
      <td>string</td>
      <td>Server timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">completionDate</td>
      <td>string | null</td>
      <td>Completion timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">resultMessage</td>
      <td>string | null</td>
      <td>Processing message.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">expiresAtDate</td>
      <td>string | null</td>
      <td>Order expiration timestamp in server date-time format.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">404 ORDER_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Order not found or not accessible in merchant scope.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 8. Get current order (optional)
Use this endpoint to fetch the current active order for a specific client.
Use the response for quick status restore when user returns to the flow.

**GET** `/api/v2/exchange/merchant/order/current?clientId={{clientId}}`

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**
```json
{
    "id": "a1fcd0f0-8286-4015-914d-04db79921435",
  "type": "BUY",
  "status": "PROCESSING",
    "creationDate": "2026-05-06T17:48:38.699568",
    "modificationDate": "2026-05-06T17:48:42.242808",
    "cryptoTransaction": null,
    "expiresAtDate": "2026-05-06T18:18:37.181180",
    "depositCryptoAddress": "TVEwq1PiFDfJKYvWiDFhVXQkzDwqWCyPXV"
}
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Params**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>Yes</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">body</td>
      <td>none</td>
      <td>-</td>
      <td>GET endpoint without request body.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Current active order id.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">type</td>
      <td>string</td>
      <td>Order type. Allowed values: BUY, SELL, SWAP.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>Current order lifecycle state. Allowed values: NEW, PROCESSING, COMPLETED, EXPIRED, ERROR.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">creationDate</td>
      <td>string</td>
      <td>Order creation timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">modificationDate</td>
      <td>string</td>
      <td>Last order update timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">number</td>
      <td>number</td>
      <td>Internal order number.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Client id is invalid or not linked to the merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 9. Get order history (optional)
Use this endpoint to fetch paged order history for a client.
Use the response to build transaction history screens and filtering/pagination UI.

**POST** `/api/v2/exchange/merchant/order/history`

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**
```json
{
  "clientId": "{{clientId}}",
  "statuses": [
    "COMPLETED"
  ],
  "types": [
    "BUY"
  ],
  "providerTypes": [
    "ASSIST"
  ],
  "number": 491000004334
}
```
**Response**
```json
{
  "content": [
    {
            "id": "f8e67902-6dd3-4554-8f50-cd0a5c8a894b",
            "type": "BUY",
            "status": "COMPLETED",
            "creationDate": "2026-05-06T17:44:59.534092",
            "modificationDate": "2026-05-06T17:47:30.210422",
            "number": 491000004334,
            "exchangeOperation": {
                "inputCurrency": "BYN",
                "inputAsset": 50,
                "outputCurrency": "TRX",
                "outputAsset": 44.692189,
                "exchangeFeeAssetInFiat": 3.35,
                "bonusOutputAsset": null,
                "plainRatio": 1.0377,
                "ratio": 1.1188,
                "currencyPair": {
                    "fromCurrency": "BYN",
                    "toCurrency": "TRX"
                }
            },
            "cryptoTransaction": {
                "hash": "8bd6f85d606d9511d28471a91b72dce634b61b51b4bcef45fb5c29f5e4d94875",
                "externalCryptoAddress": "TCT2pKJXo233hrKWQMeCptC8My1KGvtsU4",
                "internalCryptoAddress": "TVEwq1PiFDfJKYvWiDFhVXQkzDwqWCyPXV",
                "fromAddress": "TVEwq1PiFDfJKYvWiDFhVXQkzDwqWCyPXV",
                "toAddress": "TCT2pKJXo233hrKWQMeCptC8My1KGvtsU4",
                "status": "CONFIRMED",
                "currency": "TRX",
                "fee": "0.268",
                "feePaymentEnabledByClient": false,
                "type": "AUTO",
                "comment": null
            },
            "fiatTransaction": {
                "status": "APPROVED",
                "paymentToken": "fc4b130e-c3bf-4a3d-abe5-9ec5900c9868",
                "post": "**** **** **** 1111",
                "brand": "VISA",
                "internalToken": "97fe9aa7-7805-438f-8c5e-aea24b4f9dc4",
                "orderIdentity": "97e3d1f09aed4442a793fb5eace5582b",
                "link": "https://payments.t.paysecure.ru/pay/p2p/cc2mc.cfm...",
                "providerType": "ASSIST",
                "paymentType": "P2P",
                "processingBank": "BELARUSBANK",
                "resultMessage": null,
                "currency": "BYN",
                "processorTransactionNumber": null
            },
            "client": {
                "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562"
            },
            "serverDate": "2026-05-08T08:24:42+0000",
            "exchangeType": "SELL",
            "operationType": "FIAT_TO_CRYPTO",
            "orderType": "DEFAULT",
            "completionDate": "2026-05-06T17:47:28+0000",
            "resultMessage": null,
            "submitByResident": null,
            "merchantName": "wb",
            "merchantBonus": null,
            "promoCodeDetails": null,
            "fromSource": "EXT",
            "toSource": "EXT",
            "expiresAtDate": null
        }
  ],
  "pageable": {
    "sort": {
      "unsorted": false,
      "sorted": true,
      "empty": false
    },
    "pageNumber": 0,
    "pageSize": 10,
    "offset": 0,
    "paged": true,
    "unpaged": false
  },
  "totalElements": 52,
  "totalPages": 6,
  "last": false,
  "numberOfElements": 10,
  "size": 10,
  "number": 0,
  "sort": {
    "unsorted": false,
    "sorted": true,
    "empty": false
  },
  "first": true,
  "empty": false
}
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Params**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">page</td>
      <td>number</td>
      <td>No</td>
      <td>Page number (Spring pageable).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">size</td>
      <td>number</td>
      <td>No</td>
      <td>Page size.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">sort</td>
      <td>string</td>
      <td>No</td>
      <td>Sort format (default by creationDate,DESC).</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>Yes</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">statuses</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by order status. Allowed values: PROCESSING, EXPIRED, COMPLETED, FAILED.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">types</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by order type values (BUY, SELL, SWAP).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderTypes</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by exchange direction types.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">operationTypes</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by operation direction (FIAT_TO_CRYPTO, CRYPTO_TO_FIAT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providerTypes</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by fiat provider ids (ASSIST, CA, MTS).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatStatuses</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by fiat transaction statuses.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoStatuses</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by crypto transaction statuses.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">creationDateFrame</td>
      <td>object</td>
      <td>No</td>
      <td>Creation date range filter.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">completionDateFrame</td>
      <td>object</td>
      <td>No</td>
      <td>Completion date range filter.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderId</td>
      <td>string</td>
      <td>No</td>
      <td>Filter by specific order id (UUID).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">number</td>
      <td>number</td>
      <td>No</td>
      <td>Filter by numeric order number.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">inputAssets</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by source asset codes.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">outputAssets</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by destination asset codes.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">assets</td>
      <td>array of string</td>
      <td>No</td>
      <td>Asset filter applied to either source or destination leg.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAddress</td>
      <td>string</td>
      <td>No</td>
      <td>Filter by crypto address involved in transaction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromSource</td>
      <td>string</td>
      <td>No</td>
      <td>Filter by source type (EXT/INT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toSource</td>
      <td>string</td>
      <td>No</td>
      <td>Filter by destination type (EXT/INT).</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content</td>
      <td>array of objects</td>
      <td>Orders page content.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].id</td>
      <td>string</td>
      <td>Entry identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].type</td>
      <td>string</td>
      <td>Order type (BUY/SELL).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].status</td>
      <td>string</td>
      <td>Order status value (NEW, PROCESSING, COMPLETED, EXPIRED, ERROR).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].creationDate</td>
      <td>string</td>
      <td>Order creation timestamp.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].modificationDate</td>
      <td>string</td>
      <td>Order last modification timestamp.</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].number</td>
      <td>number</td>
      <td>Human-readable order number.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].exchangeOperation</td>
      <td>object</td>
      <td>Exchange operation details (input/output assets, ratio, fees).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].cryptoTransaction</td>
      <td>object | null</td>
      <td>Crypto transaction details (addresses, hash, status, fee, type, comment).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].fiatTransaction</td>
      <td>object | null</td>
      <td>Fiat transaction details (provider, status, payment metadata).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].client</td>
      <td>object</td>
      <td>Client block with clientId.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].exchangeType</td>
      <td>string</td>
      <td>Exchange direction (BUY/SELL/SWAP).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].operationType</td>
      <td>string</td>
      <td>Operation type (FIAT_TO_CRYPTO/CRYPTO_TO_FIAT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].orderType</td>
      <td>string</td>
      <td>Order subtype (for example DEFAULT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].expiresAtDate</td>
      <td>string | null</td>
      <td>Order expiration timestamp.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].clientId</td>
      <td>string</td>
      <td>Client identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].userId</td>
      <td>string | null</td>
      <td>User identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].creationDate</td>
      <td>string</td>
      <td>Creation timestamp.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].completionDate</td>
      <td>string | null</td>
      <td>Completion timestamp.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable</td>
      <td>object</td>
      <td>Spring pageable metadata object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.pageNumber</td>
      <td>number</td>
      <td>Current page number.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.pageSize</td>
      <td>number</td>
      <td>Current page size.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.offset</td>
      <td>number</td>
      <td>Current page offset.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.paged</td>
      <td>boolean</td>
      <td>Whether pageable mode is enabled.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.unpaged</td>
      <td>boolean</td>
      <td>Whether unpaged mode is enabled.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.sort</td>
      <td>object</td>
      <td>Pageable sort metadata.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.sort.unsorted</td>
      <td>boolean</td>
      <td>True when pageable sort is not set.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.sort.sorted</td>
      <td>boolean</td>
      <td>True when pageable sort is set.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.sort.empty</td>
      <td>boolean</td>
      <td>True when pageable sort metadata is empty.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">totalElements</td>
      <td>number</td>
      <td>Total number of matching orders.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">totalPages</td>
      <td>number</td>
      <td>Total number of pages.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">last</td>
      <td>boolean</td>
      <td>True when this page is the last page.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">numberOfElements</td>
      <td>number</td>
      <td>Number of elements in current page.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">number</td>
      <td>number</td>
      <td>Current page number.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">size</td>
      <td>number</td>
      <td>Current page size.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">sort</td>
      <td>object</td>
      <td>Top-level sort metadata.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">sort.unsorted</td>
      <td>boolean</td>
      <td>True when top-level sort is not set.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">sort.sorted</td>
      <td>boolean</td>
      <td>True when top-level sort is set.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">sort.empty</td>
      <td>boolean</td>
      <td>True when top-level sort metadata is empty.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">first</td>
      <td>boolean</td>
      <td>True when this page is the first page.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">empty</td>
      <td>boolean</td>
      <td>True when page content is empty.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Client id is invalid or not linked to the merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 Bad Request</td>
      <td>HTTP</td>
      <td>Invalid pageable/filter request.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>


## 2. OffRamp (crypto -> fiat) — Merchant API

### Step 1. Get available assets
Use this endpoint to fetch available fiat and crypto assets for OffRamp flow in the current merchant context.
Use the response to validate selected source crypto and target fiat assets before requesting limits/quotes.

**POST** `/api/v2/exchange/merchant/assets`

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**
```json
{
  "fiatAssets": [
        {
            "id": "BYN",
            "code": "BYN"
        },
        {
            "id": "EUR",
            "code": "EUR"
        },
        {
            "id": "RUB",
            "code": "RUB"
        },
        {
            "id": "USD",
            "code": "USD"
        }
  ],
  "cryptoAssets": [
        {
            "id": "BTC",
            "code": "BTC",
            "network": "Bitcoin"
        },
        {
            "id": "AAVE",
            "code": "AAVE",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "ETH",
            "code": "ETH",
            "network": "Ethereum"
        },
        {
            "id": "LINK",
            "code": "LINK",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "PAXG",
            "code": "PAXG",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "UNI",
            "code": "UNI",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "USDC_ERC",
            "code": "USDC",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "USDT_ERC",
            "code": "USDT",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "XAUT",
            "code": "XAUT",
            "network": "Ethereum",
            "protocol": "ERC-20"
        },
        {
            "id": "TON",
            "code": "TON",
            "network": "Ton"
        },
        {
            "id": "USDT_TON",
            "code": "USDT",
            "network": "Ton",
            "protocol": "TON"
        },
        {
            "id": "TRX",
            "code": "TRX",
            "network": "Tron"
        },
        {
            "id": "USDT_TRC",
            "code": "USDT",
            "network": "Tron",
            "protocol": "TRC-20"
        },
        {
            "id": "BNB",
            "code": "BNB",
            "network": "BNB"
        },
        {
            "id": "USDT_BNB",
            "code": "USDT",
            "network": "BNB",
            "protocol": "BEP-20"
        },
        {
            "id": "USDC_BNB",
            "code": "USDC",
            "network": "BNB",
            "protocol": "BEP-20"
        },
        {
            "id": "SOL",
            "code": "SOL",
            "network": "SOLANA"
        },
        {
            "id": "USDT_SPL",
            "code": "USDT",
            "network": "SOLANA",
            "protocol": "SPL"
        },
        {
            "id": "USDC_SPL",
            "code": "USDC",
            "network": "SOLANA",
            "protocol": "SPL"
        }
  ]
}
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">destination</td>
      <td>string</td>
      <td>No</td>
      <td>Optional flow destination filter. Recommended value: EXCHANGE.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAssets</td>
      <td>array of objects</td>
      <td>List of fiat assets that can be shown to the client as available payment or payout currencies for OnRamp/OffRamp flows.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAssets[].id</td>
      <td>string</td>
      <td>Internal fiat asset identifier used in API requests and routing logic.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAssets[].code</td>
      <td>string</td>
      <td>Currency code that can be displayed to the client in UI.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAssets</td>
      <td>array of objects</td>
      <td>List of crypto assets/networks that can be used in deposit, withdrawal, buy, sell, or conversion flows.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAssets[].id</td>
      <td>string</td>
      <td>Internal crypto asset identifier used in API requests; may include network-specific suffixes such as USDT_TRC.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAssets[].code</td>
      <td>string</td>
      <td>Asset ticker displayed to the client; can differ from id when asset is network-specific.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAssets[].network</td>
      <td>string</td>
      <td>Blockchain network that must be used for sending or receiving this crypto asset.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAssets[].protocol</td>
      <td>string</td>
      <td>Token protocol shown to prevent sending funds through the wrong network.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CURRENCY_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Invalid/unknown asset mapping requested by merchant configuration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 2. Get payment providers
Use this endpoint to get payout providers available for the selected OffRamp context.
Use the response to select provider and payout corridor before requesting payment methods.

**POST** `/api/v2/exchange/merchant/payment/provider`

**Headers**
- `x-api-key: {{x-api-key}}`

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
              "currencies": [
                                {
                                    "currency": "RUB",
                                    "countries": [
                                        "Russia"
                                    ]
                                }
                            ]
            }
          ]
        }
      ]
    },
    "commissions": [
            {
                "buyCommission": "2,5",
                "sellCommission": "2,0"
            },
            {
                "destination": "EXCHANGE",
                "buyCommission": "2,5"
            },
            {
                "destination": "SDK_EXCHANGE",
                "buyCommission": "2,5",
                "sellCommission": "2,0"
            },
            {
                "destination": "ACCOUNTING",
                "buyCommission": "0"
            },
            {
                "bank": "RF_CARDS",
                "destination": "EXCHANGE",
                "sellCommission": "2,0"
            },
            {
                "bank": "RF_CARDS",
                "destination": "ACCOUNTING",
                "sellCommission": "1,5"
            }
    ]
  }
]
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>No</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAsset</td>
      <td>string</td>
      <td>No</td>
      <td>Fiat asset code used for filtering providers.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderType</td>
      <td>string</td>
      <td>No</td>
      <td>Order direction. Allowed values: BUY, SELL, SWAP.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">destination</td>
      <td>string</td>
      <td>No</td>
      <td>Optional flow destination filter. Recommended value: EXCHANGE.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providers</td>
      <td>array of string</td>
      <td>No</td>
      <td>Optional list of allowed fiat providers.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">isCrypto</td>
      <td>boolean</td>
      <td>No</td>
      <td>Crypto-method filter.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">countryGroup</td>
      <td>array of string</td>
      <td>No</td>
      <td>Optional country group filter.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Provider identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">name</td>
      <td>string</td>
      <td>Provider display name.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">addPaymentMethod</td>
      <td>boolean</td>
      <td>Indicates whether payment method binding/creation is available.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config</td>
      <td>object</td>
      <td>Provider routing configuration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems</td>
      <td>array of objects</td>
      <td>Supported payment systems/directions/currencies.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].paymentSystem</td>
      <td>string</td>
      <td>Payment system name (for example VISA, MIR, BelCard).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].type</td>
      <td>string</td>
      <td>Provider channel type.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions</td>
      <td>array of objects</td>
      <td>Supported operation directions for this payment system.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].direction</td>
      <td>string</td>
      <td>Direction for payment system route (BUY/SELL).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies</td>
      <td>array of objects</td>
      <td>Supported currencies for selected direction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies[].currency</td>
      <td>string</td>
      <td>Fiat currency for this route.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies[].banks</td>
      <td>array of strings</td>
      <td>Optional bank restrictions for this route.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies[].countries</td>
      <td>array of strings</td>
      <td>Optional country restrictions for this route.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions</td>
      <td>array of objects</td>
      <td>Provider commission configuration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions[].bank</td>
      <td>string</td>
      <td>Bank group key for commission row.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions[].destination</td>
      <td>string</td>
      <td>Optional destination scope for commission row.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions[].buyCommission</td>
      <td>string</td>
      <td>Commission value/range for buy direction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions[].sellCommission</td>
      <td>string</td>
      <td>Commission value/range for sell direction.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Client id is invalid or not linked to the merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 3. Get payment methods
Use this endpoint to get available payout methods for the selected OffRamp direction.
Use the response to select `paymentMethodToken` for quote and sell order creation.

**POST** `/api/v2/exchange/merchant/payment/method`

> Take payment method `id` and pass it as `paymentMethodToken` in quote creation. Use records with status=`ENABLED` and `isRestricted=false`.

**Headers**
- `x-api-key: {{x-api-key}}`

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
    }
]
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>Yes</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAsset</td>
      <td>string</td>
      <td>No</td>
      <td>Fiat asset filter.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderType</td>
      <td>string</td>
      <td>No</td>
      <td>Order direction. Allowed values: BUY, SELL, SWAP.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">destination</td>
      <td>string</td>
      <td>No</td>
      <td>Optional flow destination filter. Recommended value: EXCHANGE.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providers</td>
      <td>array of string</td>
      <td>No</td>
      <td>Optional list of allowed fiat providers.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">isCrypto</td>
      <td>boolean</td>
      <td>No</td>
      <td>Flag for crypto-only payment methods.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">countryGroup</td>
      <td>array of strings</td>
      <td>No</td>
      <td>Optional country group filter.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Payment method token. Pass this value as paymentMethodToken in OnRamp/OffRamp quote requests.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">number</td>
      <td>string</td>
      <td>Masked payment method number shown to client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">brand</td>
      <td>string</td>
      <td>Payment method brand, for example VISA.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providerId</td>
      <td>string</td>
      <td>Payment provider identifier used in integrations and filters (for example ASSIST, CA, MTS).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providerType</td>
      <td>string</td>
      <td>Provider category/type returned by provider integration. Usually matches providerId for standard routes.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">name</td>
      <td>string</td>
      <td>Deprecated provider display field that may be returned by some integrations.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>Payment method status. Allowed values: ENABLED, DIRECTION_DISABLED, CURRENCY_DISABLED, UNKNOWN.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">isRestricted</td>
      <td>boolean</td>
      <td>Shows whether this payment method is restricted. Use only methods with isRestricted=false.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">isCrypto</td>
      <td>boolean</td>
      <td>Indicates crypto payment method.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">country</td>
      <td>string</td>
      <td>Payment method country.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">currency</td>
      <td>string</td>
      <td>Primary fiat currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">supportedCurrencies</td>
      <td>array of strings</td>
      <td>Fiat currencies supported by this payment method.</td>
    </tr>
  </tbody>
</table>

#**Payment method status values**

<table width="100%">
  <thead>
    <tr>
      <th width="220" style="word-break: break-word; white-space: normal;">Status</th>
      <th width="680">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">ENABLED</td>
      <td>Payment method can be used for the selected flow, direction, and currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">DIRECTION_DISABLED</td>
      <td>Payment method exists, but is not available for selected orderType.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">CURRENCY_DISABLED</td>
      <td>Payment method exists, but does not support selected fiatAsset.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">UNKNOWN</td>
      <td>Status cannot be resolved because required filters were not provided.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Client id is invalid or not linked to the merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 4. Calculate limits 
Use this endpoint to calculate min/max available amount for the selected OffRamp pair and payout method.
Use the response to validate the amount before quote creation.

**POST** `/api/v2/exchange/merchant/limit`

**Headers**
- `x-api-key: {{x-api-key}}`

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

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>No</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset</td>
      <td>object</td>
      <td>Yes</td>
      <td>Source asset object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.code</td>
      <td>string</td>
      <td>Yes</td>
      <td>Source asset code.</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.network</td>
      <td>string | null</td>
      <td>No</td>
      <td>Source asset network for crypto assets.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset</td>
      <td>object</td>
      <td>Yes</td>
      <td>Target asset object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.code</td>
      <td>string</td>
      <td>Yes</td>
      <td>Target asset code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.network</td>
      <td>string | null</td>
      <td>No</td>
      <td>Target asset network for crypto assets.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">paymentMethod</td>
      <td>string</td>
      <td>Yes</td>
      <td>Payment provider type used for the operation, for example CA or ASSIST.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">asset</td>
      <td>object</td>
      <td>Asset used for limit values.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">asset.id</td>
      <td>string</td>
      <td>Internal asset identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">asset.code</td>
      <td>string</td>
      <td>Asset code.</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">asset.network</td>
      <td>string | null</td>
      <td>Asset network if applicable.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">min</td>
      <td>number</td>
      <td>Minimum allowed amount.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">max</td>
      <td>number</td>
      <td>Maximum allowed amount.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CURRENCY_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Invalid fromAsset/toAsset pair.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Client id is invalid or not linked to the merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 Bad Request</td>
      <td>HTTP</td>
      <td>Request validation failed for one or more fields.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 5. Create quote 
Use this endpoint to calculate executable OffRamp quote values for crypto-to-fiat exchange.
Use the response `quoteId` to create the sell order.

**POST** `/api/v2/exchange/merchant/quote`

> From response take quoteId.

**Headers**
- `x-api-key: {{x-api-key}}`

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

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>No</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset</td>
      <td>object</td>
      <td>Yes</td>
      <td>Source asset object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.code</td>
      <td>string</td>
      <td>Yes</td>
      <td>Source asset code.</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.network</td>
      <td>string | null</td>
      <td>No</td>
      <td>Source asset network for crypto assets.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.amount</td>
      <td>number</td>
      <td>No</td>
      <td>Source amount when quote is input-side based.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset</td>
      <td>object</td>
      <td>Yes</td>
      <td>Target asset object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.code</td>
      <td>string</td>
      <td>Yes</td>
      <td>Target asset code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.network</td>
      <td>string | null</td>
      <td>No</td>
      <td>Target asset network for crypto assets.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.amount</td>
      <td>number</td>
      <td>No</td>
      <td>Target amount when quote is output-side based.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">paymentMethod</td>
      <td>string</td>
      <td>No</td>
      <td>Payment provider type used for the operation, for example CA or ASSIST.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">paymentMethodToken</td>
      <td>string</td>
      <td>No</td>
      <td>Selected payment method token/id.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">destinationCryptoAddress</td>
      <td>string</td>
      <td>No</td>
      <td>Destination wallet address for crypto-out flows (used when output.type is CRYPTO_TRANSFER).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">comment</td>
      <td>string</td>
      <td>No</td>
      <td>Optional. Used only for the TON network as a transfer memo for the recipient. For other networks the value is ignored. Format: free-form string provided by the receiving party (exchange/wallet).</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">quoteId</td>
      <td>string</td>
      <td>Quote identifier used for order creation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset</td>
      <td>Quote identifier returned by quote creation; required to create an order before quote expiration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.code</td>
      <td>string</td>
      <td>Source asset code.</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.network</td>
      <td>string | null</td>
      <td>Source asset network if returned.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromAsset.amount</td>
      <td>string</td>
      <td>Source amount used in quote calculation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset</td>
      <td>object</td>
      <td>Resolved target asset and amount.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.code</td>
      <td>string</td>
      <td>Target asset code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.network</td>
      <td>string | null</td>
      <td>Target asset network if returned.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toAsset.amount</td>
      <td>string</td>
      <td>Target amount resolved by quote calculation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">rate</td>
      <td>number</td>
      <td>Final client-facing rate applied to the quote/order. Show this value to the client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">plainRate</td>
      <td>Rate pair for the operation. Display this value to the client as the final pair label.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fee</td>
      <td>object</td>
      <td>Fee breakdown object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fee.total</td>
      <td>number</td>
      <td>Total fee amount in fee.asset currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fee.service</td>
      <td>number | null</td>
      <td>Service fee component in fee.asset currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fee.network</td>
      <td>number | null</td>
      <td>Network/payment component in fee.asset currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fee.asset</td>
      <td>string</td>
      <td>Asset code in which fee.total, fee.service, and fee.network are expressed.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">expirationDate</td>
      <td>string</td>
      <td>Quote expiration timestamp in server date-time format.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 INVALID_QUOTE</td>
      <td>BUSINESS</td>
      <td>Quote cannot be calculated for provided values.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CURRENCY_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Invalid asset or network.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Client id is invalid or not linked to the merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 Bad Request</td>
      <td>HTTP</td>
      <td>Request validation failed for one or more fields.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 6. Sell Crypto 
Use this endpoint to create a crypto-to-fiat order from an existing quote.
Use the response `id` as `orderId` for polling and status tracking.

**GET** `/api/v2/exchange/merchant/sell?failureDepositAddress={{walletAddress}}&quoteId={{quoteId}}&sourceAddress={{walletAddress1}}`

> From response take orderId (id).

**Headers**
- `x-api-key: {{x-api-key}}`

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

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Params**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">quoteId</td>
      <td>string (UUID)</td>
      <td>Yes</td>
      <td>Quote identifier returned by quote creation; required to create an order before quote expiration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">failureDepositAddress</td>
      <td>string</td>
      <td>No</td>
      <td>Refund wallet address used if the order fails before completion. The address must belong to the network selected in fromAsset.network.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">sourceAddress</td>
      <td>string</td>
      <td>No</td>
      <td>Sender wallet address for compliance checks. The address must belong to the network selected in fromAsset.network.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">bankIdentifier</td>
      <td>string</td>
      <td>No</td>
      <td>Optional bank identifier used by selected payment provider.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">body</td>
      <td>none</td>
      <td>-</td>
      <td>GET endpoint without request body.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Order identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">type</td>
      <td>string</td>
      <td>Order type. Allowed values: BUY, SELL, SWAP.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>Current order lifecycle state. Allowed values: NEW, PROCESSING, COMPLETED, EXPIRED, ERROR.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">creationDate</td>
      <td>string</td>
      <td>Order creation timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">modificationDate</td>
      <td>string</td>
      <td>Last order update timestamp in server date-time format.</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction</td>
      <td>object | null</td>
      <td>Crypto transaction summary (nullable at creation step).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">expiresAtDate</td>
      <td>string</td>
      <td>Deposit/order expiration timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">depositCryptoAddress</td>
      <td>string</td>
      <td>Address where user must send crypto for sell flow.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 QUOTE_NOT_FOUND</td>
      <td>Blockchain address that must be shown to the client as the destination for crypto deposit.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 INVALID_QUOTE</td>
      <td>BUSINESS</td>
      <td>Quote cannot be calculated or cannot be used for order creation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 AML_FRAUD_VALIDATION_ERROR</td>
      <td>BUSINESS</td>
      <td>AML/fraud checks blocked order creation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 7. Get order by ID
Use this endpoint to fetch complete OffRamp order details by `orderId`.
Use the response to track payout processing, crypto deposit state, and final status.

**GET** `/api/v2/exchange/merchant/order?orderId={{Order_ID}}`

**Headers**
- `x-api-key: {{x-api-key}}`

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

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Params**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderId</td>
      <td>string (UUID)</td>
      <td>Yes</td>
      <td>Order identifier returned by sell API.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">body</td>
      <td>none</td>
      <td>-</td>
      <td>GET endpoint without request body.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Order identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">type</td>
      <td>string</td>
      <td>Order type. Allowed values: BUY, SELL, SWAP.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>Current order lifecycle state. Allowed values: NEW, PROCESSING, COMPLETED, EXPIRED, ERROR.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">creationDate</td>
      <td>string</td>
      <td>Order creation timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">modificationDate</td>
      <td>string</td>
      <td>Last order update timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">number</td>
      <td>number</td>
      <td>Internal order number.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation</td>
      <td>object</td>
      <td>Exchange side details (input/output, rates, fees).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.inputCurrency</td>
      <td>string</td>
      <td>Source asset/currency code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.inputAsset</td>
      <td>number</td>
      <td>Source amount.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.outputCurrency</td>
      <td>string</td>
      <td>Destination asset/currency code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.outputAsset</td>
      <td>number</td>
      <td>Destination amount.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.exchangeFeeAssetInFiat</td>
      <td>number</td>
      <td>Exchange fee represented in fiat asset currency (exchangeOperation.inputCurrency / fiat leg currency).</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.bonusOutputAsset</td>
      <td>number | null</td>
      <td>Bonus amount, if promo bonus is applied.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.plainRatio</td>
      <td>number</td>
      <td>Base system rate at the moment of quote calculation. Used as a reference value.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.ratio</td>
      <td>number</td>
      <td>Final client-facing rate applied to the quote/order. Show this value to the client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.currencyPair</td>
      <td>object</td>
      <td>Currency pair metadata.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.currencyPair.fromCurrency</td>
      <td>string</td>
      <td>Pair source currency code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeOperation.currencyPair.toCurrency</td>
      <td>string</td>
      <td>Pair destination currency code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction</td>
      <td>object</td>
      <td>Crypto transfer details.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.hash</td>
      <td>string | null</td>
      <td>Blockchain transaction hash when available.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.externalCryptoAddress</td>
      <td>string | null</td>
      <td>External wallet address used in operation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.internalCryptoAddress</td>
      <td>string | null</td>
      <td>Internal wallet/deposit address used in operation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.fromAddress</td>
      <td>string | null</td>
      <td>Source blockchain address.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.toAddress</td>
      <td>string | null</td>
      <td>Destination blockchain address.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.status</td>
      <td>string</td>
      <td>Crypto transaction status. Allowed values: NEW, PENDING_REVIEW, NOT_FOUND, REJECTED, TIMEOUT, INVALID_AMOUNT, ERROR, AML_ERROR, AML_BLOCKED, ARREST, SUBMITTING, SUBMITTED, PENDING, SELECTED, CONFIRMED, PENDING_RESOLVE.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.currency</td>
      <td>string</td>
      <td>Crypto asset code used in transaction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.fee</td>
      <td>number | null</td>
      <td>Blockchain/network fee amount in cryptoTransaction.currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.feePaymentEnabledByClient</td>
      <td>boolean</td>
      <td>Whether client-paid network fee mode is enabled.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.type</td>
      <td>string</td>
      <td>Crypto transfer processing type.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction.comment</td>
      <td>string | null</td>
      <td>Transaction comment, if provided.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction</td>
      <td>object</td>
      <td>Fiat processing details.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.status</td>
      <td>string</td>
      <td>Fiat transaction status. Allowed values: NEW, PENDING_REVIEW, REJECTED, TIMEOUT, DECLINED, INVALID_AMOUNT, ERROR, AML_BLOCKED, PENDING, PROCESSING, APPROVED.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.paymentToken</td>
      <td>string | null</td>
      <td>Payment method token used by provider leg.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.post</td>
      <td>string | null</td>
      <td>Provider postback payload/reference, if returned.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.brand</td>
      <td>string | null</td>
      <td>Card/payment brand returned by provider.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.internalToken</td>
      <td>string | null</td>
      <td>Internal provider token/reference.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.orderIdentity</td>
      <td>string | null</td>
      <td>Provider-side order reference used for reconciliation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.link</td>
      <td>string | null</td>
      <td>Provider payment link/reference.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.providerType</td>
      <td>string | null</td>
      <td>Fiat provider type.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.paymentType</td>
      <td>string | null</td>
      <td>Provider payment channel/type.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.processingBank</td>
      <td>string | null</td>
      <td>Processing bank, if returned by provider.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.resultMessage</td>
      <td>string | null</td>
      <td>Provider processing message.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.currency</td>
      <td>string | null</td>
      <td>Fiat currency used by provider transaction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatTransaction.processorTransactionNumber</td>
      <td>string | null</td>
      <td>Provider processor transaction identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">client</td>
      <td>object</td>
      <td>Client scope details.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">client.clientId</td>
      <td>string</td>
      <td>Client identifier used for request scoping.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">operationType</td>
      <td>string</td>
      <td>Internal operation type, for example FIAT_TO_CRYPTO or CRYPTO_TO_FIAT.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">serverDate</td>
      <td>string</td>
      <td>Server timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeType</td>
      <td>string</td>
      <td>Exchange direction as internal enum value.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderType</td>
      <td>string</td>
      <td>Internal order subtype.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">completionDate</td>
      <td>string | null</td>
      <td>Completion timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">resultMessage</td>
      <td>string | null</td>
      <td>Processing message.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">submitByResident</td>
      <td>boolean | null</td>
      <td>Resident submission flag, if applicable.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">merchantName</td>
      <td>string</td>
      <td>Merchant name associated with order.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">merchantBonus</td>
      <td>number | null</td>
      <td>Merchant bonus amount, if applicable.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">promoCodeDetails</td>
      <td>string | null</td>
      <td>Promo code details payload, if applied.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromSource</td>
      <td>string</td>
      <td>Source side enum value (EXT/INT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toSource</td>
      <td>string</td>
      <td>Destination side enum value (EXT/INT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">expiresAtDate</td>
      <td>string | null</td>
      <td>Order expiration timestamp in server date-time format.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">404 ORDER_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Order not found or not accessible in merchant scope.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 8. Get current order (optional)
Use this endpoint to fetch the current active OffRamp order for a client.
Use the response to restore flow state when user comes back to the session.

**GET** `/api/v2/exchange/merchant/order/current?clientId={{clientId}}`

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**
```json
{
    "id": "a1fcd0f0-8286-4015-914d-04db79921435",
    "type": "SELL",
  "status": "PROCESSING",
    "creationDate": "2026-05-06T17:48:38.699568",
    "modificationDate": "2026-05-06T17:48:42.242808",
    "cryptoTransaction": null,
    "expiresAtDate": "2026-05-06T18:18:37.181180",
    "depositCryptoAddress": "TVEwq1PiFDfJKYvWiDFhVXQkzDwqWCyPXV"
}
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Params**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>Yes</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">body</td>
      <td>none</td>
      <td>-</td>
      <td>GET endpoint without request body.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Current active order id.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">type</td>
      <td>string</td>
      <td>Order type. Allowed values: BUY, SELL, SWAP.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>Current order lifecycle state. Allowed values: NEW, PROCESSING, COMPLETED, EXPIRED, ERROR.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">creationDate</td>
      <td>string</td>
      <td>Order creation timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">modificationDate</td>
      <td>string</td>
      <td>Last order update timestamp in server date-time format.</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTransaction</td>
      <td>object | null</td>
      <td>Crypto transaction summary (nullable at creation step).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">expiresAtDate</td>
      <td>string</td>
      <td>Deposit/order expiration timestamp in server date-time format.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">depositCryptoAddress</td>
      <td>string</td>
      <td>Address where user must send crypto for sell flow.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>Blockchain address that must be shown to the client as the destination for crypto deposit.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Step 9. Get order history (optional)
Use this endpoint to fetch paged OffRamp order history for a client.
Use the response for history UI, status analytics, and reconciliation.

**POST** `/api/v2/exchange/merchant/order/history`

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**
```json
{
  "clientId": "{{clientId}}",
  "statuses": [
    "COMPLETED"
  ],
  "types": [
    "BUY"
  ],
  "providerTypes": [
    "ASSIST"
  ],
  "number": 491000004334
}
```
**Response**
```json
{
  "content": [
    {
            "id": "f8e67902-6dd3-4554-8f50-cd0a5c8a894b",
            "type": "BUY",
            "status": "COMPLETED",
            "creationDate": "2026-05-06T17:44:59.534092",
            "modificationDate": "2026-05-06T17:47:30.210422",
            "number": 491000004334,
            "exchangeOperation": {
                "inputCurrency": "BYN",
                "inputAsset": 50,
                "outputCurrency": "TRX",
                "outputAsset": 44.692189,
                "exchangeFeeAssetInFiat": 3.35,
                "bonusOutputAsset": null,
                "plainRatio": 1.0377,
                "ratio": 1.1188,
                "currencyPair": {
                    "fromCurrency": "BYN",
                    "toCurrency": "TRX"
                }
            },
            "cryptoTransaction": {
                "hash": "8bd6f85d606d9511d28471a91b72dce634b61b51b4bcef45fb5c29f5e4d94875",
                "externalCryptoAddress": "TCT2pKJXo233hrKWQMeCptC8My1KGvtsU4",
                "internalCryptoAddress": "TVEwq1PiFDfJKYvWiDFhVXQkzDwqWCyPXV",
                "fromAddress": "TVEwq1PiFDfJKYvWiDFhVXQkzDwqWCyPXV",
                "toAddress": "TCT2pKJXo233hrKWQMeCptC8My1KGvtsU4",
                "status": "CONFIRMED",
                "currency": "TRX",
                "fee": "0.268",
                "feePaymentEnabledByClient": false,
                "type": "AUTO",
                "comment": null
            },
            "fiatTransaction": {
                "status": "APPROVED",
                "paymentToken": "fc4b130e-c3bf-4a3d-abe5-9ec5900c9868",
                "post": "**** **** **** 1111",
                "brand": "VISA",
                "internalToken": "97fe9aa7-7805-438f-8c5e-aea24b4f9dc4",
                "orderIdentity": "97e3d1f09aed4442a793fb5eace5582b",
                "link": "https://payments.t.paysecure.ru/pay/p2p/cc2mc.cfm?...",
                "providerType": "ASSIST",
                "paymentType": "P2P",
                "processingBank": "BELARUSBANK",
                "resultMessage": null,
                "currency": "BYN",
                "processorTransactionNumber": null
            },
            "client": {
                "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562"
            },
            "serverDate": "2026-05-08T08:24:42+0000",
            "exchangeType": "SELL",
            "operationType": "FIAT_TO_CRYPTO",
            "orderType": "DEFAULT",
            "completionDate": "2026-05-06T17:47:28+0000",
            "resultMessage": null,
            "submitByResident": null,
            "merchantName": "wb",
            "merchantBonus": null,
            "promoCodeDetails": null,
            "fromSource": "EXT",
            "toSource": "EXT",
            "expiresAtDate": null
        }
  ],
  "pageable": {
    "sort": {
      "unsorted": false,
      "sorted": true,
      "empty": false
    },
    "pageNumber": 0,
    "pageSize": 10,
    "offset": 0,
    "paged": true,
    "unpaged": false
  },
  "totalElements": 52,
  "totalPages": 6,
  "last": false,
  "numberOfElements": 10,
  "size": 10,
  "number": 0,
  "sort": {
    "unsorted": false,
    "sorted": true,
    "empty": false
  },
  "first": true,
  "empty": false
}
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>External client mapping identifier.</td>
    </tr>
  </tbody>
</table>

**Params**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">page</td>
      <td>number</td>
      <td>No</td>
      <td>Page number (Spring pageable).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">size</td>
      <td>number</td>
      <td>No</td>
      <td>Page size.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">sort</td>
      <td>string</td>
      <td>No</td>
      <td>Sort format (default by creationDate,DESC).</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>Yes</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">statuses</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by order status. Allowed values: PROCESSING, EXPIRED, COMPLETED, FAILED.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">types</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by order type values (BUY, SELL, SWAP).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderTypes</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by exchange direction types.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">operationTypes</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by operation direction (FIAT_TO_CRYPTO, CRYPTO_TO_FIAT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providerTypes</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by fiat provider ids (ASSIST, CA, MTS).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatStatuses</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by fiat transaction statuses.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoStatuses</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by crypto transaction statuses.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">creationDateFrame</td>
      <td>object</td>
      <td>No</td>
      <td>Creation date range filter.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">completionDateFrame</td>
      <td>object</td>
      <td>No</td>
      <td>Completion date range filter.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderId</td>
      <td>string</td>
      <td>No</td>
      <td>Filter by specific order id (UUID).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">number</td>
      <td>number</td>
      <td>No</td>
      <td>Filter by numeric order number.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">inputAssets</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by source asset codes.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">outputAssets</td>
      <td>array of string</td>
      <td>No</td>
      <td>Filter by destination asset codes.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">assets</td>
      <td>array of string</td>
      <td>No</td>
      <td>Asset filter applied to either source or destination leg.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoAddress</td>
      <td>string</td>
      <td>No</td>
      <td>Filter by crypto address involved in transaction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fromSource</td>
      <td>string</td>
      <td>No</td>
      <td>Filter by source type (EXT/INT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">toSource</td>
      <td>string</td>
      <td>No</td>
      <td>Filter by destination type (EXT/INT).</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content</td>
      <td>array of objects</td>
      <td>Orders page content.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].id</td>
      <td>string</td>
      <td>Entry identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].type</td>
      <td>string</td>
      <td>Order type (BUY/SELL).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].status</td>
      <td>string</td>
      <td>Order status value (NEW, PROCESSING, COMPLETED, EXPIRED, ERROR).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].creationDate</td>
      <td>string</td>
      <td>Order creation timestamp.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].modificationDate</td>
      <td>string</td>
      <td>Order last modification timestamp.</td>
    </tr>
  </tbody>
</table>
<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].number</td>
      <td>number</td>
      <td>Human-readable order number.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].exchangeOperation</td>
      <td>object</td>
      <td>Exchange operation details (input/output assets, ratio, fees).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].cryptoTransaction</td>
      <td>object | null</td>
      <td>Crypto transaction details (addresses, hash, status, fee, type, comment).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].fiatTransaction</td>
      <td>object | null</td>
      <td>Fiat transaction details (provider, status, payment metadata).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].client</td>
      <td>object</td>
      <td>Client block with clientId.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].exchangeType</td>
      <td>string</td>
      <td>Exchange direction (BUY/SELL/SWAP).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].operationType</td>
      <td>string</td>
      <td>Operation type (FIAT_TO_CRYPTO/CRYPTO_TO_FIAT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].orderType</td>
      <td>string</td>
      <td>Order subtype (for example DEFAULT).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].expiresAtDate</td>
      <td>string | null</td>
      <td>Order expiration timestamp.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].clientId</td>
      <td>string</td>
      <td>Client identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].userId</td>
      <td>string | null</td>
      <td>User identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].creationDate</td>
      <td>string</td>
      <td>Creation timestamp.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">content[].completionDate</td>
      <td>string | null</td>
      <td>Completion timestamp.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable</td>
      <td>object</td>
      <td>Spring pageable metadata object.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.pageNumber</td>
      <td>number</td>
      <td>Current page number.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.pageSize</td>
      <td>number</td>
      <td>Current page size.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.offset</td>
      <td>number</td>
      <td>Current page offset.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.paged</td>
      <td>boolean</td>
      <td>Whether pageable mode is enabled.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.unpaged</td>
      <td>boolean</td>
      <td>Whether unpaged mode is enabled.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.sort</td>
      <td>object</td>
      <td>Pageable sort metadata.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.sort.unsorted</td>
      <td>boolean</td>
      <td>True when pageable sort is not set.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.sort.sorted</td>
      <td>boolean</td>
      <td>True when pageable sort is set.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">pageable.sort.empty</td>
      <td>boolean</td>
      <td>True when pageable sort metadata is empty.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">totalElements</td>
      <td>number</td>
      <td>Total number of matching orders.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">totalPages</td>
      <td>number</td>
      <td>Total number of pages.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">last</td>
      <td>boolean</td>
      <td>True when this page is the last page.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">numberOfElements</td>
      <td>number</td>
      <td>Number of elements in current page.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">number</td>
      <td>number</td>
      <td>Current page number.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">size</td>
      <td>number</td>
      <td>Current page size.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">sort</td>
      <td>object</td>
      <td>Top-level sort metadata.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">sort.unsorted</td>
      <td>boolean</td>
      <td>True when top-level sort is not set.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">sort.sorted</td>
      <td>boolean</td>
      <td>True when top-level sort is set.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">sort.empty</td>
      <td>boolean</td>
      <td>True when top-level sort metadata is empty.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">first</td>
      <td>boolean</td>
      <td>True when this page is the first page.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">empty</td>
      <td>boolean</td>
      <td>True when page content is empty.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Client id is invalid or not linked to the merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 Bad Request</td>
      <td>HTTP</td>
      <td>Invalid pageable/filter request.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

## 3. Quote fields

This section explains quote response fields and fee calculation logic.

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
