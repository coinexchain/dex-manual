# websocket 订阅的信息

## 订阅income信息
`income:<address>`

订阅示例：`income:coinex1q7nm8r8v2wzhrq8wxel9qpc832vnhmf5kvxuxs`

应答:
```
{"type":"income", "payload":{"signers":["coinex1q7nm8r8v2wzhrq8wxel9qpc832vnhmf5kvxuxs"],"transfers":[{"sender":"coinex1q7nm8r8v2wzhrq8wxel9qpc832vnhmf5kvxuxs","recipient":"coinex16ur229a4xkj9e0xu06nqge9c23y70g7sl5vj98","amount":"600000000cet"}],"serial_number":24638,"msg_types":["MsgSend"],"tx_json":"{\"msg\":[{\"from_address\":\"coinex1q7nm8r8v2wzhrq8wxel9qpc832vnhmf5kvxuxs\",\"to_address\":\"coinex16ur229a4xkj9e0xu06nqge9c23y70g7sl5vj98\",\"amount\":[{\"denom\":\"cet\",\"amount\":\"600000000\"}],\"unlock_time\":0}],\"fee\":{\"amount\":[{\"denom\":\"cet\",\"amount\":\"200000000\"}],\"gas\":1000000},\"signatures\":[{\"pub_key\":[2,248,63,173,181,253,229,228,228,124,52,132,112,65,124,37,0,19,131,25,25,77,63,79,7,210,0,25,44,126,174,88,242],\"signature\":\"skkAap2ltmG2VNulgIuTLPnfuh0wxy8Id4iwzc/i+QFS56pCY28h9cIGxz9ajQxGupm6C4NWLq87wU4l4sQbEQ==\"}],\"memo\":\"send money from one account to another\"}","height":2367}}
```

## 订阅txs信息

`txs:<address>`

订阅示例： `txs:coinex16ur229a4xkj9e0xu06nqge9c23y70g7sl5vj98`

应答：

```
{"type":"txs", "payload":{"signers":["coinex16ur229a4xkj9e0xu06nqge9c23y70g7sl5vj98"],"transfers":[{"sender":"coinex16ur229a4xkj9e0xu06nqge9c23y70g7sl5vj98","recipient":"coinex1x6u3jacjqaqdfwvj4fs7adtumpsv2y6p6258jl","amount":"600000000cet"}],"serial_number":49984,"msg_types":["MsgSend"],"tx_json":"{\"msg\":[{\"from_address\":\"coinex16ur229a4xkj9e0xu06nqge9c23y70g7sl5vj98\",\"to_address\":\"coinex1x6u3jacjqaqdfwvj4fs7adtumpsv2y6p6258jl\",\"amount\":[{\"denom\":\"cet\",\"amount\":\"600000000\"}],\"unlock_time\":0}],\"fee\":{\"amount\":[{\"denom\":\"cet\",\"amount\":\"200000000\"}],\"gas\":1000000},\"signatures\":[{\"pub_key\":[3,147,127,6,163,84,111,186,5,197,115,8,105,97,215,142,146,25,9,250,99,108,15,250,104,136,115,35,8,148,156,158,41],\"signature\":\"F3Zo4xzmc8skzpldzEmcUkpgwnQ65cnoUv7SQh5bLNMd9uNDr08/YRwFzJqHExtiBAZG9VJMHkRnVtUDVPq2RQ==\"}],\"memo\":\"send money from one account to another\"}","height":3977}}
```

## 订阅blockinfo 信息

`blockinfo`

示例：`blockinfo`

应答：

```
{"type":"blockinfo", "payload":{"height":3873,"timestamp":"2019-08-19T11:48:54.507868Z","last_block_hash":"0CB05CB285A61837248F3133A3EBD7C32AD61A5B3729B616274C506E1A886EED"}}
```

## 订阅Ticker 信息

`ticker:<trading-pair>`

示例： `ticker:sdu1/cet`

应答：

```
{"type":"ticker","payload":{"tickers":[{"market":"sdu1/cet","new":"4.799999999600122541","old":"4.799999999600122541"}]}}
```

## 订阅 交易对的成交(deal)信息

`deal:<trading-pair>`

示例：`deal:sdu1/cet`

应答：

```
{"type":"deal", "payload":{"order_id":"coinex1c87uzudwwrgjmq5zude7k0s5t7r8cl6333v6uv-12524","trading_pair":"sdu1/cet","height":4421,"side":2,"price":"4.300000000000000000","left_stock":0,"freeze":0,"deal_stock":587912094,"deal_money":2939560470,"curr_stock":587912094,"curr_money":2939560470,"fill_price": "0.233"}}
```

## 订阅order信息

`order:<address>`

示例：`order:coinex16ur229a4xkj9e0xu06nqge9c23y70g7sl5vj98`

应答：

```
// 订单创建
{"type":"create_order", "payload":{"order_id":"coinex16ur229a4xkj9e0xu06nqge9c23y70g7sl5vj98-11678","sender":"coinex16ur229a4xkj9e0xu06nqge9c23y70g7sl5vj98","trading_pair":"sdu1/cet","order_type":2,"price":"3.100000000000000000","quantity":563125147,"side":2,"time_in_force":3,"feature_fee":0,"height":4422,"frozen_fee":2815626,"freeze":563125147}}

// 订单成交
{"type":"fill_order", "payload":{"order_id":"coinex16ur229a4xkj9e0xu06nqge9c23y70g7sl5vj98-11667","trading_pair":"sdu1/cet","height":4421,"side":1,"price":"5.300000000000000000","left_stock":0,"freeze":174882868,"deal_stock":582942891,"deal_money":2914714455,"curr_stock":582942891,"curr_money":2914714455,"fill_price": "0.233"}}

// 订单删除

{"type":"cancel_order", "payload":{"order_id":"coinex16ur229a4xkj9e0xu06nqge9c23y70g7sl5vj98-11679","trading_pair":"sdu1/cet","height":4422,"side":1,"price":"7.000000000000000000","del_reason":"The order was fully filled","used_commission":2598017,"left_stock":0,"remain_amount":935285865,"deal_stock":519603258,"deal_money":2701936941}}

```

## 订阅订单深度信息

`depth:<trading-pair>`

示例：`depth:sdu1/cet`

应答：

```
{"type":"depth_full", "payload":{"trading_pair":"abc/cet","bids":[{"p":"0.500000000000000000","a":"10000000000"}],"asks":[{"p":"0.600000000000000000","a":"10000000000"}]}}

{"type":"depth_change", "payload":{"trading_pair":"abc/cet","bids":[],"asks":[{"p":"0.520000000000000000","a":"10000000000"}]}}

{"type":"depth_delta", "payload":{"trading_pair":"abc/cet","bids":[],"asks":[{"p":"0.600000000000000000","a":"10000000000"}]}}
```

## 订阅K线信息

`kline:<trading-pair>:<level>`

示例：`kline:sdu1/cet:1min`

应答：
```
{"type":"kline", "payload":{"open":"5.199999998894717703","close":"4.899999999013645027","high":"5.300000000000000000","low":"4.899999991850623725","total":"507280335742","unix_time":1566220136,"time_span":"1min","market":"sdu1/cet"}}
```

## 订阅bancor 信息

`bancor:<trading-pair>`

示例：`bancor:sdu1/cet`

应答：

```
{"type":"bancor", "payload":{"sender":"coinex1kc2nguz9xfttfpav4drldh2w96xyzrnqss9scw","stock":"sdu1","money":"cet","init_price":"1.000000000000000000","max_supply":"10000000000000","max_price":"500.000000000000000000","price":"1.000000002994000000","stock_in_pool":"9999999999940","money_in_pool":"60","earliest_cancel_time":1917014400}}
```

## 订阅 bancor trade 信息

`bancor-trade`

示例: `bancor-trade:coinex18c3hryxtjdtjjvnjm63r8k3p8tlhm0l6k96l9v`

应答：

```
{"type":"bancor-trade", "payload":{"sender":"coinex18c3hryxtjdtjjvnjm63r8k3p8tlhm0l6k96l9v","stock":"sdu1","money":"cet","amount":60,"side":1,"money_limit":100,"transaction_price":"5.400000000000000000","block_height":9532}}
```


## 订阅comment 信息

`comment:<symbol>`

示例：`comment:cet`

应答：

```
{"type":"comment", "payload":{"id":0,"height":9710,"sender":"coinex18c3hryxtjdtjjvnjm63r8k3p8tlhm0l6k96l9v","token":"cet","donation":2,"title":"I love cet.","content":"CET to da moon","content_type":3,"references":null}}
```

## 订阅unlock 信息

`unlock:<address>`

示例: `unlock:coinex18c3hryxtjdtjjvnjm63r8k3p8tlhm0l6k96l9v`

应答:

```
{"type":"unlock", "payload":{"address":"coinex18c3hryxtjdtjjvnjm63r8k3p8tlhm0l6k96l9v","unlocked":[{"denom":"cet","amount":"1000000"}],"locked_coins":null,"frozen_coins":[{"denom":"cet","amount":"1711856251008"},{"denom":"sdu1","amount":"663131006349"}],"coins":[{"denom":"cet","amount":"12483077110822102"},{"denom":"sdu1","amount":"12499299245036521"}],"height":10281}}
```

## 订阅 Redelegation 信息

`redelegation:<address>`

示例: `redelegation:coinex18rdsh78t4ds76p58kum34rye2pmrt3hj8z2ehg`

应答:

```
{"type":"redelegation", "payload":{"delegator":"coinex18rdsh78t4ds76p58kum34rye2pmrt3hj8z2ehg","src":"coinexvaloper1z6vr3s5nrn5d6fyxl5vmw77ehznme07w9dan6x","dst":"coinexvaloper16pr4xqlsglwu6urkyt975nxzl65hlt2fw0n58d","amount":"200000000000","completion_time":"2019-08-21T16:00:50+08:00"}}
```

## 订阅 unbonding 信息

`unbonding:<address>`

示例: `unboding:coinex19yl5cehvef8aghtpv0xf9wrmgn3s244deafs57`

应答:

```
{"type":"unbonding", "payload":{"delegator":"coinex19yl5cehvef8aghtpv0xf9wrmgn3s244deafs57","validator":"coinexvaloper1htx25v7pex9lkqr77axdq33cy50ws8dquf0psu","amount":"100000","completion_time":"2019-08-20T14:19:49Z"}}
```

## 订阅slash 信息

`slash`

示例:

应答:

```
{"type":"slash", "payload":{"validator":"coinexvalcons1qwztwxzzndpdc94tujv8fux9phfenqmvx296zw","power":"1000000","reason":"double_sign","jailed":true}}
```