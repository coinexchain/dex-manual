# Slashing

## tx

### unjail

```bash
$ cetcli tx slashing unjail --from mykey

Usage:
  cetcli tx slashing unjail [flags]

```

The `unjail` command releases a validator from jail. Only the jailed validator can use this command to unjail itself.
Please note a validator who was jailed because of double-signing can not use this command to unjail itself.
A vaalidator who was jailed because of poor availability can only unjail itself after `downtime_jail_duration`.


## query

### signing-info

`signing-info` queries a particular validator's signing information related to slashing events.

```bash
Usage:
  cetcli query slashing signing-info [validator-conspub] [flags]
```

Parameter:

| Parameter   | Name               | Type   | Required | Default Value | Comment                        |
| ------------| ------------------ | ------ | -------- | ------------  | ------------------------------ |
| Parameter 1 | validator-conspub  | string | ✔        |               | validator's ed25519 public key |

To get a validator's ed25519 public key, you can use the `cetcli q staking validator` command.

```BASH
cetcli q staking validator coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx --chain-id=coinexdex

  operatoraddress: coinexvaloper120e8m9myace3ud4uw5mk30etyx0jnpptgsnclx
  conspubkey: coinexvalconspub1zcjduepqs5s5c7lj4dfuszvze3kqgeh8l36zeht4tv7uhrlphxpmldsy9tssmp4js9
  jailed: false
  status: 2
  tokens: "5062422790541992"
  delegatorshares: "5062422790541992.000000000000000000"
  description:
    moniker: ViaWallet
    identity: 9A30CBDA5872CED8
    website: wallet.viabtc.com
    details: Multi-crypto currency wallet Powered by ViaBTC
  unbondingheight: 0
  unbondingcompletiontime: 1970-01-01T00:00:00Z
  commission:
    commission_rates:
      rate: "0.100000000000000000"
      max_rate: "0.500000000000000000"
      max_change_rate: "0.100000000000000000"
    update_time: 2019-11-10T10:11:41.370802956Z
  minselfdelegation: "500000000000000"

```

The above example queries the validator ViaWallet's information. The conspubkey field in the query result is what we need. We can use it for `signing-info` query.

```bash
 cetcli q slashing signing-info coinexvalconspub1zcjduepqs5s5c7lj4dfuszvze3kqgeh8l36zeht4tv7uhrlphxpmldsy9tssmp4js9 --chain-id=coinexdex
 
address: coinexvalcons1am5738tz87xzsu0stlh6pfpzgapj0neqj4xsw3
start_height: 0
index_offset: 503571
jailed_until: 1970-01-01T00:00:00Z
tombstoned: false
missed_blocks_counter: 0
```

In the query result:
1. address is the validator's consensus address
2. start\_height is the latest block height at which it unjailed itself. start\_heig==0 means it was never jailed.
3. index\_offset is the difference between current height and start\_height.
4. jailed\_until is the earliest possible time to unjail itself. If the validator is not jailed now, it would be `1970-01-01T00:00:00Z`.
5. tombstoned shows whether the validator was sent to tomb because of double-signing.
6. missed\_blocks\_counter records in current counting period, the count of missed blocks (blocks that are not voted by this validator)

### params

To query the parameters of slashing module.

```BASH
cetcli q slashing params --chain-id=coinexdex

max_evidence_age: 504h0m0s
signed_blocks_window: 10000
min_signed_per_window: "0.050000000000000000"
downtime_jail_duration: 10m0s
slash_fraction_double_sign: "0.050000000000000000"
slash_fraction_downtime: "0.000100000000000000"
```

The parameters' meaning are:
1. A double-signing evidence is valid util `max_evidence_age` after the occurrence time.
2. `signed_blocks_window` is the size of sliding window within which the missed blocks are counted.
3. `min_signed_per_window` is the minimum signed blocks required within the sliding window. If a validator signed less blocks than its value, it will be slashed because of poor availability.
4. `downtime_jail_duration` is the minimum time during which a validator must stay in jail.
5. `slash_fraction_double_sign` is the fraction to be slashed because of double sign.
6. `slash_fraction_downtime` is the fraction to be slashed because of poor availability.

