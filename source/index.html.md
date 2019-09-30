---
title: Runo Wallet API Doc

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://runosoft.com'>Runo Wallet API v1</a>

includes:
  - errors

search: true
---

# Introduction



<b>Coin List: </b>

`BTC`

`ETH`

`XRP`

`ETC`

`ERC-20 Tokens`



# Authentication

Runo Wallet API use bearer token to authenticate.
The token must be added as a HTTP header to all API calls in the HTTP “Authorization” header:

Authorization: Bearer <code>your token goes here</code>

> To authorize, use this code:



```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Bearer $ACCESS_TOKEN"
```

> Make sure to replace `ACCESS_TOKEN` with your API key.


<aside class="notice">
You must replace <code>ACCESS_TOKEN</code> with your personal API key.
</aside>

# Wallet

## createnewwallet

```shell


LABEL='wallet-label'
PASSWORD='wallet-password'

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"label\": \"$LABEL\", \"password\": $PASSWORD}" \
https://runowallethost.com/api/btc/createnewwallet
```

> The above command returns JSON structured like this:

```json
{
    "coin": "etc",
    "coinSpecific": null,
    "label": "wallet-label",
    "wallet": "0x1b79312531b34a4233882eaedf7e0b3b45423c67"
}
```


Creates a new wallet in the given coin network.

### HTTP Request

`POST /api/{coin}/createnewwallet`

### URL Parameters
| Parameter        | Type   | Required | Default     | Description      |
| :--------------: | :----: | :------: | :---------: | :----:           |
| coin             | string | yes      | -           | The type of coin |


### Body Parameters
| Parameter |  Type  |             Description             |
| :-------: | :----: | :---------------------------------: |
| password  | string | To decrypt the wallet's private key |
|   label   | string |   Human-readable name for wallet    |


### Response

#### Success
| Parameter        | Type        | Description                              |
| :--------------: | :----:      | :----:                                   |
| coin             | string      | The type of code                         |
| coinSpecific     | JSON-Object | Contains the special information of coin |
| label            | string      | Wallet's human readable label            |
| wallet           | string      | The address of created wallet            |



<aside class="success">
Remember — There is no way to recover the wallet password!
</aside>

## listwallets


```shell
curl -X GET \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-H "Content-Type: application/json" \
https://runowallethost.com/api/btc/listwallets
```


> The above command returns JSON structured like this:

```json
{
    "coin": "btc",
    "limit": 25,
    "wallets": [
        {
            "address": "2NAsgr2qtyQZeDnEYzAzEuiMWCUBQDSn9j8",
            "balance": 22847174,
            "label": "runoMain"
        },
        {
            "address": "2NFnjv1QCFvbcmiz8f31MTZfWZg3t5hnLDD",
            "balance": 7089229,
            "label": "runoTest"
        }
    ]
}
```


Lists all of a user's wallets for a given coin.

### HTTP GET
`GET /api/{coin}/listwallets`

### Query Parameters
| Parameter        | Type    | Required | Default(min) | Description                                 |
| :--------------: | :----:  | :------: | :---------:  | :----:                                      |
| coin             | string  | yes      | -            | The type of coin                            |
| limit            | integer | No       | 25(1)        | The number of wallets that you want to list |


### Response

#### Success
| Parameter        | Type       | Description     |
| :--------------: | :----:     | :----:          |
| wallets          | JSON-Array | List of wallets |

## newaddress

```shell
curl -X POST \
  https://runowallethost.com/api/btc/2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf/newaddress \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
```


> The above command returns JSON structured like this:

```json
{
    "address": "2NAKXdhBmoxzUe4iXAhCgcHHqFRbjUx3HnK",
    "coin": "btc",
    "coinSpecific": null,
    "wallet": "2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf"
}
```

Creates a new receive address for your wallet.

### HTTP POST
`POST /api/{coin}/{wallet}/newaddress`

### URL Parameters
| Parameter | Type   | required    | Default | Description                   |
| :-------: | :----: | :---------: | :----:  | :---:                         |
| coin      | string | yes         | -       | The type of coin              |
| wallet    | string | yes         | -       | Which wallet own this address |



### Response

#### Success
| Parameter        | Type        | Description                             |
| :--------------: | :----:      | :----:                                  |
| address          | string      | The address of newly created            |
| coin             | string      | The type of coin                        |
| coinSpecific     | JSON-Object | Contains special information about coin |
| wallet           | string      | The wallet that owns created address    |

## getinfo

```shell
curl -X GET \
  https://runowallethost.com/api/btc/2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf/getinfo \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
```


> The above command returns JSON structured like this:

```json
{
    "balance": 7007218,
    "coin": "btc",
    "coinSpecific": null,
    "label": "runo",
    "wallet": "2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf"
}
```

Returns information about given wallet.

### HTTP GET
`GET /api/{coin}/{wallet}/getinfo`

### URL Parameters
| Parameter | Type   | Required    | Default(min) | Description                          |
| :-------: | :----: | :---------: | :---------:  | :---------:                          |
| coin      | string | yes         | -            | The type of coin                     |
| wallet    | string | yes         | -            | The wallet that you want to get info |

### Response

#### Success
| Parameter        | Type        | Description                               |
| :--------------: | :----:      | :----:                                    |
| balance          | integer     | Wallet's balance                          |
| coin             | string      | The type of coin                          |
| coinSpecific     | JSON-Object | Contains special information about coin   |
| label            | string      | Wallet's label                            |
| wallet           | string      | The wallet that returns information about |


## listaddresses

```shell
curl -X GET \
  https://runowallethost.com/api/btc/2NEK5HffPMCZAf5xkd6KcNsM2QwKKsNaFVd/listaddresses?limit=7 \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
```


> The above command returns JSON structured like this:

```json
{
    "addresses": [
        {
            "address": "2MuvLKqXW1DRjMc7cYuATkPi9R25rTA8H4A"
        },
        {
            "address": "2N76PJpkMFd4LpRiobkKs1VHjGECCW8PiSE"
        },
        {
            "address": "2Mu7aVWze1ytnhmCUSqyHRp7mA2HUWb18MN"
        },
        {
            "address": "2N1X1VbMJg1dBrB834SiMq1EU2nRmEV6bJi"
        },
        {
            "address": "2NAzKJJE379yHFqPsRKVC1HFfwUqnDWBzYK"
        },
        {
            "address": "2Msu7hN7oKEj7mB5yoXs6ZzbuH6kspSvu5a"
        },
        {
            "address": "2NDN5LcxJMeTHASLydnbuo1uBdT5VC8qfBo"
        }
    ],
    "coin": "btc",
    "limit": 7,
    "nextid": 247,
    "wallet": "2NEK5HffPMCZAf5xkd6KcNsM2QwKKsNaFVd"
}
```

Returns addresses of given wallet.

### HTTP GET

`GET /api/{coin}/{wallet}/listaddresses`

### URL Parameters
| Parameter | Type   | Required    | Default(min-max) | Description                                 |
| :-------: | :----: | :---------: | :---------:      | :---------:                                 |
| coin      | string | yes         | -                | The type of coin                            |
| wallet    | string | yes         | -                | The wallet that you want to list addresses' |
| limit     | int    | no          | 25(1-200)        | Number of addresses return in response      |
| startid   | int    | no          | 0(1--)           | Start return addresses with given id        |


### Response

#### Success
| Parameter        | Type        | Description                                |
| :--------------: | :----:      | :----:                                     |
| addresses        | JSON-Object | Contains address and this address' balance |
| coin             | string      | The type of coin                           |
| limit            | integer     | Number of addresses return in response     |
| nextid           | integer     | The remaining not listed number of addresses |
| wallet           | string      | The wallet that listed addresses           |


## sendcoins

```shell
curl -X POST \
  https://runowallethost.com/api/btc/2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf/sendcoins \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "address": "2Mvr3HuUQEUiQ1K3f1z3awQXiaEdbLCmYxo",
    "amount": 90000,
    "password": "walletpassword"
}'
```


> The above command returns JSON structured like this:

```json
{
    "coin": "btc",
    "coinSpecific": null,
    "trackingKey": "out-kzfqcrfyumi2ry46hcx7f9fpbz7838ly69tdmbfv2"
}
```

Creates and sends cryptocurrency to the destination address.

### HTTP POST
`POST /api/{coin}/{wallet}/sendcoins`

### URL Parameters
| Parameter | Type   | Required    | Default(min) | Description                                 |
| :-------: | :----: | :---------: | :---------:  | :---------:                                 |
| coin      | string | yes         | -            | The type of coin                            |
| wallet    | string | yes         | -            | The wallet that you want to send coins from |


### Body Parameters
| Parameter        | Type   | Description                                   |
| :--------------: | :----: | :----------------------------------:          |
| address          | string | The recipient wallet address                  |
| amount           | int    | Amount to be sent to the recipient in shannon |
| password         | string | The password to decrypt the wallet            |


### Response

#### Success
| Parameter        | Type        | Description                                            |
| :--------------: | :----:      | :----:                                                 |
| coin             | string      | The type of coin                                       |
| coinSpecific     | JSON-Object | Contains special information about coin                |
| trackingKey      | string      | A string that let you get information about send order |



## checktxbyid

```shell
curl -X GET \
  https://runowallethost.com/api/btc/checktxbyid/14e15e9fcac5ef491d57297ae888a886fd21e542eac9365b3a462d8db6c05e6d \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
```


> The above command returns JSON structured like this:

```json
{
    "blocknumber": 1575231,
    "coinSpecific": null,
    "confirmation": 5,
    "fee": 172671,
    "from": [
        {
            "address": "2N6LqzopwKXc89eoz4Ydjq4hB2k7b545yDs",
            "value": 51143342
        }
    ],
    "status": true,
    "timestamp": 1566296006,
    "to": [
        {
            "address": "mk2vEGU1xj1GfrZibsngi6satqvYrU6J8M",
            "value": 3954832
        },
        {
            "address": "2MycbNBrGatL6MKBaJd2sM6W215UVCA4SDX",
            "value": 47015839
        }
    ]
}
```

Returns information about given transaction id.

### HTTP GET
`GET /api/{coin}/checktxbyid/{tx}`

### URL Parameters
| Parameter | Type   | Required    | Default(min-max) | Description                 |
| :-------: | :----: | :---------: | :---------:      | :---------:                 |
| coin      | string | yes         | -                | The type of coin            |
| tx        | string | yes         | -                | TX hash that you want check |

### Response

#### Success
| Parameter        | Type        | Description                                                                                                                 |
| :--------------: | :----:      | :----:                                                                                                                      |
| blocknumber      | integer     | Block Number                                                                                                                |
| coinSpecific     | JSON-Object | Contains the special information of coin                                                                                    |
| confirmation     | integer     | The number of block confirmation                                                                                            |
| fee              | integer     | Amount of satoshi(shannon) when sending transaction as fee                                                                  |
| from             | JSON-Object | Sending recepient wallet address and amount of satoshi(shannon) send coin                                                   |
| status           | boolean     | If true transaction is success else transaction failed but transaction writed to the block (special for Ethereum and token) |
| timestamp        | integer     | Unix timestamp of transaction                                                                                               |
| to               | JSON-Object | Receiving wallet addresses and amount of satoshi(shannon) send coin                                                 |


## checktxbytk

```shell
curl -X GET \
  https://runowallethost.com/api/btc/checktxbytk/14e15e9fcac5ef491d57297ae888a886fd21e542eac9365b3a462d8db6c05e6d \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
```


> The above command returns JSON structured like this:

```json
{
    "amount": 90000,
    "from": "2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf",
    "status": "pending",
    "to": "2Mvr3HuUQEUiQ1K3f1z3awQXiaEdbLCmYxo",
    "txID": "d6d507c32aad8d79ed7d2723635308b988e60590636d81af48e7c50a865122c7"
}
```

Returns information about newly created transaction by tracking key.

### HTTP GET
`GET /api/{coin}/checktxbytk/{trackingKey}`

### Query Parameters

### Query Parameters
| Parameter   | Type   | Required    | Default(min-max) | Description                           |
| :-------:   | :----: | :---------: | :---------:      | :---------:                           |
| coin        | string | yes         | -                | The type of coin                      |
| trackingkey | string | yes         | -                | The trackingKey that given by the API |


### Response

#### Success
| Parameter        | Type    | Description                   |
| :--------------: | :----:  | :----:                        |
| amount           | integer | The amount of coin that send  |
| from             | string  | The wallet that sends coins   |
| status           | string  | The status of payout          |
| to               | string  | The wallet of receiving coins |


## updatewalletpassword

```shell
curl -X PUT \
  https://runowallethost.com/api/btc/2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf/updatewalletpassword \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
  -d '{
    "currentPassword": "walletpassword",
    "newPassword": "myNewPassword"
    }'
```


> The above command returns JSON structured like this:

```json
{
    "code": 200,
    "message": "Successfully updated password."
}
```

### HTTP PUT
`PUT /api/{coin}/{wallet}/updatewalletpassword`

### URL Parameters

| Parameter | Type   | Required    | Default(min-max) | Description                                      |
| :-------: | :----: | :---------: | :---------:      | :---------:                                      |
| coin      | string | yes         | -                | The type of coin                                 |
| wallet    | string | yes         | -                | The wallet that you want to update the password. |




### Body Parameters
| parameter        | type   | Description                                     |
| :--------------: | :----: | :----------------------------------:            |
| currentPassword  | string | The wallet's current password.                  |
| newPassword      | string | The wallet's new password that you want to set. |

### Response

#### Success
| Parameter        | Type    | Description            |
| :--------------: | :----:  | :----:                 |
| code             | integer | Type of code           |
| message          | string  | Description of message |




## getlasttxs

```shell
curl -X GET \
  https://runowallethost.com/api/btc/2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf/getlasttxs \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
```


> The above command returns JSON structured like this:

```json
[
    {
        "amount": 90000,
        "coin": "btc",
        "from": "2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf",
        "status": "confirmed",
        "to": "2Mvr3HuUQEUiQ1K3f1z3awQXiaEdbLCmYxo",
        "txHash": "bd34b66b6e25389af4668926900e991bae974ccd31641acedce181d6941dc91f"
    },
    {
        "amount": 97043,
        "coin": "btc",
        "from": "2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf",
        "status": "confirmed",
        "to": "2MswmeMjqe1oLX8mTGoqnzWfvEQjKWc3fHn",
        "txHash": "603bc2da1c626e23b94e40b620368d0435e2ea07675f303ebf219a69dfb87c68"
    },
    {
        "amount": 97043,
        "coin": "btc",
        "from": "2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf",
        "status": "confirmed",
        "to": "2NAYFH8qEnuMi7hXt95dX26XnrpR7MW6dUD",
        "txHash": "622df2b1740fcf04099d3cb12354ef58db788a8d5ca040e02e0e5836cbaab63d"
    },
    {
        "amount": 77043,
        "coin": "btc",
        "from": "2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf",
        "status": "confirmed",
        "to": "2MsrMWgsT6xvhjvwhW5tcKNCzKHDvcM6ozT",
        "txHash": "7009cfb11936e89450b523f1451a2ec9a4169c24eebf65c3d8a3a7143ada17cc"
    },
    {
        "amount": 95706,
        "coin": "btc",
        "from": "2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf",
        "status": "confirmed",
        "to": "2MvyPep3tLKiSfcz1y96iJhGmEaFSWMfR77",
        "txHash": "81c8408428a8c3efc754e9e616b6665f0185e0a192d51b476bf964517f98ef5a"
    }
]
```

Returns amount, from wallet, status, to wallet, transaction hash of last transactions of given wallets.

### HTTP GET
`GET /api/{coin}/{wallet}/getlasttxs`

### Query Parameters
| Parameter        | Type    | Required | Default(min-max) | Description                                                   |
| :--------------: | :----:  | :------: | :---------:      | :----:                                                        |
| coin             | string  | yes      | -                | The type of coin                                              |
| wallet           | string  | yes      | -                | The wallet that you want to get confirmed last transactions'. |
| limit            | integer | no       | 5(1-250)         | Number of  transaction you want to list.                      |

### Response

#### Success
| Parameter        | Type    | Description                                   |
| :--------------: | :----:  | :----:                                        |
| amount           | integer | The amount of coin that you send              |
| coin             | string  | Coin type                                     |
| from             | string  | The wallet of sending coin                    |
| status           | string  | The status of sending transaction             |
| to               | string  | The wallet of receives the coin               |
| txHash           | string  | The transaction hash of payout on the network |



## getpendingtxs

```shell
curl -X GET \
  https://runowallethost.com/api/btc/2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf/getpendingtxs \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
```


> The above command returns JSON structured like this:

```json
[
    {
        "amount": 91000,
        "coin": "btc",
        "from": "2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf",
        "status": "que",
        "to": "2Mvr3HuUQEUiQ1K3f1z3awQXiaEdbLCmYxo",
        "txHash": null
    },
    {
        "amount": 90000,
        "coin": "btc",
        "from": "2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf",
        "status": "pending",
        "to": "2Mvr3HuUQEUiQ1K3f1z3awQXiaEdbLCmYxo",
        "txHash": "d6d507c32aad8d79ed7d2723635308b988e60590636d81af48e7c50a865122c7"
    }
]
```

### HTTP GET
`GET /api/{coin}/{wallet}/getpendingtxs`

### Query Parameters
| Parameter        | Type    | Required | Default(min-max) | Description                                            |
| :--------------: | :----:  | :------: | :---------:      | :----:                                                 |
| coin             | string  | yes      | -                | The type of coin                                       |
| wallet           | string  | yes      | -                | The wallet that you want to get pending transactions'. |
| limit            | integer | no       | 5(1-250)         | Number of pending transaction you want to list.        |

### Response

#### Success

| Parameter        | Type    | Description                                   |
| :--------------: | :----:  | :----:                                        |
| amount           | integer | The amount of coin that you send              |
| coin             | string  | Coin type                                     |
| from             | string  | The wallet of sending coin                    |
| status           | string  | The status of sending transaction             |
| to               | string  | The wallet of receives the coin               |
| txHash           | string  | The transaction hash of payout on the network |


# Ping

## ping

```shell
curl -X POST \
  https://runowallethost.com/api/btc/ping \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
```


> The above command returns JSON structured like this:

```json
{
    "code": 200,
    "message": "pong"
}
```

If API is online returns `pong` message, If it is not online doesn't return anything.

### HTTP POST
`POST /api/{coin}/ping`

### Query Parameters
| Parameter        | Type    | Required | Default(min-max) | Description                                            |
| :--------------: | :----:  | :------: | :---------:      | :----:                                                 |
| coin             | string  | yes      | -                | The type of coin                                       |

### Response

#### Success
| parameter        | type    | Description                    |
| :--------------: | :----:  | :----:                         |
| code             | integer | Type of code                   |
| message          | string  | Returns pong if API is online. |


# Webhook

## setnewwebhook

```shell
curl -X POST \
  https://runowallethost.com/api/btc/2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf/setnewwebhook \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
  -d '{
    "url":"https://example-webhook.com"
    }'
```


> The above command returns JSON structured like this:

```json
{
    "code": 200,
    "message": "Successfully set to given URL 'https://example-webhook.com'"
}
```

Sets a given URL as webhook url for the given wallet.

### HTTP POST
`POST /api/{coin}/{wallet}/setnewwebhook`

### URL Parameters
| Parameter        | Type   | Required | Default     | Description                         |
| :--------------: | :----: | :------: | :---------: | :----:                              |
| coin             | string | yes      | -           | The type of coin                    |
| wallet           | string | yes      | -           | Wallet that you want to set webhook |


### Body Parameters

| Parameter  | Type   | Description                      |
| :--------: | :----: | :------------------------------: |
| url        | string | Webhook url for the given wallet |


### Response

#### Success
| Parameter        | Type    | Description         |
| :--------------: | :----:  | :----:              |
| code             | integer | Type of code        |
| message          | string  | Description of code |


## updatewebhookurl

```shell
curl -X PUT \
  https://runowallethost.com/api/btc/2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf/setnewwebhook \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
  -d '{
    "url":"https://example-webhook.com"
    }'
```


> The above command returns JSON structured like this:

```json
{
    "code": 200,
    "message": "Successfully set to given URL 'https://example-webhook.com'"
}
```

Updates given wallet's webhook URL.

### HTTP PUT
`PUT /api/{coin}/{wallet}/updatewebhookurl`

### URL Parameters
| Parameter | Ttype  | Required    | Default(min-max) | Description                                         |
| :-------: | :----: | :---------: | :---------:      | :---------:                                         |
| coin      | string | yes         | -                | The type of coin                                    |
| wallet    | string | yes         | -                | The wallet that you want to update the webhook URL. |


### Body Parameters
| Parameter        | Type   | Description                              |
| :--------------: | :----: | :----------------------------------:     |
| url              | string | The URL that you want to set as webhook. |


```json
{
    "url": "https://webhook-example.com"
}
```

### Response

#### Success
| Parameter        | Type    | Description            |
| :--------------: | :----:  | :----:                 |
| code             | integer | Type of code           |
| message          | string  | Description of message |



# Merchant

## createorder

```shell
curl -X POST \
  https://runowallethost.com/api/btc/2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf/createorder \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
  -d '{
    "priceCurrency": "try",
    "priceAmount": 50
    }'
```


> The above command returns JSON structured like this:

```json
{
    "trackingKey": "ord-o5x4sn3l58otbcyoydu9nrq1myx81n49dvpor0g8v",
    "timestamp": 1566073395,
    "priceAmount": 88042,
    "paymentAddress": "2MurYzXoLWdKaPNZFNwoDf5Mg6NnYwxQWXe",
    "status": "new",
    "currency": "TRY"
}
```

Creates a new order on the given coin network. Returns tracking key of order that allows
getting information about order on the api, also returns a payment address where your customer
send the price and other information about order.

### HTTP POST
`POST /api/{coin}/{wallet}/createorder`

### URL Parameters
| Parameter        | Type   | Required | Default(min-max) | Description                              |
| :--------------: | :----: | :------: | :---------:      | :----:                                   |
| coin             | string | yes      | -                | The type of coin                         |
| wallet           | string | yes      | -                | The wallet that you want to create order |

### Body Parameters
| Parameter        | Type   | Description                          |
| :--------------: | :----: | :----------------------------------: |
| priceCurrency    | string | Type of the price(TRY or USD)        |
| priceAmount      | string | Amount of price in given currency    |


### Response

#### Success

| parameter        | type    | Description                                           |
| :--------------: | :----:  | :----:                                                |
| trackingKey      | string  | order tracking key                                    |
| timestamp        | integer | creation time of order in unix timestamp (in seconds) |
| priceAmount      | integer | amount of price in satoshi                            |
| paymentAddress   | string  | address that customer send the price                  |
| status           | string  | status of the order                                   |
| currency         | string  | currency of the order                                 |


## getorder

```shell
curl -X GET \
  https://runowallethost.com/api/btc/2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf/getorder/ord-o5x4sn3l58otbcyoydu9nrq1myx81n49dvpor0g8v \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
```


> The above command returns JSON structured like this:

```json
{
    "timestamp": 1566073395,
    "priceAmount": 88042,
    "paymentAddress": "2MurYzXoLWdKaPNZFNwoDf5Mg6NnYwxQWXe",
    "status": "failed",
    "currency": "TRY",
    "trackingKey": "ord-o5x4sn3l58otbcyoydu9nrq1myx81n49dvpor0g8v",
    "invoiceBtcUrl": "bitcoin:2MurYzXoLWdKaPNZFNwoDf5Mg6NnYwxQWXe?amount=0.00088042",
    "maxSeconds": 900,
    "rate": 56790.9
}
```

getorder API endpoint returns information about order with the given tracking key on the URL.

### HTTP GET
`GET /api/{coin}/{wallet}/getorder/{trackingKey}`

### URL Parameters
| Parameter        | Type   | Required | Default(min-max) | Description                                   |
| :--------------: | :----: | :------: | :---------:      | :----:                                        |
| coin             | string | yes      | -                | The type of coin                              |
| wallet           | string | yes      | -                | The wallet that you want to list create order |
| trackingKey      | string | yes      | -                | The tracking key that given by the API        |

### Response

#### Success
| parameter        | type    | Description                                                                    |
| :--------------: | :----:  | :----:                                                                         |
| timestamp        | integer | creation time of order in unix timestamp (in seconds)                          |
| priceAmount      | integer | amount of price in satoshi                                                     |
| paymentAddress   | string  | address that customer send the price                                           |
| status           | string  | status of the order                                                            |
| currency         | string  | currency of the order                                                          |
| trackingKey      | string  | order tracking key                                                             |
| invoiceBtcUrl    | string  | QR code url of the order                                                       |
| maxSeconds       | integer | max seconds that customer have to send amount of coin to given payment address |
| rate             | float   | one unit of coin price in USD($)                                               |



## getorderws
getorderws API endpoint sends order information every 2 seconds via WebSocket until the order is failed or payment time is up.

### WebSocket
`ws://api/{coin}/ws/getorder/{trackingKey}`

### URL Parameters
| Parameter        | Type   | Required | Default(min-max) | Description                                   |
| :--------------: | :----: | :------: | :---------:      | :----:                                        |
| coin             | string | yes      | -                | The type of coin                              |
| trackingKey      | string | yes      | -                | The tracking key that given by the API        |

### Response

#### Success
| parameter         | type    | Description                                                                    |
| :--------------:  | :----:  | :----:                                                                         |
| timestamp         | integer | creation time of order in unix timestamp (in seconds)                          |
| trackingKey       | string  | order tracking key                                                             |
| status            | string  | status of the order                                                            |
| paymentAddress    | string  | address that customer send the amount of coin                                  |
| orderAmount       | float   | amount of order BTC in satoshi                                                 |
| maxSeconds        | integer | max seconds that customer have to send amount of coin to given payment address |
| expirationSeconds | integer | time remaning until order failed in seconds                                    |
| expirationTime    | string  | time remaning until order failed in human readable form                        |
| rate              | float   | one unit of coin price in USD($)                                               |
| invoiceBtcUrl     | string  | QR code url of the order                                                       |
| currency          | string  | currency of the order                                                          |


## listorders

```shell
curl -X GET \
  https://runowallethost.com/api/btc/2Msa5Vu4cAN4HjFDywDtxjN9DeiddnvHcWf/listorders?limit=4 \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -H 'cache-control: no-cache'
```


> The above command returns JSON structured like this:

```json
{
    "orders": [
        {
            "trackingKey": "ord-sn411zaj2b2rstg320exu1rhow28h33pquxh51zww",
            "timestamp": 1566156091,
            "priceAmount": 172273,
            "paymentAddress": "2N1RvrtaaG1fQyKMTX1otVP7jPHkcpi6JKL",
            "status": "failed",
            "currency": "TRY"
        },
        {
            "trackingKey": "ord-ij0jrcc2gf3tjc7rs0j91g2thbykn5hbfwe7jzr23",
            "timestamp": 1566152380,
            "priceAmount": 172406,
            "paymentAddress": "2NFwbY8dCCzqEBGV69fAkxc2Y3Qjo6ixAWw",
            "status": "failed",
            "currency": "TRY"
        },
        {
            "trackingKey": "ord-lf1butzsd83b91rkc69n2dt985nsa4609oc9icl7z",
            "timestamp": 1566079338,
            "priceAmount": 176562,
            "paymentAddress": "2N58djAWDJbdxVmvsJ4cpe3Ly5oiHtNnMT2",
            "status": "failed",
            "currency": "TRY"
        },
        {
            "trackingKey": "ord-yy5r9x9e7br4m33dklje1uv2tdehpvbz0g8a7pk0s",
            "timestamp": 1566079270,
            "priceAmount": 176562,
            "paymentAddress": "2N6A7Qb1fN44h8dWTYmhvP8C3d69xXBGxU2",
            "status": "failed",
            "currency": "TRY"
        }
    ]
}
```

listorders API endpoint list all the orders with the given wallet.

### HTTP GET
`GET /api/{coin}/{wallet}/listorders`

### URL Parameters
| Parameter        | Type    | Required | Default(min-max) | Description                                   |
| :--------------: | :----:  | :------: | :---------:      | :----:                                        |
| coin             | string  | yes      | -                | The type of coin                              |
| wallet           | string  | yes      | -                | The wallet that you want to list create order |
| limit            | integer | no       | 5(1-250)         | The number of orders that you want to list    |

### Response

#### Success
| parameter         | type    | Description                                                                    |
| :--------------:  | :----:  | :----:                                                                         |
| trackingKey       | string  | order tracking key                                                             |
| timestamp         | integer | creation time of order in unix timestamp (in seconds)                          |
| priceAmount       | integer | amount of price in satoshi                                                     |
| paymentAddress    | string  | address that customer send the amount of coin                                  |
| status            | string  | status of the order                                                            |
| currency          | string  | currency of the order                                                          |

