# gov command

The commands related to governance and proposals are listed under the `gov` command.

## Tx

`cetcli tx go` provides three sub commands for: submitting a proposal, depositing for a proposal, and voting for a proposal.

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

Global Flags: ...
```


### submit-proposal (Text)

To submit a text proposal.

Usage:

```
$ ./cetcli tx gov submit-proposal -h
Submit a proposal along with an initial deposit.
Proposal title, description, type and deposit can be given directly or through a proposal JSON file.

Example:
$ cetcli tx gov submit-proposal --proposal="path/to/proposal.json" --from mykey

Usage:
  cetcli tx gov submit-proposal [flags]
  cetcli tx gov submit-proposal [command]

Available Commands:
  param-change         Submit a parameter change proposal
  community-pool-spend Submit a community pool spend proposal

Global Flags: ...
```

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------  |
| --proposal       | string           | ✔        |               | path to a json file |

Example:

```
$ echo '{
  "type": "Text",
  "title": "proposal002",
  "description": "my second proposal",
  "deposit": "100cet"
}' > proposal002.json
$ ./cetcli tx gov submit-proposal --proposal=proposal002.json \
	--node=3.13.170.79:26657 --chain-id=coinexdex \
  --gas=30000 --fees=800000cet \
  --from=bob
```

In this command, the "type" field of the json file must be "Text". The "title" is the proposal's title and the "description" is the proposal's content. The "deposite" field is the initial deposit from the proposor.


### param-change

To sumbit a parameter-changing proposal.

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

Usage:
  cetcli tx gov submit-proposal param-change [proposal-file] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | proposal file path | string | ✔        |               | path to a json file |

Example:

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

The "title" is the proposal's title and the "description" is the proposal's content. The "deposite" field is the initial deposit from the proposor. In the "changes" field, list several parameters' subspaces, keys and new values. If this proposal passes, these parameters will be changed to the new value.


### community-pool-spend

To send the coins in community pool to some account.

```
$ ./cetcli tx gov submit-proposal community-pool-spend -h                                                                        
Submit a community pool spend proposal along with an initial deposit.
The proposal details must be supplied via a JSON file.

Usage:
  cetcli tx gov submit-proposal community-pool-spend [proposal-file] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | proposal file path | string | ✔        |               | path to a json file |

Example:

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
	--node=3.13.170.79:26657 --chain-id=coinexdex \
  --gas=30000 --fees=800000cet \
  --from=bob
```

The "title" is the proposal's title and the "description" is the proposal's content. The "deposite" field is the initial deposit from the proposor. The "recipient" is the address which will get the coins and the "amount" field specifies the amount to be sent to recipient. If this proposal passes, these coins will be sent to the recipient.


### deposit

A proposal must have enough deposit to enter the voting phase. Enough deposit means enough insterest in this proposal.

To deposit coins to a proposal:

```
$ ./cetcli tx gov deposit -h
Submit a deposit for an active proposal. You can
find the proposal-id by running "cetcli query gov proposals".

Example:
$ cetcli tx gov deposit 1 10cet --from mykey

Usage:
  cetcli tx gov deposit [proposal-id] [deposit] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | proposal-id        | int    | ✔        |               |  |
| Parameter 2 | deposit            | string | ✔        |               | amount to deposit |


Example: to deposit 5000CET for the proposal #1.

```
$ ./cetcli tx gov deposit 1 500000000000cet \
	--node=3.13.170.79:26657 --chain-id=coinexdex \
  --gas=30000 --fees=800000cet \
  --from=bob
```


### vote

To vote for a proposal.

```
$ ./cetcli tx gov vote -h
Submit a vote for an active proposal. You can
find the proposal-id by running "cetcli query gov proposals".

Example:
$ cetcli tx gov vote 1 yes --from mykey

Usage:
  cetcli tx gov vote [proposal-id] [option] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | proposal-id        | int    | ✔        |               |                            |
| Parameter 2 | option             | string | ✔        |               | (Yes\|Abstain\|No\|NoWithVeto) |


Example 1: vote "Yes" to proposal #1.

```
$ ./cetcli tx gov vote 1 Yes \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 \
  --gas=30000 --fees=800000cet \
  --from=bob
```



## Query

Please use `cetcli query gov -h` to see the sub command list.

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

Global Flags: ...
```

### params

Query the parameters in the gov module.

```
$ ./cetcli query gov params -h
Query the all the parameters for the governance process.

Example:
$ cetcli query gov params

Usage:
  cetcli query gov params [flags]

Flags: ...
Global Flags: ...
```

Example:

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

It returns the parameters in three subsystems: voting, tally and deposit.

### param

Query the parameters in one of the three subsystems: voting, tallying and deposit.

```
$ ./cetcli query gov param -h
Query the all the parameters for the governance process.

Example:
$ cetcli query gov param voting
$ cetcli query gov param tallying
$ cetcli query gov param deposit

Usage:
  cetcli query gov param [param-type] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | param-type         | string | ✔        |               | voting\|tallying\|deposit  |

Example 1: Query the deposit parameters:

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

Example 2: Query the voting parameters:

```
$ ./cetcli query gov param voting \
	--node=3.13.170.79:26657 --chain-id=coinexdex -o json --indent
{
  "voting_period": "1209600000000000"
}
```

Example 3: Query the tallying parameters:

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

To view all the proposals:

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
| --depositor | string                                                   |          |        | filter the proposals with depositor's address             |
| --voter     | string                                                   |          |        | filter the proposals with proposer's address    |
| --status    | string (deposit_period\|voting_period\|passed\|rejected) |          |        | filter the proposals with their status |

Example: to view all the proposals in testnet-3000:

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

Query one specific proposal, given its ID.

```
$ ./cetcli query gov proposal -h
Query details for a proposal. You can find the
proposal-id by running "cetcli query gov proposals".

Example:
$ cetcli query gov proposal 1

Usage:
  cetcli query gov proposal [proposal-id] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | proposal-id        | int    | ✔        |               |   |

Example: View the information of proposal #3 at testnet-3000.

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

To view all the deposits on a particular proposal.

```
$ ./cetcli query gov deposits -h
Query details for all deposits on a proposal.
You can find the proposal-id by running "cetcli query gov proposals".

Example:
$ cetcli query gov deposits 1

Usage:
  cetcli query gov deposits [proposal-id] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | proposal-id        | int    | ✔        |               |   |

Example: View the information of proposal #3 at testnet-3000.

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

To view the deposit on a particular proposal, from a given depositer.

```
$ ./cetcli query gov deposit -h
Query details for a single proposal deposit on a proposal by its identifier.

Example:
$ cetcli query gov deposit 1 coinex1skjwj5whet0lpe65qaq4rpq03hjxlwd9cwqql5

Usage:
  cetcli query gov deposit [proposal-id] [depositer-addr] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | proposal-id        | int    | ✔        |               |   |
| Parameter 2 | depositer-addr     | string | ✔        |               |   |

Example: View the information of proposal #3 from cettest1ww4767g5sf5y4pn5gutf7hgetc42w7hughzscm at testnet-3000.

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

To view all the votes of a proposal:

```
$ ./cetcli query gov votes -h
Query vote details for a single proposal by its identifier.

Example:
$ cetcli query gov votes 1

Usage:
  cetcli query gov votes [proposal-id] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | proposal-id        | int    | ✔        |               |   |

Example: View the votes of proposal #3 at testnet-3000.

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

To view the vote of a proposal, from a particular voter.

```
$ ./cetcli query gov vote -h
Query details for a single vote on a proposal given its identifier.

Example:
$ cetcli query gov vote 1 coinex1skjwj5whet0lpe65qaq4rpq03hjxlwd9cwqql5

Usage:
  cetcli query gov vote [proposal-id] [voter-addr] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | proposal-id        | int    | ✔        |               |   |
| Parameter 2 | voter-addr         | string | ✔        |               |   |


Example: View the vote of proposal #3 at testnet-3000, from cettest1ylvuksyr0kwg05v9h9pwgj76e0gv2why6hrany.

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

To view the proposer of a proposal.

```
$ ./cetcli query gov proposer -h
Query which address proposed a proposal with a given ID.

Example:
$ cetcli query gov proposer 1

Usage:
  cetcli query gov proposer [proposal-id] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | proposal-id        | int    | ✔        |               |   |

Example: View the proposer of proposal #1 at testnet-3000.

```
$ ./cetcli query gov proposer 1 \
	--node=3.13.170.79:26657 --chain-id=coinexdex-test3000 -o json --indent
{
  "proposal_id": "1",
  "proposer": "cettest1ww4767g5sf5y4pn5gutf7hgetc42w7hughzscm"
}
```



### tally

To view the voting results of a proposal.

```
$ ./cetcli query gov tally -h
Query tally of votes on a proposal. You can find
the proposal-id by running "cetcli query gov proposals".

Example:
$ cetcli query gov tally 1

Usage:
  cetcli query gov tally [proposal-id] [flags]

Flags: ...
Global Flags: ...
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                    |
| ------------| ------------------ | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | proposal-id        | int    | ✔        |               |   |


Example: view the voting result of proposal #1, on testnet-3000.

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









