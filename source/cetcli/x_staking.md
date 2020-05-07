# staking

The sub commands related to delegation are listed under the `staking` command.

## tx


You can use `create-validator` to initiate a validator node, then use `edit-validator` to edit validator's information and use `delegate`, `redelegate` and `unbond` to do staking.

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

Flags: ...
Global Flags: ...
```



### create-validator

Usage:

```
$ ./cetcli tx staking create-validator -h
create new validator initialized with a self-delegation to it

Usage:
  cetcli tx staking create-validator [flags]

Flags: ...
Global Flags: ...
```

Major Options:

| Option                       | Type             | Required | Default Value | Comment                       |
| ---------------------------- | ---------------- | -------- | ------------- | ------------------------------  |
| --pubkey                     | string           | ✔        |        | consensus public key starting with `coinexvalconspub1` |
| --amount                     | string           | ✔        |        | initial self-delegation                                        |
| --moniker                    | string           | ✔        |        | name of the validator node |
| --identity                   | string           |          |        | An logo's url (Browsers and wallets can show icons based on this url)             |
| --website                    | string           |          |        |                                                   |
| --details                    | string           |          |        | Some description                                      |
| --commission-max-change-rate | string (decimal) |          |        | How fast can the commission rate be changed |
| --commission-max-rate        | string (decimal) |          |        | The maximum commission rate |
| --commission-rate            | string (decimal) |          |        | The initial commission rate |
| --min-self-delegation        | string (int)     |          |        | The minimum self-delegation |

Example:

```
$ ./cetcli tx staking create-validator \
	--pubkey=coinexvalconspub1zcjduepqgx6fm2szcuf6sqc787d20r7860zumcr40ck9yl4gcqzphl0pu07s4wau0e \
	--amount=500000000000000cet \
	--moniker=bearhome --identity=keybaseNumber --website=www.bearhome.com \
	--commission-rate=0.1 --commission-max-rate=0.2 --commission-max-change-rate=0.01 \
	--min-self-delegation=500000000000000
```



### edit-validator

Edit the information of a validator:

```
$ ./cetcli tx staking edit-validator -h
edit an existing validator account

Usage:
  cetcli tx staking edit-validator [flags]

Flags: ...
Global Flags: ...
```

Major Options:

| Option                       | Type             | Required | Default Value   | Comment                       |
| ---------------------------- | ---------------- | -------- | --------------- | ------------------------------  |
| --moniker                    | string           | ✔        | [do-not-modify] | name of the validator node |
| --identity                   | string           |          | [do-not-modify] | An logo's url (Browsers and wallets can show icons based on this url)             |
| --website                    | string           |          | [do-not-modify] |                                                   |
| --details                    | string           |          | [do-not-modify] | Some description                                      |
| --commission-rate            | string (decimal) |          |                 | The initial commission rate |
| --min-self-delegation        | string (int)     |          |                 | The minimum self-delegation |

> Note💡: The new commission rate can not exceed the maximum commission rate specified when creating this validator, and the change rate can not exceed the pre-defined limit at creation.

Example:

```
$ ./cetcli tx staking edit-validator \
	--moniker=bearhome2 --identity=keybaseNumber2 --website=www.bearhome2.com \
	--commission-rate=0.11 --min-self-delegation=200000000000000
```



### delegate

To delegate some CET to a validator:

```
$ ./cetcli tx staking delegate -h
Delegate an amount of liquid coins to a validator from your wallet.

Example:
$ cetcli tx staking delegate coinexvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7f56jav7 1000cet --from mykey

Usage:
  cetcli tx staking delegate [validator-addr] [amount] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | validator-addr  | string | ✔        |               | a validator's address starting with `coinexvaloper1` |
| Parameter 2 | amount          | string | ✔        |               | the amount to be delegated |


Example: delegate 10cet to CETDAC (whose address is coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30)

```
$ ./cetcli tx staking delegate \
	coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30 \
	1000000000cet \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
  --gas=30000 --fees=800000cet \
  --from=bob
```

### redelegate

`redelegate` command moves some stakes from one validator to another.

```
$ ./cetcli tx staking redelegate -h
Redelegate an amount of illiquid staking tokens from one validator to another.

Example:
$ cetcli tx staking redelegate coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th coinexvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7f56jav7 100cet --from mykey

Usage:
  cetcli tx staking redelegate [src-validator-addr] [dst-validator-addr] [amount] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ---------------    | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | src-validator-addr | string | ✔        |               | the source validator's address |
| Parameter 2 | dst-validator-addr | string | ✔        |               | the destination validator's address |
| Parameter 3 | amount             | string | ✔        |               | the amount to be redelegated |


Example: redelegatee 5 CET from CETDAC (coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30) to IFWallet (coinexvaloper1y6xsfgt9e7l2thrzu8j8mv0ahys34jaap83ql0).

```
$ ./cetcli tx staking redelegate \
	coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30 \
	coinexvaloper1y6xsfgt9e7l2thrzu8j8mv0ahys34jaap83ql0 \
	500000000cet \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
  --gas=30000 --fees=800000cet \
  --from=myaddr
```


### unbond

`unbond` command cancels the delegation at a validator.

```
$ ./cetcli tx staking unbond -h
Unbond an amount of bonded shares from a validator.

Example:
$ cetcli tx staking unbond coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th 100cet --from mykey

Usage:
  cetcli tx staking unbond [validator-addr] [amount] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | validator-addr  | string | ✔        |               | a validator's address starting with `coinexvaloper1` |
| Parameter 2 | amount          | string | ✔        |               | the amount to be unbonded |

Example: unbond 2 CET from CETDAC (coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30).

```
$ ./cetcli tx staking unbond \
	coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30 \
	200000000cet \
	--node=47.252.23.106:26657 --chain-id=coinexdex \
  --gas=30000 --fees=800000cet \
  --from=myaddr
```



## query

Please use `cetcli query staking -h` to view the sub commands.

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

Flags: ...
Global Flags: ...
```


### params

Query the parameters of the staking module.

```
$ ./cetcli query staking params -h
Query values set as staking parameters.

Example:
$ cetcli query staking params

Usage:
  cetcli query staking params [flags]

Flags: ...
Global Flags: ...
```

Example: 

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



### pool

Query the summary information about the staking pool.

```
$ ./cetcli query staking pool -h
Query values for amounts stored in the staking pool:

$ cetcli query staking pool

Usage:
  cetcli query staking pool [flags]

Flags: ...
Global Flags: ...
```

Example:

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

About the query result:

| Field Name            | Field Value          | Comment                                  |
| --------------------- | -------------------- | ---------------------------------------- |
| total\_supply         | 586884903761317189   | The total supplay of CET   |
| bonded\_tokens        | 131564379202878071   | amount of bonded CET                    |
| not\_bonded\_tokens   | 24071074374067855    | amount of not-bonded CET                 |
| non\_bondable\_tokens | 267960653453737554   | amount of CET which can not be bonded    |
| bonded\_ratio         | 0.412525479250930698 | `bonded / (total - not-bounded) ` |



### validators

Query all the validators.

```
$ ./cetcli query staking validators -h
Query details about all validators on a network.

Example:
$ cetcli query staking validators

Usage:
  cetcli query staking validators [flags]

Flags: ...
Global Flags: ...
```

Examples:

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

Query the detail of one validator.

```
$ ./cetcli query staking validator -h
Query details about an individual validator.

Example:
$ cetcli query staking validator coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking validator [validator-addr] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | validator-addr  | string | ✔        |               | a validator's address starting with `coinexvaloper1` |

Example: query detail of the validator CETDAC (coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30).

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

Query all the delegations of a delegator.

```
$ ./cetcli query staking delegations -h
Query delegations for an individual delegator on all validators.

Example:
$ cetcli query staking delegations coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r

Usage:
  cetcli query staking delegations [delegator-addr] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | delegator-addr  | string | ✔        |               | a delegator's address |

Example:

```
$ ./cetcli query staking delegations coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
[
  {
    "delegator_address": "coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h",
    "validator_address": "coinexvaloper1y6xsfgt9e7l2thrzu8j8mv0ahys34jaap83ql0",
    "shares": "500000000.000000000000000000",
    "balance": "500000000"
  },
  {
    "delegator_address": "coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h",
    "validator_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
    "shares": "300000000.000000000000000000",
    "balance": "300000000"
  },
  {
    "delegator_address": "coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h",
    "validator_address": "coinexvaloper1ccdv4k2fndd0pe5amq3dfgwexhcxmzx2l7xqvh",
    "shares": "2000000000.000000000000000000",
    "balance": "2000000000"
  }
]
```



### delegation

Query the delegation from a delegator to a validator.

```
$ ./cetcli query staking delegation -h
Query delegations for an individual delegator on an individual validator.

Example:
$ cetcli query staking delegation coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking delegation [delegator-addr] [validator-addr] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | delegator-addr  | string | ✔        |               | a delegator's address |
| Parameter 2 | validator-addr  | string | ✔        |               | a validator's address |

Example: query the delegation from coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h to coinexvaloper1y6xsfgt9e7l2thrzu8j8mv0ahys34jaap83ql0.

```
$ ./cetcli query staking delegation \
	coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h \
	coinexvaloper1y6xsfgt9e7l2thrzu8j8mv0ahys34jaap83ql0 \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
{
  "delegator_address": "coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h",
  "validator_address": "coinexvaloper1y6xsfgt9e7l2thrzu8j8mv0ahys34jaap83ql0",
  "shares": "500000000.000000000000000000",
  "balance": "500000000"
}
```



### redelegations

Query the on-the-fly (not-completed-yet) redelegations of a delegator.

```
$ ./cetcli query staking redelegations -h
Query all redelegation records for an individual delegator.

Example:
$ cetcli query staking redelegation coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r

Usage:
  cetcli query staking redelegations [delegator-addr] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | delegator-addr  | string | ✔        |               | a delegator's address |

Example:

```
$ ./cetcli query staking redelegations coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
[
  {
    "delegator_address": "coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h",
    "validator_src_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
    "validator_dst_address": "coinexvaloper1y6xsfgt9e7l2thrzu8j8mv0ahys34jaap83ql0",
    "entries": [
      {
        "creation_height": 496328,
        "completion_time": "2019-12-17T02:46:56.440020619Z",
        "initial_balance": "500000000",
        "shares_dst": "500000000.000000000000000000",
        "balance": "500000000"
      }
    ]
  }
]
```



### redelegation

Query a delegator's redelegation from a source validator to a destination validator.

```
$ ./cetcli query staking redelegation -h
Query a redelegation record for an individual delegator between a source and destination validator.

Example:
$ cetcli query staking redelegation coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r coinexvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7f56jav7 coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking redelegation [delegator-addr] [src-validator-addr] [dst-validator-addr] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name                | Type   | Required | Default Value | Comment                    |
| ------------| ---------------     | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | delegator-addr      | string | ✔        |               | a delegator's address |
| Parameter 2 | src-validator-addr  | string | ✔        |               | the source validator's address |
| Parameter 3 | dst-validator-addr  | string | ✔        |               | the destination validator's address |

Example:

```
$ ./cetcli query staking redelegation \
	coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h \
	coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30 \
	coinexvaloper1y6xsfgt9e7l2thrzu8j8mv0ahys34jaap83ql0 \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
[
  {
    "delegator_address": "coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h",
    "validator_src_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
    "validator_dst_address": "coinexvaloper1y6xsfgt9e7l2thrzu8j8mv0ahys34jaap83ql0",
    "entries": [
      {
        "creation_height": 496328,
        "completion_time": "2019-12-17T02:46:56.440020619Z",
        "initial_balance": "500000000",
        "shares_dst": "500000000.000000000000000000",
        "balance": "500000000"
      }
    ]
  }
]
```



### unbonding-delegations

Query the on-the-fly (not-completed-yet) unbondings of a delegator.

```
$ ./cetcli query staking unbonding-delegations -h
Query unbonding delegations for an individual delegator.

Example:
$ cetcli query staking unbonding-delegation coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r

Usage:
  cetcli query staking unbonding-delegations [delegator-addr] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name                | Type   | Required | Default Value | Comment                    |
| ------------| ---------------     | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | delegator-addr      | string | ✔        |               | a delegator's address |

Example:

```
$ ./cetcli query staking unbonding-delegations coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
[
  {
    "delegator_address": "coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h",
    "validator_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
    "entries": [
      {
        "creation_height": "496471",
        "completion_time": "2019-12-17T02:53:15.932282995Z",
        "initial_balance": "200000000",
        "balance": "200000000"
      }
    ]
  }
]
```



### unbonding-delegation

Query the on-the-fly (not-completed-yet) unbonding delegations between a delegator and a validator.

```
$ ./cetcli query staking unbonding-delegation -h
Query unbonding delegations for an individual delegator on an individual validator.

Example:
$ cetcli query staking unbonding-delegation coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking unbonding-delegation [delegator-addr] [validator-addr] [flags]

Flags: ...
Global Flags: ..
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | delegator-addr  | string | ✔        |               | a delegator's address |
| Parameter 2 | validator-addr  | string | ✔        |               | a validator's address |

Example:

```
$ ./cetcli query staking unbonding-delegation \
	coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h \
	coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30 \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
  {
    "delegator_address": "coinex1e7rqqtwdtmyu3l46u9fgl24tgj7268zt9kun2h",
    "validator_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
    "entries": [
      {
        "creation_height": "496471",
        "completion_time": "2019-12-17T02:53:15.932282995Z",
        "initial_balance": "200000000",
        "balance": "200000000"
      }
    ]
  }
```



### delegations-to

Query all the delegations to a validator.

```
$ ./cetcli query staking delegations-to -h
Query delegations on an individual validator.

Example:
$ cetcli query staking delegations-to coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking delegations-to [validator-addr] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | validator-addr  | string | ✔        |               | a validator's address |

Example: query all the delegations to CETDAC (coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30).

```
$ ./cetcli query staking delegations-to coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30 \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
[
  {
    "delegator_address": "coinex1qzqqhz64c69pnzrlrqfkzvnyyau99fynw5q9d9",
    "validator_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
    "shares": "34427245795803.000000000000000000",
    "balance": "34427245795803"
  },
  {
    "delegator_address": "coinex1qycvmghvzp57nlsz68ux9x8kyz2753jxn9xl7n",
    "validator_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
    "shares": "100318809598587.000000000000000000",
    "balance": "100318809598587"
  },
  ...
]
```



### redelegations-from

Query all the redelegations whose source validator is the specified one.

```
$ ./cetcli query staking redelegations-from -h
Query delegations that are redelegating _from_ a validator.

Example:
$ cetcli query staking redelegations-from coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking redelegations-from [validator-addr] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | validator-addr  | string | ✔        |               | a validator's address |

Example:

```
$ ./cetcli query staking redelegations-from \
	coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30 \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
[
  {
    "delegator_address": "coinex1pzh2r2tlny70mpee3rfvqa3zey4xgvjef9l0vv",
    "validator_src_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
    "validator_dst_address": "coinexvaloper1qqpz9t2zep6cmrvk2astfsetjuqt0t3zpsw9qt",
    "entries": [
      {
        "creation_height": 12411,
        "completion_time": "2019-12-02T09:35:10.372978513Z",
        "initial_balance": "301019400000000",
        "shares_dst": "301019400000000.000000000000000000",
        "balance": "301019400000000"
      }
    ]
  },
  ...
]
```



### unbonding-delegations-from

Query a validator's all unbonding delegations.

```
$ ./cetcli query staking unbonding-delegations-from -h
Query delegations that are unbonding _from_ a validator.

Example:
$ cetcli query staking unbonding-delegations-from coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query staking unbonding-delegations-from [validator-addr] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | validator-addr  | string | ✔        |               | a validator's address |

Example:

```
$ ./cetcli query staking unbonding-delegations-from \
	coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30 \
	--node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
[
  {
    "delegator_address": "coinex1pcvngpk584acrpvtxp7u6sp0sksfmvxznqtq2r",
    "validator_address": "coinexvaloper13apesrc22aa2enl56fnz563v9fgzxwpm57zt30",
    "entries": [
      {
        "creation_height": "418349",
        "completion_time": "2019-12-14T17:32:53.477641841Z",
        "initial_balance": "717600000000",
        "balance": "717600000000"
      }
    ]
  },
  ...
]
```

