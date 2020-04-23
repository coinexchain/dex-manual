# slashing命令

## query

### signing-info

Signing-info 子命令用来查询指定validator的签名信息（主要是和slashing相关的）：

```bash
Usage:
  cetcli query slashing signing-info [validator-conspub] [flags]
```

输入一个参数，指定validator的共识公钥（也就是validator用来做底层共识的ed25519公钥），可以通过q staking validator命令查询：

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

以上命令以ViaWallet为例，查询了该validator的相关信息。其中conspubkey字段即是我们需要的公钥，以其为参数再进行signing-info查询：

```bash
 cetcli q slashing signing-info coinexvalconspub1zcjduepqs5s5c7lj4dfuszvze3kqgeh8l36zeht4tv7uhrlphxpmldsy9tssmp4js9 --chain-id=coinexdex
 
address: coinexvalcons1am5738tz87xzsu0stlh6pfpzgapj0neqj4xsw3
start_height: 0
index_offset: 503571
jailed_until: 1970-01-01T00:00:00Z
tombstoned: false
missed_blocks_counter: 0
```

查询结果中，第一个地址为validator的共识地址。start_height为他上次被slash并主动unjail自己的区块高度。index_offset为当前高度相对于起始高度的偏移量。jailed_until字段表示validator在被关监狱后，最早的出狱时间，`1970-01-01T00:00:00Z`则代表没有被关监狱。tombstoned字段标识validator是否因为双签而被送进坟墓了。missed_blocks_counter记录了validator在这个计算周期内，未签名的区块总数。

### params

该命令用来查询slashing模块的相关参数：

```BASH
cetcli q slashing params --chain-id=coinexdex

max_evidence_age: 504h0m0s
signed_blocks_window: 10000
min_signed_per_window: "0.050000000000000000"
downtime_jail_duration: 10m0s
slash_fraction_double_sign: "0.050000000000000000"
slash_fraction_downtime: "0.000100000000000000"
```

Max_evidence_age表示举证证据的有效期。signed_blocks_window表示统计validator可用性的滑动窗口大小。min_signed_per_window表示在滑动窗口大小内，validator需要签名的最小区块比例（否则就会因可用性差被关监狱）。downtime_jail_duration表示因为可用性差被关监狱的时长。slash_fraction_double_sign表示因双签被惩罚的资金比例。slash_fraction_downtime表示因可用性差被惩罚的资金比例。

## tx

### unjail

```bash
$ cetcli tx slashing unjail --from mykey

Usage:
  cetcli tx slashing unjail [flags]

```

该命令用来将一个被关监狱的validator从监狱中释放出来。该交易只能由validator自己发送。注意：因双签被送进坟墓的validator不能通过发送此交易被释放。因可用性差被关监狱的validator只有在度过downtime_jail_duration了之后，才能通过发送该交易来释放自己。



