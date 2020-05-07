# Crisis

## Tx

```BASH
Usage:
  cetcli tx crisis invariant-broken [module-name] [invariant-route] [flags]
```

This command initiate an invariant check in the specified module. If the invariant is broken, the chain will stop. In most of the cases, the reason of invariant breaking is bugs in the full node.
Initiating an invariant check needs a huge amount of feature fee (currently it is 100000CET).

The following invariants are supported now:

| module\_name  | invariant\_route         | function                                                     |
| ------------ | ----------------------- | ------------------------------------------------------------ |
| bancorlite   | bancor-info-consistency | bancor markets should have consistent status               |
| bank         | nonnegative-outstanding | all the accounts should have non-negative balances   |
| distribution | nonnegative-outstanding | all the validators non-withdrawn rewards should be non-negative |
| distribution | can-withdraw            | all the validators non-withdrawn rewards should be withdraw-able |
| distribution | reference-count         | the reference count of historical reward records must be correct |
| distribution | module-account          | The coins in the `distribution` module account, should be equal to all the validators' non-withdrawn rewards plus the coins in community pool |
| gov          | module-account          | The coins in the `gov` module account, should be equal to the sum of all the deposits of the proposals |
| staking      | module-accounts         | The total bonded(unbonded) coins should be equal to the sum of all the bonded(unbonded) coins of all the validators |
| staking      | nonnegative-power       | All the validators should have non-negative voting power    |
| staking      | positive-delegation     | All the validators should have positive delegation shares   |
| staking      | delegator-shares        | Each validator's delegations shares should be equal to the sum of its delegators' shares. |
| supply       | total-supply            | For each token, the total supply should be equal to the sum of tokens owned by accounts  |

Example：

```bash
cetcli tx crisis invariant-broken supply total-supply --from your_key_name --gas=30000 --fees=600000cet --chain-id=coinexdex
```

This command initiates the `total-supply` invariant check in `supply` module.

