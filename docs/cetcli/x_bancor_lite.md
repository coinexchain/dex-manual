# bancorlite命令

与轻量级bancor市场相关的交易命令挂在`cetcli tx bancorlite`命令下，查询命令挂在`cetcli query bancorlite`命令下。



## 交易

`cetcli tx bancor`提供了3个子命令，分别用于：创建和取消轻量级bancor市场，以及在市场中进行交易。

```
$ ./cetcli tx bancorlite -h
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

用法：

```
$ ./cetcli tx bancorlite init -h
Initialize a bancor pool for a stock/money pair, specifying the maximum supply of this pool
and the maximum reachable price when all the supply are sold out, specifying the init price, 
and specifying the time before which no cancellation is allowed.

Example: 
	 cetcli tx bancorlite init stock money \
	 	--max-supply=10000000000000 --stock-precision=3 --max-price=5 --init-price=1 \
	 	--earliest-cancel-time=1563954165

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
| --max-price            | decimal       | ✔        |        | 价格                                                   |
| --init-price           | decimal | ✔        |        | 初始价格         |
| --stock-precision      | int (0 ~ 8)    | ✔        |        | 数量精度，控制交易时的数量粒度 |
| --earliest-cancel-time | int              | ✔        |        | Unix时间戳，单位是秒 |

> 💡提示：只有某token的发行者才能创建以该token为基础token的bancorlite市场。例如，只有`abc`的发行者才能创建`abc/def`市场。

例1，创建`abc/def`市场，最大供应是10000000000000，最高价格是500，初始价格是5，交易量必须是1000（10^3）的倍数，最早退市时间是一个月后：

```
cancelTime=`python -c "import time; print(int((time.time() + 30*24*3600)))"` # one month later
$ ./cetcli tx bancorlite init abc def \
	--max-supply=10000000000000 \
	--max-price=500 \
	--init-price=5 \
	--stock-precision=3 \
	--earliest-cancel-time=$cancelTime \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



### 取消bancorlite

用法：

```
$ ./cetcli tx bancorlite cancel -h
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

> 💡提示：只有某bancorlite市场的创建者才能取消该市场。

例1，取消bancorlite市场`abc/def`：

```
$ ./cetcli tx bancorlite cancel abc def \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



### 在bancorlite交易

用法：

```
$ ./cetcli tx bancorlite trade -h
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
| --amount      | int                  | ✔        |        | 价格      |
| --money-limit | int                  | ✔        |        | 初始价格  |

例1，从`abc/def`市场买入`50000abc`，花费不超过`100000def`

```
$ ./cetcli tx bancorlite trade abc def \
	--side buy --amount 50000 --money-limit=100000 \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=myaddr
```

例2，向`abc/def`市场卖出`50000abc`，收益不低于`100000def`

```
$ ./cetcli tx bancorlite trade abc def \
	--side sell --amount 50000 --money-limit=100000 \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=myaddr
```



## 查询

可以执行`cetcli query bancorlite`子命令查询bancorlite系统参数或者某个bancorlite市场的信息。

```
$ ./cetcli query bancorlite -h
Querying commands for the bancorlite module

Usage:
  cetcli query bancorlite [command]

Available Commands:
  params      Query bancorlite params
  info        query the banor pool's information about a symbol pair

Flags:
  -h, --help   help for bancorlite

Global Flags: 省略
```



### 查询bancorlite系统参数

用法：

```
$ ./cetcli query bancorlite params -h
Query bancorlite params

Usage:
  cetcli query bancorlite params [flags]

Flags: 省略
Global Flags: 省略
```

例1，查询CoinEx链主网bancorlite系统参数：

```
$ ./cetcli query bancorlite params --node=47.252.23.106:26657 --chain-id=coinexdex
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
$ ./cetcli query bancorlite info -h
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
$ ./cetcli query bancorlite info dac cet \
	--node=47.252.23.106:26657 --chain-id=coinexdex
{
  "stock": "dac",
  "money": "cet",
  "init_price": "2.000000000000000000",
  "max_supply": "10000000000000",
  "stock_precision": "0",
  "max_price": "5.000000000000000000",
  "current_price": "2.056565988257900000",
  "stock_in_pool": "9811446705807",
  "money_in_pool": "382439440099",
  "earliest_cancel_time": "1970-01-01T08:00:00+08:00"
}
```

