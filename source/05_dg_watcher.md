# Execution Logs Dumped For Watchers

A witness node can dump execution logs to hard disk. A "watcher program" can analyze these logs to trace the status updates of CoinEx DEX Chain. If you want to write your own "watcher program" to fulfill some special needs, you must understand what data can be dumped in execution logs. And here is the document specifying data format of execution logs.

### Markets and Orders

#### create_market_info

| Field          | Type   | Description                              |
| -------------- | ------ | ---------------------------------------- |
| Stock          | string | the stock token                          |
| Money          | string | the money token                          |
| Creator        | string | who is creating this market              |
| PricePrecision | byte   | maximum price precision for orders       |
| OrderPrecision | byte   | Controlls the order amount's granularity |

#### create_order_info

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

#### fill_order_info

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

#### del_order_info

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





### Bancor

#### bancor_info and bancor_create

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



#### bancor_trade

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



### Block and Transactions

#### notify_tx

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



#### height_info

| Field           | json Type | Description             |
| --------------- | --------- | ----------------------- |
| chain_id        | String    | Current chain ID        |
| height          | Number    | Current block height    |
| timestamp       | Number    | Current block timestamp |
| last_block_hash | String    | Last block hash         |



### Staking 

#### begin_redelegation

| Field           | Type   | Description                |
| --------------- | ------ | -------------------------- |
| delegator       | String | Delegator address          |
| src             | String | Previous validator address |
| dst             | String | Current validator address  |
| amount          | String | Amount                     |
| completion_time | Number | Completion time            |
| tx_hash         | String | Tx hash                    |

 	

#### begin_unbonding

| Field           | Type   | Description       |
| --------------- | ------ | ----------------- |
| delegator       | String | Delegator address |
| validator       | String | Validator address |
| amount          | String | Amount            |
| completion_time | Number | Completion time   |
| tx_hash         | String | Tx hash           |

 

#### complete_redelegation

| Field        | Type   | Description           |
| ------------ | ------ | --------------------- |
| Delegator    | string | the delegator         |
| ValidatorSrc | string | the orignal validator |
| ValidatorDst | string | the new validator     |

#### complete_unbonding

| Field     | Type   | Description |
| --------- | ------ | ----------- |
| Delegator | string |             |
| Validator | string |             |



#### slash

A validator may be slashed because of poor availability or double-signing.

| Field     | Type    | Description              |
| --------- | ------- | ------------------------ |
| validator | String  | Validator address        |
| power     | String  | Validator's voting power |
| reason    | String  | Reason for slash         |
| jailed    | Boolean | Jailed?                  |

#### validator_commission

| Field      | Type   | Description          |
| ---------- | ------ | -------------------- |
| Validator  | string |                      |
| Commission | string | amount of commission |

#### delegator_rewards

| Field     | Type   | Description       |
| --------- | ------ | ----------------- |
| Validator | string |                   |
| Rewards   | string | amount of rewards |



### Others

#### send_lock_coins

| Field       | Type      | Description                       |
| ----------- | --------- | --------------------------------- |
| FromAddress | string    | sender                            |
| ToAddress   | string    | receiver                          |
| Amount      | sdk.Coins | coins to be sent                  |
| UnlockTime  | int64     | a future time to unlock the coins |
| Supervisor  | string    | supervisor who can unlock earlier |
| Reward      | int64     | rewards for supervisor            |



#### notify_unlock

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



#### token_comment

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

| Field         | Type     | Description                        |
| ------------- | -------- | ---------------------------------- |
| id            | Number   | id of the comment to be followed   |
| reward_target | String   | which address to reward            |
| reward_token  | String   | name of the token used for reward  |
| reward_amount | Number   |                                    |
| attitudes     | []Number | attitudes in following the comment |

**meaning of content_type**

| Content Type | value |
| ------------ | ----- |
| RawBytes     | 6     |
| UTF8Text     | 3     |
| HTTP         | 2     |
| Magnet       | 1     |
| IPFS         | 0     |


### 