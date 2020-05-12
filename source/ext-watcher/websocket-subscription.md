# Trade-server Websocket Subscription

## Link

Link your Websocket client to `ws://localhost:8000/ws`

## All Command

After successful link, subscribe or unsubscribe to related topic by sending command data in the following format.


Basic Format
`{"op":"<command>", "args":["args1", "args2", "args3", ...], "depth": <10> }`

*	Subscription
	*  subscribe
	*  unsubscribe
* Heartbeat
	* Ping
	
	
## Subscription

Trade-server is applied for real-time data subscription and it's the best choice. Once successfully linked, you can get the latest updates of your subscribed topics. 

To subscribe to the topic, you can follow the method below.

*	Once linked, send the given information to trade-server. The same goes for subscribing to a new topic. 
	* `{"op":"subscribe", "args":[<subscriptionTopic>, ...]}`

By sending an array of subscription topics, you can subscribe to multiple topics at a time. 

Currently, no authentication is required for topic subscription.

When a subscribed topic needs to carry parameters, you can use a colon `:` to separate the topic name and its parameters;

*	e.g.`kline:etc/cet:1min`; The meaning of this topic: Subscribe to one-minute K-line data of etc/cet.

> [golang client example](https://github.com/coinexchain/trade-server/blob/master/examples/websocket_examples.go)

> [Response data example](https://github.com/coinexchain/trade-server/blob/master/docs/websocket-data-examples.md)

### 	

## Heartbeat

Send "ping" on the client, and the server will respond with "pong".

Ping Format：`{"op":"ping"}`
Pong Response: `{"type":"pong"}`

If you are concerned that your connection will be silently terminated, we recommend that you use the following process:

*	After receiving each message, set a 5-second timer.
*	If you have received new message when the timer is triggered, reset the timer.
*	If the timer is triggered, which means no new message has been received within 5 seconds, send a ping packet.


## Unsubscription

If you want to unsubscribe to a topic after the user program has been running for a period of time, you can use the `unsubscribe` command.

e.g. `{"op":"unsubscribe", "args":[<subscriptionTopic>, ...]}`





## Response Format

The response of Websocket may include the following three types:

`Success`(When successfully subscribed to a topic)
`{"subscribe": subscriptionName, "success": true}`

`Error`(When a format error has occured)
`{"error": errorMessage}`

`Data`(When the subscribed data is pushed): 

```json
{
	"type": "blockinfo",   		// data response type
	"payload":{
        ...
        // data response
        ...	
	}
}
```

All data pushes have a `type` attribute, which is used to identify different type in order to respond to them.

## Topic List

### Block Confirmation 

When a block is confirmed on the chain every time, related data includes the height, time and hash of this block will be provided.

**SubscriptionTopic** : `blockinfo`

**Response** : 

```json

{
	"type": "blockinfo",
	"payload": {	
            "chain_id": "coinexdex2",
            "height": 162537, 			// height
            "time": 673571293,			// unix time second
            "last_block_hash": "000000000000000000ac6c4c9a6c2e406ac32b53af5910039be27f669d767356" // block hash
	}
}
```

### 

**payload:**

| Field           | json Type | Description             |
| --------------- | --------- | ----------------------- |
| chain_id        | String    | Current chain ID        |
| height          | Number    | Current block height    |
| timestamp       | Number    | Current block timestamp |
| last_block_hash | String    | Last block hash         |



### Confirmed Transactions

Get transactions that require user's signature in each block. 

**SubscriptionTopic** : `txs:<address>`

**Response** : 

```json

{
	"type": "txs",
	"payload": {
            "transfers": [
                {
                    "sender": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
                    "recipient": "coinex17qtadt7356l0sf0hq5fjycnflq9lnx9c6cx5k7",
                    "amount": "8467.863"		// amount
                },
                {
                    "sender": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
                    "recipient": "coinex17qtadt7356l0sf0hq5fjycnflq9lnx9c6cx5k7",
                    "amount": "8467.863"		// amount
                }
            ],
            "signers": [
                "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
                "coinex17qtadt7356l0sf0hq5fjycnflq9lnx9c6cx5k7"
            ],							// signers 
            "serial_number": 100, 				// tx global serial number
            "msg_types": [
                "asset/MsgIssueToken"
            ],							// tx messages type
            "tx_json": "...",                                   // raw tx json byte
            "height": 16728				        // block height
	} 
}

```

**payload** : 

| Field         | Type                   | Description                                           |
| ------------- | ---------------------- | ----------------------------------------------------- |
| signers       | String                 | Signers                                               |
| transfers     | Object(TransferRecord) | See the chart below                                   |
| serial_number | Number                 | Current Tx serial number                              |
| msg_types     | String                 | Tx message types                                      |
| tx_json       | String                 | Tx serialized data (json)                             |
| height        | Number                 | Current Tx height                                     |
| hash          | String                 | Tx hash                                               |
| extra_info    | String                 | Extra information provided when the transaction fails |

**TransferRecord**

| Field     | Type   | Description  |
| --------- | ------ | ------------ |
| sender    | String | Tx sender    |
| recipient | String | Tx recipient |
| amount    | String | Amount       |

### Validator's Slash

Get slash in the block.

**SubscriptionTopic** : `slash`

**Response** : 

```json

{
	"type": "slash",
	"payload": {
            "validator": "coinex17qtadt7356l0sf0hq5fjycnflq9lnx9c6cx5k7",	// validator address
            "power":	"67.2",				                        // vote power
            "reason": "Double Sign", 		                                // reason
            "jailed": true					                // jailed
	}
}	
```

**payload** : 

| Field     | Type    | Description              |
| --------- | ------- | ------------------------ |
| validator | String  | Validator address        |
| power     | String  | Validator's voting power |
| reason    | String  | Reason for slash         |
| jailed    | Boolean | Jailed?                  |



### Ticker of Trading Pairs

Get ticker of the trading pairs.

**SubscriptionTopic** : 

*   market : `ticker:<trading-pair>`;
*   bancor : `ticker:B:<trading-pair>`

**Response** : 

```json
{
	"type": "ticker",
	"payload": {
            "tickers": [
                {
                    "market": "bch/cet",			// market
                    "new": "0.986",		                // new price
                    "old": "1.12"                               // old price
                }
            ]       
	}
}
```

**payload:** 

| Field         | Type   | Description                             |
| ------------- | ------ | --------------------------------------- |
| market        | String | Trading pair                            |
| new           | String | The latest price for the current minute |
| old           | String | Price one day before the current minute |
| minute_in_day | Number | Ticker in the xx minute of a day        |

### 

### K-line of a Certain Precision for Trading Pairs 

Get K-line of the specific precision for trading pairs. 

K-line Precision : Currently, 1min, 1hour and 1day are supported.

**SubscriptionTopic** : 

*   market : `kline:<trading-pair>:<1min>`
*   bancor : `kline:B:<trading-pair>:<1min>`

**Response**:

```json

{
	"type": "kline",
	"payload": {
            "open": "0.989",				// open price
            "close": "0.97", 				// close price
            "high": "1.29",			        // high price
            "low": "0.93", 			        // low price
            "total": "8652",				// total deal stock
            "unix_time":	"786367672",		// ending unix time
            "time_span": 16,				// Kline internal
            "market": "etc/cet"                         // trading-pai
	}
}	 	
```

**payload** : 

| Field     | Type   | Description                   |
| --------- | ------ | ----------------------------- |
| open      | String | Opening                       |
| close     | String | Closing                       |
| high      | String | High                          |
| low       | String | Low                           |
| total     | String | Total executions              |
| unix_time | Number | Timestamp of pushing K-line   |
| time_span | String | Time range: 1min、1hour、1day |
| market    | String | Trading pair                  |

### 

### Depth of Trading Pairs

Get depth of trading pairs.

**SubscriptionTopic** : `depth:<trading-pair>:<level>`

**Response**:

```json
{
    "type": "depth_change", // depth_delta, depth_full
    "payload": {
        "trading-pair": "ethsw/cet",
        "bids": [
            {
                "price": "0.936",		// price
                "amount": "100000"		// amount
            }
        ],
        "asks": [
            {
                "price": "0.936",		// price
                "amount": "100000"		// amount
            }
        ]
    }
}
```

**Supported Level ** : "0.00000001", "0.0000001", "0.000001", "0.00001", "0.0001", "0.001", "0.01", "0.1", "1", "10", "100", "all"

**There are three types of depth response** : depth_delta, depth_change, depth_full；The details are as follows:

*   depth_change : Use this response to replace the same depth data on the client.
*   depth_full : Use this response to replace all the depth data on the client.
*   depth_delta : Indicates incremental depth merging, the client needs to calculate the depth of the specified price based on the response.

depth_change: * depth_full: * depth_delta: **payload** : Refer to `../swagger/swagger.yaml  /market/depths request response`



### Execution of Trading Pairs

Subscribe to specific trading pairs with execution data.

**SubscriptionTopic** : `deal:<trading-pair>`

**Response**:

```json
{
    "type": "deal",
    "payload": {
        "order_id": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x-9",		// order id
        "trading_pair":	"eth/cet",		      // trading-pair
        "height": 2773,				      // block height
        "side":	1, 			              // order side; BUY / SELL
        "price": "0.73", 			      // order price
        "freeze": 836382,			      // freeze sato.CET amount
        "left_stock": 7753, 		              // order left stock
        "deal_stock": 773,			      // order deal stock
        "deal_money": 726,			      // order deal money
        "curr_stock": 8262,		              // curr stock
        "curr_money": 7753,			      // curr money 
        "fill_price": "0.233"              // the order deal price
    }
}

```

**payload** : 

| Field        | Type   | Description                            |
| ------------ | ------ | -------------------------------------- |
| order_id     | String | Order ID                               |
| trading_pair | String | Trading pair                           |
| height       | Number | Executed height                        |
| side         | Number | Side; BUY:1, SELL:2                    |
| price        | String | Price                                  |
| left_stock   | Number | Unexecuted stock quantity              |
| freeze       | Number | Remaining unexecuted frozen assets     |
| deal_stock   | Number | Executed stock quantity                |
| deal_money   | Number | Executed amount                        |
| curr_stock   | Number | Executed stock quantity for this order |
| curr_money   | Number | Executed amount for this order         |
| fill_price   | String | Executed price for this order          |

### 


### Orders of Trading Pairs

Get orders of the specific user.

There are three main types: Create order, Fill order and Cancel order.

**SubscriptionTopic** : `order:<address>`

**Response**:

```json
// create order info
{
	"type": "create_order",
	"payload": {
            "order_id": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x-9",	    // order id
            "sender": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x", 		    // order sender
            "trading_pair":	"eth/cet",           // trading-pair
            "order_type": 2,                         // order type; Limit Order
            "price": "0.73", 			     // order price
            "quantity": 8763892, 	             // order quantity
            "side":	1, 			     // order side; BUY / SELL
            "time_in_force": 3,                      // GTC / IOC
            "feature_fee": 21562,	             // order feature fee; sato.CET as the unit
            "height": 2773,		             // block height
            "frozen_fee": 782553,	             // order frozen fee; sato.CET as the unit
            "freeze": 836382			     // freeze sato.CET amount
	}
}

// **payload** : Refer to `../swagger/swagger.yaml  CreateOrderInfo type definition`

// fill order info
{
	"type": "fill_order",
	"payload": {
            "order_id": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x-9",		// order id
            "trading_pair":	"eth/cet",		// trading-pair
            "height": 2773,				// block height
            "side":	1, 				// order side; BUY / SELL
            "price": "0.73", 			        // order price
            "freeze": 836382,			        // freeze sato.CET amount
            "fill_price": "0.233",              // the order deal price
            "left_stock": 7753, 		        // order left stock
            "deal_stock": 773,			        // order deal stock
            "deal_money": 726,			        // order deal money
            "curr_stock": 8262,		                // curr stock
            "curr_money": 7753			        // curr money 
	}
}

// **payload** : Refer to `../swagger/swagger.yaml  FillOrderInfo type definition`

// cancel order info 
{
	"type": "cancel_order",
	"payload": {
            "order_id": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x-9",		// order id
            "trading_pair":	"eth/cet",			// trading-pair
            "height": 2773,				        // block height
            "side":	1, 					// order side; BUY / SELL
            "price": "0.73",                                    // order price
            "del_reason": "munual delete order", 
            "used_commission": 37253, 	                        // order used commission
            "left_stock": 7753, 				// order left stock
            "remain_amount": 7762,			        // order remain amount
            "deal_stock": 773,			                // order deal stock
            "deal_money": 722		                        // order deal money
	}
}

// **payload** : Refer to `../swagger/swagger.yaml  CancelOrderInfo type definition`

```

**Payload:**

**create_order**

| Field              | Type   | Description                                                  |
| ------------------ | ------ | ------------------------------------------------------------ |
| order_id           | String | Order ID                                                     |
| sender             | String | Creator's address                                            |
| trading_pair       | String | Trading pair                                                 |
| order_type         | Number | 2:Limit order; Currently, Limit order available only         |
| price              | String | Price                                                        |
| quantity           | Number | Stock quantity                                               |
| side               | Number | Side; BUY:1, SELL:2                                          |
| time_in_force      | Number | Order type; GTE: Good Till Expire; IOC: Immediate or Cancel  |
| height             | Number | Block height when creating order                             |
| freeze             | Number | Used funds                                                   |
| frozen_feature_fee | Number | Freeze the max. service fee of order; Return it in terms of storage time when cancelling order |
| frozen_commission  | Number | Freeze the max. commission of order; Return it according to the deal status when cancelling order |
| tx_hash            | String | Tx hash                                                      |

**fill_order**

Same as the above **execution of trading pairs.**

**cancel_order**

| Field               | Type   | Description                        |
| ------------------- | ------ | ---------------------------------- |
| order_id            | String | OrderID                            |
| trading_pair        | String | Trading pair                       |
| height              | Number | Block height when cancelling order |
| side                | Number | Side; BUY:1, SELL:2                |
| price               | String | Price                              |
| del_reason          | String | Reason for cancellation            |
| used_commission     | Number | Actual used commission             |
| used_feature_fee    | Number | Actual used service fee            |
| rebate_amount       | Number | Commission amount                  |
| rebate_referee_addr | String | Commission address                 |
| left_stock          | Number | Unexcuted stock quantity           |
| remain_amount       | Number | Remaining unexecuted amount        |
| deal_stock          | Number | Executed stock quantity            |
| deal_money          | Number | Executed amount                    |

### 

### Comments 

Get comments of specific token on the forum.

**SubscriptionTopic** : `comment:<symbol>`

**Response**:

```json
{
	"type": "comment",
	"payload": {
            "id": 2,
            "height": 2773,				// block height
            "sender": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x", 		// comment sender
            "token": "eth",				// token symbol
            "donation": 112,			        // donation amount
            "title": "The bese value token", 	        // comment title
            "content": "Its price will grow",           // content
            "content_type": 2, 			        // content type
            "references": [
                {
                    "id": 3,
                    "reward_target": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
                    "reward_token": "set",	
                    "reward_amount": 726712,
                    "attitudes": [
                        1, 
                        2,
                        3
                    ]		
                }
            ]
	}
}

```

**payload** :

| Field        | Type                 | Description          |
| ------------ | -------------------- | -------------------- |
| id           | Number               |                      |
| height       | Number               | Comment block height |
| sender       | String               | Comment sender       |
| token        | String               | Token                |
| donation     | Number               | Donated amount       |
| title        | String               | Title                |
| content      | String               | Content              |
| content_type | Number               | Content type         |
| references   | []Object(CommentRef) |                      |
| tx_hash      | String               | Tx hash              |

**CommentRef**

| Field         | Type     | Description |
| ------------- | -------- | ----------- |
| id            | Number   |             |
| reward_target | String   |             |
| reward_token  | String   |             |
| reward_amount | Number   |             |
| attitudes     | []Number |             |

**meaning of content_type**

Content Type | value |
-----------|------------|
RawBytes | 6
UTF8Text | 3
HTTP | 2 
Magnet | 1
IPFS | 0


### Bancor Contract Information

Get information of specific trading pairs in the bancor contract.

**SubscriptionTopic** : `bancor:<trading-pair>`

**Response**:

```json
// bancor info

{
	"type": "bancor",
	"payload": {
            "sender": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
            "stock": "set",
            "money": "cet",
            "init_price": "67351", 				// bancor init price
            "max_supply": "27367129", 			        // bancor max supply 
            "max_price":	"3.212", 			// bancor max price
            "price": "1.12", 	                                // bancor curr price
            "stock_in_pool": "322",			        // stock in pool
            "money_in_pool": "21636", 		                // money in pool
            "enable_cancel_time": 76637128,			// bancor enable cancel time 
            "block_height": 7632 			        // block height
	} 
}

```

**payload** : 

| Field                | Type   | Description                   |
| -------------------- | ------ | ----------------------------- |
| owner                | String | Bancor owner                  |
| stock                | String | Stock quantity                |
| money                | String | Amount                        |
| init_price           | String | Initial price of trading pair |
| max_supply           | String | Max. supply                   |
| max_price            | String | Max. price                    |
| current_price        | String | Current price                 |
| stock_in_pool        | String | Stock quantity                |
| money_in_pool        | String | Deposit amount                |
| earliest_cancel_time | Number | Earliest cancel time          |
| stock_precision      | String | Stock Precision               |
| max_money            | String | Max. deposit amount           |
| ar                   | String | Supply-price index            |

### 

### Execution of Bancor Contract Address

Get execution information of specific address in the bancor contract.

**SubscriptionTopic** : `bancor-trade:<address>`

**Response**:

```json
// bancor trade
{
	"type": "bancor-trade",
	"payload": {
            "sender": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
            "stock": "set",
            "money": "cet",
            "amount": 27372,
            "side": 1,
            "monet_limit": 231,
            "transaction_price": "2123.2", 
            "block_height": 3631
	}
}
```

**payload** : 

| Field               | Type   | Description                                                  |
| ------------------- | ------ | ------------------------------------------------------------ |
| sender              | String | Order creator                                                |
| stock               | String | stock quantity                                               |
| money               | String | amount                                                       |
| amount              | Number | Order quantity                                               |
| side                | Number | Side; BUY:1, SELL:2                                          |
| money_limit         | Number | Bid price of buying order, ask price of selling order        |
| transaction_price   | String | Executed average price for this order, unit: price per stock |
| block_height        | Number | Block height                                                 |
| tx_hash             | String | Tx hash                                                      |
| used_commission     | Number | Actual used commission                                       |
| rebate_amount       | Number | Commission amount                                            |
| rebate_referee_addr | String | Commission address                                           |

### Execution of Bancor Contract

**SubscriptionTopic** : `bancor-deal:<trading-pair>`

**Response**:

```json
// bancor deal
{
	"type": "bancor-trade",
	"payload": {
            "sender": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
            "stock": "set",
            "money": "cet",
            "amount": 27372,
            "side": 1,
            "monet_limit": 231,
            "transaction_price": "2123.2", 
            "block_height": 3631
	}
}
```

**payload** : See above **Bancor Contract Information**.

### Amount Change of Account

Get amount change of specific users. 

**SubscriptionTopic** : `income:<address>`

**Response**:

```json
{
    "type": "income",
        "payload": {
            "transfers": [
                {
                    "sender": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
                    "recipient": "coinex17qtadt7356l0sf0hq5fjycnflq9lnx9c6cx5k7",
                    "amount": "8467.863"		// amount
                },
                {
                    "sender": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
                    "recipient": "coinex17qtadt7356l0sf0hq5fjycnflq9lnx9c6cx5k7",
                    "amount": "8467.863"		// amount
                }
            ],
            "signers": [
                "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
                "coinex17qtadt7356l0sf0hq5fjycnflq9lnx9c6cx5k7"
            ],						        // signers 
            "serial_number": 100, 				// tx global serial number
            "msg_types": [
                "asset/MsgIssueToken"
            ],					                // tx messages type
            "tx_json": "...",                                   // raw tx json byte
            "height": 16728                                     // block height
        }
}
```

**payload** : Same as above **Confirmed Transactions**.

### Redelegation

Get redelegation information of users.

**SubscriptionTopic** : `redelegation:<address>`

**Response**:

```json
{
	"type": "redelegation",
	"payload": {        
            "delegator": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
            "src": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
            "dst": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
            "amount": "32313",
            "completion_time": "132432"
	}
}
```

**payload** : 

| Field           | Type   | Description                |
| --------------- | ------ | -------------------------- |
| delegator       | String | Delegator address          |
| src             | String | Previous validator address |
| dst             | String | Current validator address  |
| amount          | String | Amount                     |
| completion_time | Number | Completion time            |
| tx_hash         | String | Tx hash                    |

### 


### Unbonding 

Get unbinding information of users.

**SubscriptionTopic** : `unbonding:<address>`

**Response**:

```json
{
	"type": "unbonding",
	"payload": {       
            "delegator": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
            "validator": "coinex1ughhs0eyames355v4tzq5nx2g806p55r323x",
            "amount": "9023",
            "completion_time": "1234323"
	}
}
```

**payload** :

| Field           | Type   | Description       |
| --------------- | ------ | ----------------- |
| delegator       | String | Delegator address |
| validator       | String | Validator address |
| amount          | String | Amount            |
| completion_time | Number | Completion time   |
| tx_hash         | String | Tx hash           |

### 


### Unlock

Get the delayed transfer information of users.

**SubscriptionTopic**: `unlock:<address>`

**Response**:

```json
{
	"type": "unlock",
	"payload": {
            "address": "coinex1ughhs0eyames355v4tzq5nx2g806p55rna0d2x",
            "unlocked": [
                {
                    "amount": "2323",
                    "denom": "cet"
                },
                {
                    "amount": "3242",
                    "denom": "eth"
                }
            ],
            "locked_coins": [
                {
                    "coin": {
                        "amount": "2323",
                        "denom": "cet"
                    },
                    "unlock_time": 212323
                },
                {
                    "coin": {
                        "amount": "2323",
                        "denom": "etv"
                    },
                    "unlock_time": 223312323
                }
            ],
            "frozen_coins": [
                {
                    "amount": "2323",
                    "denom": "cet"
                },
                {
                    "amount": "3242",
                    "denom": "eth"
                }
            ],
            "coins": [
                {
                    "amount": "2323",
                    "denom": "cet"
                },
                {
                    "amount": "3242",
                    "denom": "eth"
                }
            ],
            "height": 100003
	}
}
```

**payload** : 

| Field        | Type                 | Description               |
| ------------ | -------------------- | ------------------------- |
| address      | String               | Address                   |
| unlocked     | []Object(sdk.Coin)   | Unlocked amount           |
| locked_coins | []Object(LockedCoin) | Remaining locked amount   |
| frozen_coins | []Object(sdk.Coin)   | Current frozen amount     |
| coins        | []Object(sdk.Coin)   | Available account balance |
| height       | Number               | Current block height      |

**LockedCoin**

| Field        | Type             | Description                  |
| ------------ | ---------------- | ---------------------------- |
| coin         | Object(sdk.Coin) | Amount                       |
| unlock_time  | Number           | Unlock time                  |
| from_address | String           | Unlock from                  |
| supervisor   | String           | Supervisor address           |
| reward       | Number           | Reward amount for supervisor |

**sdk.Coin**

| Field  | Type   | Description    |
| ------ | ------ | -------------- |
| denom  | String | Token name     |
| amount | String | Token quantity |

























