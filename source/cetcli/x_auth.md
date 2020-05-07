# auth command

This module has only one query command to query some parameters.

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

** Meaning of the parameters **

- `max_memo_characters:` The maximum bytes in the memo of a transaction

- `tx_sig_limit:` The maximum signature count in one transaction

- `tx_size_cost_per_byte:` The gas cost of each byte in a transaction

- `sig_verify_cost_ed25519:` The gas cost of verifying one ed25519 signature

- `sig_verify_cost_secp256k1:` The gas cost of verifying one secp256k1 signature

- `min_gas_price_limit:` The minimum gas price that validators can set

  

