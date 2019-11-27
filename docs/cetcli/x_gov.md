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



param-change

community-pool-spend





### deposit



```
$ ./cetcli tx gov deposit -h
Submit a deposit for an active proposal. You can
find the proposal-id by running "cetcli query gov proposals".

Example:
$ cetcli tx gov deposit 1 10cet --from mykey

Usage:
  cetcli tx gov deposit [proposal-id] [deposit] [flags]

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
  -h, --help                    help for deposit
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```





### vote



```
$ ./cetcli tx gov vote -h
Submit a vote for an active proposal. You can
find the proposal-id by running "cetcli query gov proposals".


Example:
$ cetcli tx gov vote 1 yes --from mykey

Usage:
  cetcli tx gov vote [proposal-id] [option] [flags]

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
  -h, --help                    help for vote
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



### proposals



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





### proposal



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





### deposits

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



### deposit



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





### votes



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





### vote



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







### proposer

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





### tally

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







### param

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









