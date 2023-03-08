# Contract REST API

## Endpoint security type

* Each API call must be signed and pass to server in HTTP header `signature`.
* Endpoints use `HMAC SHA256` signatures. The `HMAC SHA256 signature` is a keyed `HMAC SHA256` operation. Use your `apiSecret` as the key and the string ` (body+expiry)` as the value for the HMAC operation.

* The `signature` is **case sensitive**.

### Signature example: HTTP POST request

* API REST Request URL: https://api200.rubydex.com
   * Request Path: /api
   * Request Body: 
```json
   {
     "server_name": "TradeSvr",
     "method": "placeorder",
     "content": {
     "symbol": "BTCUSDT",
     "side": 1,
     "price": "19494.0",
     "order_type": "0",
     "order_qty": "1",
     "display_qty": "1",
     "text": "test",
     "cid": "test-123",
     "time_in_fore": 1,
     "close_on_trigger": 1,
     "peg_off_set_value": "19494.0",
     "trailing_stop_price": "19494.0",
     "peg_price_type": 1,
     "reduce_only": 1,
     "stop_px": "19494.0",
     "stop_loss_px": "19494.0",
     "take_profit_px" "19494.0",
     "trigger": 1,
     "sl_trigger": 1,
     "tp_trigger": 1
  }
```

   * Request Expiry: 1673399997993
   * Signature: 
```json
   HMacSha256({
     "server_name": "TradeSvr",
     "method": "placeorder",
     "content": {
     "symbol": "BTCUSDT",
     "side": 1,
     "price": "19494.0",
     "order_type": "0",
     "order_qty": "1",
     "display_qty": "1",
     "text": "test",
     "cid": "test-123",
     "time_in_fore": 1,
     "close_on_trigger": 1,
     "peg_off_set_value": "19494.0",
     "trailing_stop_price": "19494.0",
     "peg_price_type": 1,
     "reduce_only": 1,
     "stop_px": "19494.0",
     "stop_loss_px": "19494.0",
     "take_profit_px" "19494.0",
     "trigger": 1,
     "sl_trigger": 1,
     "tp_trigger": 1
     }+1673399997993)
```
   * signed string is `{
     "server_name": "TradeSvr",
     "method": "placeorder",
     "content": {
     "symbol": "BTCUSDT",
     "side": 1,
     "price": "19494.0",
     "order_type": "0",
     "order_qty": "1",
     "display_qty": "1",
     "text": "test",
     "cid": "test-123",
     "time_in_fore": 1,
     "close_on_trigger": 1,
     "peg_off_set_value": "19494.0",
     "trailing_stop_price": "19494.0",
     "peg_price_type": 1,
     "reduce_only": 1,
     "stop_px": "19494.0",
     "stop_loss_px": "19494.0",
     "take_profit_px" "19494.0",
     "trigger": 1,
     "sl_trigger": 1,
     "tp_trigger": 1
     }1673399997993`

## Request/Response field explaination

### Leverage
   * The absolute value of `leverage` determines initial-margin-rate, i.e. `initialMarginRate = 1/abs(leverage)`
   * The sign of `leverage` indicates margin mode, i.e. `leverage = 0` means `cross-margin-mode`, `leverage > 0` means `isolated-margin-mode`.
   * The result of setting `leverage` to `0` is leverage to maximum leverage supported by user selected risklimit, and margin-mode is `cross-margin-mode`.

### Cross margin mode
   * `Position margin` includes two parts, one part is balance assigned to position, another part is account available balance.
   * Position in cross-margin-mode may be affected by other position, because account available balance is shared among all positions in cross mode.

### Isolated margin mode
   * `Position margin` only includes balance assgined to position, by default it is initial-margin.
   * Position in isolatd-margin-mode is independent of other positions.

## Query product information

> Request

```
POST /api
```
```json
{
    "server_name": "AdminSvr",
    "method": "querySymbol",
    "content": {}
}
```
| Field       | Type   | Requried | Description | Value       |
|-------------|--------|----------|-------------|-------------|
| server_name | String | Yes      |             | AdminSvr    |
| method      | String | Yes      |             | querySymbol |
| content     | Object | Yes      |             |             |

> Response sample

```json
{
	"msg": "NO_ERROR",
	"code": 0,
	"data": {
		"symbols": [
			{
				"activity": "1",
				"adl_enable": "1",
				"base_currency": "BTC",
				"close_by": "500012",
				"contract_size": "1",
				"create_time": "2023-01-03 14:31:00",
				"expiried": "0",
				"funding_interval": 28800,
				"funding_symbol": ".BTCUSDTFR",
				"id": 1,
				"index_symbol": ".BTCUSDT",
				"initial_margin": "0.01000000",
				"maint_margin": "0.00500000",
				"maker_commission": "0.00020000",
				"mark_symbol": ".BTCUSDTMP",
				"max_order_qty": "10.00000000",
				"max_price": " ",
				"predicate_funding_symbol": ".BTCUSDTPREDFR",
				"predicted_rate": "",
				"price_precision": 1,
				"qty_precision": 4,
				"qty_tick_size": "0.00010000",
				"quote_currency": "USDT",
				"risk_limit": "",
				"risk_step": "9",
				"symbol": "BTCUSDT",
				"symbol_cn_name": "BTC/USDT",
				"symbol_en_name": "BTCUSDT",
				"taker_commission": "0.00060000",
				"tick_size": "0.10000000",
				"ticker_root": "USDT",
				"tip_order_qty": "2.00000000",
				"update_time": "2023-01-03 14:31:00",
				"value_precision": 4,
				"volume_precision": 4
			}
		]
	},
	"cid": ""
}
```
| Field                    | Description                          |
|--------------------------|--------------------------------------|
| activity                 | Trade active level                   |
| adl_enable               | Automatic position is enabled        |
| base_currency            | Base currency                        |
| close_by                 | Operator                             |
| contract_size            | Contract size                        |
| create_time              | Create time                          |
| expiried                 | go off the market                    |
| funding_interval         | funding interval                     |
| funding_symbol           | Funding symbol                       |
| id                       | id                                   |
| index_symbol             | Index price symbol                   |
| initial_margin           | Initial Margin rate                  |
| maint_margin             | Maintenance Margin rate              |
| maker_commission         | Maker commission                     |
| mark_symbol              | Mark price symbol                    |
| max_order_qty            | Maximum order quantity               |
| max_price                | Max price                            |
| predicate_funding_symbol | Predicate funding symbol             |
| predicted_rate           | Predicted rate                       |
| price_precision          | price precision                      |
| qty_presicion            | Quantity precision                   |
| qty_tick_size            | Quantity tick size                   |
| quote_currency           | Quote currency                       |
| risk_limit               | Rsik limit                           |
| risk_step                | Rsik step                            |
| symbol                   | Product symbol                       |
| symbol_cn_name           | Product symbol chinese name          |
| symebol_en_name          | Product symbol english name          |
| taker_commission         | Taker commission                     |
| tick_size                | Price tick size                      |
| ticker_root              | Ticker root                          |
| tip_order_qty            | Remind when touch tip order quantity |
| update_time              | Update time                          |
| value_precision          | Value precision                      |
| volume_precision         | Chain precision                      |

## Common order fields

* Order Type

| Order Type | Description |
|-----------|-------------|
| Limit | -- |
| Market | -- |
| MarketIfTouched | -- |
| LimitIfTouched | -- |


* Order Status

| Order Status | Description |
|------------|-------------|
| Created | order acked from order request, a transient state |
| Init | Same as `Created`, order acked from order request, a transient state |
| Untriggered | Conditional order waiting to be triggered |
| Triggered | Conditional order being triggered|
| Deactivated | untriggered conditonal order being removed |
| Rejected | Order rejected |
| New | Order placed into orderbook |
| PartiallyFilled | Order partially filled |
| Filled | Order fully filled |
| Canceled | Order canceled |

* TimeInForce

| TimeInForce | Description |
|------------|-------------|
| GoodTillCancel | -- |
| PostOnly | -- |
| ImmediateOrCancel | -- |
| FillOrKill | -- |


* Execution instruction

| Execution Instruction | Description |
|------------|-------------|
| ReduceOnly | reduce position size, never increase position size |
| CloseOnTrigger | close the position  |


* Trigger Source

| Trigger | Description |
|------------|-------------|
| ByMarkPrice | trigger by mark price|
| ByLastPrice | trigger by last price |

## Place order

> Request example

```
POST /api
```

```json
{
    "server_name": "TradeSvr",
    "method": "placeorder",
    "content": {
        "symbol": "BTCUSDT",
        "side": 1,
        "price": "19494.0",
        "order_type": "0",
        "order_qty": "1",
        "display_qty": "1",
        "text": "test",
        "cid": "test-123",
        "time_in_fore": 1,
        "close_on_trigger": 1,
        "peg_off_set_value": "19494.0",
        "trailing_stop_price": "19494.0",
        "peg_price_type": 1,
        "reduce_only": 1,
        "stop_px": "19494.0",
        "stop_loss_px": "19494.0",
        "take_profit_px" "19494.0",
        "trigger": 1,
        "sl_trigger": 1,
        "tp_trigger": 1
        
    }
}
```
| Field               | Type    | Required | Description                                                                                               | Possible values                                                                            |
|---------------------|---------|----------|-----------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| server_name         | String  | Yes      |                                                                                                           | TradeSvr                                                                                   |
| method              | String  | Yes      |                                                                                                           | placeorder                                                                                 |
| content             | Object  | Yes      |                                                                                                           |                                                                                            |
| symbol              | String  | Yes      | Which symbol to place order                                                                               |                                                                                            |
| side                | Integer | Yes      | Order direction, Buy or Sell                                                                              | 1:Buy, 2:Sell                                                                              |
| order_qty           | Integer | Yes      | Order quantity                                                                                            |                                                                                            |
| price               | Integer | Yes      | Price, required for limit order                                                                    |                                                                                            |
| display_qty         | String  | Yes      |                                                                                                           | same value of order_qty                                                                    |
| text                | String  | Yes      |                                                                                                           | rubydex                                                                                    |
| ord_type            | Integer | Yes      | Order type                                                                                                | 1:Market,2:Limit,5:MarketIfTouched,6:LimitIfTouched                                        |
| time_in_Force       | Integer | Yes       | Time in force.                                                                                            | 1:GoodTillCancel,2:PostOnly,3:ImmediateOrCancel,4:FillOrKill                               |
| close_on_trigger    | Boolean | -        | implicitly reduceOnly, plus cancel other orders in the same direction(side) when necessary                | 0:false, 1:true                                                                            |
| peg_off_set_value | string  | -        | Trailing offset from current price. Negative value when position is long, positive when position is short |                                                                                            |
| trailing_stop_price | string  | -        |                                                                                                           |                                                                                            |
| peg_price_type      | Integer | -        | Trailing order price type                                                                                 | 1:LastPeg,2:MidPricePeg,3:MarketPeg,4:PrimaryPeg,5:TrailingStopPeg,6:TrailingTakeProfitPeg |                                                                           
| reduceOnly          | Integer | -        | whether reduce position side only. Enable this flag, i.e. reduceOnly=true, position side won't change     | 0:false, 1:true                                                                            |                                                                                                                                                                                
| sl_trigger          | Integer | -        | Trigger source, by mark-price or last-price                                                               | 1:ByMarkPrice,3:ByLastPrice                                                                |                                                                                                                        
| take_profit_px      | string  | -        |                                                                                                           |                                                                                            |
| trigger             | Integer | -        | Trigger source, by mark-price or last-price                                                               | 1:ByMarkPrice,3:ByLastPrice                                                                |
| tp_trigger          | Integer | -        | Trigger source, by mark-price or last-price                                                               | 1:ByMarkPrice,3:ByLastPrice                                                                |

> Response sample

```json
{
    "data": {
       
    },
    "ret_msg": "NO_ERROR",
    "ret_code": "0",
    "cid": "test-123"
}
```


## Update order by order ID

> Request example

```
POST /api
```

```json
{
    "server_name": "TradeSvr",
    "method": "updateorder",
    "content": {       
        "symbol": "BTCUSDT",
        "order_id": "1c788498-6e50-48d6-9c8b-d6a7d6eb8aae",
        "price": "19492.0",
        "order_qty": "1",
        "display_qty": "1",
        "cid": "test-123"
    }
}
```
| Field       | Type   | Requried | Description | Possible Value          |
|-------------|--------|----------|-------------|-------------------------|
| server_name | String | Yes      |             | TradeSvr                |
| method      | String | Yes      |             | updateorder             |
| content     | Object | Yes      |             |                         |
| symbol      | String | Yes      |             |                         |
| order_id    | String | Yes      |             |                         |
| price       | String | Yes      |             |                         |
| order_qty   | String | Yes      |             |                         |
| display_qty | String | Yes      |             | same value of order_qty |
| text        | String | Yes      |             |                         |

> Response sample

```json
{
   "data": {
},
"ret_msg": "NO_ERROR",
"ret_code": 0,
"cid": "test-123"
}
```

## Cancel order by order ID 

> Request format

```
POST /api
```

> Request sample

```json
{
   "server_name": "TradeSvr",
   "method": "cancelorder",
   "content": {
      "symbol": "BTCUSDT",
      "order_id": "31e43fb2-0471-4467-8cb9-0c7fec1f3202",
      "text": "test",
      "cid": "test-123"
   }
}

```
| Field       | Type   | Requried | Description | Possible Value |
|-------------|--------|----------|-------------|----------------|
| server_name | String | Yes      |             | TradeSvr       |
| method      | String | Yes      |             | cancelorde     |
| content     | Object | Yes      |             |                |
| symbol      | String | Yes      |             |                |
| order_id    | String | Yes      |             |                |
| cid         | String | Yes      |             |                |
| text        | String | Yes      |             |                |
> Response sample

```json
{
   "data": {
             
},
"ret_msg": "NO_ERROR",
"ret_code": 0,
"cid": "test-123"
}
```

## Cancel all orders （NOT Support conditional order）

> Request format

```
POST /api
```

> Request sample

```json
{
   "server_name": "TradeSvr",
   "method": "cancelAll",
   "content": {      
      "symbol": "BTCUSDT",
      "text": "test",
      "cid": "test-123"
   }
}

```
| Field       | Type   | Requried | Description | Possible Value |
|-------------|--------|----------|-------------|----------------|
| server_name | String | Yes      |             | TradeSvr       |
| method      | String | Yes      |             | cancelAll      |
| content     | Object | Yes      |             |                |
|    symbol   | String | Yes      |             |                |
|    order_id | String | Yes      |             |                |
|    cid      | String | Yes      |             |                |
|    text     | String | Yes      |             |                |

> Response sample

```json
{
   "data": {
             
},
"ret_msg": "NO_ERROR",
"ret_code": 0,
"cid": "test-123"
}
```

## Query trading account

> Request format

```
POST /api
```
> Request sample

```json
{
   "server_name": "AdminSvr",
   "method": "queryBalance",
   "content": {
      "cid": "test-123"
   }
}

```
| Field       | Type    | Requried | Description | Possible Value |
|-------------|---------|----------|-------------|----------------|
| server_name | String  | Yes      |             | AdminSvr       |
| method      | String  | Yes      |             | queryBalance   |
| content     | Object  | Yes      |             |                |
|       cid   | String  | Yes      |             |                |

> Response sample

```json
{
	"msg": "NO_ERROR",
	"code": 0,
	"data": {
		"balances": [
			{
				"account_balance": 1289.293332,
				"account_id": 705232705,
				"bonus_balance": 0.0,
				"close_by": "500020",
				"currency": "USDT",
				"currency_id": 1,
				"id": 0,
				"lock_balance": 181.0,
				"update_time": "2023-03-07 02:00:00",
				"used_balance": 435.369406,
				"user_id": 500020
			}
		]
	}
}
```

| Field           | Type    | Description                                                         | Possible values |
|-----------------|---------|---------------------------------------------------------------------|-----------------|
| account_balance | String  | User total account balance                                          |                 |
| account_id      | Integer | Account id                                                          |                 |
| bonus_balance   | String  | User bonus balance                                                  |                 |
| close_by        | String  | Last operator                                                       |                 |
| currency        | String  | Trading account's settle currency. Use to identify trading account. | 1:USDT          |
| currency_id     | Integer | Currency id                                                         |                 |
| id              | Integer | Self-growth id                                                      |                 |
| lock_balance    | decimal | Lock balance                                                        |                 |
| update_time     | String  | Last update time                                                    |                 |
| user_id         | Integer | User id                                                             |                 |
| used_balance    | decimal | User used balance                                                   |                 |

## Query positions by symbol

> Request format

```
POST /api
```
> Request sample

```json
{
   "server_name": "AdminSvr",
   "method": "queryPosition",
   "content": {
      "symbol": "BTCUSDT",
      "start_date": "2022-09-20",
      "end_date": "2022-09-21",
      "page_num": 0,
      "cid": "test-123"
   }
}

```
| Field       | Type    | Requried | Description       | Possible Value |
|-------------|---------|----------|-------------------|---------------|
| server_name | String  | Yes      |                   | AdminSvr      |
| method      | String  | Yes      |                   | queryPosition |
| content     | Object  | Yes      |                   |               |
| symbol      | String  | Yes      | Trading symbol    |               |
| start_date  | String  | Yes      | Start time        |               |
| end_time    | String  | Yes      | End time          |               |
| page_num    | Integer | Yes      | Page number       |               |
| cid         | String  | Yes      | client request id |               |

> Response sample

```json
{
	"msg": "NO_ERROR",
	"code": 0,
	"data": {
		"positions": [
			{
				"account_id": 705232705,
				"avg_entry_px": 20307.997465,
				"bank_rupt_comm_fee": 2.050702,
				"bank_rupt_price": 18277.2,
				"buy_leaves_qty": 0.0,
				"buy_leaves_value": 0.0,
				"buy_value_to_cost": 0.10114,
				"close_by": "500020",
				"closed_pnl": -30.266474,
				"currency": "USDT",
				"currency_id": 1,
				"deleverage_percentile": 0.0,
				"display_leverage": 10.0,
				"external_create_time": "1678147200000639337",
				"free_cost": 0.0,
				"free_qty": -0.187,
				"id": 0,
				"init_margin_rate": 0.1,
				"leverage": "10.0",
				"liq_price": 18378.8,
				"maint_margin_rate": 0.005,
				"order_cost": 0.0,
				"pos_balance": 381.810255,
				"pos_cost": 381.810255,
				"position_margin": 379.759553,
				"risk_limit": 500000.0,
				"sell_leaves_qty": 0.0,
				"sell_leaves_value": 0.0,
				"sell_value_to_cost": 0.0,
				"side": 1,
				"size": 0.187,
				"status": 0,
				"symbol": "BTCUSDT",
				"symbol_id": 1,
				"update_time": "2023-03-07 02:00:00",
				"user_id": 500020,
				"value": 3797.595526
			}
		]
	},
	"cid": "test-123"
}
```

| Field                    | Type    | Description                                                         | Possible values                      |
|--------------------      |---------|---------------------------------------------------------------------|--------------------------------------|
| position                 | List    | Position information                                                |                                      |
| account_id               | Integer | Account id                                                          |                                      |
| avg_entry_price          | decimal | Position average entry price                                        |                                      |
| bank_rupt_comm_fee       | decimal | Bankruptcy Price Liquidation Fees                                   |                                      |
| bank_rupt_price          | decimal | Bankrupt price                                                      |                                      |
| buy_leaves_qty           | decimal | Buy side  Cumulative pending orders remaining quantity              |                                      |
| buy_leaves_value         | decimal | Buy side  Cumulative pending order value                            |                                      |
| buy_value_to_cost        | decimal | Coefficient from value to cost in the direction of Buy              |                                      |
| close_by                 | String  | Last operator                                                       |                                      |
| closed_pnl               | decimal | Position realised PNL                                               |                                      |
| currency                 | String  | trading account's settle currency. Use to identify trading account. | 1:USDT                               |
| currency_id              | Integer | Currency id                                                         |                                      |
| deleverage_percentile    | decimal | Rank of automatic position reduction ranking                        |                                      |
| display_leverage         | decimal | Display leverage                                                    |                                      |
| external_create_time     | String  | External create time                                                |                                      |
| free_cost                | decimal | Buy | Sell difference between OrderMargin in two directions         |                                      |
| free_qty                 | decimal | Size of positions that haven't been deducted by the active order    |                                      |
| id                       | Integer | Self-growth id                                                      |                                      |
| init_margin_rate         | decimal | Initial margin rate                                                 |                                      |
| leverage                 | String  | Postion Leverage                                                    | 0:cross(100X); >0 isolate            |
| liq_price                | decimal | Liquidation price                                                   |                                      |
| maint_margin_rate        | decimal | Maintenance margin rate                                             |                                      |
| order_cost               | decimal | Occupancy cost of activity order                                    |                                      |
| pos_balance              | decimal | Position Remaining Balance Amount                                   |                                      |
| pos_cost                 | decimal | Position value Margin based on leverage                             |                                      |
| position_margin          | decimal | Position margin                                                     |                                      |
| pos_balance              | decimal | Margin converted from position value to level                       |                                      |
| pos_cost                 | decimal | Margin converted from position value to level                       |                                      |
| risk_limit               | decimal | Risk limit                                                          |                                      |
| sell_leaves_qty          | decimal | Sell side  Cumulative pending orders remaining quantity             |                                      |
| sell_leaves_value        | decimal | Sell side  Cumulative pending orders value                          |                                      |
| sell_value_to_cost       | decimal | Coefficient from value to cost in the direction of Sell             |                                      |
| side                     | Integer | Postion direction                                                   | 1:buy,2:sell                         |
| size                     | decimal | Position size                                                       |                                      |
| status                   | Integer | Position status                                                     | 0:normal;1.liqdation;2.adl,3:disable |
| symbol                   | String  | Trading symbol                                                      |                                      |
| symbol_id                | Integer | Symbol id                                                           |                                      |
| update_time              | String  | Update time                                                         |                                      |
| user_id                  | Integer | User id                                                             |                                      |
| value                    | decimal | Positon total value                                                 |                                      |                   

## Set leverage

> Request format

```
POST /api
```
> Request sample

```json
{
   "server_name": "TradeSvr",
   "method": "setLeverage",
   "content": {     
      "symbol": "BTCUSDT",
      "leverage": "0",
      "cid": "test-123"
   }
}

```
| Field       | Type    | Requried | Description | Possible Value  |
|-------------|---------|----------|-------------|-----------------|
| server_name | String  | Yes      |             | TradeSvr        |
| method      | String  | Yes      |             | setLeverage     |
| content     | Object  | Yes      |             |                 |
| symbol      | String  | Yes      |             |                 |
| leverage    | String  | Yes      |             |                 |
| cid         | String  | Yes      |             |                 |


> Response sample

```json
{
   "data": {         
},
"ret_msg": "NO_ERROR",
"ret_code": 0,
"cid": "test-123"
}
```

## Assign position balance in isolated marign mode

> Request format

```
POST /api
```
> Request sample

```json
{
   "server_name": "TradeSvr",
   "method": "assignPosBalance",
   "content": {     
      "symbol": "BTCUSDT",
      "assign_pos_balance": "3000",
      "cid": "test-123"
   }
}

```
| Field              | Type    | Requried | Description | Possible Value  |
|--------------------|---------|----------|-------------|-----------------|
| server_name        | String  | Yes      |             | TradeSvr        |
| method             | String  | Yes      |             | assignPosBalance|
| content            | Object  | Yes      |             |                 |
| symbol             | String  | Yes      |             |                 |
| assign_pos_balance | String  | Yes      |             |                 |
| cid                | String  | Yes      |             |                 |


> Response sample

```json
{
   "data": {
       
},
"ret_code": 2006,
"cid": "test-123"
}
```

## Query  orders by order staus

> Request format

```
POST /api
```
> Request sample

```json
{
	"server_name": "AdminSvr",
	"method": "queryOrder",
	"content": {
		"ord_status": "5,6,8",
		"order_type": "2",
		"start_date": "2022-09-20",
		"end_date": "2023-10-20",
		"page_num": 0,
		"page_size": 10,
		"sort_field": "update_time",
		"sort_type": "asc",
		"cid": "test-123"
	}
}

```
| Field        | Type    | Requried | Description | Possible Value                                                                               |
|--------------|---------|----------|-------------|----------------------------------------------------------------------------------------------|
| server_name  | String  | Yes      |             | AdminSvr                                                                                     |
| method       | String  | Yes      |             | queryOrder                                                                                   |
| content      | Object  | Yes      |             |                                                                                              |
| order_status | String  | Yes      |             | 1.untriggered,2.Deactived,3.Triggered,4.Rejected,5.New,6.PartiallyFilled,7.Filled,8.Canceled |
| order_type   | String  | Yes      |             |                                                                                              |
| start_date   | String  | Yes      |             |                                                                                              |
| end_date     | String  | Yes      |             |                                                                                              |
| page_num     | Integer | Yes      |             |                                                                                              |
| page_size    | Integer | Yes      |             |                                                                                              |
| sort_field   | String  | No       |             |                                                                                              |
| sort_type    | String  | No       |             |                                                                                              |
| cid          | String  | No       |             |                                                                                              |


> Response sample

```json
{
	"msg": "NO_ERROR",
	"code": 0,
	"data": {
		"total": 102571,
		"orders": [
			{
				"account_id": 705232705,
				"action": 2,
				"action_by": 1,
				"biz_error": "0",
				"clord_id": "C1061038551942037504",
				"close_by": "500020",
				"closed_pnl": 0.0,
				"closed_size": 0.0,
				"create_time": "2023-01-16 14:28:35",
				"cum_qty": 0.01,
				"cum_value": 167.8,
				"currency": "USDT",
				"currency_id": 1,
				"cxl_rej_reason": "0",
				"display_qty": 0.02,
				"exec_fee": 0.0,
				"exec_id": "4ae71336-e44b-39bf-b9d2-752e234818a5",
				"exec_inst": "0",
				"exec_price": 0.0,
				"exec_qty": 0.01,
				"exec_status": "11",
				"exec_value": 0.0,
				"fee_rate": 0.0,
				"id": 0,
				"last_liquidity_ind": "0",
				"leaves_qty": 0.0,
				"leaves_value": 0.0,
				"leverage": "0.0",
				"ord_status": "8",
				"ord_type": "2",
				"order_qty": 0.02,
				"orderid": "1c6cb594-a41a-32d9-890a-981df5d6eaec",
				"orig_ord_type": "0",
				"peg_offset_value": 0.0,
				"peg_price_type": 0.0,
				"price": 16780.0,
				"side": 1,
				"sl_trigger": 0.0,
				"stop_direction": "0",
				"stop_loss_price": 0.0,
				"stop_price": 0.0,
				"symbol": "BTCUSDT",
				"symbol_id": 1,
				"take_profit_price": 0.0,
				"timeinforce": "1",
				"tp_trigger": 0.0,
				"tradetype": "0",
				"trigger_type": "0",
				"update_time": "2023-01-16 14:28:35",
				"user_id": 500020
			}
		]
	},
	"cid": "test-123"
}

```
| Field                    | Type    | Description                                                         | Possible values                      |
|--------------------      |---------|---------------------------------------------------------------------|--------------------------------------|
| orders                   | List    | Orders information                                                  |                                      |
| account_id               | Integer | Account id                                                          |                                      |
| action                   | Integer | Operat type                                                         |                                      |
| action_by                | Integer | Classification identification of operation                          |                                      |
| biz_error                | Integer | Error code                                                          |                                      |
| clord_id                 | String  | External incoming order number                                      |                                      |
| close_by                 | String  | Last operator                                                       |                                      |
| closed_pnl               | decimal | Closing profit and loss                                             |                                      |
| closed_size              | decimal | Closing size                                                        |                                      |
| create_time              | String  | Create time                                                         |                                      |
| cum_qty                  | decimal | Cumulative number of transactions                                   |                                      |
| cum_value                | decimal | Accumulated transaction value                                       |                                      |
| currency                 | String  | trading account's settle currency. Use to identify trading account. | 1:USDT                               |
| currency_id              | Integer | Current id                                                          |                                      |
| cxl_rej_reason           | String  | Canceled | Rejected, note the specific reason                       |                                      |
| display_qty              | decimal | Order display quantity                                              |                                      |
| exec_fee                 | decimal | Settlement expenses                                                 |                                      |
| exec_id                  | String  | Exec id                                                             |                                      |
| exec_inst                | String  | Additional instructions                                             |                                      |
| exec_price               | decimal | Transaction price                                                   |                                      |
| exec_qty                 | decimal | Number of transactions                                              |                                      |
| exec_status              | String  | Transaction status                                                  |                                      |
| exec_value               | decimal | Transaction value                                                   |                                      |
| fee_rate                 | decimal | settlement rate                                                     |                                      |
| id                       | Integer | Id                                                                  |                                      |
| last_liquidity_ind       | String  | Last liquidity                                                      |                                      |
| leaves_qty               | decimal | Remaining quantity                                                  |                                      |
| leaves_value             | decimal | Surplus value                                                       |                                      |
| leverage                 | String  | Position lever                                                      |                                      |
| ord_status               | String  | Order status                                                        |                                      |
| ord_type                 | String  | Order type                                                          |                                      |
| order_qty                | decimal | Order qty                                                           |                                      |
| orderid                  | String  | Order id                                                            |                                      |
| orig_ord_type            | String  | Original order type                                                 |                                      |
| peg_offset_value         | decimal | Track the price distance of stop loss                               |                                      |
| peg_price_type           | String  | Peg price type                                                      |                                      |
| price                    | decimal | Price                                                               |                                      |
| side                     | Integer | Side                                                                |                                      |
| sl_trigger               | decimal | Sl trigger                                                          |                                      |
| stop_direction           | String  | Calculated trigger price direction                                  |                                      |
| stop_loss_price          | decimal | Stop loss price                                                     |                                      |
| stop_price               | decimal | Stop price                                                          |                                      |
| symbol                   | String  | Symbol                                                              |                                      |
| symbol_id                | Integer | Symbol id                                                           |                                      |
| take_profit_price        | decimal | Opening order with closing profit price                             |                                      |
| timeinforce              | String  | Time in force                                                       |                                      |
| tp_trigger               | decimal | Tp trigger                                                          |                                      |
| tradetype                | String  | Trade type                                                          |                                      |
| trigger_type             | String  | Trigger type                                                        |                                      |
| update_time              | String  | Update time                                                         |                                      |
| user_id                  | Integer | User id                                                             |                                      |

```

## Query user trade by symbol

> Request format

```
POST /api
```
> Request sample

```json
{
    "server_name": "AdminSvr",
    "method": "queryExecOrder",
    "content": {
        "symbol": "BTCUSDT",
        "start_date": "2022-09-20",
        "end_date": "2023-09-21",
        "page_num": 0,
        "page_size": 10,
        "sort_field": "update_time",
        "sort_type": "asc",
        "cid": "test-123"
    }
}

```
| Field       | Type   | Requried | Description | Possible Value |
|-------------|--------|----------|-------------|----------------|
| server_name | String | Yes      |             | AdminSvr       |
| method      | String | Yes      |             | queryExecOrder |
| content     | Object | Yes      |             |                |
| symbol      | String | Yes      |             |                |
| start_date  | String | Yes      |             |                |
| end_date    | String | Yes      |             |                |
| page_num    | Integer| Yes      |             |                |
| page_size   | Integer| Yes      |             |                |
| sort_field  | String | No       |             |                |
| sort_type   | String | No       |             |                |
| cid         | String | Yes      |             |                |


> Response sample

```json
{
	"msg": "NO_ERROR",
	"code": 0,
	"data": {
		"total": 1119,
		"orders": [
			{
				"account_id": 705232705,
				"close_by": "500020",
				"closed_pnl": 0.0,
				"closed_size": 0.0,
				"create_time": "2023-01-16 01:24:17",
				"currency": "USDT",
				"currency_id": 1,
				"exec_fee": 0.24,
				"exec_id": "9c1626ff-5b18-3a39-b23d-25d7cdc0bb64",
				"exec_price": 1200.0,
				"exec_qty": 1.0,
				"exec_type": "6",
				"exec_value": 1200.0,
				"fee_rate": 0.0002,
				"order_id": "9b0aff86-06c0-370b-9ea8-e44058a2edc9",
				"side": "1",
				"symbol": "ETHUSDT",
				"symbol_id": 11,
				"update_time": "2023-01-16 01:24:17",
				"user_id": 500020
			}
		]
	},
	"cid": "test-123"
}
```

```
| Field                    | Type    | Description                                                         | Possible values                      |
|--------------------      |---------|---------------------------------------------------------------------|--------------------------------------|
| orders                   | List    | Orders information                                                  |                                      |
| account_id               | Integer | Account id                                                          |                                      |
| close_by                 | String  | Last operator                                                       |                                      |
| closed_pnl               | decimal | Closing profit and loss                                             |                                      |
| closed_size              | decimal | Closing size                                                        |                                      |
| create_time              | String  | Create time                                                         |                                      |
| currency                 | String  | trading account's settle currency. Use to identify trading account. | 1:USDT                               |
| currency_id              | Integer | Current id                                                          |                                      |
| exec_fee                 | decimal | Settlement expenses                                                 |                                      |
| exec_id                  | String  | Exec id                                                             |                                      |
| exec_price               | decimal | Transaction price                                                   |                                      |
| exec_qty                 | decimal | Number of transactions                                              |                                      |
| exec_type                | String  | Exec type                                                           |                                      |
| exec_value               | decimal | Exec value                                                          |                                      |
| fee_rate                 | decimal | Settle fee                                                          |                                      |
| orderid                  | String  | Order id                                                            |                                      |
| side                     | Integer | Side                                                                |                                      |
| symbol                   | String  | Symbol                                                              |                                      |
| symbol_id                | Integer | Symbol id                                                           |                                      |
| update_time              | String  | Update time                                                         |                                      |
| user_id                  | Integer | User id                                                             |                                      |

```

## Query kline

> Request format

```
POST /api
```
> Request sample

```json
{
   "serverName": "AdminSvr",
   "method": "search",
   "content": {
      "symbol": "BTCUSDT",
      "interval": "86400",
      "start_time": "1672790400",
      "end_time": "1672876800",
      "count": 2,
      "cid": "test"
   }
}

```
| Field       | Type    | Requried | Description                           | Possible Value  |
|-------------|---------|----------|---------------------------------------|-----------------|
| server_name | String  | Yes      |                                       | AdminSvr        |
| method      | String  | Yes      |                                       | search          |
| content     | Object  | Yes      |                                       |                 |
| symbol      | String  | Yes      |                                       |                 |
| interval    | String  | No       |                                       | 60，300，900，1800，3600，14400，86400，604800，2592000，7776000，31104000 |
| start_time  | String  | No       |                                       |                 |
| end_time    | String  | No       |                                       |                 |
| count       | Integer | No       | Kline total Amount you want to search |                 |
| cid         | String  | No       |                                       |                 |


> Response sample

```json
{
   "msg": "NO_ERROR",
   "code": 0,
   "data": {
      "kline_data": [
         {
            "close": "16823.2",
            "create_time": "2023-01-06 02:19:45",
            "high": "16871",
            "interval": "86400",
            "last_close": "16880",
            "low": "16800",
            "open": "16871",
            "symbol": "BTCUSDT",
            "timestamp": "1672876800",
            "turnover": "606.929776",
            "update_time": "2023-01-06 02:19:45",
            "volume": "0.036024"
         },
         {
            "close": "16823.2",
            "create_time": "2023-01-06 02:19:46",
            "high": "16871",
            "interval": "86400",
            "last_close": "16880",
            "low": "16800",
            "open": "16871",
            "symbol": "BTCUSDT",
            "timestamp": "1672876800",
            "turnover": "606.929776",
            "update_time": "2023-01-06 02:19:46",
            "volume": "0.036024"
         }
      ]
   },
   "cid": "test"
}
```
```
| Field                    | Type    | Description                                                         | Possible values                      |
|--------------------      |---------|---------------------------------------------------------------------|--------------------------------------|
| kline_data               | List    | Orders information                                                  |                                      |
| close                    | String  | Close price                                                         |                                      |
| create_time              | String  | Create time                                                         |                                      |
| high                     | String  | High price                                                          |                                |
| interval                 | String  | KLine type                                                          |                                      |
| last_close               | String  | Last close price                                                    |                                      |
| low                      | String  | Low price                                                           |                                      |
| open                     | String  | Open price                                                          |                                      |
| symbol                   | String  | Symbol                                                              |                                      |
| timestamp                | String  | Timestamp                                                           |                                      |
| turnover                 | String  | Turnover                                                            |                                      |
| update_time              | String  | Update time                                                         |                                      |
| volume                   | String  | Volume                                                              |                                      |

```

# Contract Websocket API

## Heartbeat

> Request

```json
{
  "id": 0,
  "method": "server.ping",
  "params": []
}
```

> Response

```json
{
  "error": null,
  "id": 0,
  "result": "pong"
}
```

* Client is required to proactively send heartbeat message to RubyDEX data gateway ('DataGW' in short) with interval less than 30 seconds, otherwise DataGW will discard the connection. Once a client sends a ping message, DataGW will reply with a pong message.
* Client can use WS built-in ping message or the application level ping message to DataGW as heartbeat. The heartbeat interval is recommended to be set as *5 seconds*, and reconnect to DataGW if don't receive any response message in *3 heartbeat intervals*.

## User authentication

> Request format

```javascript
{
  "method": "user.auth",
  "params": {
    "apikey": <apikey>,
    "expiry": "<expiry>",
    "signature": "<signature>"
  },
  "id": 0
}
```

> Request sample

```json
{
  "method": "user.auth",
  "params": {
    "apikey": "9606288e10c644fbbf3acd97d9aa8639",
    "expiry": 1673399997993,
    "signature": "756fa7d8f249defec6ff2ed06c2e8366d28248bf2abf255b54513c1ac75f8496"
  },
  "id": 0
}

```

Public channels like trade/orderbook/kline are published publicly without user authentication.
While for private channels like account/position/order data, the client should send user.auth message to Data Gateway to authenticate the session.

| Field       | Type   | Description      | Possible values |
|-------------|--------|------------------|-----------------|
| apikey      | String | API Key     |                 |
| signature   | String | Signature generated by a funtion as HMacSha256(API Key + expiry) with ***API Secret*** ||
| expiry      | Integer| A future time point to expire the give request. In epoch ***millisecond***. Maximum expiry is request time plus 2 minutes ||

## Subscribe orderbook

> Request format

```javascript
{
  "id": <id>,
  "method": "orderbook.subscribe",
  "params": {
    "symbol": "BTCUSDT"
  }
}
```

Subscribe orderbook update messages with **depth = 30 and interval = 20ms**.

On each successful subscription, DataGW will immediately send the current Order Book snapshot to client and all later order book updates will be published.

> Request sample：

```json
{
  "method": "orderbook.subscribe",
  "params": {
    "symbol": "BTCUSDT"
  },
  "id": 7
}
```

## OrderBook message

> Message format：

```javascript
{
  "data": {
    "asks": [
      [
        <price>,
        <qty>
      ],
      .
      .
      .
    ],
    "bids": [
      [
        <price>,
        <qty>
      ],
      .
      .
      .
    ],
    "keys": [
      "price",
      "size"
    ],
    "depth": <depth>,
    "symbol": "<symbol>",
    "sequence": <sequence>,
    "timestamp": <timestamp>,
    "type": "<type>"
  },
  "method": "book.update"
}
```

DataGW publishes order book message with types: incremental, snapshot. Snapshot messages are published with 60-second interval for client self-verification.

| Field       | Type   | Description      | Possible values |
|-------------|--------|------------------|-----------------|
| price       | String | price            |                 |
| qty         | String | level size. Non-zero qty indicates price level insertion or updation, and qty 0 indicates price level deletion. |                 |
| sequence    | Integer| Latest message sequence |          |
| depth       | Integer| Market depth     |                 |
| type        | String | Message type     | snapshot, incremental |


> Message sample: snapshot

```json
{
  "data": {
    "asks": [
      [
        "23190.5",
        "0.0196"
      ]
    ],
    "bids": [
      [
        "23178.9",
        "0.0065"
      ]
    ],
    "depth": 30,
    "keys": [
      "price",
      "size"
    ],
    "symbol": "BTCUSDT",
    "timestamp": 1677425760005974500,
    "type": "snapshot"
  },
  "method": "book.update"
}
```

> Message sample: incremental update

```json
{
  "data": {
    "asks": [
      [
        "23192.8",
        "0"
      ],
      [
        "23213.2",
        "0.0213"
      ]
    ],
    "bids": [
      [
        "23201.9",
        "0.0266"
      ]
    ],
    "depth": 30,
    "symbol": "BTCUSDT",
    "timestamp": 1677425824576510500,
    "type": "incremental"
  },
  "method": "book.update"
}
```

## Unsubscribe orderbook

> Request sample

```json
{
  "id": 0,
  "method": "orderbook.unsubscribe",
  "params": {}
}
```

It unsubscribes all orderbook subscriptions.

## Subscribe trade

> Request format

```javascript
{
  "method": "trade.subscribe",
  "params": {
    "symbol": "<symbol>"
  },
  "id": <id>
}
```

On each successful subscription, DataGW will send the some history trades immediately for the subscribed symbol and all later trades will be published.

> Request sample

```json
{
  "method": "trade.subscribe",
  "params": {
    "symbol": "BTCUSDT"
  },
  "id": 5
}
```

## Trade message

> Message format

```javascript
{
  "data": {
    "keys": [
      "timestamp",
      "side",
      "price",
      "size"
    ],
    "symbol": "<symbol>",
    "trades": [
      [
        <timestamp>,
        "<side>",
        "<price>",
        "<qty>"
      ]
    ],
    "type": "<type>"
  },
  "method": "trade.update"
}
}
```

DataGW publishes trade message with types: incremental, snapshot. Incremental messages are published with 20ms interval. And snapshot messages are published on connection initial setup for client recovery.


| Field       | Type   | Description      | Possible values |
|-------------|--------|------------------|-----------------|
| timestamp   | Integer| Timestamp in nanoseconds ||
| side        | String | Execution taker side| bid, ask        |
| price       | String | Execution price  |                 |
| qty         | String | Execution size   |                 |
| symbol      | String | Contract symbol name     ||
| type        | String | Message type     |snapshot, incremental |


> Message sample: snapshot

```json
{
  "data": {
    "keys": [
      "timestamp",
      "side",
      "price",
      "size"
    ],
    "symbol": "BTCUSDT",
    "trades": [
      [
        1677425760005974500,
        "Sell",
        "23178.9",
        "0.0001"
      ]
    ],
    "type": "snapshot"
  },
  "method": "trade.update"
}
```

> Message sample: snapshot

```json
{
  "sequence": 1188273,
  "symbol": "BTCUSD",
  "trades": [
    [
      1573717116484024300,
      "Buy",
      86730000,
      21
    ]
  ],
  "type": "incremental"
}
```

## Unsubscribe trade

> Request format: unsubscribe all trade subsciptions

```javascript
{
  "id": <id>,
  "method": "trade.unsubscribe",
  "params": {}
}
```

> Request format: unsubscribe all trade subsciptions for a symbol

```javascript
{
  "id": <id>,
  "method": "trade.unsubscribe",
  "params": {
    "symbol": "BTCUSDT"
  }
}
```

It unsubscribes all trade subscriptions or for a single symbol.

## Subscribe kline

> Request format

```javascript
{
  "method": "kline.subscribe",
  "params": {
    "symbol": "<symbol>",
    "interval": <interval>
  },
  "id": <id>
}
```

On each successful subscription, DataGW will send the some history klines immediately for the subscribed symbol and all the later kline update will be published in real-time.

> Request sample: subscribe 30-minutes kline

```json
{
  "method": "kline.subscribe",
  "params": {
    "symbol": "BTCUSDT",
    "interval": 1800
  },
  "id": 8
}
```

## Kline message

> Message format

```javascript
{
  "data": {
    "keys": [
      "timestamp",
      "interval",
      "last_close",
      "open",
      "high",
      "low",
      "close",
      "volume",
      "turnover"
    ],
    "kline": [
      [
        <timestamp>,
        <interval>,
        <lastClose>,
        <open>,
        <high>,
        <low>,
        <close>,
        <volume>,
        <turnover>,
      ],
      .
      .
      .
    ],
    "symbol": "<symbol>",
    "type": "<type>"
  },
	"method": "kline.update"
}
```

> Message sample: snapshot

```json
{
  "data": {
    "keys": [
      "timestamp",
      "interval",
      "last_close",
      "open",
      "high",
      "low",
      "close",
      "volume",
      "turnover"
    ],
    "kline": [
      [
        1677425400,
        1800,
        "23206.7",
        "23206",
        "23206",
        "23178.9",
        "23178.9",
        "0.0002",
        "4.63849"
      ],
      [
        1677423600,
        1800,
        "23205.9",
        "23193.1",
        "23229.9",
        "23178.2",
        "23206.7",
        "0.0005",
        "11.60305"
      ]
    ],
    "symbol": "BTCUSDT",
    "type": "snapshot"
  },
  "method": "kline.update"
}
```

> Message sample: snapshot

```json
{
  "data": {
    "kline": [
      [
        1677425400,
        1800,
        "23206.7",
        "23206",
        "23206",
        "23178.9",
        "23178.9",
        "0.0002",
        "4.63849"
      ]
    ],
    "symbol": "BTCUSDT",
    "type": "incremental"
  },
  "method": "kline.update"
}
```

DataGW publishes kline message with types: incremental, snapshot. Incremental messages are published with 20ms interval. A history message will be published on subscription for client recovery.

| Field       | Type   | Description      | Possible values |
|-------------|--------|------------------|-----------------|
| timestamp   | Integer| Timestamp in nanoseconds for each trade ||
| interval    | Integer| Kline interval type      | 60, 300, 900, 1800, 3600, 14400, 86400, 604800, 2592000, 7776000, 31104000 |
| lastClose   | String | last close price |                 |
| open        | String | open price       |                 |
| high        | String | high price       |                 |
| low         | String | low price        |                 |
| close       | String | close price      |                 |
| volume      | String | Trade voulme     |                 |
| turnover    | String | Turnover value   |                 |
| symbol      | String | Contract symbol name     ||
| type        | String | Message type     |snapshot, incremental |


## Unsubscribe kline

> Request format: unsubscribe all kline subscriptions

```javascript
{
  "id": <id>,
  "method": "kline.unsubscribe",
  "params": {}
}
```

> Request format: unsubscribe all kline subscriptions of a symbol

```javascript
{
  "id": <id>,
  "method": "kline.unsubscribe",
  "params": {
    "symbol": "<symbol>"
  }
}
```

It unsubscribes all kline subscriptions or for a single symbol.


## Subscribe account data

> Request format

```javascript
{
  "method": "perp_account.subscribe",
  "params": {
    "account": true,
    "order": true,
    "position": true,
    "all_positions": true
  },
  "id": <id>
}
```

Perpetual account data subscription requires the session been authorized successfully.

## Account message

> Message format

```javascript
{
  "data": {
    "keys": [
      "user_id",
      "currency",
      "account_balance",
      "used_balance",
      "locked_balance"
    ],
    "accounts": [
      <user_id>,
      "<currency>",
      "<account_balance>",
      "<used_balance>",
      "<locked_balance>"
    ],
    "type": "<type>"
  },
  "method": "account.update"
}
```

| Field       | Type   | Description      | Possible values |
|-------------|--------|------------------|-----------------|
| user_id     | Integer| user ID | |
| currency    | String | settle currency    |          |
| account_balance | String | account balance    |          |
| used_balance | String | account used balance    |          |
| locked_balance | String | account locked balance    |          |
| type        | String | Message type     | snapshot, incremental |



> Message sample: snapshot

```json
{
  "data": {
    "keys": [
      "user_id",
      "currency",
      "account_balance",
      "used_balance",
      "locked_balance"
    ],
    "accounts": [
        501821,
        "USDT",
        "0",
        "0",
        "0"
    ],
    "type": "snapshot"
  },
  "method": "account.update"
}
```

> Message sample: incremental

```json
{
  "data": {
    "accounts": [
      [
        501821,
        "USDT",
        "0",
        "0",
        "0"
      ]
    ],
    "timestamp": 1677513985866265600,
    "type": "incremental"
  },
  "method": "account.update"
}
```

## Account order message

> Message sample: snapshot

```javascript
{
  "data": {
    "closed": [],
    "fill_keys": [
      "user_id",
      "symbol",
      "currency",
      "ord_type",
      "order_id",
      "side",
      "price",
      "order_qty",
      "exec_status",
      "exec_id",
      "exec_price",
      "exec_qty",
      "exec_value",
      "exec_fee",
      "closed_size",
      "closed_pnl",
      "trade_type",
      "exec_seq",
      "transact_time"
    ],
    "fills": [],
    "open": [],
    "order_keys": [
      "user_id",
      "action",
      "symbol",
      "currency",
      "ord_type",
      "orig_ord_type",
      "time_in_force",
      "clord_id",
      "order_id",
      "side",
      "price",
      "order_qty",
      "leaves_qty",
      "cum_qty",
      "cum_value",
      "ord_status",
      "exec_status",
      "exec_inst",
      "action_time",
      "transact_time",
      "code",
      "cxl_rej_reason",
      "trigger",
      "stop_direction",
      "stop_price",
      "peg_price_type",
      "peg_price_offset",
      "tp_trigger",
      "take_profit_price",
      "sl_trigger",
      "stop_loss_price"
    ],
    "type": "snapshot"
  },
  "method": "order.update"
}
```

> Message sample: incremental

```json
{
  "data": {
    "closed": [
      [
        501821,
        "New",
        "BTCUSDT",
        "USDT",
        "Limit",
        "",
        "GoodTillCancel",
        "7bc23af2-3bb4-475f-96c6-7dd864b2ebd0",
        "fdcb2370-d54f-4da2-a16d-ed31e2708c7f",
        "Buy",
        "1",
        "0.0001",
        "0",
        "0",
        "0",
        "Rejected",
        "CreateRejected",
        "",
        1677513983032305700,
        1677513983032305700,
        2055,
        0,
        "",
        "",
        "0",
        "",
        "0",
        "",
        "0",
        "",
        "0"
      ]
    ],
    "fills": [],
    "open": [],
    "timestamp": 1677513983033673000,
    "type": "incremental"
  },
  "method": "order.update"
}
```

## Account position message

> Message sample: snapshot

```javascript
{
  "data": {
    "keys": [
      "user_id",
      "symbol",
      "currency",
      "side",
      "position_status",
      "leverage",
      "risk_limit",
      "size",
      "init_margin_rate",
      "maint_margin_rate",
      "avg_entry_price",
      "pos_cost",
      "pos_balance",
      "pos_margin",
      "bankrupt_price",
      "liq_price",
      "order_cost",
      "buy_value_cost_rate",
      "sell_value_cost_rate",
      "transact_time",
      "created_time",
      "updated_time",
      "mark_price",
      "unrealised_pnl",
      "realised_pnl"
    ],
    "positions": [],
    "type": "snapshot"
  },
  "method": "position.update"
}
```

> Message sample: incremental

```json
{
  "data": {
    "positions": [
      [
        501821,
        "BTCUSDT",
        "USDT",
        "None",
        "Normal",
        "0",
        "500000",
        "0",
        "0.01",
        "0.005",
        "0",
        "0",
        "0",
        "0",
        "0",
        "0",
        "0",
        "0.011194",
        "0.011206",
        "2023-02-27T16:06:25.865317134Z",
        "1970-01-01T00:00:00.000000000Z",
        "1970-01-01T00:00:00.000000000Z",
        "23577.821801",
        "0",
        "0"
      ]
    ],
    "timestamp": 1677513985866265600,
    "type": "incremental"
  },
  "method": "position.update"
}
```

## Unsubscribe account

> Request format

```javascript
{
  "method": "perp_account.unsubscribe",
  "params": {},
  "id": <id>
}
```

It unsubscribes account order updates.

## Subscribe 24 hours ticker
> Reuqest sample

```json
{
  "method": "ticker.subscribe",
  "params": {
    "all": true
  },
  "id": 1
}
```

## 24-Hours ticker message

> Message format

```javascript
{
  "data": {
    "keys": [
      "symbol",
      "last_price",
      "bid_price",
      "ask_price",
      "open_price",
      "high_price",
      "low_price",
      "volume",
      "turnover",
      "open_interest",
      "index_price",
      "mark_price",
      "predict_funding_rate",
      "funding_rate"
    ],
    "tickers": [
      [
        "<symbol>",
        "<last_price>",
        "<bid_price>",
        "<ask_price>",
        "<open_price>",
        "<high_price>",
        "<low_price>",
        "<volume>",
        "<turnover>",
        "<open_interest>",
        "<index_price>",
        "<mark_price>",
        "<predict_funding_rate>",
        "<funding_rate>"
      ]
    ],
    "timestamp": <timestamp>
  },
  "method": "ticker.update"
}
```

> Message sample

```json
{
  "data": {
    "keys": [
      "symbol",
      "last_price",
      "bid_price",
      "ask_price",
      "open_price",
      "high_price",
      "low_price",
      "volume",
      "turnover",
      "open_interest",
      "index_price",
      "mark_price",
      "predict_funding_rate",
      "funding_rate"
    ],
    "tickers": [
      [
        "ADAUSDT",
        "0.365",
        "0.3608",
        "0.3654",
        "0.3672",
        "0.3685",
        "0.35",
        "286",
        "103.422",
        "0",
        "0.361922",
        "0.362506",
        "0.00407199",
        "0.00457199"
      ]
    ],
    "timestamp": 1677425772605386500
  },
  "method": "ticker.update"
}
```

On each successful subscription, DataGW will publish 24-hour ticker updates for all symbols every 1 second.

| Field         | Type   | Description                                | Possible values |
|---------------|--------|--------------------------------------------|--------------|
| open price    | String | The open price in last 24 hours     |              |
| high price    | String | The highest price in last 24 hours  |              |
| low price     | String | The lowest price in last 24 hours   |              |
| close price   | String | The close price in last 24 hours    |              |
| index price   | String | Index price                         |              |
| mark price    | String | Mark price                          |              |
| open interest | String | Current open interest                      |              |
| funding rate  | String | Funding rate                        |              |
| predicated funding rate| String | Predicated funding rate  |              |
| timestamp     | Integer| Timestamp in nanoseconds                   |              |
| symbol        | String | Contract symbol name                       |              |
| turnover      | String | Turnover value in last 24 hours |              |
| volume        | String | Trade volume in last 24 hours       |              |

