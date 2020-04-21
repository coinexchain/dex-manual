# alias命令

Alias模块用来维护账户地址和别名的对应关系。

## tx命令

用于发起设置别名的交易。


```
$ cetcli tx alias -h
alias transactions subcommands

Usage:
  cetcli tx alias [command]

Available Commands:
  add         Add an alias for current account
  remove      Remove an alias for current account

Flags:
  -h, --help   help for alias

Global Flags:
      --chain-id string       Chain ID of tendermint node
      --default-http          Use Http as default schema
  -e, --encoding string       Binary encoding (hex|b64|btc) (default "hex")
      --home string           directory for config and data (default "/Users/a/.cetcli")
  -o, --output string         Output format (text|json) (default "text")
      --swagger-host string   Default host of swagger API
      --trace                 print out full stack trace on errors

Use "cetcli tx alias [command] --help" for more information about a command.

```

用户最多可以有MaxAliasCount个别名，参数MaxAliasCount的取值为5，通过投票可以修改。在这些别名当中，可以设置一个作为默认别名，也可以不设置默认别名。

### 添加别名

可以使用`add`子命令添加一个别名。

用法：

```
$./cetcli tx alias add -h
Add an alias for current account.

Example: 
	 cetcli tx alias add super_super_boy --from local_user_1 --as-default yes

Usage:
  cetcli tx alias add [alias] [flags]

```

主要选项：

| 选项名              | 类型（取值范围） | 是否必填 | 默认值 | 说明                                     |
| ------------------- | ---------------- | -------- | ------ | ---------------------------------------- |
| --as-default        | bool             | ✔        |        | 是否将此别名作为默认别名                 |

参数：

| 参数  | 类型   | 是否必填 | 默认值 | 说明   |
| ----- | ------ | -------- | ------ | ------ |
| 参数1 | string | ✔        |        | 别名   |

示例：

```
cetcli tx alias add superman --as-default yes --from coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
```

此命令为地址coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg增加一个“superman”的别名，并且将它设置为默认别名。如果“--as-default”选项的取值改为“no”，则它会被设置为非默认别名。当你把一个新的别名设置为默认别名时，旧的默认别名（如果有的话）就会自动变成非默认别名。

选项“--as-default”不但可以控制新设置的别名是不是默认别名，还可以让已有的别名在默认和非默认之间切换。在刚才的命令之后，如果再执行如下命令：

```
cetcli tx alias add superman --as-default no --from coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
```

那么就把默认别名superman改成了非默认别名。


### 删除别名

可以使用`remove`子命令删除一个别名。

用法：

```
$./cetcli tx alias remove -h
Remove an alias for current account.

Example: 
	 cetcli tx alias remove super_super_boy --from local_user_1

Usage:
  cetcli tx alias remove [alias] [flags]
```

参数：

| 参数  | 类型   | 是否必填 | 默认值 | 说明   |
| ----- | ------ | -------- | ------ | ------ |
| 参数1 | string | ✔        |        | 别名   |

示例：

```
cetcli tx alias remove superman --from coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
```

这条命令执行之后，superman就不再是账户coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg的别名了。

## query命令

用来进行别名相关的查询：

```
~ $ cetcli query alias -h
Querying commands for the alias module

Usage:
  cetcli query alias [command]

Available Commands:
  params             Query alias params
  aliases-of-address query the aliases of an address
  address-of-alias   query the corresponding address of an alias

Flags:
  -h, --help   help for alias

Global Flags:
      --chain-id string       Chain ID of tendermint node
      --default-http          Use Http as default schema
  -e, --encoding string       Binary encoding (hex|b64|btc) (default "hex")
      --home string           directory for config and data (default "/Users/a/.cetcli")
  -o, --output string         Output format (text|json) (default "text")
      --swagger-host string   Default host of swagger API
      --trace                 print out full stack trace on errors

Use "cetcli query alias [command] --help" for more information about a command.
```

### 查询alias模块参数

用法：

```
./cetcli query alias params -h
Query alias params

Usage:
  cetcli query alias params [flags]
```

示例：

```
$./cetcli query alias params --trust-node=true
{
  "fee_for_alias_length_2": "1000000000000",
  "fee_for_alias_length_3": "500000000000",
  "fee_for_alias_length_4": "200000000000",
  "fee_for_alias_length_5": "100000000000",
  "fee_for_alias_length_6": "10000000000",
  "fee_for_alias_length_7_or_higher": "1000000000",
  "max_alias_count": "5"
}
```

参数`fee_for_alias_length_*`描述了设置不同长度（从2到7甚至更长）的alias所需要支付的功能费，记得去掉八个零才是真实的花费数额。参数`max_alias_count`指定了一个账户所能拥有的别名数量。


### 根据别名查询账户

子命令address-of-alias查询某别名所对应的唯一账户。

用法：

```
$./cetcli query alias address-of-alias -h
query the corresponding address of an alias. 

Example : 
	cetcli query alias address-of-alias super_super_boy

Usage:
  cetcli query alias address-of-alias [alias] [flags]
```

参数：

| 参数  | 类型   | 是否必填 | 默认值 | 说明   |
| ----- | ------ | -------- | ------ | ------ |
| 参数1 | string | ✔        |        | 别名   |

示例：

```
~ $cetcli query alias address-of-alias coinex --chain-id=coinexdex
[
  "coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg"
]
```

经过以上命令的查询可知，“coinex”这个别名属于账户coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg

## 根据账户查询别名

子命令aliases-of-address查询账户所对应的若干别名。

用法：

```
$./cetcli query alias aliases-of-address -h
query the aliases of an address. 

Example : 
	cetcli query alias aliases-of-address coinex1qw508d6qejxtdg4y5r3zarvary0c5xw7kv8f3t4

Usage:
  cetcli query alias aliases-of-address [address] [flags]
```

参数：

| 参数  | 类型   | 是否必填 | 默认值 | 说明         |
| ----- | ------ | -------- | ------ | ------------ |
| 参数1 | string | ✔        |        | bech32地址   |

示例：

```
~ $cetcli query alias aliases-of-address coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
[
  "coinex",
  "coinex.com",
  "coinex.org"
]
```

经过以上命令的查询可知，账户coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg拥有三个别名"coinex", "coinex.com", 和"coinex.org"，其中，第一项“coinex”是默认别名。默认别名总是放在列表的第一项被返回。如果没有默认别名，第一项将是一个空字符串。

