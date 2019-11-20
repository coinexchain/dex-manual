## Wallets

### GUI-Based Wallets

There are several teams working on GUI-based third-party wallets. The CoinEx Chain Team will help them in development. But they completely owns their projects. They decide the user interfaces, functionalities and whether to open the source code.

When such wallets are ready, we will put their links in this document.

### CLI-Based Wallet

The CoinEx Chain Team delivers a wallet program (cetcli) with command line interface (CLI) along with the full node program (cetd). This wallet is officially maintained, full-featured, open-sourced and up-to-date with the full node program.  If you are a professional user, we suggest you learn to use it.

This wallet also provides a RESTful interface through TCP port. When designing GUI-based wallets, it can be used as the back-end.


### Getting Help

The CLI of cetcli is based on [Cobra](https://github.com/spf13/cobra). It automatically add [help commands](https://github.com/spf13/cobra#help-command) and`--help` option. Taking cetcli for example:

```
$ ./cetcli help help
Help provides help for any command in the application.
Simply type cetd help [path to command] for full details.

Usage:
  cetcli help [command] [flags]

Flags:
  -h, --help   help for help
```

The following table summaries different ways to get help:

| Command                          | Usage                                            |
| -------------------------------- | ------------------------------------------------ |
| `cetcli`                         | Print help message for the `cetcli` command      |
| `cetcli help`                    | Print help message for the `cetcli` command      |
| `cetcli -h`                      | Print help message for the `cetcli` command      |
| `cetcli --help`                  | Print help message for the `cetcli` command      |
| `cetcli help subcmd`             | Print help message for `cetcli subcmd`           |
| `cetcli subcmd --help`           | Print help message for `cetcli subcmd`           |
| `cetcli subcmd -h`               | Print help message for `cetcli subcmd`           |
| `cetcli help subcmd subsubcmd`   | Print help message for `cetcli subcmd subsubcmd` |
| `cetcli subcmd subsubcmd -h`     | Print help message for `cetcli subcmd subsubcmd` |
| `cetcli subcmd subsubcmd --help` | Print help message for `cetcli subcmd subsubcmd` |



### The "cetcli keys" subcommand

Please refer to [CETCLI reference](../cetcli/keys.md).



### The "cetcli status" subcommand

The cetcli program needs to connect to a full node program (cetd) to finish most of its work.  The "cetcli status" subcommand is used to query the status of this connected full node.

```
$ ./cetcli status -h
Query remote node for status

Usage:
  cetcli status [flags]

Flags:
  -h, --help          help for status
      --indent        Add indent to JSON response
  -n, --node string   Node to connect to (default "tcp://localhost:26657")

Global Flags: <omitted>
```

For example：

```
$ ./cetcli status --indent
{
  "node_info": {
    "protocol_version": {
      "p2p": "7",
      "block": "10",
      "app": "0"
    },
    "id": "9b36f457b798e2e167a1862cb9c689253e63e75e",
    "listen_addr": "tcp://0.0.0.0:26656",
    "network": "coinexdex",
    "version": "0.31.5",
    "channels": "4020212223303800",
    "moniker": "moniker0",
    "other": {
      "tx_index": "on",
      "rpc_address": "tcp://0.0.0.0:26657"
    }
  },
  "sync_info": {
    "latest_block_hash": "887F58509B79B973276E2564508668553AD1EE9C41234B05116A263A3C67E197",
    "latest_app_hash": "92FE08904425DF85177058AE1B647198BE6DF356786733AF96E19A34D6A56C44",
    "latest_block_height": "21",
    "latest_block_time": "2019-06-29T04:38:58.414442Z",
    "catching_up": false
  },
  "validator_info": {
    "address": "BF0503108B6EAF1E87B4C0827C9B0651948EF693",
    "pub_key": {
      "type": "tendermint/PubKeyEd25519",
      "value": "0X6yl8KoNW+FmyPjqHzY2s7/t/7A/xeW0ej+1zks3nI="
    },
    "voting_power": "100000000"
  }
}
```



### The "cetcli query" subcommand

The level-two subcommands under "cetcli query" are used to get the status of the upper-level application, the status of the underlying tendermint engine, and some historical information.

```
/cetcli q -h

Usage:
  cetcli query [command]

Aliases:
  query, q
  
Available Commands:
  tendermint-validator-set Get the full tendermint validator set at given height
  block                    Get verified data for a the block at given height
  txs                      Search for paginated transactions that match a set of tags
  tx                       Find a transaction by hash in a committed block.    

  account                  Query account balance
  asset                    Querying commands for the asset module
  market                   Querying commands for the market module
  gov                      Querying commands for the governance module
  distr                    Querying commands for the distribution module
  staking                  Querying commands for the staking module
  slashing                 Querying commands for the slashing module
```



#### Tendermint-validator-set

**Usage**

```
Get the full tendermint validator set at given height

Usage:
  cetcli query tendermint-validator-set [height] [flags]

Flags:
  -h, --help          help for tendermint-validator-set
      --indent        indent JSON response
  -n, --node string   Node to connect to (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)
```

**Examples**

- The current height is used as default when no height is specified.

```
./cetcli query tendermint-validator-set --chain-id=coinexdex
block height: 740

  Address:          coinexvalcons1mg40w65qc9rvtugr6tppkdwqh4qz9mzghepuy7
  Pubkey:           coinexvalconspub1zcjduepq5l37smq3gvjm69sa0tmj499hdqvx2vchvv4lgcw2r9rnn5ufdnxsca3dta
  ProposerPriority: 0
  VotingPower:      1000000
```

- Specify a height:

```
./cetcli query tendermint-validator-set 700 --chain-id=coinexdex
block height: 700

  Address:          coinexvalcons1mg40w65qc9rvtugr6tppkdwqh4qz9mzghepuy7
  Pubkey:           coinexvalconspub1zcjduepq5l37smq3gvjm69sa0tmj499hdqvx2vchvv4lgcw2r9rnn5ufdnxsca3dta
  ProposerPriority: 0
  VotingPower:      1000000
```

- Specify a full node to connect to:

```
./cetcli query tendermint-validator-set 700 -n tcp://localhost:26657 --trust-node=true
block height: 700

  Address:          coinexvalcons1mg40w65qc9rvtugr6tppkdwqh4qz9mzghepuy7
  Pubkey:           coinexvalconspub1zcjduepq5l37smq3gvjm69sa0tmj499hdqvx2vchvv4lgcw2r9rnn5ufdnxsca3dta
  ProposerPriority: 0
  VotingPower:      1000000
```



#### block

Query the information of a block at specified height:

```
./cetcli query block 700 --trust-node=true 
{"block_meta":{"block_id":{"hash":"6B977B51C59ACEBDA1E6258C71B7F754797A0DB0D03BDA02CDF5A357186C4D13","parts":{"total":"1","hash":"23C6CF357D7A46799041FD382BE20C77A19FE12483CD7F9BC2DDDB9069905755"}},"he
......
{"total":"1","hash":"449A7DEE253A736FBCE0D37A369631200CC1137457D82357B3D1CE42C1F56B7F"}},"timestamp":"2019-06-16T02:01:09.757229Z","validator_address":"DA2AF76A80C146C5F103D2C21B35C0BD4022EC48","validator_index":"0","signature":"1YvlGttRNZ2JEIY2vyn8dLaNqGdBtY+Jzbn6avcEu58nkihjD3QQicGqLK7ORjtG5WnF4ANL+LbxDwk8lPWiBA=="}]}}}
```

After formatting the json string we can see the four parts of a block: header, data, evidence, last_commit.

```
{
    "block_meta": {
        "block_id": {
            "hash": "3EAEAC30829AE9FC1668D9E8462F228E6B1F07A813E2082CF7474ED031F02C1D",
            "parts": {
                "total": "1",
                "hash": "006EE12C7E76E4A93276AA81A1AC373B7E58DDAF48FB4AB6BC9236F1574B8D92"
            }
        },
        "header": {
            "version": {
                "block": "10",
                "app": "0"
            },
            "chain_id": "coinexdex",
            "height": "1037",
            "time": "2019-06-24T03:59:00.592658Z",
            "num_txs": "0",
            "total_txs": "2",
            "last_block_id": {
                "hash": "25E53D34F65BE9EFA88A6BBD0B85B4E41DDAB1AA19D0D5F22E858E9CA4E0AD92",
                "parts": {
                    "total": "1",
                    "hash": "48A3D25C934CBDD6AD9AB03DE67EA2986CD962E90C0796D1CBA35832D436F407"
                }
            },
            "last_commit_hash": "4D5B8B5585959D4283D23A092D33701EE75914598588F9036F56181EBC7D18D3",
            "data_hash": "",
            "validators_hash": "5C38CB954CFDBB7FE784A3A0EA803E6E5927738A6E8E166A4CCFC9C1652C88B0",
            "next_validators_hash": "5C38CB954CFDBB7FE784A3A0EA803E6E5927738A6E8E166A4CCFC9C1652C88B0",
            "consensus_hash": "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
            "app_hash": "7296D33F3B8EDEEBEE2CB8106EF1E5ADD60650AE6D4E4AF3F08580AFC31A427B",
            "last_results_hash": "",
            "evidence_hash": "",
            "proposer_address": "DA2AF76A80C146C5F103D2C21B35C0BD4022EC48"
        }
    },
    "block": {
        "header": {
           ......
        },
        "data": {
            "txs": null
        },
        "evidence": {
            "evidence": null
        },
        "last_commit": {
            "block_id": {
                "hash": "25E53D34F65BE9EFA88A6BBD0B85B4E41DDAB1AA19D0D5F22E858E9CA4E0AD92",
                "parts": {
                    "total": "1",
                    "hash": "48A3D25C934CBDD6AD9AB03DE67EA2986CD962E90C0796D1CBA35832D436F407"
                }
            },
            "precommits": [
                {
                    "type": 2,
                    "height": "1036",
                    "round": "0",
                    "block_id": {
                        "hash": "25E53D34F65BE9EFA88A6BBD0B85B4E41DDAB1AA19D0D5F22E858E9CA4E0AD92",
                        "parts": {
                            "total": "1",
                            "hash": "48A3D25C934CBDD6AD9AB03DE67EA2986CD962E90C0796D1CBA35832D436F407"
                        }
                    },
                    "timestamp": "2019-06-24T03:59:00.592658Z",
                    "validator_address": "DA2AF76A80C146C5F103D2C21B35C0BD4022EC48",
                    "validator_index": "0",
                    "signature": "JQnGjKTkrGp/5Z0YbiEgOAZj15aeacoxYtpj7SgNtJ9x5EPfhTWyfYH5XgGFmXcRnQfOb7RiR/TGMqTyHKHXDw=="
                }
            ]
        }
    }
}
```



#### txs

After executing a transaction in the upper-level application, one or more tags (key:value pairs) are attached to it by the execution logic. This subcommand can utilize such tags to query transactions. The returned results are paged. You can can control the number of transactions in a page and specify one page to view.

```
./cetcli query txs -h
Search for transactions that match the exact given tags where results are paginated.

Example:
$ cetcli query txs --tags '<tag1>:<value1>&<tag2>:<value2>' --page 1 --limit 30

Usage:
  cetcli query txs [flags]

Flags:
  -h, --help           help for txs
      --limit uint32   Query number of transactions results per page returned (default 30)
  -n, --node string    Node to connect to (default "tcp://localhost:26657")
      --page uint32    Query a specific page of paginated results (default 1)
      --tags string    tag:value list of tags that must match
      --trust-node     Trust connected full node (don't verify proofs for responses)
```

**Examples:**

```
./cetcli query txs --tags='tx.height:18&token:cet' --trust-node=true
[{"height":"18","txhash":"31025AC1F42F7DDE41FDC4907DD41923881CD583B8478E984177EF123A0F7523","raw_log":"[{\"msg_index\":\"0\",\"success\":true,\"log\":\"\"}]","logs":[{"msg_index":"0","success":true,"log":""}],"gas_wanted":"400000","gas_used":"36478","tags":[{"key":"action","value":"issue_token"},{"key":"category","value":"asset"},{"key":"token","value":"cet"},{"key":"owner","value":"coinex1zklle5dqe0njzulcaghvmmt83c0lmvn7rrd8uq"}],"tx":{"type":"auth/StdTx","value":{"msg":[{"type":"asset/MsgIssueToken","value":{"name":"Cet","symbol":"cet","total_supply":"2100000000000000","owner":"coinex1zklle5dqe0njzulcaghvmmt83c0lmvn7rrd8uq","mintable":false,"burnable":true,"addr_forbiddable":false,"token_forbiddable":false}}],"fee":{"amount":[{"denom":"cet","amount":"80000000"}],"gas":"400000"},"signatures":[{"pub_key":{"type":"tendermint/PubKeySecp256k1","value":"AznuBH8oLQuO7gcWGEEtjMvD21M8InMn9Al8iqiuM+R1"},"signature":"Am0Ss2q65CBMNEphnWxswfPTKT3aEUjKica3SVDDSrkWUl0bgfJYJbt8+jfWTI6zmHtHmXJsSCT7GwOCvjz9Eg=="}],"memo":""}},"timestamp":"2019-06-24T06:18:03Z"}]
```



#### tx

After a transaction is committed in a block, you can query it with its hashid.

```
./cetcli query tx -h
Find a transaction by hash in a committed block.

Usage:
  cetcli query tx [hash] [flags]
```



#### account

Query the balance of an account. The freely-spendable coins, frozen coins and locked coins are shown. The delegated coins are not shown.

```
./cetcli query account -h
Query account balance

Usage:
  cetcli query account [address] [flags]

Flags:
  -h, --help          help for account
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)
```

**Example**

```
./cetcli query account coinex1zklle5dqe0njzulcaghvmmt83c0lmvn7rrd8uq --trust-node=true
Account:
  Address:       coinex1zklle5dqe0njzulcaghvmmt83c0lmvn7rrd8uq
  Pubkey:        coinexpub1addwnpepqvu7uprl9qkshrhwqutpssfd3n9u8k6n8s38xfl5p97g429wx0j82c5lq9h
  Coins:         11998999920000000cet
  AccountNumber: 0
  Sequence:      2
  LockedCoins:   
  FrozenCoins:   
  MemoRequired:  false
```



#### asset

Query the information of different assets:

```
./cetcli query asset -h
Querying commands for the asset module

Usage:
  cetcli query asset [command]

Available Commands:
  token            Query token info
  tokens           Query all token
  whitelist        Query whitelist
  forbid-addr      Query forbidden addresses
  reserved-symbols Query reserved symbols
```

**Examples**

- Query the information of some token

```
./cetcli query asset token cet --chain-id=coinexdex
{
  "type": "asset/Token",
  "value": {
    "name": "Cet",
    "symbol": "cet",
    "total_supply": "2100000000000000",
    "owner": "coinex1zklle5dqe0njzulcaghvmmt83c0lmvn7rrd8uq",
    "mintable": false,
    "burnable": true,
    "addr_forbiddable": false,
    "token_forbiddable": false,
    "total_burn": "0",
    "total_mint": "0",
    "is_forbidden": false
  }
}
```

- Show the informations of all the issued tokens:

```
./cetcli query asset tokens --trust-node=true
[
  {
    "type": "asset/Token",
    "value": {
      "name": "Cet",
      "symbol": "cet",
      "total_supply": "2100000000000000",
      "owner": "coinex1zklle5dqe0njzulcaghvmmt83c0lmvn7rrd8uq",
      "mintable": false,
      "burnable": true,
      "addr_forbiddable": false,
      "token_forbiddable": false,
      "total_burn": "0",
      "total_mint": "0",
      "is_forbidden": false
    }
  },
  {
    "type": "asset/Token",
    "value": {
      "name": "eoo",
      "symbol": "eoo",
      "total_supply": "2100000000000000",
      "owner": "coinex1zklle5dqe0njzulcaghvmmt83c0lmvn7rrd8uq",
      "mintable": false,
      "burnable": true,
      "addr_forbiddable": true,
      "token_forbiddable": true,
      "total_burn": "0",
      "total_mint": "0",
      "is_forbidden": false
    }
  }
]
```

- View the whitelist of some token:

```
./cetcli query asset whitelist eoo --trust-node=true
[
  "coinex1p43gzugn44znc2nnmf2psf7kkvtsy4mf40d7kk"
]
```

- View the forbidden addresses of some token:

```
./cetcli query asset forbid-addr eoo --chain-id=coinexdex
[
  "coinex1p43gzugn44znc2nnmf2psf7kkvtsy4mf40d7kk"
]
```

- View the reserve symbols. Normal users can not use such symbols to create token.

```
./cetcli query asset reserved-symbols --chain-id=coinexdex
[
  "btc",
  "eth",
  "xrp",
......
]
```



**market**

Query the information of markets, i.e., trading pairs:

```
./cetcli query market -h
Querying commands for the market module

Usage:
  cetcli query market [command]

Available Commands:
  market-info         query market info
  order-info          Query order info
  user-order-list     Query user order list in blockchain
  wait-cancel-markets 
```

**Usage**

Query the basic information about a trading pair:

```
cetcli query market marketinfo eth/cet
```

Query the information of an order, given its orderid:

```
cetcli query market orderinfo --orderid=[orderid]
```

Query a user's existing orders, given her address:

```
cetcli query market userorderlist --address=[userAddress]
```

Query the markets which are waiting to be canceled at a future height.

```
cetcli query market wait-cancel-markets 
```



#### distr

Query the distribution of incentives:

```
./cetcli query distr -h
Querying commands for the distribution module

Usage:
  cetcli query distr [command]

Available Commands:
  params                        Query distribution params
  validator-outstanding-rewards Query distribution outstanding (un-withdrawn) rewards for a validator and all their delegations
  commission                    Query distribution validator commission
  slashes                       Query distribution validator slashes
  rewards                       Query all distribution delegator rewards or rewards from a particular validator
  community-pool                Query the amount of coins in the community pool
```

**Example**

- Query the parameters related to incentive distribution:

```
./cetcli query distr params --chain-id=coinexdex
Distribution Params:
  Community Tax:          "0.020000000000000000"
  Base Proposer Reward:   "0.010000000000000000"
  Bonus Proposer Reward:  "0.040000000000000000"
  Withdraw Addr Enabled:  true
```

- Query some validator's pending rewards which are waiting for delegators' withdrawal:

```
./cetcli query distr validator-outstanding-rewards  --chain-id=coinexdex
```

- Query some validator's commission:

```
./cetcli query distr commission coinexvaloper1zklle5dqe0njzulcaghvmmt83c0lmvn7cvw0j5 --chain-id=coinexdex
196023520000.000000000000000000cet
```

- Query some validator's slash events, given a range of height:

```
./cetcli query distr slashes  coinexvaloper1zklle5dqe0njzulcaghvmmt83c0lmvn7cvw0j5 1 100 --chain-id=coinexdex
Validator Slash Events:
```

- Query the total awards given to a delegator or a validator:

```
cetcli query distr rewards [delegator-addr] [<validator-addr>] [flags]
```

- Query the balance in community pool:

```
./cetcli query distr community-pool --chain-id=coinexdex
40004800000.000000000000000000cet
```



#### staking

Query staking-related information.

```
./cetcli query staking -h
Querying commands for the staking module

Usage:
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
  params                     Query the current staking parameters information
  pool                       Query the current staking pool values
```

**Example**

- Query the delegation between a delegator and a validator:

```
./cetcli query staking delegation -h
Query delegations for an individual delegator on an individual validator:

$ cetcli query staking delegation cosmos1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p cosmosvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj

Usage:
  cetcli query staking delegation [delegator-addr] [validator-addr] [flags]
```

- Query all the delegations of an address:

```
./cetcli query staking delegations -h
Query delegations for an individual delegator on all validators:

$ cetcli query staking delegations cosmos1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p

Usage:
  cetcli query staking delegations [delegator-addr] [flags]
```

- Query redelegation and redelegations, in similar ways.

```
./cetcli query staking redelegation -h
Query a redelegation record  for an individual delegator between a source and destination validator:

$ cetcli query staking redelegation cosmos1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p cosmosvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7fpx59wm cosmosvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj

Usage:
  cetcli query staking redelegation [delegator-addr] [src-validator-addr] [dst-validator-addr] [flags]
```

```
./cetcli query staking redelegations -h
Query all redelegation records for an individual delegator:

$ cetcli query staking redelegation cosmos1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p

Usage:
  cetcli query staking redelegations [delegator-addr] [flags]
```

- Query unbonding-delegation and unbonding-delegations, also in similar ways.
- Query a validator's information.

```
./cetcli query staking validator coinexvaloper1zklle5dqe0njzulcaghvmmt83c0lmvn7cvw0j5 --chain-id=coinexdex
Validator
  Operator Address:           coinexvaloper1zklle5dqe0njzulcaghvmmt83c0lmvn7cvw0j5
  Validator Consensus Pubkey: coinexvalconspub1zcjduepqwm5zaxxxzm66p0c9zwv4kcze38h4ctncg2ummuazvr6ynh0acamq7f3jtz
  Jailed:                     false
  Status:                     Bonded
  Tokens:                     100000000000000
  Delegator Shares:           100000000000000.000000000000000000
  Description:                {node0   }
  Unbonding Height:           0
  Unbonding Completion Time:  1970-01-01 00:00:00 +0000 UTC
  Minimum Self Delegation:    100000000000000
  Commission:                 rate: 0.100000000000000000, maxRate: 0.200000000000000000, maxChangeRate: 0.010000000000000000, updateTime: 2019-06-24 06:16:28.652322 +0000 UTC
```

- Query the validators

```
./cetcli query staking validators --chain-id=coinexdex
```

- Query all the delegations to a validator

```
./cetcli query staking delegations-to coinexvaloper1zklle5dqe0njzulcaghvmmt83c0lmvn7cvw0j5 --chain-id=coinexdex
Delegation:
  Delegator: coinex1zklle5dqe0njzulcaghvmmt83c0lmvn7rrd8uq
  Validator: coinexvaloper1zklle5dqe0njzulcaghvmmt83c0lmvn7cvw0j5
  Shares:    100000000000000.000000000000000000
```

- Query all the delegations which were unbonded from a validator and are in the unbonding queue.

```
./cetcli query staking unbonding-delegations-from -h
Query delegations that are unbonding _from_ a validator:

$ cetcli query staking unbonding-delegations-from cosmosvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj

Usage:
  cetcli query staking unbonding-delegations-from [validator-addr] [flags]
```

- Query all the delegations which were unbonded from a validator and are in the rebonding queue.

```
./cetcli query staking redelegations-from -h
Query delegations that are redelegating _from_ a validator:

$ cetcli query staking redelegations-from cosmosvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj

Usage:
  cetcli query staking redelegations-from [validator-addr] [flags]
```

- Query the parameters related to staking

```
./cetcli query staking params --chain-id=coinexdex
Params:
  Unbonding Time:    504h0m0s
  Max Validators:    42
  Max Entries:       7
  Bonded Coin Denom: cet
```

- Query the information of staking pool, i.e., all the staked tokens.

```
./cetcli query staking pool --chain-id=coinexdex
Pool:
  Loose Tokens:  9900000000000000
  Bonded Tokens: 100000000000000
  Token Supply:  10000000000000000
  Bonded Ratio:  0.010000000000000000
```



#### slashing

- Query a validator's evil behavior:

```
./cetcli query slashing signing-info coinexvalconspub1zcjduepqwm5zaxxxzm66p0c9zwv4kcze38h4ctncg2ummuazvr6ynh0acamq7f3jtz --chain-id=coinexdex
Start Height:          0
Index Offset:          2603
Jailed Until:          1970-01-01 00:00:00 +0000 UTC
Tombstoned:            false
Missed Blocks Counter: 0
```

- Query slashing-related parameters

```
./cetcli query slashing params --chain-id=coinexdex
Slashing Params:
  MaxEvidenceAge:          504h0m0s
  SignedBlocksWindow:      1000
  MinSignedPerWindow:      0.050000000000000000
  DowntimeJailDuration:    10m0s
  SlashFractionDoubleSign: 0.050000000000000000
  SlashFractionDowntime:   0.000100000000000000
```



#### gov

Query informations about governance.

```
./cetcli query gov -h
Querying commands for the governance module

Usage:
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
```

**Examples**

- Query a specific proposal:

```
./cetcli query gov proposal 1 --chain-id=coinexdex
Proposal 1:
  Title:              Test Proposal
  Type:               Text
  Status:             VotingPeriod
  Submit Time:        2019-06-25 09:47:03.446046 +0000 UTC
  Deposit End Time:   2019-07-09 09:47:03.446046 +0000 UTC
  Total Deposit:      1000000000020cet
  Voting Start Time:  2019-06-25 10:01:24.853022 +0000 UTC
  Voting End Time:    2019-07-09 10:01:24.853022 +0000 UTC
  Description:        My awesome proposal
```

- Query all the proposals, given a voter, a depositor or a status of the proposal.

```
./cetcli query gov proposals -h
Query for a all proposals. You can filter the returns with the following flags:

$ cetcli query gov proposals --depositor cosmos1skjwj5whet0lpe65qaq4rpq03hjxlwd9nf39lk
$ cetcli query gov proposals --voter cosmos1skjwj5whet0lpe65qaq4rpq03hjxlwd9nf39lk
$ cetcli query gov proposals --status (DepositPeriod|VotingPeriod|Passed|Rejected)

Usage:
  cetcli query gov proposals [flags]

Flags:
      --depositor string   (optional) filter by proposals deposited on by depositor
  -h, --help               help for proposals
      --indent             Add indent to JSON response
      --ledger             Use a connected Ledger device
      --limit string       (optional) limit to latest [number] proposals. Defaults to all proposals
      --node string        <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --status string      (optional) filter proposals by proposal status, status: deposit_period/voting_period/passed/rejected
      --trust-node         Trust connected full node (don't verify proofs for responses)
      --voter string       (optional) filter by proposals voted on by voted
```

- Query all the proposals:

```
./cetcli query gov proposals --chain-id=coinexdex
ID - (Status) [Type] Title
1 - (VotingPeriod) [Text] Test Proposal
```

- Query the vote from some account to a proposal:

```
./cetcli query gov vote 1 coinex1p43gzugn44znc2nnmf2psf7kkvtsy4mf40d7kk --chain-id=coinexdex
Voter coinex1p43gzugn44znc2nnmf2psf7kkvtsy4mf40d7kk voted with option Yes on proposal 1
```

- Query all the votes to a proposal:

```
./cetcli query gov votes  1 --chain-id=coinexdex
Votes for Proposal 1:
  coinex1p43gzugn44znc2nnmf2psf7kkvtsy4mf40d7kk: Yes
```

- Query the parameters related to voting, tallying and deposit:

```
./cetcli query gov param voting --chain-id=coinexdex
Voting Params:
  Voting Period:      336h0m0s
```

```
./cetcli query gov param tallying --chain-id=coinexdex
Tally Params:
  Quorum:             0.400000000000000000
  Threshold:          0.500000000000000000
  Veto:               0.334000000000000000
```

```
./cetcli query gov param deposit --chain-id=coinexdex
Deposit Params:
  Min Deposit:        1000000000000cet
  Max Deposit Period: 336h0m0s
```

- Query all the parameters:

```
./cetcli query gov params --chain-id=coinexdex
Voting Params:
  Voting Period:      336h0m0s
Tally Params:
  Quorum:             0.400000000000000000
  Threshold:          0.500000000000000000
  Veto:               0.334000000000000000
Deposit Params:
  Min Deposit:        1000000000000cet
  Max Deposit Period: 336h0m0s
```

- Query a proposal's proposer:

```
./cetcli query gov proposer 1 --chain-id=coinexdex
Proposal with ID 1 was proposed by coinex1zklle5dqe0njzulcaghvmmt83c0lmvn7rrd8uq
```

- Query the deposit to a proposal from an address:

```
./cetcli query gov deposit 1 coinex1p43gzugn44znc2nnmf2psf7kkvtsy4mf40d7kk --chain-id=coinexdex 
Deposit by coinex1p43gzugn44znc2nnmf2psf7kkvtsy4mf40d7kk on Proposal 1 is for the amount 10cet
```

- Query the deposits to a proposal:

```
/cetcli query gov deposits 1  --chain-id=coinexdex 
Deposits for Proposal 1:
  coinex1p43gzugn44znc2nnmf2psf7kkvtsy4mf40d7kk: 10cet
  coinex1zklle5dqe0njzulcaghvmmt83c0lmvn7rrd8uq: 1000000000010cet
```

- Query the tally results:

```
./cetcli query gov tally 1  --chain-id=coinexdex 
Tally Result:
  Yes:        0
  Abstain:    0
  No:         0
  NoWithVeto: 0
```



### The "cetcli tx" subcommand

The subcommands under "cetcli tx" are all used to generate, sign and broadcast transactions.

```
$ ./cetcli tx -h
Transactions subcommands

Usage:
  cetcli tx [command]

Available Commands:
  send         Create and sign a send tx
  require-memo Mark if memo is required to receive coins
              
  sign         Sign transactions generated offline
  multisign    Generate multisig signatures for transactions generated offline
  broadcast    Broadcast transactions generated offline
  encode       Encode transactions generated offline
              
  asset        Asset transactions subcommands
  market       market transactions subcommands
  gov          Governance transactions subcommands
  distr        Distribution transactions subcommands
  staking      Staking transaction subcommands
  slashing     Slashing transactions subcommands
  crisis       crisis transactions subcommands
```

The `sign`, `multisign`, `broadcast` and `encode` subcommands are used to implement a cold wallet, as is shown in following figure:

```
     <cmd>
   +--------------------------------------------------------------+
   |                                                              |
   | <cmd> --generate-only   +------+  sign   +------+  broadcast |     +------+
o--+-----------------------> | file | ------> | file | -----------+---> | Node |
                             +------+         +------+                  +------+
                                                  | encode   +--------+
                                                  +--------> | base64 |
                                                             +--------+
```

The normal flow is upper line, the generation, signing and broadcasting are all done by one cetcli program.

You can also use an online computer to run cetcli for generation and broadcasting a transaction, while using another off-line computer to run cetcli for signing the transaction. This off-line computer with the cetcli on it, is called "cold wallet". The flow is as the middle line of the above figure: 

1. The online computer generates a file, which is a json presentation of the transaction.
2. This file is transferred to the cold wallet, using a USB flash disk, QR code or something else.
3. The cold wallet signs the file and outputs a file containing the signed transaction.
4. The signed file is transferred back to the online computer, which will broadcast it to a full node.
5. The signed file can also be encoded using base64, then it can be posted to some websites which will help broadcasting it.

#### send

Send coins to other accounts:

```
$ ./cetcli tx send -h
Create and sign a send tx

Usage:
  cetcli tx send [to_address] [amount] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 100cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 100cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for send
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
      --unlock-time int         The unix timestamp when tokens can transfer again
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```



#### sign

```
$ ./cetcli tx sign -h
Sign transactions created with the --generate-only flag.
It will read a transaction from [file], sign it, and print its JSON encoding.

If the flag --signature-only flag is set, it will output a JSON representation
of the generated signature only.

If the flag --validate-signatures is set, then the command would check whether all required
signers have signed the transactions, whether the signatures were collected in the right
order, and if the signature is valid over the given transaction. If the --offline
flag is also set, signature validation over the transaction will be not be
performed as that will require RPC communication with a full node.

The --offline flag makes sure that the client will not reach out to full node.
As a result, the account and sequence number queries will not be performed and
it is required to set such parameters manually. Note, invalid values will cause
the transaction to fail.

The --multisig=<multisig_key> flag generates a signature on behalf of a multisig account
key. It implies --signature-only. Full multisig signed transactions may eventually
be generated via the 'multisign' command.

Usage:
  cetcli tx sign [file] [flags]

Flags:
  -a, --account-number uint      The account number of the signing account (offline mode only)
      --append                   Append the signature to the existing ones. If disabled, old signatures would be overwritten. Ignored if --multisig is on (default true)
  -b, --broadcast-mode string    Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                  ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string              Fees to pay along with transaction; eg: 100cet
      --from string              Name or address of private key with which to sign
      --gas string               gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float     adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string        Gas prices to determine the transaction fee (e.g. 100cet)
      --generate-only            Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                     help for sign
      --indent                   Add indent to JSON response
      --ledger                   Use a connected Ledger device
      --memo string              Memo to send along with transaction
      --multisig string          Address of the multisig account on behalf of which the transaction shall be signed
      --node string              <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --offline                  Offline mode; Do not query a full node
      --output-document string   The document will be written to the given file instead of STDOUT
      --print-response           return tx response (only works with async = false) (default true)
  -s, --sequence uint            The sequence number of the signing account (offline mode only)
      --signature-only           Print only the generated signature, then exit
      --trust-node               Trust connected full node (don't verify proofs for responses) (default true)
      --validate-signatures      Print the addresses that must sign the transaction, those who have already signed it, and make sure that signatures are in the correct order
  -y, --yes                      Skip tx broadcasting prompt confirmation

Global Flags: 省略
```



#### broadcast

```
$ ./cetcli tx broadcast -h
Broadcast transactions created with the --generate-only
flag and signed with the sign command. Read a transaction from [file_path] and
broadcast it to a node. If you supply a dash (-) argument in place of an input
filename, the command reads from standard input.

$ cetcli tx broadcast ./mytxn.json

Usage:
  cetcli tx broadcast [file_path] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 100cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 100cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for broadcast
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: <omitted>
```



#### encode

```
$ ./cetcli tx encode -h
Encode transactions created with the --generate-only flag and signed with the sign command.
Read a transaction from <file>, serialize it to the Amino wire protocol, and output it as base64.
If you supply a dash (-) argument in place of an input filename, the command reads from standard input.

Usage:
  cetcli tx encode [file] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 100cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 100cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for encode
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: <omitted>
```