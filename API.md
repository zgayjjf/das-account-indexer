* [API List](#api-list)
    * [Get Server Info](#get-server-info)
    * [Get Reverse Record Info](#get-reverse-record-info)
    * [Get Account Basic Info](#get-account-basic-info)
    * [Get Account List](#get-account-list)  
    * [Get Account Records Info](#get-account-records-info) 
    * [Get Account Records Info V2](#get-account-records-info-v2)
* [<em>Deprecated API List</em>](#deprecated-api-list)
    * [<em>Get Account Basic Info And Records</em>](#get-account-basic-info-and-records-deprecated)
    * [<em>Get Related Accounts By Owner Address</em>](#get-related-accounts-by-owner-address-deprecated)
* [Error](#error)
    * [Error Example](#error-example)
    * [Error Code](#error-code)

  
## API List

Please familiarize yourself with the meaning of some common parameters before reading the API list:

| param                                                                                    | description                                                         |
| :-------------------------                                                               | :------------------------------------------------------------------ |
| type                                                                                     | Filled with "blockchain" for now                                    |
| coin\_type <sup>[1](https://github.com/satoshilabs/slips/blob/master/slip-0044.md)</sup> | 60: eth, 195: trx, 9006: bsc, 966: matic                             |
| chain\_id <sup>[2](https://github.com/ethereum-lists/chains)</sup>                       | 1: eth, 56: bsc, 137: polygon; 5: goerli, 97: bsct, 80001: mumbai   |
| account                                                                                  | Contains the suffix `.bit` in it                                    |
| key                                                                                      | Generally refers to the blockchain address for now                  |

_You can provide either `coin_type` or `chain_id`. The `coin_type` will be used, if you provide both._


#### Full Functional Indexer

```json
https://indexer-v1.did.id
```

This service can query all data, but it is recommended that developers setup their own Indexer for the sake of decentralization.

#### Basic Functional Indexer

```json
https://indexer-basic.did.id
```

This service can only query basic account data and .bit Alias data. The specific interfaces are as follows:

* das_serverInfo
* das_reverseRecord
* das_accountInfo
* das_accountList


### Get Server Info

**Request**
* host: `indexer-v1.did.id`
* path: `/v1/server/info`
* param: none

**Response**

```json
{
  "errno": 0,
  "errmsg": "",
  "data": {
    "is_latest_block_number": true,
    "current_block_number": 6088191
  }
}
```

**Usage**

```shell
curl -X POST https://indexer-v1.did.id/v1/server/info
```

or json rpc style:

```shell
curl -X POST https://indexer-v1.did.id -d'{"jsonrpc": "2.0","id": 1,"method": "das_serverInfo","params": []}'
```

### Get Reverse Record Info
* You need to set an alias for it to take effect.
* [❓How to set an alias](https://app.did.id/alias)

**Request**
* host: `indexer-v1.did.id`
* path: `/v1/reverse/record`
* param:

```javascript
{
  "type": "blockchain",
  "key_info": {
    "coin_type": "", // 60: ETH, 195: TRX, 9006: BNB, 966: Matic
    "chain_id": "", // 1: ETH, 56: BSC, 137: Polygon
    "key": "" // address
  }
}
```

**Response**

```json
{
  "errno": 0,
  "errmsg": "",
  "data": {
    "account": ""
  }
}
```


**Usage**

```shell
curl -X POST https://indexer-v1.did.id/v1/reverse/record -d'{"type": "blockchain","key_info":{"coin_type": "60","chain_id": "1","key": "0x9176acd39a3a9ae99dcb3922757f8af4f94cdf3c"}}'
```

or json rpc style:

```shell
curl -X POST https://indexer-v1.did.id -d'{"jsonrpc": "2.0","id": 1,"method": "das_reverseRecord","params": [{"type": "blockchain","key_info":{"coin_type": "60","chain_id": "1","key": "0x9176acd39a3a9ae99dcb3922757f8af4f94cdf3c"}}]}'
```

### Get Account Basic Info

**Request**

* host: `indexer-v1.did.id`
* path: `/v1/account/info`
* param:
  * You can provide either `account` or `account_id`. The `account_id` will be used, if you provide both.

```json
{
  "account": "phone.bit",
  "account_id": ""
}
```

**Response**

```javascript
{
  "errno": 0,
  "errmsg": "",
  "data": {
    "out_point": {
      "tx_hash": "0xabb6b2f502e9d992d00737a260e6cde53ad3f402894b078f60a52e0392a17ec8",
      "index": 0
    },
    "account_info": {
      "account": "phone.bit",
      "account_id_hex": "0x5f560ec1edc638d7dab7c7a1ca8c3b0f6ed1848b",
      "next_account_id_hex": "0x5f5c20f6cd95388378771ca957ce665f084fe23b",
      "create_at_unix": 1626955542,
      "expired_at_unix": 1658491542,
      "status": 1,
      "das_lock_arg_hex": "0x0559724739940777947c56c4f2f2c9211cd5130fef0559724739940777947c56c4f2f2c9211cd5130fef",
      "owner_algorithm_id": 5, // 3: eth personal sign, 4: tron sign, 5: eip-712
      "owner_key": "0x59724739940777947c56c4f2f2c9211cd5130fef",
      "manager_algorithm_id": 5,
      "manager_key": "0x59724739940777947c56c4f2f2c9211cd5130fef"
    }
  }
}
```

**Usage**

```shell
curl -X POST https://indexer-v1.did.id/v1/account/info -d'{"account":"phone.bit"}'
```

or json rpc style:

```shell
curl -X POST https://indexer-v1.did.id -d'{"jsonrpc": "2.0","id": 1,"method": "das_accountInfo","params": [{"account":"phone.bit"}]}'
```

### Get Account List

**Request**

* host: `indexer-v1.did.id`
* path: `/v1/account/list`
* param:

```javascript
{
  "type": "blockchain",
  "key_info": {
    "coin_type": "", // 60: ETH, 195: TRX, 9006: BNB, 966: Matic
    "chain_id": "", // 1: ETH, 56: BSC, 137: Polygon
    "key": "" // address
  }
}
```

**Response**

```javascript
{
  "errno":0,
  "errmsg":"",
  "data":{
    "account_list":[
      {
        "account":""
      }
      // ...
    ]
  }
}
```

**Usage**

```shell
curl -X POST https://indexer-v1.did.id/v1/account/list -d'{"type": "blockchain","key_info":{"coin_type": "60","chain_id": "1","key": "0x3a6cab3323833f53754db4202f5741756c436ede"}}'
```

or json rpc style:

```shell
curl -X POST https://indexer-v1.did.id -d'{"jsonrpc": "2.0","id": 1,"method": "das_accountList","params": [{"type": "blockchain","key_info":{"coin_type": "60","chain_id": "1","key": "0x3a6cab3323833f53754db4202f5741756c436ede"}}]}'
```

### Get Account Records Info

**Request**

* host: `http://127.0.0.1:8122`
* path: `/v1/account/records`
* param:

```json
{
  "account": "phone.bit"
}
```

**Response**
* key: https://github.com/dotbitHQ/cell-data-generator/blob/master/data/record_key_namespace.txt

```javascript
{
  "errno": 0,
  "errmsg": "",
  "data": {
    "account": "phone.bit",
    "records": [
      {
        "key": "address.btc",
        "label": "Personal account",
        "value": "3EbtqPeAZbX6wmP6idySu4jc2URT8LG2aa",
        "ttl": "300"
      },
      {
        "key": "address.eth",
        "label": "Personal account",
        "value": "0x59724739940777947c56C4f2f2C9211cd5130FEf",
        "ttl": "300"
      }
      // ...
    ]
  }
}
```


**Usage**

```shell
curl -X POST https://indexer-v1.did.id/v1/account/records -d'{"account":"phone.bit"}'
```

or json rpc style:

```shell
curl -X POST https://indexer-v1.did.id -d'{"jsonrpc": "2.0","id": 1,"method": "das_accountRecords","params": [{"account":"phone.bit"}]}'
```

### Get Account Records Info V2

The return field [key] from [SLIP-0044](https://github.com/satoshilabs/slips/blob/master/slip-0044.md).

**Request**

* host: `http://127.0.0.1:8122`
* path: `/v2/account/records`
* param:

```json
{
  "account": "phone.bit"
}
```

**Response**
* key: https://github.com/satoshilabs/slips/blob/master/slip-0044.md

```javascript
{
  "errno": 0,
  "errmsg": "",
  "data": {
    "account": "phone.bit",
    "records": [
      {
        "key": "address.0",
        "label": "Personal account",
        "value": "3EbtqPeAZbX6wmP6idySu4jc2URT8LG2aa",
        "ttl": "300"
      },
      {
        "key": "address.60",
        "label": "Personal account",
        "value": "0x59724739940777947c56C4f2f2C9211cd5130FEf",
        "ttl": "300"
      }
      // ...
    ]
  }
}
```


**Usage**

```shell
curl -X POST https://indexer-v1.did.id/v2/account/records -d'{"account":"phone.bit"}'
```

or json rpc style:

```shell
curl -X POST https://indexer-v1.did.id -d'{"jsonrpc": "2.0","id": 1,"method": "das_accountRecordsV2","params": [{"account":"phone.bit"}]}'
```


## _Deprecated API List_

### _Get Account Basic Info And Records_ `Deprecated`

 _**Request**_

* path: `/v1/search/account`
* param:

```json
{
  "account": "phone.bit"
}
```

 _**Response**_

```javascript
{
  "errno": 0,
  "errmsg": "",
  "data": {
    "out_point": {
      "tx_hash": "0xabb6b2f502e9d992d00737a260e6cde53ad3f402894b078f60a52e0392a17ec8",
      "index": 0
    },
    "account_data": {
      "account": "phone.bit",
      "account_id_hex": "0x5f560ec1edc638d7dab7c7a1ca8c3b0f6ed1848b",
      "next_account_id_hex": "0x5f5c20f6cd95388378771ca957ce665f084fe23b",
      "create_at_unix": 1626955542,
      "expired_at_unix": 1658491542,
      "status": 1,
      "das_lock_arg_hex": "0x0559724739940777947c56c4f2f2c9211cd5130fef0559724739940777947c56c4f2f2c9211cd5130fef",
      "owner_address_chain": "ETH",
      "owner_lock_args_hex": "0x0559724739940777947c56c4f2f2c9211cd5130fef",
      "owner_address": "0x59724739940777947c56c4f2f2c9211cd5130fef",
      "manager_address_chain": "ETH",
      "manager_address": "0x59724739940777947c56c4f2f2c9211cd5130fef",
      "manager_lock_args_hex": "0x0559724739940777947c56c4f2f2c9211cd5130fef",
      "records": [
        {
          "key": "address.btc",
          "label": "Personal account",
          "value": "3EbtqPeAZbX6wmP6idySu4jc2URT8LG2aa",
          "ttl": "300"
        },
        {
          "key": "address.eth",
          "label": "Personal account",
          "value": "0x59724739940777947c56C4f2f2C9211cd5130FEf",
          "ttl": "300"
        }
        // ...
      ]
    }
  }
}
```

 _**Usage**_

```shell
curl -X POST https://indexer-v1.did.id/v1/search/account -d'{"account":"phone.bit"}'
```

or json rpc style:

```shell
curl -X POST https://indexer-v1.did.id -d'{"jsonrpc": "2.0","id": 1,"method": "das_searchAccount","params": ["phone.bit"]}'
```

### _Get Related Accounts By Owner Address_ `Deprecated`

 _**Request**_

* path: `/v1/address/account`
* param:

```json
{
  "address": "0x773BCCE3B8b41a37CE59FD95F7CBccbff2cfd2D0"
}
```

 _**Response**_

```javascript
{
  "errno": 0,
  "errmsg": "",
  "data": [
    {
      "out_point": {
        "tx_hash": "0xdad77b108e447f4ddd905214021594d69ef50a5b06baf84686031a0d9b45265c",
        "index": 0
      },
      "account_data": {
        "account": "werwefdsft3.bit",
        "account_id_hex": "0xb97565e427dca668f9989c6a2149d8ab3ef37a29",
        "next_account_id_hex": "0xb97577b49a2f5889627d1baa5af5129c4c1ebf9d",
        "create_at_unix": 1631618255,
        "expired_at_unix": 1664968655,
        "status": 1,
        "das_lock_arg_hex": "0x05773bcce3b8b41a37ce59fd95f7cbccbff2cfd2d005773bcce3b8b41a37ce59fd95f7cbccbff2cfd2d0",
        "owner_address_chain": "ETH",
        "owner_lock_args_hex": "0x05773bcce3b8b41a37ce59fd95f7cbccbff2cfd2d0",
        "owner_address": "0x773bcce3b8b41a37ce59fd95f7cbccbff2cfd2d0",
        "manager_address_chain": "ETH",
        "manager_address": "0x773bcce3b8b41a37ce59fd95f7cbccbff2cfd2d0",
        "manager_lock_args_hex": "0x05773bcce3b8b41a37ce59fd95f7cbccbff2cfd2d0",
        "records": [
          {
            "key": "profile.twitter",
            "label": "",
            "value": "egtfghdfhfg",
            "ttl": "300"
          },
          {
            "key": "profile.facebook",
            "label": "",
            "value": "沃尔特图和",
            "ttl": "300"
          }
        ]
      }
    }
    // ...
  ]
}
```

 _**Usage**_

```shell
curl -X POST https://indexer-v1.did.id/v1/address/account -d'{"address":"0x773BCCE3B8b41a37CE59FD95F7CBccbff2cfd2D0"}'
```

or json rpc style:

```shell
curl -X POST https://indexer-v1.did.id -d'{"jsonrpc": "2.0","id": 1,"method": "das_getAddressAccount","params": ["0x773BCCE3B8b41a37CE59FD95F7CBccbff2cfd2D0"]}'
```

## Error
### Error Example
```json
{
  "errno": 20007,
  "errmsg": "account not exist",
  "data": null
}
```
### Error Code
```go

const (
  ApiCodeSuccess              Code = 0
  ApiCodeError500             Code = 500
  ApiCodeParamsInvalid        Code = 10000
  ApiCodeMethodNotExist       Code = 10001
  ApiCodeDbError              Code = 10002
  
  ApiCodeAccountFormatInvalid Code = 20006
  ApiCodeAccountNotExist      Code = 20007
)

```
    
