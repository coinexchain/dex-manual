# alias命令

`alias`命令用来维护账户地址和别名的对应关系。

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

## 添加别名

可以使用`add`子命令添加一个别名：

```
cetcli tx alias add superman --as-default yes --from coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
```

此命令为地址coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg增加一个“superman”的别名，并且将它设置为默认别名。如果“--as-default”选项的取值改为“no”，则它会被设置为非默认别名。一个用户最多可以拥有5个别名，其中最多只能有一个默认别名（可以没有）。当你把一个新的别名设置为默认别名时，旧的默认别名（如果有的话）就会自动变成非默认别名。

选项“--as-default”不但可以控制新设置的别名是不是默认别名，还可以让已有的别名在默认和非默认之间切换。在刚才的命令之后，如果再执行如下命令：

```
cetcli tx alias add superman --as-default no --from coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
```

那么就把默认别名superman改成了非默认别名。

## 删除别名

可以使用`remove`子命令删除一个别名：

```
cetcli tx alias remove superman --from coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
```

这条命令执行之后，superman就不再是账户coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg的别名了。

## 根据别名查询账户

子命令address-of-alias查询某别名所对应的唯一账户：

```
~ $cetcli query alias address-of-alias coinex --chain-id=coinexdex
[
  "coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg"
]
```

经过以上命令的查询可知，“coinex”这个别名属于账户coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg

## 根据账户查询别名

子命令aliases-of-address查询账户所对应的若干别名：

```
~ $cetcli query alias aliases-of-address coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
[
  "coinex",
  "coinex.com",
  "coinex.org"
]
```

经过以上命令的查询可知，账户coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg拥有三个别名"coinex", "coinex.com", 和"coinex.org"，其中，第一项“coinex”是默认别名。默认别名总是放在列表的第一项被返回。如果没有默认别名，第一项将是一个空字符串。

