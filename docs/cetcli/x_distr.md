# distribution命令

Distribution 相关命令用来处理与收益的提取、查询等操作。

## Query

### validator-outstanding-rewards

该子命令用来查询validator上的尚未提取的总收益（包括他自己的和所有的delegator的）。

```bash
Example：
cetcli q distribution validator-outstanding-rewards coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --chain-id=coinexdex
- denom: cet
  amount: "24290734698440.431229509097869021"
```

以上命令查询了dex主网上的ViaWallet验证人的所有未提取收益。

### commission

```bash
Example:
cetcli q distribution commission coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --chain-id=coinexdex
- denom: cet
  amount: "14698237761160.677398821199727054"
```

该命令查询指定validator自己尚未提取的commission收益。

### slashes

```bash
Example:
cetcli q distribution slashes coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx 0 100 --chain-id=coinexdex
```

该命令查询指定validator在指定高度区间（0-100）之间所有的slash事件。

### rewards

```BASH
Usage:
  cetcli query distribution rewards [delegator-addr] [<validator-addr>] [flags]
```

该命令用来查询delegator所有的或者对应于一个validator的尚未提取的收益，validator-addr参数是可选的。在不指定validator-addr的情况下，查询delegator所有的质押收益。否则，查询对特定validator的质押收益。

```BASH
cetcli q distribution rewards coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv --chain-id=coinexdex
rewards:
- validator_address: coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx
  reward:
  - denom: cet
    amount: "37226617.249081969349257470"
total:
- denom: cet
  amount: "37226617.249081969349257470"nexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx
```

指定validator-addr参数的命令如下：

```BASH
cetcli q distribution rewards coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --chain-id=coinexdex
- denom: cet
  amount: "37227967.372295219595678570"
```

### community-pool

```bash
cetcli q distribution community-pool --chain-id=coinexdex
- denom: cet
  amount: "38114146960448.248544706943390354"
```

该子命令用来查询community-pool中币的总量。

### params

```bash
cetcli q distribution params --chain-id=coinexdex --output json

{"community_tax":"0.020000000000000000","base_proposer_reward":"0.010000000000000000","bonus_proposer_reward":"0.040000000000000000","withdraw_addr_enabled":true}
```

该命令用来查询distribution模块的相应参数。community_tax表示当前的社区税比例。base_proposer_reward表示proposer在打包区块时，能够获得的基本propose奖励。validator能够获得的额外的propose奖励则等于bonus_proposer_reward乘以包含的validator投票power占总投票power的比例。withdraw_addr_enabled表示当前是否允许通过发交易来设置自己的withdraw_addr。

## Tx

### Withdraw-rewards

```BASH
Usage:
  cetcli tx distribution withdraw-rewards [validator-addr] [flags]

```

withdraw-rewards命令主要用来提取用户(--from指定的)在给定validtor上的质押收益。需要指定一个参数即validator的operator地址（以"coinexvaloper"作为前缀的地址）。

例如：

```bash
cetcli tx distribution withdraw-rewards coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --from your_key --gas=30000 --fees=600000cet --chain-id=coinexdex
```

运行完此命令后，即可将delegator在指定validator上的收益全部提取。

当--from来自一个validator用户时，如果指定--commission，则在提取validator的质押收益的同时，也提取他自己的佣金收益。

例如：

```bash
cetcli tx distr withdraw-rewards coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --from your_key_name --commission
--gas=30000 --fees=600000cet --chain-id=coinexdex
```

### set-withdraw-addr

此命令用来设置delegator相关的收益提取地址。

```bash
Usage:
  cetcli tx distribution set-withdraw-addr [withdraw-addr] [flags]
Example:
cetcli tx set-withdraw-addr your_addr --from your_key --chain-id=coinexdex

```

示例命令用来设置从your_key作为delegator的收益提取地址your_addr，设置了该地址之后，提取的收益将会分发到your_addr中。

### withdraw-all-rewards

该命令用来提取delegator所有的delegation收益。

```bash
Usage:
  cetcli tx distribution withdraw-all-rewards [flags]
Example:
  cetcli tx distribution withdraw-all-rewards --from your_key --gas=30000 --fees=600000cet --chain-id=coinexdex
```



