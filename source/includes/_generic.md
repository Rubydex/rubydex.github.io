# Overview
Welcome to the [RubyDex](https://rubydex.com) API documentation. We offer REST and Websocket APIs to interact with our systems.

## General API information
* Rest API base endpoints:
  * Default endpoint: **https://api200.rubydex.com**
  * TestNet endpoint: **https://testnet-api.rubydex.com**
* WebSocket API base endpoints:
  * Default endpoint: **wss://rubydex.com/ws**
  * TestNet endpoint: **wss://testnet.rubydex.com/ws**
* RubyDex provides HTTP Rest API for client to operate Orders, all endpoints return a JSON object.

## REST API Standards
### HTTP return codes

* HTTP `401` return code is used when unauthenticated
* HTTP `403` return code is used when lack of priviledge.
* HTTP `429` return code is used when breaking a request ratelimit.
* HTTP `5XX` return codes are used for  internal errors. Note: This doesn't means the operation failed, the execution status is **UNKNOWN** and could be Succeed.

### REST request header

Every HTTP Rest Request must have the following Headers:

* **apikey** : This is *APIKEY* from Rubydex site.
* **expiry** : This describes the Unix *EPoch Microseconds* to expire the request, normally it should be (Now() + 1 minute)
* **signature** : This is HMAC SHA256 signature of the http request. Secret is *API Secret*, its formula is : HMacSha256(body + expiry)

### REST request format

> Request general format

```javascript
{
    "server_name": "AdminSvr",
    "method": "querySymbol",
    "content": {}
}
```
* All restful API  shares same request format.

| Field       | Description                  |
|-------------|------------------------------|
| server_name | Which server to send request |
| methond     | Request service Topic        |
| content     | Parameter object             |

### REST response format

> Response general format

```javascript
{
  "data": <data>,
  "ret_msg": <msg>,
  "ret_code": <code>
  "cid": <cid>
}
```

* All restful API  shares same response format.

| Field    | Description                                             |
|----------|---------------------------------------------------------|
| data     | operation dependant                                     |
| ret_msg  | when code is non-zero, it gives short error description |
| ret_code | 0 means `success`, non-zero means `error`               |
| cid      | customer request id                                     |

## Rate limits
* In order to protect the exchange, Rubydex apply `API RateLimit` and `IP Ratelimit` on all requests.
* Rest API has a request capacity in one minute window on user basis.
* IP has request capacity in 5 minutes window.
* Ratelimit of API is **independant** of that in WEB/APP, so if one get ratelimited in API, one can place/cancel orders via WEB or APP.

### IP ratelimits
   Currently Rubydex restrict every IP 5,00 request in 5 minutes window.
   If exceeded this IP capacity, the user would be blocked in the following 5 minutes.

### REST API ratelimit rules
* [Order spamming limitations]
* RubyDex API now only support `contract`.


* Capacity

| Name      | Capacity   |
|-----------|------------|
| Contract  | 500/minute |

## Contact us
* [Rubydex Customer Support](https://support.rubydex.com/)

## Code samples
* Java Sample Client: **

## Error codes

### BizError field
| Code | Description                                                                                                                                            |
|------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| 9999 | ID for a duplicate request                                                                                                                             |
| 1001 | ID for a duplicate order                                                                                                                               |
| 1002 | Unable to locate order ID                                                                                                                              |
| 1003 | Orders that are already in the pending cancel status cannot be cancelled                                                                               | 
| 1004 | Cannot cancel an order that has already reached the pending cancel status                                                                              |
| 1005 | Orders that are in the pending cancel status cannot be cancelled at this time                                                                          | 
| 2001 | Insufficient balance on hand                                                                                                                           |
| 2002 | Risk cap value that is not valid                                                                                                                       | 
| 2003 | Insufficient balance on hand                                                                                                                           |
| 2004 | Incorrect input or increased leverage over the maximum permitted leverage                                                                              | 
| 2005 | Insufficient balance on hand                                                                                                                           |
| 2006 | Size of a position is zero. Margin cannot be changed                                                                                                   |
| 2007 | Margin under CrossMargin cannot be changed                                                                                                             |
| 2008 | The detachable Margin is greater than the maximum                                                                                                      |
| 2009 | The maximum Detachable Margin exceeds that                                                                                                             |
| 2010 | Insufficient balance on hand                                                                                                                           |
| 2011 | Reduce only cannot be accepted                                                                                                                         |
| 2012 | Error in order quantity                                                                                                                                |
| 2013 | Size of a position is zero. cannot establish the quantity of a conditional order                                                                       |
| 2014 | The side of a close position conditional order is the same                                                                                             |
| 2015 | Conditional order already triggered or canceled                                                                                                        |
| 2016 | The incorrect trading engine receives the request                                                                                                      |
| 2017 | Requested position cannot be found on the current account                                                                                              |
| 2018 | A funding fee is not required to be paid by the current account                                                                                        |
| 2019 | The funding fee is already covered by the current account                                                                                              |
| 2020 | All remaining bonus must be removed by withdrawing to wallet. But if the bonus is spent for the position or order cost, the withdrawal is unsuccessful |
| 2021 | Bonus amount is invalid                                                                                                                                |
| 2022 | Account is suspended                                                                                                                                   |
| 2023 | Account is now being liquidated                                                                                                                        | 
| 2024 | Account is now undergoing auto-deleverage                                                                                                              |
| 2025 | The incorrect trading engine receives the request                                                                                                      |
| 2026 | Account mismatch                                                                                                                                       |
| 2027 | Symbol that is not valid                                                                                                                               |
| 2028 | Currency that is not valid                                                                                                                             |
| 2029 | Activity is ineffective                                                                                                                                |
| 2030 | Activity is ineffective                                                                                                                                |
| 2031 | The maximum number of conditional orders has been reached                                                                                              |
| 2032 | The maximum number of active orders is exceeded                                                                                                        |
| 2033 | ID for a duplicate order ID                                                                                                                            |
| 2034 | Incorrect side                                                                                                                                         |
| 2035 | Incorrect order type                                                                                                                                   |
| 2036 | Icorrect timeinforce                                                                                                                                   |
| 2037 | Incorrect trade type                                                                                                                                   |
| 2038 | Incorrect trigger type                                                                                                                                 |
| 2039 | Incorrect stop direction type                                                                                                                          |
| 2040 | No appropriate mark price is available to generate a conditional order                                                                                 |
| 2041 | No relevant index price is available to generate a conditional order                                                                                   |
| 2042 | No valid last market price was available to create a conditional order                                                                                 |
| 2043 | The immediate triggering of the conditional order                                                                                                      |
| 2044 | The immediate triggering of the conditional order                                                                                                      |
| 2045 | Too high of a conditional order trigger price                                                                                                          |
| 2046 | Too low of a conditional order trigger price                                                                                                           |
| 2047 | The trigger price for a TakeProfit BUY conditional order must be higher than the reference price                                                       |
| 2048 | StopLoss condition: BUY The order price must be lower than the reference price                                                                         |
| 2049 | StopLoss condition: BUY If the order price is lower than the liquidation price, the trigger will not go off                                            |
| 2050 | The trigger price for a TakeProfit SELL conditional order must be lower than the reference price                                                       |
| 2051 | Condition of SELL for StopLoss For the order to trigger, the order price must be lower than the liquidation price                                      |
| 2052 | Order price for the StopLoss SELL condition must be higher than the reference price                                                                    |
| 2053 | The order price is excessive                                                                                                                           |
| 2054 | If an order contains instructions to close a position, its price cannot be more aggressive than the bankrupt price                                     |
| 2055 | Cost of the order is too low                                                                                                                           |
| 2056 | Order volume is excessive                                                                                                                              |
| 2057 | Not permitted Reduce only a positionless order                                                                                                         |
| 2058 | Order quantity is insufficient                                                                                                                         |
| 2059 | Size of a position is zero. cannot accept any orders with a take profit or stop loss                                                                   |
| 2060 | The wrong side of a take-profit or stop-loss order. position cannot be closed                                                                          |
| 2061 | Repeated requests to cancel                                                                                                                            |
| 2062 | Order has been canceled already                                                                                                                        |
| 2063 | Order cannot be canceled as it stands right now                                                                                                        |
| 2064 | Order is already in pending replace state, hence replace request is denied                                                                             | 
| 2065 | Replace requests do not alter any order parameters                                                                                                     | 
| 2066 | Order cannot be changed in its current state                                                                                                           | 
| 2067 | Market conditional orders are not allowed to amend prices                                                                                              |
| 2068 | Condtional orders for closing positions cannot affect order quantities because position sizes already determine the order quantities                   |
| 2069 | The request's account ID is invalid or beyond the acceptable range for the current process                                                             |
| 2070 | Data that is not valid                                                                                                                                 |
| 2071 | Order staus  unable to trigger                                                                                                                         |
| 2072 | Commission that is not valid                                                                                                                           |
| 2073 | Commission that is not valid                                                                                                                           |
| 2074 | TP/SL parameters cannot be included in an order request if the account already holds positions                                                         |
| 2075 | Take-profit pricing is excessive                                                                                                                       |
| 2076 | TakeProfit pricing is inadequate                                                                                                                       |
| 2077 | Trigger type that is not valid                                                                                                                         |
| 2078 | Stop-loss pricing is excessive                                                                                                                         |
| 2079 | Stop-loss pricing is inadequate                                                                                                                        |
| 2080 | Trigger type that is not valid                                                                                                                         |
| 2081 | Total position potential exceeds existing risk cap                                                                                                     |
| 2082 | The remaining balance does not have enough money to pay for this new order's possible unrealized PnL                                                   |
| 2083 | Existing Take Profit order                                                                                                                             |
| 2084 | Existing Stop Lossorder                                                                                                                                |
| 2085 | CID for a duplicate request                                                                                                                            |
| 2086 | Pegprice type that is not valid                                                                                                                        |
| 2087 | StopPrice for the trailing order must be lower than the most recent price                                                                              |
| 2088 | The StopPrice of the traling order ought to be higher than the current liquidation price                                                               |
| 2089 | The StopPrice of the traling order ought to be higher than the current liquidation price                                                               |
| 2090 | The StopPrice of the traling order ought to be lower than the current liquidation price                                                                |
| 2091 | PegOffset ought to be lower than zero                                                                                                                  |
| 2092 | PegOffset must be higher than zero                                                                                                                     |
| 2093 | The activation price need to be higher than the most recent pricing                                                                                    |
| 2094 | The activation cost ought to be less than the most recent price                                                                                        |
| 2095 | Trailing order for a duplicate request                                                                                                                 |
| 2096 | There cannot be a trailing instruction on an order to close position                                                                                   |
| 2097 | This cryptocurrency is not accepted                                                                                                                    |
| 2098 | Invalid wallet action                                                                                                                                  |
| 2099 | The wallet vid requested for wallet functioning is incorrect.                                                                                          |
| 2100 | Balance in wallet is insufficient                                                                                                                      |
| 2101 | Locked balance in wallet is insufficient to grant a request to unlock or withdraw                                                                      |
| 2102 | The deposit must be for a larger than zero.                                                                                                            |
| 2103 | The amount being withdrawn must not be zero                                                                                                            |
| 2104 | Wallet's maximum allowable amount is exceeded by a deposit                                                                                             |
| 2105 | No money in the base wallet                                                                                                                            |
| 2106 | The quote wallet has insufficient money                                                                                                                |
| 2107 | CrossEngine and TradingEngine were unable to connect                                                                                                   |
| 2108 | Market orders cannot be changed or replaced                                                                                                            |
| 2109 | Neither replace nor modify Order ImmediatelyOrCancel                                                                                                   |
| 2110 | Can't change or replace a FillOrKill order                                                                                                             |
| 2111 | OrderId is empty                                                                                                                                       |
| 2112 | Quality  that is not valid                                                                                                                             |
| 2113 | User_id is not valid                                                                                                                                   |
| 2114 | Order value is excessive                                                                                                                               |
| 2115 | Order value is inadequate                                                                                                                              |
| 2116 | There should be no more than 5 total brakcet orders                                                                                                    |
| 2117 | There should be a single Side for all bracket orders                                                                                                   | 
| 2118 | Invalid Bracker order take profit price                                                                                                                |
| 2119 | Invalid Bracker order stop loss price                                                                                                                  |
| 2120 | Invalid Bracker order stop loss trigger price                                                                                                          |
| 2121 | Unable to change the bracket order                                                                                                                     |
| 2122 | Invalid bracket take profit order status                                                                                                               |
| 2123 | No bracket take profit orders allowed                                                                                                                  |
| 2124 | No bracket stop loss order can be placed                                                                                                               |
| 2125 | Order for brackets SL/TP cannot be cancelled                                                                                                           |
| 2126 | Does not allow API bracket ordering                                                                                                                    |
| 2128 | Invalid ExecInst value                                                                                                                                 |
| 2129 | The side of the position should match the side of the bracket order                                                                                    |
| 2130 | Invalid Bracket stop loss order trigger type                                                                                                           |
| 2131 | Invalid Bracket stop loss order trigger type                                                                                                           |
| 2132 | Void due bracket stop loss order not producing a take profit order                                                                                     |
| 2133 | The primary order is canceled, cancel the bracket stop loss order                                                                                      |
| 2134 | The primary order is canceled, cancel the bracket take profit order                                                                                    |
| 2135 | Invalid order quantity                                                                                                                                 |

