# bancorlite command


## Transactions (under the tx sub command)

`cetcli tx bancor` provides three sub commands, which are used for: creating and canceling bancor markets, and trading in them.

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

Global Flags: ...
```

### Create bancor market

Usage:

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

Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                 |
| ------------| --------------- | ------ | -------- | ------------  | ------------------------|
| Parameter 1 | The stock token | string | ✔        |               | The token used as stock |
| Parameter 2 | The money token | string | ✔        |               | The token used as money |

Major options:

| Option                 | Type      | Required | Default Value | Comment                       |
| ---------------------- | ----------| -------- | ------------- | ------------------------------  |
| --max-supply           | int       | ✔        |              | The maximum stock supply       |
| --max-price            | decimal   | ✔        |              | The maximum price when all stock are sold out |
| --init-price           | decimal   | ✔        |              | The initial price when no stock are sold out |
| --stock-precision      | int (0 ~ 8) | ✔      |              | Controls the granularity in trading |
| --earliest-cancel-time | int       | ✔        |              | Unix timestamp (in seconds) |


> 💡Note: only the token owner (who issued this token) can create a bancor market with this token as stock. For example, only the owner of `abc` can create a bancor market `abc/def`.

Example 1: create a bancor market `abc/def`, whose max supply is 10000000000000, max price is 500, initial price is 5, stock volumn in each trade must integral multiple of 100 (10^3), and earliest cancel time is one month later.

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



### Cancel a bancor market

Usage:

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

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                 |
| ------------| --------------- | ------ | -------- | ------------  | ------------------------|
| Parameter 1 | The stock token | string | ✔        |               | The token used as stock |
| Parameter 2 | The money token | string | ✔        |               | The token used as money |

> 💡Note: Only the creator of the bancor market can cancel this market.

Example 1: cancel the bancor market `abc/def`.

```
$ ./cetcli tx bancorlite cancel abc def \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=abc_owner
```



### Trade in a bancor market

Usage:

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

Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                 |
| ------------| --------------- | ------ | -------- | ------------  | ------------------------|
| Parameter 1 | The stock token | string | ✔        |               | The token used as stock |
| Parameter 2 | The money token | string | ✔        |               | The token used as money |

Major options:

| Option                 | Type      | Required | Default Value | Comment                       |
| ---------------------- | ----------| -------- | ------------ | ------------------------------  |
| --side     | string (buy \| sell)  | ✔        |              |        |
| --amount               | int       | ✔        |              | amount of stock to be traded |
| --money-limit          | int       | ✔        |              | maximum money that may be paid |



Example 1: Buy `50000abc` at `abc/def`, paying no more than `100000def`

```
$ ./cetcli tx bancorlite trade abc def \
	--side buy --amount 50000 --money-limit=100000 \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=myaddr
```

Example 2: Sell `50000abc` to `abc/def`, to get no less than `100000def`

```
$ ./cetcli tx bancorlite trade abc def \
	--side sell --amount 50000 --money-limit=100000 \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
	--gas=30000 --fees=800000cet \
	--from=myaddr
```



## Query

To query the module parameters of bancorlite and information about bancor markets.

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

Global Flags: ...
```



### Query the parameters of bancorlite module

Usage:

```
$ ./cetcli query bancorlite params -h
Query bancorlite params

Usage:
  cetcli query bancorlite params [flags]

Flags: ...
Global Flags: ...
```

Example:

```
$ ./cetcli query bancorlite params --node=47.252.23.106:26657 --chain-id=coinexdex
{
  "create_bancor_fee": "10000000000",
  "cancel_bancor_fee": "10000000000",
  "trade_fee_rate": "10"
}
```

`create_bancor_fee` and `cancel_bancor_fee` are features fee in creating and canceling a bancor market. `trade_fee_rate` should be divided by 1e4 to get the real fee rate (10 means 0.1%).

### Query information about a bancor market

Usage:

```
$ ./cetcli query bancorlite info -h
query the banor pool's information about a symbol pair. 

Example : 
	cetcli query bancorlite info stock money --trust-node=true --chain-id=coinexdex

Usage:
  cetcli query bancorlite info [stock] [money] [flags]

Flags: ...
Global Flags: ...
```

Example 1: query the information about the bancor market `dac/cet`

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


### Query information about all the bancor markets

Usage:

```
$ cetcli query bancorlite infos -h
query all bancor infos in blockchain.

Example :
	cetcli query bancorlite infos \
	--trust-node=true --chain-id=coinexdex

Usage:
  cetcli query bancorlite infos [flags]
```

Example:

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


