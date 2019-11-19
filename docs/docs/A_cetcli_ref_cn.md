# CETCLI参考手册

CETCLI是与CoinEx链交互的命令行工具。



* 下载CETCLI
* 基本用法
  * 版本信息
  * 获取帮助
  * 通用选项

* 子命令参考
  * [管理配置文件](../cetcli/config.md)
  * [管理地址和密钥](../cetcli/keys.md)
  * [发送交易](../cetcli/tx.md)
  * [查询信息](../cetcli/query.md)
  * 查询节点状态

  

## 下载CETCLI

可以从这里下载预编译好的CETCLI二进制可执行文件（目前仅支持64位的Linux和macOS操作系统）：

* 测试网：https://github.com/coinexchain/testnets
* 主网：https://github.com/coinexchain/artifacts/tree/master/coinexdex



## 基本用法

根据平台选择相应的CETCLI二进制文件，下载到指定目录，然后即可从命令行执行：

```
$ # download cetcli
$ ./cetcli
Command line interface for interacting with cetd

Usage:
  cetcli [command]

Available Commands:
  status      Query remote node for status
  config      Create or query an application CLI configuration file
  query       Querying subcommands
  tx          Transactions subcommands
              
  rest-server Start LCD (light-client daemon), a local REST server
              
  keys        Add or view local private keys
              
  version     Print the app version
              
  help        Help about any command

Flags:
      --chain-id string       Chain ID of tendermint node
      --default-http          Use Http as default schema
  -e, --encoding string       Binary encoding (hex|b64|btc) (default "hex")
  -h, --help                  help for cetcli
      --home string           directory for config and data (default "~/.cetcli")
  -o, --output string         Output format (text|json) (default "text")
      --swagger-host string   Default host of swagger API
      --trace                 print out full stack trace on errors
```

CETCLI提供了丰富的功能，不同的功能分别由不同的**子命令**提供。这些子命令，按**命令树**的形式进行组织。CETCLI是**根命令**，它提供了若干子命令，某个子命令可能又包含若干子命令，以此类推。



### 版本信息

使用CETCLI时一定要注意版本，测试网CETCLI**只能**操作测试网，主网CETCLI**只能**操作主网。如果使用测试网的CETCLI操作主网（或者反之），或者使用了错误的版本，都有可能导致命令执行失败。通过`cetcli version`子命令可以查看CETCLI的版本号，搭配`--long`选项可以查看更详细的版本信息。以主网为例：

```
$ ./cetcli version --long
name: CoinEx Chain
server_name: cetd
client_name: cetcli
version: 0.1.0
commit: c73377e7d95fe741a138caf8527dfad1247fce4c
build_tags: netgo,ledger,libsecp256k1
go: go version go1.13.1 darwin/amd64
```

如果是测试网，那么在`build_tags`里会出现`testnet`字样。



### 获取帮助

CETCLI各个子命令都提供了非常详细的帮助信息，可以通过下面几种方式获取帮助信息：

* `cetcli subcmd subcmd` 不带任何选项执行命令
* `cetcli subcmd subcmd -h` 通过`-h`选项执行命令
* `cetcli subcmd subcmd --help` 通过`--help`选项执行命令
* `cetcli help subcmd subcmd` 通过`help`子命令查看帮助

以`version`命令搭配`-h`选项为例：

```
h$ ./cetcli version -h
Print the app version

Usage:
  cetcli version [flags]

Flags:
  -h, --help   help for version
      --long   Print long version information

Global Flags:
      --chain-id string       Chain ID of tendermint node
      --default-http          Use Http as default schema
  -e, --encoding string       Binary encoding (hex|b64|btc) (default "hex")
      --home string           directory for config and data (default "~/.cetcli")
  -o, --output string         Output format (text|json) (default "text")
      --swagger-host string   Default host of swagger API
      --trace                 print out full stack trace on errors
```



### 通用选项

有些选项是大部分CETCLI子命令都支持的，这里对这些选项进行统一说明。



#### home选项 

有些CETCLI子命令需要访问本地文件，比如说`keys`子命令可以用本地文件来存放密钥信息，`tx`等命令也可能需要访问这些密钥来对交易进行签名。CETCLI使用统一的目录来存放这些文件，这个目录就是CETCLI的home目录。默认情况下，home目录的位置是`$HOME/.cetcli/`。如果希望指定不同的目录，可以使用`--home`选项来覆盖默认值。例如：

```
$ ./cetcli keys add bob --home path/to/my/home
$ ls -lF path/to/my/home
config/
keys/
```



#### node选项

CETCLI通过RPC接口和CoinEx链全节点进行交互，因此必须知道全节点的RPC地址和端口。全节点的RPC地址和端口通过`--node`选项指定，如果不指定，则取默认值`tcp://localhost:26657`。例如：

```
./cetcli status --node=47.252.23.106:26657
```



#### chain-id选项

TODO



#### output选项

TODO

