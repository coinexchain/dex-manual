# auth命令



## query

query命令用来查询与auth相关的状态：

```bash
cetcli query auth -h
Querying commands for the auth module

Usage:
  cetcli query auth [command]

Available Commands:
  params      Query auth params

Flags:
  -h, --help   help for auth

Global Flags:
      --chain-id string       Chain ID of tendermint node
      --default-http          Use Http as default schema
  -e, --encoding string       Binary encoding (hex|b64|btc) (default "hex")
      --home string           directory for config and data (default "/Users/bitmain/.cetcli")
  -o, --output string         Output format (text|json) (default "text")
      --swagger-host string   Default host of swagger API
      --trace                 print out full stack trace on errors

```



### 查询auth模块参数

查询auth模块默认参数：

用法：

```bash
cetcli q auth params -h
Query auth params

Usage:
  cetcli query auth params [flags]
```

示例：

```
cetcli query auth params --chain-id=coinexdex
{
  "max_memo_characters": "512",
  "tx_sig_limit": "7",
  "tx_size_cost_per_byte": "20",
  "sig_verify_cost_ed25519": "11800",
  "sig_verify_cost_secp256k1": "20000",
  "min_gas_price_limit": "20.000000000000000000"
}
```

| 参数名称                    | 含义                                   |
| --------------------------- | -------------------------------------- |
| `max_memo_characters`       | 交易memo的最大字节数                   |
| `tx_sig_limit`              | 单笔交易最大签名个数                   |
| `tx_size_cost_per_byte`     | 交易中每个字节消耗的gas数量            |
| `sig_verify_cost_ed25519`   | 验证一个ed25519签名需要消耗的gas数量   |
| `sig_verify_cost_secp256k1` | 验证一个secp256k1签名需要消耗的gas数量 |
| `min_gas_price_limit`       | 验证人节点可设置的gas price的最小值    |



