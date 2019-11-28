# auth命令

该模块只有一个查询参数的命令，返回了一些系统参数

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

**字段解释**

- `max_memo_characters:` 交易memo的最大字节数

- `tx_sig_limit:` 单笔交易最大签名个数

- `tx_size_cost_per_byte:` 交易中每个字节消耗的gas数量

- `sig_verify_cost_ed25519:` 验证一个ed25519签名需要消耗的gas数量

- `sig_verify_cost_secp256k1:` 验证一个secp256k1签名需要消耗的gas数量

- `min_gas_price_limit:` 验证人节点可设置的gas price的最小值

  

