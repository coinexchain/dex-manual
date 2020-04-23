# crisis命令



## tx命令

该命令用来发送一条检查指定模块指定不变量的检查，用来检查该模块的指定不变量到现在为止是否被破坏。需要注意的是，该交易除了gas费之外，需要从发起的账户扣除一定功能费（当前coinex主网默认为100000CET）。用法：

```
$ cetcli tx crisis -h
Crisis transactions subcommands

Usage:
  cetcli tx crisis [flags]
  cetcli tx crisis [command]

Available Commands:
  invariant-broken submit proof that an invariant broken to halt the chain

Flags: 省略
Global Flags: 省略
```

目前支持的不变量有：

| module_name  | invariant_route         | function                                                     |
| ------------ | ----------------------- | ------------------------------------------------------------ |
| bancorlite   | bancor-info-consistency | 对所有创建了bancor市场的交易对状态做基本的检查               |
| bank         | nonnegative-outstanding | 检查所有账户的coins有没有出现负值。                          |
| distribution | nonnegative-outstanding | 检查所有validator的未提取收益是否有负的金额。                |
| distribution | can-withdraw            | 检查所有validator的未提取收益是否能被完全提取。              |
| distribution | reference-count         | 检查distribution内历史收益记录的引用计数是否正确。           |
| distribution | module-account          | 检查distribution的module account中的coins总量是否与所有validator的未提取收益和communitypool中的coins之和一致。 |
| gov          | module-account          | 检查gov的module account中的coins总量是否与所有proposal的质押总量一致。 |
| staking      | module-accounts         | 检查staking的bonded coins与unbonded coins总量是否与所有validator上的相应tokens总量一致。 |
| staking      | nonnegative-power       | 检查系统中的所有validator的质押tokens是否有负值。            |
| staking      | positive-delegation     | 检查所有validator的delegation的shares是否为正值。            |
| staking      | delegator-shares        | 检查validator上的所有delegations的shares总量是否与validator的shares总量一致。 |
| supply       | total-supply            | 检查系统中所有已发行币的总量是否与所有账户的币总量一致。     |

例1，检查supply模块的total-supply不变量是否被破坏：

```bash
$ cetcli tx crisis invariant-broken supply total-supply \
	--from your_key_name --gas=30000 --fees=600000cet --chain-id=coinexdex
```

