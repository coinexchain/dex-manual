# gov命令

治理和提案相关的交易命令挂在`cetcli tx gov`命令下，查询命令挂在`cetcli query gov`命令下。



## 交易

`cetcli tx go`提供了三个子命令，分别用于：提交提案、给提案充值、投票。

```
$ ./cetcli tx gov -h
Governance transactions subcommands

Usage:
  cetcli tx gov [flags]
  cetcli tx gov [command]

Available Commands:
  deposit         Deposit tokens for an active proposal
  vote            Vote for an active proposal, options: yes/no/no_with_veto/abstain
  submit-proposal Submit a proposal along with an initial deposit

Flags:
  -h, --help   help for gov

Global Flags: 省略
```



### submit-proposal

发起提案，用法：

```
$ ./cetcli tx gov submit-proposal -h
Submit a proposal along with an initial deposit.
Proposal title, description, type and deposit can be given directly or through a proposal JSON file.

Example:
$ cetcli tx gov submit-proposal --proposal="path/to/proposal.json" --from mykey

Where proposal.json contains:

{
  "title": "Test Proposal",
  "description": "My awesome proposal",
  "type": "Text",
  "deposit": "10test"
}

Which is equivalent to:

$ cetcli tx gov submit-proposal --title="Test Proposal" --description="My awesome proposal" --type="Text" --deposit="10test" --from mykey

Usage:
  cetcli tx gov submit-proposal [flags]
  cetcli tx gov submit-proposal [command]

Available Commands:
  param-change         Submit a parameter change proposal
  community-pool-spend Submit a community pool spend proposal

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --deposit string          deposit of proposal
      --description string      description of proposal
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for submit-proposal
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --proposal string         proposal file path (if this path is given, other proposal flags are ignored)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --title string            title of proposal
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
      --type string             proposalType of proposal, types: text/parameter_change/software_upgrade
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```

主要选项：

| 选项          | 类型（取值范围）                                  | 是否必填 | 默认值 | 说明     |
| ------------- | ------------------------------------------------- | -------- | ------ | -------- |
| --title       | string                                            |          |        | 提案标题 |
| --description | string                                            |          |        | 提案描述 |
| --type        | string (text\|parameter_change\|software_upgrade) |          |        | 最大供应 |
| --deposit     | string                                            |          |        | 价格     |
| --proposal    | string                                            |          |        | 初始价格 |

例1，在CoinEx测试网3000发起文本提案，初始充值5000CET，提案参数由命令行选项指定：

```
$ ./cetcli tx gov submit-proposal --type=text \
	--title=proposal001 \
	--description='my first proposal' \
	--deposit=500000000000cet \
  --node=3.13.170.79:26657 --chain-id=coinexdex-test3000 \
  --gas=30000 --fees=800000cet \
  --from=bob
```

例2，在CoinEx测试网3000发起文本提案，初始充值100CET，提案参数由文件制定：

```
$ echo '{
  "title": "proposal002",
  "description": "my second proposal",
  "type": "Text",
  "deposit": "100cet"
}' > proposal002.json
$ ./cetcli tx gov submit-proposal --proposal=proposal002.json \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 \
  --gas=30000 --fees=800000cet \
  --from=bob
```

> 注意⚠️：提案类型必须是首字母大写的`Text`



### param-change

发起参数变更提案，用法：

```
$ ./cetcli tx gov submit-proposal param-change -h
Submit a parameter proposal along with an initial deposit.
The proposal details must be supplied via a JSON file. For values that contains
objects, only non-empty fields will be updated.

IMPORTANT: Currently parameter changes are evaluated but not validated, so it is
very important that any "value" change is valid (ie. correct type and within bounds)
for its respective parameter, eg. "MaxValidators" should be an integer and not a decimal.

Proper vetting of a parameter change proposal should prevent this from happening
(no deposits should occur during the governance process), but it should be noted
regardless.

Example:
$ cetcli tx gov submit-proposal param-change <path/to/proposal.json> --from=<key_or_address>

Where proposal.json contains:

{
  "title": "Staking Param Change",
  "description": "Update max validators",
  "changes": [
    {
      "subspace": "staking",
      "key": "MaxValidators",
      "value": 105
    }
  ],
  "deposit": [
    {
      "denom": "stake",
      "amount": "10000"
    }
  ]
}

Usage:
  cetcli tx gov submit-proposal param-change [proposal-file] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围） | 是否必填 | 默认值 | 说明     |
| ----- | ---------------- | -------- | ------ | -------- |
| 参数1 | string           | ✔        |        | 提案标题 |

例1，在CoinEx测试网3000发起参数变更提案，将验证者上限提高到50（当前为42）：

```
$ echo '{
  "title": "Staking Param Change",
  "description": "Update max validators",
  "changes": [
    {
      "subspace": "staking",
      "key": "MaxValidators",
      "value": 50
    }
  ],
  "deposit": [
    {
      "denom": "cet",
      "amount": "10000"
    }
  ]
}' > proposal003.json
$ ./cetcli tx gov submit-proposal param-change proposal003.json \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 \
  --gas=30000 --fees=800000cet \
  --from=bob
```



### community-pool-spend

发起“community pool spend”提案，用法：

```
$ ./cetcli tx gov submit-proposal community-pool-spend -h                                                                        
Submit a community pool spend proposal along with an initial deposit.
The proposal details must be supplied via a JSON file.

Example:
$ cetcli tx gov submit-proposal community-pool-spend <path/to/proposal.json> --from=<key_or_address>

Where proposal.json contains:

{
  "title": "Community Pool Spend",
  "description": "Pay me some Atoms!",
  "recipient": "cettest1s5afhd6gxevu37mkqcvvsj8qeylhn0rzp3yvun",
  "amount": [
    {
      "denom": "stake",
      "amount": "10000"
    }
  ],
  "deposit": [
    {
      "denom": "stake",
      "amount": "10000"
    }
  ]
}

Usage:
  cetcli tx gov submit-proposal community-pool-spend [proposal-file] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围） | 是否必填 | 默认值 | 说明 |
| ----- | ---------------- | -------- | ------ | ---- |
| 参数1 | string           | ✔        |        |      |

例1，在CoinEx测试网3000发起“community pool spend”提案：

```
$ echo '{
  "title": "Community Pool Spend",
  "description": "Pay me some CET!",
  "recipient": "cettest1ww4767g5sf5y4pn5gutf7hgetc42w7hughzscm",
  "amount": [
    {
      "denom": "cet",
      "amount": "10000"
    }
  ],
  "deposit": [
    {
      "denom": "cet",
      "amount": "10000"
    }
  ]
}' > proposal004.json
$ ./cetcli tx gov submit-proposal community-pool-spend proposal004.json \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 \
  --gas=30000 --fees=800000cet \
  --from=bob
```



### deposit

给提案充值，用法：

```
$ ./cetcli tx gov deposit -h
Submit a deposit for an active proposal. You can
find the proposal-id by running "cetcli query gov proposals".

Example:
$ cetcli tx gov deposit 1 10cet --from mykey

Usage:
  cetcli tx gov deposit [proposal-id] [deposit] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围） | 是否必填 | 默认值 | 说明                   |
| ----- | ---------------- | -------- | ------ | ---------------------- |
| 参数1 | uint             | ✔        |        | 提案ID                 |
| 参数2 | string           | ✔        |        | 充值金额，例如10000cet |

例1，在CoinEx链测试网3000给提案1充值5000CET：

```
$ ./cetcli tx gov deposit 1 500000000000cet \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 \
  --gas=30000 --fees=800000cet \
  --from=bob
```



### vote

给提案投票，用法：

```
$ ./cetcli tx gov vote -h
Submit a vote for an active proposal. You can
find the proposal-id by running "cetcli query gov proposals".

Example:
$ cetcli tx gov vote 1 yes --from mykey

Usage:
  cetcli tx gov vote [proposal-id] [option] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围）                      | 是否必填 | 默认值 | 说明                   |
| ----- | ------------------------------------- | -------- | ------ | ---------------------- |
| 参数1 | uint                                  | ✔        |        | 提案ID                 |
| 参数2 | string (Yes\|Abstain\|No\|NoWithVeto) | ✔        |        | 投票（赞同、弃权、反对、强烈反对） |

例1，在CoinEx链测试网3000给提案1投赞同票：

```
$ ./cetcli tx gov vote 1 Yes \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 \
  --gas=30000 --fees=800000cet \
  --from=bob
```



## 查询

可以执行`cetcli query gov -h`查看查询子命令列表：

```
$ ./cetcli query gov -h
Querying commands for the governance module

Usage:
  cetcli query gov [flags]
  cetcli query gov [command]

Available Commands:
  proposal    Query details of a single proposal
  proposals   Query proposals with optional filters
  vote        Query details of a single vote
  votes       Query votes on a proposal
  param       Query the parameters (voting|tallying|deposit) of the governance process
  params      Query the parameters of the governance process
  proposer    Query the proposer of a governance proposal
  deposit     Query details of a deposit
  deposits    Query deposits on a proposal
  tally       Get the tally of a proposal vote

Flags:
  -h, --help   help for gov

Global Flags: 省略
```



### params

查询gov系统参数，用法：

```
$ ./cetcli query gov params -h
Query the all the parameters for the governance process.

Example:
$ cetcli query gov params

Usage:
  cetcli query gov params [flags]

Flags: 省略
Global Flags: 省略
```

例1，查询CoinEx链主网gov系统参数：

```
$ ./cetcli query gov params --node=47.252.23.106:26657 --chain-id=coinexdex -o json --indent
{
  "voting_params": {
    "voting_period": "1209600000000000"
  },
  "tally_params": {
    "quorum": "0.400000000000000000",
    "threshold": "0.500000000000000000",
    "veto": "0.334000000000000000"
  },
  "deposit_params": {
    "min_deposit": [
      {
        "denom": "cet",
        "amount": "1000000000000"
      }
    ],
    "max_deposit_period": "1209600000000000"
  }
}
```

参数详细解释见[参数说明文档](../docs/C_param_ref_cn.md)。



### param

查询gov系统子系统参数，用法：

```
$ ./cetcli query gov param -h
Query the all the parameters for the governance process.

Example:
$ cetcli query gov param voting
$ cetcli query gov param tallying
$ cetcli query gov param deposit

Usage:
  cetcli query gov param [param-type] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围）                   | 是否必填 | 默认值 | 说明     |
| ----- | ---------------------------------- | -------- | ------ | -------- |
| 参数1 | string (voting\|tallying\|deposit) | ✔        |        | 参数类型 |

例1，查询CoinEx链主网gov系统deposit参数：

```
$ ./cetcli query gov param deposit \
	--node=3.13.170.79:26657 --chain-id=coinexdex -o json --indent
{
  "min_deposit": [
    {
      "denom": "cet",
      "amount": "1000000000000"
    }
  ],
  "max_deposit_period": "1209600000000000"
}
```

例2，查询CoinEx链主网gov系统voting参数：

```
$ ./cetcli query gov param voting \
	--node=3.13.170.79:26657 --chain-id=coinexdex -o json --indent
{
  "voting_period": "1209600000000000"
}
```

例2，查询CoinEx链主网gov系统voting参数：

```
$ ./cetcli query gov param tallying \
	--node=3.13.170.79:26657 --chain-id=coinexdex -o json --indent
{
  "quorum": "0.400000000000000000",
  "threshold": "0.500000000000000000",
  "veto": "0.334000000000000000"
}
```



### proposals

查看全部提案，用法：

```
$ ./cetcli query gov proposals -h
Query for a all proposals. You can filter the returns with the following flags.

Example:
$ cetcli query gov proposals --depositor coinex1skjwj5whet0lpe65qaq4rpq03hjxlwd9cwqql5
$ cetcli query gov proposals --voter coinex1skjwj5whet0lpe65qaq4rpq03hjxlwd9cwqql5
$ cetcli query gov proposals --status (DepositPeriod|VotingPeriod|Passed|Rejected)

Usage:
  cetcli query gov proposals [flags]

Flags:
      --depositor string   (optional) filter by proposals deposited on by depositor
      --height int         Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help               help for proposals
      --indent             Add indent to JSON response
      --ledger             Use a connected Ledger device
      --limit string       (optional) limit to latest [number] proposals. Defaults to all proposals
      --node string        <host>:<port> to Tendermint RPC interface for this chain (default "tcp://localhost:26657")
      --status string      (optional) filter proposals by proposal status, status: deposit_period/voting_period/passed/rejected
      --trust-node         Trust connected full node (don't verify proofs for responses)
      --voter string       (optional) filter by proposals voted on by voted

Global Flags: 省略
```

主要选项：

| 参数        | 类型（取值范围）                                         | 是否必填 | 默认值 | 说明                                             |
| ----------- | -------------------------------------------------------- | -------- | ------ | ------------------------------------------------ |
| --depositor | string                                                   |          |        | 通过提案赞助者（Bech32地址）过滤提案             |
| --voter     | string                                                   |          |        | 通过提案投票者（Bech32地址）过滤提案             |
| --status    | string (deposit_period\|voting_period\|passed\|rejected) |          |        | 通过提案状态（预案、投票中、通过、拒绝）过滤提案 |

例1，查看CoinEx链测试网3000全部提案：

```
$ ./cetcli query gov proposals --node=3.13.170.79:26657 --chain-id=coinexdex-test3000 -o json --indent
[
  {
    "content": {
      "type": "cosmos-sdk/TextProposal",
      "value": {
        "title": "proposal001",
        "description": "my first proposal"
      }
    },
    "id": "1",
    "proposal_status": "VotingPeriod",
    "final_tally_result": {
      "yes": "0",
      "abstain": "0",
      "no": "0",
      "no_with_veto": "0"
    },
    "submit_time": "2019-11-27T02:37:16.602399366Z",
    "deposit_end_time": "2019-12-11T02:37:16.602399366Z",
    "total_deposit": [
      {
        "denom": "cet",
        "amount": "1010000000000"
      }
    ],
    "voting_start_time": "2019-11-27T03:36:54.939461379Z",
    "voting_end_time": "2019-12-11T03:36:54.939461379Z"
  },
  ...
]
```



### proposal

根据提案ID查看提案信息，用法：

```
$ ./cetcli query gov proposal -h
Query details for a proposal. You can find the
proposal-id by running "cetcli query gov proposals".

Example:
$ cetcli query gov proposal 1

Usage:
  cetcli query gov proposal [proposal-id] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围） | 是否必填 | 默认值 | 说明   |
| ----- | ---------------- | -------- | ------ | ------ |
| 参数1 | int              | ✔        |        | 提案ID |

例1，在CoinEx链测试网3000查看提案3信息：

```
$ ./cetcli query gov proposal 3 \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 -o json --indent
{
  "content": {
    "type": "cosmos-sdk/ParameterChangeProposal",
    "value": {
      "title": "Staking Param Change",
      "description": "Update max validators",
      "changes": [
        {
          "subspace": "staking",
          "key": "MaxValidators",
          "value": "50"
        }
      ]
    }
  },
  "id": "3",
  "proposal_status": "DepositPeriod",
  "final_tally_result": {
    "yes": "0",
    "abstain": "0",
    "no": "0",
    "no_with_veto": "0"
  },
  "submit_time": "2019-11-27T03:13:57.499830513Z",
  "deposit_end_time": "2019-12-11T03:13:57.499830513Z",
  "total_deposit": [
    {
      "denom": "cet",
      "amount": "10000"
    }
  ],
  "voting_start_time": "0001-01-01T00:00:00Z",
  "voting_end_time": "0001-01-01T00:00:00Z"
}
```



### deposits

 查看某提案的全部充值信息，用法：

```
$ ./cetcli query gov deposits -h
Query details for all deposits on a proposal.
You can find the proposal-id by running "cetcli query gov proposals".

Example:
$ cetcli query gov deposits 1

Usage:
  cetcli query gov deposits [proposal-id] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围） | 是否必填 | 默认值 | 说明   |
| ----- | ---------------- | -------- | ------ | ------ |
| 参数1 | int              | ✔        |        | 提案ID |

例1，在CoinEx链测试网3000查看提案3的充值信息：

```
$ ./cetcli query gov deposits 3 \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 -o json --indent
[
  {
    "proposal_id": "3",
    "depositor": "cettest1ylvuksyr0kwg05v9h9pwgj76e0gv2why6hrany",
    "amount": [
      {
        "denom": "cet",
        "amount": "50000000000"
      }
    ]
  },
  {
    "proposal_id": "3",
    "depositor": "cettest1ww4767g5sf5y4pn5gutf7hgetc42w7hughzscm",
    "amount": [
      {
        "denom": "cet",
        "amount": "10000"
      }
    ]
  }
]
```



### deposit

查看某提案来自某地址的充值信息，用法：

```
$ ./cetcli query gov deposit -h
Query details for a single proposal deposit on a proposal by its identifier.

Example:
$ cetcli query gov deposit 1 coinex1skjwj5whet0lpe65qaq4rpq03hjxlwd9cwqql5

Usage:
  cetcli query gov deposit [proposal-id] [depositer-addr] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围） | 是否必填 | 默认值 | 说明       |
| ----- | ---------------- | -------- | ------ | ---------- |
| 参数1 | int              | ✔        |        | 提案ID     |
| 参数2 | string           | ✔        |        | Bech32地址 |

例1，在CoinEx链测试网3000查看提案3来自给定地址的充值信息：

```
$ ./cetcli query gov deposit 3 cettest1ww4767g5sf5y4pn5gutf7hgetc42w7hughzscm \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 -o json --indent
{
  "proposal_id": "3",
  "depositor": "cettest1ww4767g5sf5y4pn5gutf7hgetc42w7hughzscm",
  "amount": [
    {
      "denom": "cet",
      "amount": "10000"
    }
  ]
}
```



### votes

查看提案的全部投票信息，用法：

```
$ ./cetcli query gov votes -h
Query vote details for a single proposal by its identifier.

Example:
$ cetcli query gov votes 1

Usage:
  cetcli query gov votes [proposal-id] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围） | 是否必填 | 默认值 | 说明   |
| ----- | ---------------- | -------- | ------ | ------ |
| 参数1 | int              | ✔        |        | 提案ID |

例1，在CoinEx链测试网3000查看提案1的投票信息：

```
$ ./cetcli query gov votes 1 \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 -o json --indent
[
  {
    "proposal_id": "1",
    "voter": "cettest1ylvuksyr0kwg05v9h9pwgj76e0gv2why6hrany",
    "option": "No"
  },
  {
    "proposal_id": "1",
    "voter": "cettest1ww4767g5sf5y4pn5gutf7hgetc42w7hughzscm",
    "option": "Yes"
  }
]
```



### vote

查看某地址对于某提案的投票信息，用法：

```
$ ./cetcli query gov vote -h
Query details for a single vote on a proposal given its identifier.

Example:
$ cetcli query gov vote 1 coinex1skjwj5whet0lpe65qaq4rpq03hjxlwd9cwqql5

Usage:
  cetcli query gov vote [proposal-id] [voter-addr] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围） | 是否必填 | 默认值 | 说明       |
| ----- | ---------------- | -------- | ------ | ---------- |
| 参数1 | int              | ✔        |        | 提案ID     |
| 参数2 | string           | ✔        |        | Bech32地址 |

例1，在CoinEx链测试网3000查看提案1的投票信息：

```
$ ./cetcli query gov vote 1 cettest1ylvuksyr0kwg05v9h9pwgj76e0gv2why6hrany \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 -o json --indent
{
  "proposal_id": "1",
  "voter": "cettest1ylvuksyr0kwg05v9h9pwgj76e0gv2why6hrany",
  "option": "No"
}
```



### proposer

查看提案的发起者，用法：

```
$ ./cetcli query gov proposer -h
Query which address proposed a proposal with a given ID.

Example:
$ cetcli query gov proposer 1

Usage:
  cetcli query gov proposer [proposal-id] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围） | 是否必填 | 默认值 | 说明   |
| ----- | ---------------- | -------- | ------ | ------ |
| 参数1 | int              | ✔        |        | 提案ID |

例1，在CoinEx链测试网3000查看提案1的发起者：

```
$ ./cetcli query gov proposer 1 \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 -o json --indent
{
  "proposal_id": "1",
  "proposer": "cettest1ww4767g5sf5y4pn5gutf7hgetc42w7hughzscm"
}
```



### tally

查看提案的点票结果，用法：

```
$ ./cetcli query gov tally -h
Query tally of votes on a proposal. You can find
the proposal-id by running "cetcli query gov proposals".

Example:
$ cetcli query gov tally 1

Usage:
  cetcli query gov tally [proposal-id] [flags]

Flags: 省略
Global Flags: 省略
```

参数：

| 参数  | 类型（取值范围） | 是否必填 | 默认值 | 说明   |
| ----- | ---------------- | -------- | ------ | ------ |
| 参数1 | int              | ✔        |        | 提案ID |

例1，在CoinEx链测试网3000查看提案1的点票结果：

```
$ ./cetcli query gov tally 1 \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 -o json --indent
{
  "yes": "0",
  "abstain": "0",
  "no": "0",
  "no_with_veto": "0"
}
```









