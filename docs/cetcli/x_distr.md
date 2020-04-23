# distribution命令

Distribution相关命令用来处理与收益的提取、查询等操作。



## tx命令

用法：

```
$ cetcli tx distribution -h
Distribution transactions subcommands

Usage:
  cetcli tx distribution [flags]
  cetcli tx distribution [command]

Available Commands:
  withdraw-rewards     Withdraw rewards from a given delegation address, and optionally withdraw validator commission if the delegation address given is a validator operator
  set-withdraw-addr    change the default withdraw address for rewards associated with an address
  withdraw-all-rewards withdraw all delegations rewards for a delegator

Flags: 省略
Global Flags: 省略
```



### 设置收益地址

此命令用来设置delegator相关的收益提取地址，用法：

```bash
$ cetcli tx distribution set-withdraw-addr -h
Set the withdraw address for rewards associated with a delegator address.

Example:
	cetcli tx set-withdraw-addr coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r --from mykey

Usage:
  cetcli tx distribution set-withdraw-addr [withdraw-addr] [flags]

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
  -h, --help                    help for set-withdraw-addr
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation
```

示例命令用来设置从your_key作为delegator的收益提取地址your_addr，设置了该地址之后，提取的收益将会分发到your_addr中。参数：

| 参数  | 参数名        | 类型（取值范围） | 是否必填 | 默认值 | 说明     |
| ----- | ------------- | ---------------- | -------- | ------ | -------- |
| 参数1 | withdraw-addr | string           | ✔        |        | 收益地址 |

选项和例子见上面的命令帮助。



### 从某验证者处提取收益

此命令主要用来提取用户(`--from`指定的)在给定validtor上的质押收益。需要指定一个参数即validator的operator地址（以"coinexvaloper"作为前缀的地址）。用法：

```BASH
$ cetcli tx distribution withdraw-rewards -h
Withdraw rewards from a given delegation address,
and optionally withdraw validator commission if the delegation address given is a validator operator.

Example:
	cetcli tx distr withdraw-rewards coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th --from mykey
	cetcli tx distr withdraw-rewards coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th --from mykey --commission

Usage:
  cetcli tx distribution withdraw-rewards [validator-addr] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --commission              also withdraw validator's commission
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for withdraw-rewards
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation
```

参数（选项见上面的命令帮助）：

| 参数  | 参数名         | 类型（取值范围） | 是否必填 | 默认值 | 说明       |
| ----- | -------------- | ---------------- | -------- | ------ | ---------- |
| 参数1 | validator-addr | string           | ✔        |        | 验证人地址 |

例1：

```bash
$ cetcli tx distribution withdraw-rewards \
	coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx \
	--from your_key --gas=30000 --fees=600000cet --chain-id=coinexdex
```

运行完此命令后，即可将delegator在指定validator上的收益全部提取。当`--from`来自一个validator用户时，如果指定`--commission`，则在提取validator的质押收益的同时，也提取他自己的佣金收益。例如：

```bash
$ cetcli tx distr withdraw-rewards coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx \
	--commission --from your_key_name --gas=30000 --fees=600000cet --chain-id=coinexdex
```



### 提取全部收益

该命令用来提取delegator所有的delegation收益。

```bash
Usage:
  cetcli tx distribution withdraw-all-rewards [flags]
Example:
  cetcli tx distribution withdraw-all-rewards --from your_key --gas=30000 --fees=600000cet --chain-id=coinexdex
```



## query命令

用法：

```
$ cetcli query distribution -h
Querying commands for the distribution module

Usage:
  cetcli query distribution [flags]
  cetcli query distribution [command]

Available Commands:
  params                        Query distribution params
  validator-outstanding-rewards Query distribution outstanding (un-withdrawn) rewards for a validator and all their delegations
  commission                    Query distribution validator commission
  slashes                       Query distribution validator slashes
  rewards                       Query all distribution delegator rewards or rewards from a particular validator
  community-pool                Query the amount of coins in the community pool

Flags: 省略
Global Flags: 省略
```



### params

```bash
$ cetcli q distribution params --chain-id=coinexdex --output json

{"community_tax":"0.020000000000000000","base_proposer_reward":"0.010000000000000000","bonus_proposer_reward":"0.040000000000000000","withdraw_addr_enabled":true}
```

该命令用来查询distribution模块的相应参数。community_tax表示当前的社区税比例。base_proposer_reward表示proposer在打包区块时，能够获得的基本propose奖励。validator能够获得的额外的propose奖励则等于bonus_proposer_reward乘以包含的validator投票power占总投票power的比例。withdraw_addr_enabled表示当前是否允许通过发交易来设置自己的withdraw_addr。



### validator-outstanding-rewards

该子命令用来查询validator上的尚未提取的总收益（包括他自己的和所有的delegator的）。用法：

```
$ cetcli query distribution validator-outstanding-rewards -h
Query distribution outstanding (un-withdrawn) rewards
for a validator and all their delegations.

Example:
	cetcli query distr validator-outstanding-rewards coinexvaloper1lwjmdnks33xwnmfayc64ycprww49n33m78rj3u

Usage:
  cetcli query distribution validator-outstanding-rewards [validator] [flags]

Flags: 省略
Global Flags: 省略
```

参数（选项见命令帮助）：

| 参数  | 参数名         | 类型（取值范围） | 是否必填 | 默认值 | 说明       |
| ----- | -------------- | ---------------- | -------- | ------ | ---------- |
| 参数1 | validator-addr | string           | ✔        |        | 验证人地址 |

例1，查询dex主网上ViaWallet验证人的所有未提取收益：

```bash
$ cetcli q distribution validator-outstanding-rewards coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --chain-id=coinexdex
- denom: cet
  amount: "24290734698440.431229509097869021"
```



### commission

该命令查询指定validator自己尚未提取的commission收益，用法：

```
$ cetcli query distribution commission -h
Query validator commission rewards from delegators to that validator.

Example:
	cetcli query distr commission coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query distribution commission [validator] [flags]

Flags: 省略
Global Flags: 省略
```

参数（选项见命令帮助）：

| 参数  | 参数名         | 类型（取值范围） | 是否必填 | 默认值 | 说明       |
| ----- | -------------- | ---------------- | -------- | ------ | ---------- |
| 参数1 | validator-addr | string           | ✔        |        | 验证人地址 |

例1，

```bash
$ cetcli q distribution commission coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --chain-id=coinexdex
- denom: cet
  amount: "14698237761160.677398821199727054"
```



### slashes

该命令查询指定validator在指定高度区间（0-100）之间所有的slash事件，用法：

```
$ cetcli query distribution slashes -h
Query all slashes of a validator for a given block range.

Example:
	cetcli query distr slashes coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th 0 100

Usage:
  cetcli query distribution slashes [validator] [start-height] [end-height] [flags]

Flags: 省略
Global Flags: 省略
```

参数（选项见命令帮助）：

| 参数  | 参数名         | 类型（取值范围） | 是否必填 | 默认值 | 说明       |
| ----- | -------------- | ---------------- | -------- | ------ | ---------- |
| 参数1 | validator-addr | string           | ✔        |        | 验证人地址 |
| 参数2 | start-height   | int              | ✔        |        | 开始高度   |
| 参数3 | end-height     | int              | ✔        |        | 结束高度   |

例1：

```bash
$ cetcli q distribution slashes \
	coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx 0 100 \
	--chain-id=coinexdex
```



### rewards

该命令用来查询delegator所有的或者对应于一个validator的尚未提取的收益，validator-addr参数是可选的。在不指定validator-addr的情况下，查询delegator所有的质押收益。否则，查询对特定validator的质押收益。用法：

```
$ cetcli query distribution rewards -h
Query all rewards earned by a delegator, optionally restrict to rewards from a single validator.

Example:
	cetcli query distr rewards coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r
	cetcli query distr rewards coinex1gghjut3ccd8ay0zduzj64hwre2fxs9ld4nje9r coinexvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldwu33th

Usage:
  cetcli query distribution rewards [delegator-addr] [<validator-addr>] [flags]

Flags: 省略
Global Flags: 省略
```

参数（选项见命令帮助）：

| 参数  | 参数名         | 类型（取值范围） | 是否必填 | 默认值 | 说明          |
| ----- | -------------- | ---------------- | -------- | ------ | ------------- |
| 参数1 | delegator-addr | string           | ✔        |        | delegator地址 |
| 参数2 | validator-addr | string           |          |        | 验证人地址    |

例1，不指定validator-addr参数：

```BASH
$ cetcli q distribution rewards coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv --chain-id=coinexdex
rewards:
- validator_address: coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx
  reward:
  - denom: cet
    amount: "37226617.249081969349257470"
total:
- denom: cet
  amount: "37226617.249081969349257470"nexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx
```

例2，指定validator-addr参数：

```BASH
$ cetcli q distribution rewards coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --chain-id=coinexdex
- denom: cet
  amount: "37227967.372295219595678570"
```



### community-pool

该子命令用来查询community-pool中币的总量，用法：

```
$ cetcli query distribution community-pool -h
Query all coins in the community pool which is under Governance control.

Example:
	cetcli query distr community-pool

Usage:
  cetcli query distribution community-pool [flags]

Flags: 省略
Global Flags: 省略
```

选项见命令帮助，例：

```bash
$ cetcli q distribution community-pool --chain-id=coinexdex
- denom: cet
  amount: "38114146960448.248544706943390354"
```


