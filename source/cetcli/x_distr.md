# Distribution 

To withdraw and query the staking rewards, you need the sub commands under `distribution`.

## Tx

### Withdraw-rewards

`withdraw-rewards` sub command withdraws the staking rewards between a delegator-validator pair.

```BASH
Usage:
  cetcli tx distribution withdraw-rewards [validator-addr] [flags]

```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | validator-addr  | string | ✔        |               | The validator's address    |

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------  |
| --from           | string           | ✔        |               |  The delegator's address |
| --commission     | bool             |          |               |  Withdraw the staking reward and commission at the same time (when the delegator is also a validator.) |


Example 1: withdraw bob's rewards at the validator coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx.

```bash
cetcli tx distribution withdraw-rewards coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --from bob --gas=30000 --fees=600000cet --chain-id=coinexdex
```

Example 2: withdraw both bob's rewards at the validator coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx, and his commission as a validator.

```bash
cetcli tx distr withdraw-rewards coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --from bob --commission
--gas=30000 --fees=600000cet --chain-id=coinexdex
```

### set-withdraw-addr

This command sets where the reward goes to when you withdraw them.

Usage:

```bash
Usage:
  cetcli tx distribution set-withdraw-addr [withdraw-addr] [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | withdraw-addr   | string | ✔        |               | The address where reward goes to    |

Example:

```bash
cetcli tx set-withdraw-addr coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --from bob --chain-id=coinexdex
```

This command sets bob's withdraw-addr to coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg. After that, each time bob withdraws his rewards, the rewards go into the account coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg.

### withdraw-all-rewards

The command can withdraw all the rewards of a delegator, at different validators.

```bash
Usage:
  cetcli tx distribution withdraw-all-rewards [flags]
Example:
  cetcli tx distribution withdraw-all-rewards --from bob --gas=30000 --fees=600000cet --chain-id=coinexdex
```



## Query

### validator-outstanding-rewards

This sub command queries all the outstanding (not-withdrawn) rewards at a validator, including the rewards of self-delegation and the rewards of other delegators.

Example：

```bash
cetcli q distribution validator-outstanding-rewards coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --chain-id=coinexdex
- denom: cet
  amount: "24290734698440.431229509097869021"
```

This command queries all the outstanding rewards at the ViaWallet validator (whose address is coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx).

### commission

This command queries the commission of a validator.

Example:

```bash
cetcli q distribution commission coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --chain-id=coinexdex
- denom: cet
  amount: "14698237761160.677398821199727054"
```


### slashes

Usage:

```bash
cetcli q distribution slashes [validator-addr] [start-height] [end-height] [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | validator-addr  | string | ✔        |               | the validator to be queried |
| Parameter 2 | start-height    | int    | ✔        |               | the start of the range to be queried |
| Parameter 3 | end-height      | int    | ✔        |               | the end of the range to be queried |

Example:

```bash
cetcli q distribution slashes coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx 0 100 --chain-id=coinexdex
```

This command queries all the slash events of the validator coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx, between height 0 and 1000.

### rewards

This command queries the rewards of a delegator, at one validator or all the validators.

Usage:

```BASH
  cetcli query distribution rewards [delegator-addr] [<validator-addr>] [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | delegator-addr  | string | ✔        |              | the delegator to be queried |
| Parameter 2 | validator-addr  | string |          |               | the validator to be queried |


If validator-addr is specified, then only the rewards at this validator is returned. If it's omitted, then return all the rewards at all the validators that this delegator delegates to.

Example 1: omit the `validator-addr` parameter.

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

Example 2: specify the `validator-addr` parameter.

```BASH
cetcli q distribution rewards coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --chain-id=coinexdex
- denom: cet
  amount: "37227967.372295219595678570"
```

### community-pool

This sub command queries the amount of coins in community pool.

```bash
cetcli q distribution community-pool --chain-id=coinexdex
- denom: cet
  amount: "38114146960448.248544706943390354"
```

### params

This sub command queries the parameters in the distribution module.

```bash
cetcli q distribution params --chain-id=coinexdex --output json

{"community_tax":"0.020000000000000000","base_proposer_reward":"0.010000000000000000","bonus_proposer_reward":"0.040000000000000000","withdraw_addr_enabled":true}
```

The meaning of the parameters are:
1. community\_tax: The rate of community tax. 
2. base\_proposer\_reward: The base rate of proposer reward.
3. bonus\_proposer\_reward: The bonus rate of proposer reward, if the proposer collects more than 2/3 voting power.
4. withdraw\_addr\_enabled: Whether withdraw-addr can be changed by issuing transactions.

The algorithm to distribute the block rewards and gas fees are as below:

```
	P = the voting power of the votes collected by the proposer
	V = total voting power
	M = all the block rewards and gas fees gathered at a given height
	ct = M * community_tax
	M = M - ct
	send ct to community pool
	proposer_reward = base_proposer_reward + bonus_proposer_reward * (P - V*2/3) / P
	p = M * proposer_reward
	M = M - p
	send p to the proposer
	distrubte M among all the validators who vote for this block, in direct proportion to their voting power
```
