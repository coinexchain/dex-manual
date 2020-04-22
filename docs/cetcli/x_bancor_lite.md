# bancorlite命令

bancorlite命令用于和bancor市场有关的活动，例如bancor市场的创建，bancor交易，bancor市场的取消，查询等。



## tx命令

用于发起bancor相关交易，具体包含3个子命令，分别用于：创建bancor市场，在bancor市场中进行交易以及取消bancor市场。

```
$ cetcli tx bancorlite -h
bancorlite transactions subcommands

Usage:
  cetcli tx bancorlite [command]

Available Commands:
  init        Initialize a bancor pool for a stock/money pair
  trade       Trade with a bancor pool
  cancel      Cancel a bancor pool for a stock/money pair

Flags:
  -h, --help   help for bancorlite

Global Flags: 省略
```



### 创建bancorlite

创建bancor市场，用法：

```
$ cetcli tx bancorlite init -h
Initialize a bancor pool for a stock/money pair, specifying the maximum supply of this pool and the maximum reachable price when all the supply are sold out, specifying the init price, and specifying the time before which no cancellation is allowed.

Example:
	 cetcli tx bancorlite init stock money --max-supply=10000000000000 --max-money=100000 --stock-precision=3 --max-price=5 --init-price=1 --earliest-cancel-time=1563954165

Usage:
  cetcli tx bancorlite init [stock] [money] [flags]

Flags:
  -a, --account-number uint           The account number of the signing account (offline mode only)
  -b, --broadcast-mode string         Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                       ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --earliest-cancel-time string   The time that bancor can be canceled (default "0")
      --fees string                   Fees to pay along with transaction; eg: 10cet
      --from string                   Name or address of private key with which to sign
      --gas string                    gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float          adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string             Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only                 Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
      --generate-unsigned-tx          Generate a unsigned tx
  -h, --help                          help for init
      --indent                        Add indent to JSON response
      --init-price string             The init price of this bancor (default "0")
      --ledger                        Use a connected Ledger device
      --max-money string              The maximum money of this pool (default "0")
      --max-price string              The maximum reachable price when all the supply are sold out (default "0")
      --max-supply string             The maximum supply of this pool. (default "0")
      --memo string                   Memo to send along with transaction
      --node string                   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint                 The sequence number of the signing account (offline mode only)
      --stock-precision string        The precision of stock (default "0")
      --trust-node                    Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                           Skip tx broadcasting prompt confirmation
Global Flags: 省略
```

参数和主要选项：

| 参数/选项              | 类型（取值范围） | 是否必填 | 默认值 | 说明                                                   |
| ---------------------- | ---------------- | -------- | ------ | ------------------------------------------------------ |
| 参数1        | string           | ✔        |        | 基础Token（含义可以参考`market`命令） |
| 参数2           | string    | ✔        |        | 计价Token（含义可以参考`market`命令）     |
| --max-supply           | int       | ✔        |        | 最大供应                               |
| --max-money | int | ✔ | | 基础Token全部售出后可获得的计价Token数量 |
| --max-price            | decimal       | ✔        |        | 价格                                                   |
| --init-price           | decimal | ✔        |        | 初始价格         |
| --stock-precision      | int (0 ~ 8)    | ✔        |        | 数量精度，控制交易时的数量粒度 |
| --earliest-cancel-time | int              | ✔        |        | Unix时间戳，单位是秒 |

例子：

创建`abc/def`市场，最大供应是10000000000000，最高价格是50，初始价格是5，计价Token数为300000000000000交易数量精度是3，最早退市时间是未来的某一个时间点$cancelTime：

```
cancelTime=`python -c "import time; print(int((time.time() + 30*24*3600)))"` # one month later
$ ./cetcli tx bancorlite init abc def \
	--max-supply=10000000000000 \
	--max-money=300000000000000 \
	--max-price=50 \
	--init-price=5 \
	--stock-precision=3 \
	--earliest-cancel-time=$cancelTime \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



### 取消bancorlite

取消已存在的bancor市场，用法：

```
$ cetcli tx bancorlite cancel -h
Cancel a bancor pool for a stock/money pair, sender must be this stock owner

Example: 
	 cetcli tx bancorlite cancel stock money

Usage:
  cetcli tx bancorlite cancel [stock] [money] [flags]

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
      --generate-unsigned-tx    Generate a unsigned tx
  -h, --help                    help for cancel
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```

参数和主要选项：

| 参数/选项 | 类型（取值范围） | 是否必填 | 默认值 | 说明      |
| --------- | ---------------- | -------- | ------ | --------- |
| 参数1     | string           | ✔        |        | 基础Token |
| 参数2     | string           | ✔        |        | 计价Token |

例子：

取消bancorlite市场`abc/def`：

```
$ ./cetcli tx bancorlite cancel abc def \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



### 在bancorlite交易

与指定symbol的bancor市场交易，用法：

```
$ cetcli tx bancorlite trade -h
Sell Stocks to a bancor pool or buy Stocks from a bancor pool.

Example: 
	 cetcli tx bancorlite trade stock money --side buy --amount=100 --money-limit=120
	 cetcli tx bancorlite trade stock money --side sell --amount=100 --money-limit=80

Usage:
  cetcli tx bancorlite trade [stock] [money] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
      --amount int              The amount of tokens to be traded.
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
      --generate-unsigned-tx    Generate a unsigned tx
  -h, --help                    help for trade
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --money-limit int         The upper bound of money you want to pay when buying, or the lower bound of money you want to get when selling. Specify zero or negative value if you do not want a such a limit.
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --side string             the side of the trade, 'buy' or 'sell'.
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```

参数和主要选项：

| 参数/选项     | 类型（取值范围）     | 是否必填 | 默认值 | 说明      |
| ------------- | -------------------- | -------- | ------ | --------- |
| 参数1         | string               | ✔        |        | 基础Token |
| 参数2         | string               | ✔        |        | 计价Token |
| --side        | string (buy \| sell) | ✔        |        | buy表示买入，sell表示卖出 |
| --amount      | int                  | ✔        |        | 基础Token的交易数量 |
| --money-limit | int                  | ✔        |        | 该参数表示买时愿意付出的最大计价Token数量或卖时必须保障的最小计价Token数量。 |

例1，从`abc/def`市场买入`50000abc`，花费不超过`100000def`

```
$ cetcli tx bancorlite trade abc def \
	--side buy --amount 50000 --money-limit=100000 \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=myaddr
```

例2，向`abc/def`市场卖出`50000abc`，收益不低于`100000def`

```
$ cetcli tx bancorlite trade abc def \
	--side sell --amount 50000 --money-limit=100000 \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=myaddr
```



## query命令

用于查询bancor的参数信息，指定symbol的bancor信息或者全部bancor信息

```
cetcli query bancorlite -h
Querying commands for the bancorlite module

Usage:
  cetcli query bancorlite [command]

Available Commands:
  params      Query bancorlite params
  info        query the banor pool's information about a symbol pair
  infos       query all bancor infos in blockchain

Global Flags: 省略
```



### 查询bancorlite系统参数

用法：

```
$ cetcli query bancorlite params -h
Query bancorlite params

Usage:
  cetcli query bancorlite params [flags]

Flags: 省略
Global Flags: 省略
```

例1，查询CoinEx链主网bancorlite系统参数：

```
$ cetcli query bancorlite params --node=47.252.23.106:26657 --chain-id=coinexdex
{
  "create_bancor_fee": "10000000000",
  "cancel_bancor_fee": "10000000000",
  "trade_fee_rate": "10"
}
```

参数详细解释见[参数说明文档](../docs/C_param_ref_cn.md)。



### 查询bancorlite市场信息

查询指定的bancorlite市场信息，用法：

```
$ cetcli query bancorlite info -h
query the banor pool's information about a symbol pair. 

Example : 
	cetcli query bancorlite info stock money --trust-node=true --chain-id=coinexdex

Usage:
  cetcli query bancorlite info [stock] [money] [flags]

Flags:省略
Global Flags: 省略
```

例1，在CoinEx链主网上查询交易对为`dac/cet`的bancorlite市场信息

```
$ cetcli query bancorlite info dac cet \
	--node=47.252.23.106:26657 --chain-id=coinexdex
{
  "owner": "coinex13apesrc22aa2enl56fnz563v9fgzxwpm03prlm",
  "stock": "dac",
  "money": "cet",
  "init_price": "0.100000000000000000",
  "max_supply": "10000000000000",
  "stock_precision": "0",
  "max_price": "3.000000000000000000",
  "max_money": "0",
  "ar": "0",
  "current_price": "0.625694031897830000",
  "stock_in_pool": "8187261958973",
  "money_in_pool": "657746588884",
  "earliest_cancel_time": "0"
}
```



### 查询全部bancorlite市场信息

查询全部的bancorlite市场信息，用法：

```
$ cetcli query bancorlite infos -h
query all bancor infos in blockchain.

Example :
	cetcli query bancorlite infos \
	--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query bancorlite infos [flags]
```

例1，查询全部的bancorlite市场信息

```
$ cetcli query bancorlite info dac cet --chain-id=coinexdex
[
  {
    "owner": "coinex1usennm7yszldclewmzkg3a570xkjngc9dj69hp",
    "stock": "aaa",
    "money": "cet",
    "init_price": "0.000000010000000000",
    "max_supply": "500000000000000",
    "stock_precision": "8",
    "max_price": "10.000000000000000000",
    "max_money": "0",
    "ar": "0",
    "current_price": "0.020000009980000000",
    "stock_in_pool": "499000000000000",
    "money_in_pool": "10000009990",
    "earliest_cancel_time": "1605024000"
  },
  {
    "owner": "coinex1d5xqs50vey8sc8eg6uta0wfqnnn5u623cmwj0y",
    "stock": "captainzxx001",
    "money": "flychen999999999",
    "init_price": "10.000000000000000000",
    "max_supply": "100000000000000",
    "stock_precision": "0",
    "max_price": "1000.000000000000000000",
    "max_money": "0",
    "ar": "0",
    "current_price": "10.080190000000000000",
    "stock_in_pool": "99991900000000",
    "money_in_pool": "81324769500",
    "earliest_cancel_time": "1585584000"
  }
]
```



