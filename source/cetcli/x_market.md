# market

The sub commands related to orders and markets (trading pairs) are listed under the `market` command.

## tx

`cetcli tx market` provides several sub commands to:
1. Create a trading pair
2. Change the price precision of a trading pair
3. Cancel a trading pair
4. Create GTE or IOC orders
5. Cancel GTE orders

```
$ ./cetcli tx market -h
market transactions subcommands

Usage:
  cetcli tx market [command]

Available Commands:
  create-trading-pair    generate tx to create trading pair
  create-gte-order       Create an GTE order and sign tx
  create-ioc-order       Create an IOC order and sign tx
  cancel-order           cancel order in blockchain
  cancel-trading-pair    cancel trading-pair in blockchain
  modify-price-precision Modify the price precision of the trading pair 

Flags:
  -h, --help   help for market

Global Flags: ...
```


### Create trading pair

Usage:

```
$ ./cetcli tx market create-trading-pair -h
generate a tx and sign it to create trading pair in dex blockchain. 

Example : 
	cetcli tx market create-trading-pair  \
		--stock=eth --money=cet --order-precision=8 --price-precision=8 \
		--from bob --chain-id=coinexdex --gas 20000 --fees=1000cet

Usage:
  cetcli tx market create-trading-pair [flags]

Global Flags: ...
```

Major options:

| Option            | Type             | Required | Default Value | Comment                       |
| ----------------- | ---------------- | -------- | ------------- | ------------------------------  |
| --stock           | string           | ✔        |               | stock of the trading pair  |
| --money           | string           | ✔        |               | money of the trading pair  |
| --price-precision | int (0 ~ 18)     | ✔        |               | the granularity of price in trading  |
| --order-precision | int (0 ~ 8)      | ✔        |               | the granularity of stock in trading  |


Example 1: create the `abc/cet` trading pair. When trading in it, the price precision must be no larger than 8 and the amount of stock must be integral multiples of 100 (10^2).

```
$ ./cetcli tx market create-trading-pair \
	--stock=abc --money=cet --price-precision=8 --order-precision=2 \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



### Modify the price precision of a trading pair

Modify the price precision of a trading pair after creating it.

```
$ ./cetcli tx market modify-price-precision -h
Modify the price precision of the trading pair in the dex.

Example: 
	cetcli tx market modify-price-precision \
	  --trading-pair=etc/cet --price-precision=9 \
	  --from=bob --chain-id=coinexdex --gas=10000000 --fees=10000cet

Usage:
  cetcli tx market modify-price-precision [flags]

Global Flags: ...
```

Major options:

| Option            | Type             | Required | Default Value | Comment                       |
| ----------------- | ---------------- | -------- | ------------- | ------------------------------  |
| --trading-pair    | string           | ✔        |               | name of the trading pair, such as `abc/cet`  |
| --price-precision | int (0 ~ 18)     | ✔        |               | the new price precision  |


Example 1: modify the price precision of `abc/cet`.

```
$ ./cetcli tx market modify-price-precision \
	--trading-pair=abc/cet --price-precision=10 \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



### Cancel a trading pair

To delist a trading pair.

```
$ ./cetcli tx market cancel-trading-pair -h
cancel trading-pair in blockchain at least a week from now. 

Example 
	cetcli tx market cancel-trading-pair \
		--time=1000000 --trading-pair=etc/cet \
		--from=bob --chain-id=coinexdex --gas=1000000 --fees=1000cet

Usage:
  cetcli tx market cancel-trading-pair [flags]

Flags: ...
Global Flags: ...
```

Major options:

| Option         | Type   | Required | Default Value | Comment                       |
| -------------- | -------| -------- | ------------- | ------------------------------  |
| --trading-pair | string | ✔        |               | The trading pair to be delisted |
| --time         | int    | ✔        |               | Delist time (Unix timestamp, in nanoseconds, at least 7 days from now |


Example 1: delist `abc/cet` after ten days.

```
$ time=`python -c "import time; print(int((time.time() + 10*24*3600)*1000000000))"`
$ ./cetcli tx market cancel-trading-pair \
	--trading-pair=abc/cet --time=$time \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```


### Create orders

There are two kinds of orders: **GTE**（Good Till Expire) and **IOC** (Immediate Or Cancel). GTE orders are created with `create-gte-order` command and IOC orders, `create-ioc-order`. The options of these two commands are almost identical (IOC orders do not need the "blocks" option.

```
$ ./cetcli tx market create-gte-order -h
Create an GTE order and sign tx, broadcast to nodes. 

Example:
	cetcli tx market create-gte-order --trading-pair=btc/cet \
		--order-type=2 --price=520 --quantity=10000000 --side=1 \
		--price-precision=10 --blocks=100000 --identify=1 \
    --from=bob --chain-id=coinexdex --gas=10000 --fees=1000cet

Usage:
  cetcli tx market create-gte-order [flags]

Flags: ...
Global Flags: ...
```

Major options:

| Option            | Type             | Required | Default Value | Comment                       |
| ----------------  | ---------------- | -------- | ------------- | ------------------------------  |
| --trading-pair    | string           | ✔        |        | name of the trading pair, such as `abc/cet` |
| --order-type      | int (2)          | ✔        |        | must be 2 (means limited order)             |
| --side            | int (1\|2)       | ✔        |        | 1 for buy, 2 for sell                       |
| --price           | int              | ✔        |        |                                           |
| --price-precision | int (0 ~ 18)     | ✔        |        | can not be larger than the price precision of the trading-pair |
| --quantity        | int              | ✔        |        | must be integral multiple of the granularity of the trading-pair     |
| --identify        | int              | ✔        |        | If multiple orders are created in one transaction, use this field to differentiate them |
| --blocks          | int              |          | 10000  | The lifetime of a GTE order |


Example 1: Create a GTE order (valid in 100000 blocks), which buyes 500abc, at the price of 0.02cet (2000 / 10^6).

```
$ ./cetcli tx market create-gte-order \
	--trading-pair=abc/cet --order-type=2 --side=1 --quantity=500 \
	--price=20000 --price-precision=6  \
	--blocks=100000 --identify=1 \
  --node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```

Example 2: Create a IOC order, which sells 500abc, at the price of 0.5cet (5000 / 10^6).

```
$ ./cetcli tx market create-ioc-order \
	--trading-pair=abc/cet --order-type=2 --side=2 --quantity=500 \
	--price=50000 --price-precision=6  \
	--identify=1 \
  --node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```

### Cancel an order

Usage:

```
$ ./cetcli tx market cancel-order -h
cancel order in blockchain. 

Examples:
	cetcli tx market cancel-order --order-id=[id] \
		--trust-node=true --from=bob --chain-id=coinexdex

Usage:
  cetcli tx market cancel-order [flags]

Flags: ...
Global Flags: ...
```

Major options:

| Option            | Type             | Required | Default Value | Comment                       |
| ----------------  | ---------------- | -------- | ------------- | ------------------------------  |
| --order-id        | string           | ✔        |        | ID of the order to be canceled |

Example 1: cancel an order.

```
$ ./cetcli tx market cancel-order --order-id=coinex1ls72x4nlhcs7qu89vx9afezgkp8w2tmdku4ppw-47104 \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```


## Query


```
$ ./cetcli query market -h
Querying commands for the market module

Usage:
  cetcli query market [command]

Available Commands:
  params        Query market params
  trading-pair  query trading-pair info in blockchain
  trading-pairs query all trading-pair infos in blockchain
  orderbook     query the orders in a market
  order-info    Query order info in blockchain
  order-list    Query user order list in blockchain

Flags:
Global Flags: ...
```

### Parameters

Query the parameters in the market module.

```
$ ./cetcli query market params -h
Query market params

Usage:
  cetcli query market params [flags]

Flags:  ...
Global Flags: ...
```

Example:

```
$ ./cetcli query market params --node=47.252.23.106:26657 --chain-id=coinexdex
{
  "create_market_fee": "1000000000000",
  "fixed_trade_fee": "1000000",
  "market_min_expired_time": "604800000000000",
  "gte_order_lifetime": "100000",
  "gte_order_feature_fee_by_blocks": "10",
  "max_executed_price_change_ratio": "25",
  "market_fee_rate": "10",
  "market_fee_min": "1000000",
  "fee_for_zero_deal": "1000000"
}
```


### Query all the trading pairs on chain

Usage:

```
$ ./cetcli query market trading-pairs -h
query all trading-pair infos in blockchain.

Example :
	cetcli query market trading-pairs \
		--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query market trading-pairs [flags]

Flags:  ...
Global Flags: ...
```

Example:

```
./cetcli query market trading-pairs --node=47.252.23.106:26657 --chain-id=coinexdex
[
  {
    "creator": "coinex13apesrc22aa2enl56fnz563v9fgzxwpm03prlm",
    "stock": "dac",
    "money": "cet",
    "price_precision": "8",
    "last_executed_price": "2.000000200000000000",
    "order_precision": "0"
  },
  ...
]
```



### Query information of a particular trading pair

Usage:

```
$ ./cetcli query market trading-pair -h
query trading-pair info in blockchain. 

Example : 
	cetcli query market trading-pair eth/cet \
		--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query market trading-pair [pair] [flags]

Flags:  ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | pair            | string | ✔        |               | the trading pair's name  |


Example:

```
./cetcli query market trading-pair dac/cet \
	--node=47.252.23.106:26657 --chain-id=coinexdex
{
  "creator": "coinex13apesrc22aa2enl56fnz563v9fgzxwpm03prlm",
  "stock": "dac",
  "money": "cet",
  "price_precision": "8",
  "last_executed_price": "2.000000200000000000",
  "order_precision": "0"
}
```


### Query all the orders from an account

Query all the effective orders sent from an account.

```
$ ./cetcli query market order-list -h
Query user order list in blockchain. 

Example:
	cetcli query market order-list [userAddress] \
		--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query market order-list [userAddress] [flags]

Flags:  ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | userAddress     | string | ✔        |               | the trading pair's name  |


Example:

```
$ ./cetcli query market order-list coinex1ghuesxfc9g587arf3zcefxntjm9ctsl3703man \
	--node=47.252.23.106:26657 --chain-id=coinexdex
```



### Query the detail of an order

Query the detail of an order, given its ID.

```
$ ./cetcli query market order-info -h
Query order info in blockchain. 

Example :
	cetcli query market order-info [orderID] \
		--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query market order-info [orderID] [flags]

Flags:  ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | orderID         | string | ✔        |               |   |


Example:

```
$ ./cetcli query market order-info coinex1ls72x4nlhcs7qu89vx9afezgkp8w2tmdku4ppw-47104 \
	--node=47.252.23.106:26657 --chain-id=coinexdex
```


### Query the order book of a trading pair

Usage:

```
$ cetcli query market orderbook -h
query the orders in a market. 

Example : 
	cetcli query market orderbook eth/cet \
		--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query market orderbook [pair] [flags]

Flags:  ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | pair            | string | ✔        |               | the trading pair's name  |


Example:

```
$ cetcli query market orderbook dac/cet \
	--node=3.20.163.168:26657 --trust-node=true --chain-id=coinexdex2
[
  {
    "order_id": "coinex1ls72x4nlhcs7qu89vx9afezgkp8w2tmdku4ppw-47104",
    "sender": "coinex1ls72x4nlhcs7qu89vx9afezgkp8w2tmdku4ppw",
    "sequence": "184",
    "identify": 0,
    "trading_pair": "dac/cet",
    "order_type": 2,
    "price": "0.511100000000000000",
    "quantity": "198812126516",
    "side": 1,
    "time_in_force": "3",
    "height": "5311075",
    "frozen_commission": "101612878",
    "exist_blocks": "2880000",
    "frozen_feature_fee": "18800000",
    "left_stock": "198812126516",
    "freeze": "101612877863",
    "deal_stock": "0",
    "deal_money": "0"
  },
  ...
]
```

