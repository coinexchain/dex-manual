# staking命令



## 交易

可以通过`create-validator`命令初始化验证者节点，之后可以通过`edit-validator`命令编辑验证者信息，通过`delegate`、`redelegate`、`unbond`命令进行委托操作。

```
$ ./cetcli tx staking -h
Staking transaction subcommands

Usage:
  cetcli tx staking [flags]
  cetcli tx staking [command]

Available Commands:
  create-validator create new validator initialized with a self-delegation to it
  edit-validator   edit an existing validator account
  delegate         Delegate liquid tokens to a validator
  redelegate       Redelegate illiquid tokens from one validator to another
  unbond           Unbond shares from a validator

Flags:
  -h, --help   help for staking

Global Flags: 省略
```



### create-validator

创建验证者，用法：

```
$ ./cetcli tx staking create-validator -h
create new validator initialized with a self-delegation to it

Usage:
  cetcli tx staking create-validator [flags]

Flags:
  -a, --account-number uint                 The account number of the signing account (offline mode only)
      --amount string                       Amount of coins to bond
  -b, --broadcast-mode string               Transaction broadcasting mode (sync|async|block) (default "sync")
      --commission-max-change-rate string   The maximum commission change rate percentage (per day)
      --commission-max-rate string          The maximum commission rate percentage
      --commission-rate string              The initial commission rate percentage
      --details string                      The validator's (optional) details
      --dry-run                             ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string                         Fees to pay along with transaction; eg: 10cet
      --from string                         Name or address of private key with which to sign
      --gas string                          gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float                adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string                   Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only                       Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                                help for create-validator
      --identity string                     The optional identity signature (ex. UPort or Keybase)
      --indent                              Add indent to JSON response
      --ip string                           The node's public IP. It takes effect only when used in combination with --generate-only
      --ledger                              Use a connected Ledger device
      --memo string                         Memo to send along with transaction
      --min-self-delegation string          The minimum self delegation required on the validator
      --moniker string                      The validator's name
      --node string                         <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --node-id string                      The node's ID
      --pubkey string                       The Bech32 encoded PubKey of the validator
  -s, --sequence uint                       The sequence number of the signing account (offline mode only)
      --trust-node                          Trust connected full node (don't verify proofs for responses) (default true)
      --website string                      The validator's (optional) website
  -y, --yes                                 Skip tx broadcasting prompt confirmation

Global Flags: 省略
```

主要选项：

| 参数/选项                    | 类型（取值范围） | 是否必填 | 默认值 | 说明                                     |
| ---------------------------- | ---------------- | -------- | ------ | ---------------------------------------- |
| --pubkey                     | string           | ✔        |        | 共识公钥（Bech32格式编码）               |
| --amount                     | string           | ✔        |        | 初始自质押金额                           |
| --moniker                    | string           | ✔        |        | 名称                                     |
| --identity                   | string           |          |        | 标识（浏览器或钱包可以根据标识显示图标） |
| --website                    | string           |          |        | 网站                                     |
| --details                    | string           |          |        | 详细信息                                 |
| --commission-max-change-rate | string (decimal) |          |        | 佣金变化率上限                           |
| --commission-max-rate        | string (decimal) |          |        | 佣金比率上限                             |
| --commission-rate            | string (decimal) |          |        | 佣金比率                                 |
| --min-self-delegation        | string (int)     |          |        | 自质押下限                               |

例如：

```
$ ./cetcli tx staking create-validator \
	--pubkey=coinexvalconspub1zcjduepqgx6fm2szcuf6sqc787d20r7860zumcr40ck9yl4gcqzphl0pu07s4wau0e \
	--amount=500000000000000cet \
	--moniker=bearhome --identity=keybaseNumber --website=www.bearhome.com \
	--commission-rate=0.1 --commission-max-rate=0.2 --commission-max-change-rate=0.01 \
	--min-self-delegation=500000000000000
```



### edit-validator

编辑验证者信息，用法：

```
$ ./cetcli tx staking edit-validator -h
edit an existing validator account

Usage:
  cetcli tx staking edit-validator [flags]

Flags:
  -a, --account-number uint          The account number of the signing account (offline mode only)
  -b, --broadcast-mode string        Transaction broadcasting mode (sync|async|block) (default "sync")
      --commission-rate string       The new commission rate percentage
      --details string               The validator's (optional) details (default "[do-not-modify]")
      --dry-run                      ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string                  Fees to pay along with transaction; eg: 10cet
      --from string                  Name or address of private key with which to sign
      --gas string                   gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float         adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string            Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only                Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                         help for edit-validator
      --identity string              The (optional) identity signature (ex. UPort or Keybase) (default "[do-not-modify]")
      --indent                       Add indent to JSON response
      --ledger                       Use a connected Ledger device
      --memo string                  Memo to send along with transaction
      --min-self-delegation string   The minimum self delegation required on the validator
      --moniker string               The validator's name (default "[do-not-modify]")
      --node string                  <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint                The sequence number of the signing account (offline mode only)
      --trust-node                   Trust connected full node (don't verify proofs for responses) (default true)
      --website string               The validator's (optional) website (default "[do-not-modify]")
  -y, --yes                          Skip tx broadcasting prompt confirmation

Global Flags: 省略
```

主要选项：

| 参数/选项             | 类型（取值范围） | 是否必填 | 默认值          | 说明                                               |
| --------------------- | ---------------- | -------- | --------------- | -------------------------------------------------- |
| --moniker             | string           |          | [do-not-modify] | 名称                                               |
| --identity            | string           |          | [do-not-modify] | 标识（浏览器或钱包可以根据标识显示图标）           |
| --website             | string           |          | [do-not-modify] | 网站                                               |
| --details             | string           |          | [do-not-modify] | 详细信息                                           |
| --commission-rate     | string (decimal) |          |                 | 佣金比率（不能超过创建验证者时指定的佣金比率上限） |
| --min-self-delegation | string (int)     |          |                 | 自质押下限                                         |

例如：

```
$ ./cetcli tx staking edit-validator \
	--moniker=bearhome2 --identity=keybaseNumber2 --website=www.bearhome2.com \
	--commission-rate=0.11 --min-self-delegation=200000000000000
```



### delegate

委托，用法：

```
$ ./cetcli tx staking delegate -h
Delegate an amount of liquid coins to a validator from your wallet.

Example:
$ cetcli tx staking delegate coinexvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7f56jav7 1000cet --from mykey

Usage:
  cetcli tx staking delegate [validator-addr] [amount] [flags]

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
  -h, --help                    help for delegate
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```





### redelegate

重新委托，用法：

```
$ ./cetcli tx staking redelegate -h
Redelegate an amount of illiquid staking tokens from one validator to another.

Example:
$ cetcli tx staking redelegate coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th coinexvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7f56jav7 100cet --from mykey

Usage:
  cetcli tx staking redelegate [src-validator-addr] [dst-validator-addr] [amount] [flags]

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
  -h, --help                    help for redelegate
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```





### unbond

解绑，用法：

```
$ ./cetcli tx staking unbond -h
Unbond an amount of bonded shares from a validator.

Example:
$ cetcli tx staking unbond coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th 100cet --from mykey

Usage:
  cetcli tx staking unbond [validator-addr] [amount] [flags]

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
  -h, --help                    help for unbond
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```







## 查询

可以执行`cetcli query staking -h`查看查询子命令列表：

```
$ ./cetcli query staking -h
Querying commands for the staking module

Usage:
  cetcli query staking [flags]
  cetcli query staking [command]

Available Commands:
  delegation                 Query a delegation based on address and validator address
  delegations                Query all delegations made by one delegator
  unbonding-delegation       Query an unbonding-delegation record based on delegator and validator address
  unbonding-delegations      Query all unbonding-delegations records for one delegator
  redelegation               Query a redelegation record based on delegator and a source and destination validator address
  redelegations              Query all redelegations records for one delegator
  validator                  Query a validator
  validators                 Query for all validators
  delegations-to             Query all delegations made to one validator
  unbonding-delegations-from Query all unbonding delegatations from a validator
  redelegations-from         Query all outgoing redelegatations from a validator
  pool                       Query the current staking pool values
  params                     Query the current staking parameters information

Flags:
  -h, --help   help for staking

Global Flags: 省略
```



### params

查询staking系统参数，用法：

```
$ ./cetcli query staking params -h
Query values set as staking parameters.

Example:
$ cetcli query staking params

Usage:
  cetcli query staking params [flags]

Flags: 省略
Global Flags: 省略
```

例1，查询CoinEx链主网staking系统参数：

```
$ ./cetcli query staking params --node=47.252.23.106:26657 --chain-id=coinexdex -o json
{
  "unbonding_time":"1814400000000000",
  "max_validators":42,"max_entries":7,
  "bond_denom":"cet",
  "min_self_delegation":"500000000000000",
  "min_mandatory_commission_rate":"0.100000000000000000"
}
```

参数详细解释见[参数说明文档](../docs/C_param_ref_cn.md)。



### pool

查询staking资金池信息，用法：

```
$ ./cetcli query staking pool -h
Query values for amounts stored in the staking pool:

$ cetcli query staking pool

Usage:
  cetcli query staking pool [flags]

Flags: 省略
Global Flags: 省略
```

例1，查询CoinEx链主网当前staking资金池：

```
$ ./cetcli query staking pool --node=47.252.23.106:26657 --chain-id=coinexdex
{
  "not_bonded_tokens": "24071074374067855",
  "bonded_tokens": "131564379202878071",
  "non_bondable_tokens": "267960653453737554",
  "total_supply": "586884903761317189",
  "bonded_ratio": "0.412525479250930698"
}
```

资金池解释：

| 字段名              | 字段值               | 说明                                     |
| ------------------- | -------------------- | ---------------------------------------- |
| total_supply        | 586884903761317189   | CET总发行量                              |
| bonded_tokens       | 131564379202878071   | 已经质押的CET总量                        |
| not_bonded_tokens   | 24071074374067855    | 尚未质押的CET总量                        |
| non_bondable_tokens | 267960653453737554   | 不可质押的CET总量                        |
| bonded_ratio        | 0.412525479250930698 | 质押率（`已经质押 / （总量 - 未质押）`） |



### validators

查询所有验证者，用法：

```
$ ./cetcli query staking validators -h
Query details about all validators on a network.

Example:
$ cetcli query staking validators

Usage:
  cetcli query staking validators [flags]

Flags: 省略
Global Flags: 省略
```

例1，查询CoinEx链主网当前所有验证者：

```
$ ./cetcli query staking validators \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
[
  ... ,
  {
    "operator_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
    "consensus_pubkey": "coinexvalconspub1zcjduepq0l56h7q3kam2pqwa3wgcr722hmyk4nvn2ckpjl4e25yk7mmgfxlsugkga6",
    "jailed": false,
    "status": 2,
    "tokens": "34946990988072881",
    "delegator_shares": "34946990988072881.000000000000000000",
    "description": {
      "moniker": "CETDAC",
      "identity": "96713B358E8D2E20",
      "website": "",
      "details": ""
    },
    "unbonding_height": "0",
    "unbonding_time": "1970-01-01T00:00:00Z",
    "commission": {
      "commission_rates": {
        "rate": "0.100000000000000000",
        "max_rate": "0.200000000000000000",
        "max_change_rate": "0.010000000000000000"
      },
      "update_time": "2019-11-11T08:05:02.132906305Z"
    },
    "min_self_delegation": "500000000000000"
  },
  ...
]
```



### validator

根据地址查询某验证者，用法：

```
$ ./cetcli query staking validator -h
Query details about an individual validator.

Example:
$ cetcli query staking validator coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking validator [validator-addr] [flags]

Flags: 省略
Global Flags: 省略
```

例1，在CoinEx主网查询CETDAC验证者信息：

```
$ ./cetcli query staking validator coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30 \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
{
  "operator_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
  "consensus_pubkey": "coinexvalconspub1zcjduepq0l56h7q3kam2pqwa3wgcr722hmyk4nvn2ckpjl4e25yk7mmgfxlsugkga6",
  "jailed": false,
  "status": 2,
  "tokens": "34946992724540251",
  "delegator_shares": "34946992724540251.000000000000000000",
  "description": {
    "moniker": "CETDAC",
    "identity": "96713B358E8D2E20",
    "website": "",
    "details": ""
  },
  "unbonding_height": "0",
  "unbonding_time": "1970-01-01T00:00:00Z",
  "commission": {
    "commission_rates": {
      "rate": "0.100000000000000000",
      "max_rate": "0.200000000000000000",
      "max_change_rate": "0.010000000000000000"
    },
    "update_time": "2019-11-11T08:05:02.132906305Z"
  },
  "min_self_delegation": "500000000000000"
}
```



### delegations

查询某地址的所有委托信息，用法：

```
$ ./cetcli query staking delegations -h
Query delegations for an individual delegator on all validators.

Example:
$ cetcli query staking delegations coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r

Usage:
  cetcli query staking delegations [delegator-addr] [flags]

Flags: 省略
Global Flags: 省略
```

例1，在CoinEx链主网上查询某地址的所有委托信息：

```
$ ./cetcli query staking delegations coinex19tn666um8a6quq96ehhfyhn3mm00qf4pqqgpww \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
[
  {
    "delegator_address": "coinex19tn666um8a6quq96ehhfyhn3mm00qf4pqqgpww",
    "validator_address": "coinexvaloper1qaxl3h4v36jn0velu6e75dhpslvr3cyxp2cws2",
    "shares": "1061483448266.000000000000000000",
    "balance": "1061483448266"
  },
  {
    "delegator_address": "coinex19tn666um8a6quq96ehhfyhn3mm00qf4pqqgpww",
    "validator_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
    "shares": "204129480582547.000000000000000000",
    "balance": "204129480582547"
  }
]
```



### delegation

给定委托者和验证者地址，查询委托信息，用法：

```
$ ./cetcli query staking delegation -h
Query delegations for an individual delegator on an individual validator.

Example:
$ cetcli query staking delegation coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking delegation [delegator-addr] [validator-addr] [flags]

Flags: 省略
Global Flags: 省略
```

例1，在CoinEx链主网上根据委托者和验证者地址查询委托信息：

```
$ ./cetcli query staking delegation \
	coinex19tn666um8a6quq96ehhfyhn3mm00qf4pqqgpww \
	coinexvaloper1qaxl3h4v36jn0velu6e75dhpslvr3cyxp2cws2 \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
{
  "delegator_address": "coinex19tn666um8a6quq96ehhfyhn3mm00qf4pqqgpww",
  "validator_address": "coinexvaloper1qaxl3h4v36jn0velu6e75dhpslvr3cyxp2cws2",
  "shares": "1061483448266.000000000000000000",
  "balance": "1061483448266"
}
```



### redelegations

```
$ ./cetcli query staking redelegations -h
Query all redelegation records for an individual delegator.

Example:
$ cetcli query staking redelegation coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r

Usage:
  cetcli query staking redelegations [delegator-addr] [flags]

Flags: 省略
Global Flags: 省略
```



### redelegation

```
$ ./cetcli query staking redelegation -h
Query a redelegation record for an individual delegator between a source and destination validator.

Example:
$ cetcli query staking redelegation coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r coinexvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7f56jav7 coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking redelegation [delegator-addr] [src-validator-addr] [dst-validator-addr] [flags]

Flags: 省略
Global Flags: 省略
```







### unbonding-delegations

```
$ ./cetcli query staking unbonding-delegations -h
Query unbonding delegations for an individual delegator.

Example:
$ cetcli query staking unbonding-delegation coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r

Usage:
  cetcli query staking unbonding-delegations [delegator-addr] [flags]

Flags: 省略
Global Flags: 省略
```



### unbonding-delegation



```
$ ./cetcli query staking unbonding-delegation -h
Query unbonding delegations for an individual delegator on an individual validator.

Example:
$ cetcli query staking unbonding-delegation coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking unbonding-delegation [delegator-addr] [validator-addr] [flags]

Flags: 省略
Global Flags: 省略
```



### delegations-to

```
$ ./cetcli query staking delegations-to -h
Query delegations on an individual validator.

Example:
$ cetcli query staking delegations-to coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking delegations-to [validator-addr] [flags]

Flags: 省略
Global Flags: 省略
```



### redelegations-from

```
$ ./cetcli query staking redelegations-from -h
Query delegations that are redelegating _from_ a validator.

Example:
$ cetcli query staking redelegations-from coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking redelegations-from [validator-addr] [flags]

Flags: 省略
Global Flags: 省略
```





### unbonding-delegations-from

```
$ ./cetcli query staking unbonding-delegations-from -h
Query delegations that are unbonding _from_ a validator.

Example:
$ cetcli query staking unbonding-delegations-from coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking unbonding-delegations-from [validator-addr] [flags]

Flags: 省略
Global Flags: 省略
```

