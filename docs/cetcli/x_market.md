# market命令

交易对和订单相关交易命令挂在`cetcli tx market`命令下，查询命令挂在`cetcli query market`命令下。



## 交易

`cetcli tx market`提供了不同的子命令，分别用于：创建交易对、修改交易对价格精读、取消交易对、创建GTE订单、创建IOC订单、删除订单。

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

Global Flags: 省略
```



### 创建交易对

用法：

```
$ ./cetcli tx market create-trading-pair -h
generate a tx and sign it to create trading pair in dex blockchain. 

Example : 
	cetcli tx market create-trading-pair  \
		--stock=eth --money=cet --order-precision=8 --price-precision=8 \
		--from bob --chain-id=coinexdex --gas 20000 --fees=1000cet

Usage:
  cetcli tx market create-trading-pair [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for create-trading-pair
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --money string            The exist token symbol as money
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --order-precision int     To control the granularity of token trade, the token amount of trade must be a multiple of granularity.
      --price-precision int     The trading-pair price precision, used to control the price accuracy of the order when token trades (default 1)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --stock string            The exist token symbol as stock
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```

主要选项：

| 选项名            | 类型（取值范围） | 是否必填 | 默认值 | 说明                                                         |
| ----------------- | ---------------- | -------- | ------ | ------------------------------------------------------------ |
| --stock           | string           | ✔        |        | 基础Token（必须是已发行Token，比如[cat](https://explorer.coinex.org/tokens/cat)，且交易发起者必须是该Token的发行者） |
| --money           | string           | ✔        |        | 计价Token（必须是已发行Token，比如[cet](https://explorer.coinex.org/tokens/cet)） |
| --price-precision | int (0 ~ 18)     | ✔        |        | 价格精度，控制下单时的价格精度                               |
| --order-precision | int (0 ~ 8)      |          | 0      | 数量精度，控制下单时的数量粒度                               |

例1，创建`abc/cet`交易对，下单时价格精度不能大于8，下单时数量必须是100（10^2）的倍数：

```
$ ./cetcli tx market create-trading-pair \
	--stock=abc --money=cet --price-precision=8 --order-precision=2 \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



### 修改交易对价格精度

创建交易对后，修改其价格精度。用法：

```
$ ./cetcli tx market modify-price-precision -h
Modify the price precision of the trading pair in the dex.

Example: 
	cetcli tx market modify-price-precision \
	  --trading-pair=etc/cet --price-precision=9 \
	  --from=bob --chain-id=coinexdex --gas=10000000 --fees=10000cet

Usage:
  cetcli tx market modify-price-precision [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for modify-price-precision
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --price-precision int     The trading-pair price precision (default 8)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trading-pair string     The market trading-pair (default "btc/cet")
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation
  
Global Flags: 省略
```

主要选项：

| 选项名            | 类型         | 是否必填 | 默认值 | 说明                  |
| ----------------- | ------------ | -------- | ------ | --------------------- |
| --trading-pair    | string       | ✔        |        | 交易对，例如`abc/cet` |
| --price-precision | int (0 ~ 18) | ✔        |        | 新的价格精度          |

例1，修改`abc/cet`的价格精度：

```
$ ./cetcli tx market modify-price-precision \
	--trading-pair=abc/cet --price-precision=10 \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



### 取消交易对

下线交易对，用法：

```
$ ./cetcli tx market cancel-trading-pair -h
cancel trading-pair in blockchain at least a week from now. 

Example 
	cetcli tx market cancel-trading-pair \
		--time=1000000 --trading-pair=etc/cet \
		--from=bob --chain-id=coinexdex --gas=1000000 --fees=1000cet

Usage:
  cetcli tx market cancel-trading-pair [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for cancel-trading-pair
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --time int                The trading pair expired after the unix timestamp is specified with nanosecond. (timestamp - time.Now() > 7days) (default 100)
      --trading-pair string     The market trading-pair (default "btc/cet")
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```

主要选项：

| 选项名         | 类型   | 是否必填 | 默认值 | 说明                                              |
| -------------- | ------ | -------- | ------ | ------------------------------------------------- |
| --trading-pair | string | ✔        |        | 交易对，例如`abc/cet`                             |
| --time         | int    | ✔        |        | 下线时间（Unix时间戳，单位是纳秒，且至少在7天后） |

例1，十天后将`abc/cet`下线：

```
$ time=`python -c "import time; print(int((time.time() + 10*24*3600)*1000000000))"`
$ ./cetcli tx market cancel-trading-pair \
	--trading-pair=abc/cet --time=$time \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



### 创建订单

目前可以创建两种类型的订单：**GTE**（Good Till Expire）、**IOC**（Immediate Or Cancel）。GTE订单通过`create-gte-order`命令创建，IOC订单通过`create-ioc-order`命令创建。除了创建的订单类型不一样之外，这两个命令的用法是完全一样的，以`create-gte-order`为例：

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

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
      --blocks int              the gte order will exist at least blocks in blockChain (default 10000)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for create-gte-order
      --identify int            A transaction can contain multiple order creation messages, the identify field was added to the order creation message to give each order a unique ID. So the order ID consists of user address, user sequence, identify.
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --order-type int          The identify of the price limit : 2; (Currently, only price limit orders are supported) (default 2)
      --price int               The price of the order (default 100)
      --price-precision int     The price precision in the order (default 8)
      --quantity int            The number of tokens will be trade in the order  (default 100)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --side int                The buying or selling direction of an order.(buy : 1; sell : 2) (default 1)
      --trading-pair string     The trading pair symbol
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```

主要选项：

| 选项名            | 类型（取值范围） | 是否必填 | 默认值 | 说明                                                   |
| ----------------- | ---------------- | -------- | ------ | ------------------------------------------------------ |
| --trading-pair    | string           | ✔        |        | 交易对，例如`abc/cet`                                  |
| --order-type      | int (2)          | ✔        |        | 目前仅支持限价单，必须填2                              |
| --side            | int (1\|2)       | ✔        |        | 买单填1，卖单填2                                       |
| --price           | int              | ✔        |        | 价格                                                   |
| --price-precision | int (0 ~ 18)     | ✔        |        | 价格精度（不能超过创建交易对时指定的精度）             |
| --quantity        | int              | ✔        |        | 数量（粒度在创建交易对时指定）                         |
| --identify        | int              | ✔        |        | 如果在一个交易内创建多比订单，则需要该字段唯一标识订单 |
| --blocks          | int              |          | 10000  |                                                        |

例1，创建GTE订单（100000个块内有效），买入500个abc，单价为0.02cet（2000 / 10^6）：

```
$ ./cetcli tx market create-gte-order \
	--trading-pair=abc/cet --order-type=2 --side=1 --quantity=500 \
	--price=20000 --price-precision=6  \
	--blocks=100000 --identify=1 \
  --node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```

例2，创建IOC订单，卖出500个abc，单价为0.5cet（5000 / 10^6）：

```
$ ./cetcli tx market create-ioc-order \
	--trading-pair=abc/cet --order-type=2 --side=2 --quantity=500 \
	--price=50000 --price-precision=6  \
	--identify=1 \
  --node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



### 取消订单

用法：

```
$ ./cetcli tx market cancel-order -h
cancel order in blockchain. 

Examples:
	cetcli tx market cancel-order --order-id=[id] \
		--trust-node=true --from=bob --chain-id=coinexdex

Usage:
  cetcli tx market cancel-order [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for cancel-order
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --order-id string         The order id
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```

主要选项：

| 选项名     | 类型   | 是否必填 | 默认值 | 说明   |
| ---------- | ------ | -------- | ------ | ------ |
| --order-id | string | ✔        |        | 订单ID |

例1，取消订单：

```
$ ./cetcli tx market cancel-order --order-id=[id] \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



## 查询

`cetcli query market`提供了不同的子命令，分别用于查询：相关参数、指定交易对、全部交易对、指定订单信息、订单列表。

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
Global Flags: 省略
```



### 查询参数

用法：

```
$ ./cetcli query market params -h
Query market params

Usage:
  cetcli query market params [flags]

Flags:  省略
Global Flags: 省略
```

例1，查询CoinEx链主网market相关参数：

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

参数详细解释见[参数说明文档](../docs/C_param_ref_cn.md)。



### 查询链上全部交易对

用法：

```
$ ./cetcli query market trading-pairs -h
query all trading-pair infos in blockchain.

Example :
	cetcli query market trading-pairs \
		--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query market trading-pairs [flags]

Flags:  省略
Global Flags: 省略
```

例1，查询CoinEx主网交易对列表：

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



### 查询指定交易对

用法：

```
$ ./cetcli query market trading-pair -h
query trading-pair info in blockchain. 

Example : 
	cetcli query market trading-pair eth/cet \
		--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query market trading-pair [pair] [flags]

Flags:  省略
Global Flags: 省略
```

主要参数：

| 参数  | 类型   | 是否必填 | 默认值 | 说明   |
| ----- | ------ | -------- | ------ | ------ |
| 参数1 | string | ✔        |        | 交易对 |

例1，查询CoinEx主网：

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



### 查询订单列表

查询某地址的全部（有效）订单，用法：

```
$ ./cetcli query market order-list -h
Query user order list in blockchain. 

Example:
	cetcli query market order-list [userAddress] \
		--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query market order-list [userAddress] [flags]

Flags:  省略
Global Flags: 省略
```

主要参数：

| 参数  | 类型   | 是否必填 | 默认值 | 说明 |
| ----- | ------ | -------- | ------ | ---- |
| 参数1 | string | ✔        |        | 地址 |

例1，在CoinEx主网查询某地址下的全部订单：

```
$ ./cetcli query market order-list coinex1ghuesxfc9g587arf3zcefxntjm9ctsl3703man \
	--node=47.252.23.106:26657 --chain-id=coinexdex
```



### 查询指定订单

根据订单ID查询订单，用法：

```
$ ./cetcli query market order-info -h
Query order info in blockchain. 

Example :
	cetcli query market order-info [orderID] \
		--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query market order-info [flags]

Flags:  省略
Global Flags: 省略
```

主要参数：

| 参数  | 类型   | 是否必填 | 默认值 | 说明   |
| ----- | ------ | -------- | ------ | ------ |
| 参数1 | string | ✔        |        | 订单ID |

例：

```
$ ./cetcli query market order-info <id> \
	--node=47.252.23.106:26657 --chain-id=coinexdex
```



### 查询订单簿

查询某交易对的订单簿，用法：

```
$ cetcli query market orderbook -h
query the orders in a market. 

Example : 
	cetcli query market orderbook eth/cet \
		--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query market orderbook [flags]

Flags:  省略
Global Flags: 省略
```

主要参数：

| 参数  | 类型   | 是否必填 | 默认值 | 说明   |
| ----- | ------ | -------- | ------ | ------ |
| 参数1 | string | ✔        |        | 交易对 |

