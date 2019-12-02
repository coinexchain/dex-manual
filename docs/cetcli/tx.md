# tx命令

## donate

```BASH
Donate to community pool

Usage:
  cetcli tx donate [amount] [flags]
  
Example:
cetcli tx donate 100cet --from your_key_name --gas=30000 --fees=600000cet --chain-id=coinexdex  

```

该命令用来向community pool捐赠，目前只支持cet的捐赠。

## sign

该命令用来对一笔已经组装好但尚未签名的交易进行签名。

```BASH
etcli tx sign -h
Sign transactions created with the --generate-only flag.
It will read a transaction from [file], sign it, and print its JSON encoding.

If the flag --signature-only flag is set, it will output a JSON representation
of the generated signature only.

If the flag --validate-signatures is set, then the command would check whether all required
signers have signed the transactions, whether the signatures were collected in the right
order, and if the signature is valid over the given transaction. If the --offline
flag is also set, signature validation over the transaction will be not be
performed as that will require RPC communication with a full node.

The --offline flag makes sure that the client will not reach out to full node.
As a result, the account and sequence number queries will not be performed and
it is required to set such parameters manually. Note, invalid values will cause
the transaction to fail.

The --multisig=<multisig_key> flag generates a signature on behalf of a multisig account
key. It implies --signature-only. Full multisig signed transactions may eventually
be generated via the 'multisign' command.

Usage:
  cetcli tx sign [file] [flags]

Flags:
  -a, --account-number uint      The account number of the signing account (offline mode only)
      --append                   Append the signature to the existing ones. If disabled, old signatures would be overwritten. Ignored if --multisig is on (default true)
  -b, --broadcast-mode string    Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                  ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string              Fees to pay along with transaction; eg: 10cet
      --from string              Name or address of private key with which to sign
      --gas string               gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float     adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string        Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only            Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                     help for sign
      --indent                   Add indent to JSON response
      --ledger                   Use a connected Ledger device
      --memo string              Memo to send along with transaction
      --multisig string          Address of the multisig account on behalf of which the transaction shall be signed
      --node string              <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --offline                  Offline mode; Do not query a full node. --account and --sequence options would be ignored if offline is set
      --output-document string   The document will be written to the given file instead of STDOUT
  -s, --sequence uint            The sequence number of the signing account (offline mode only)
      --signature-only           Print only the generated signature, then exit
      --trust-node               Trust connected full node (don't verify proofs for responses) (default true)
      --validate-signatures      Print the addresses that must sign the transaction, those who have already signed it, and make sure that signatures are in the correct order
  -y, --yes                      Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string       Chain ID of tendermint node
      --default-http          Use Http as default schema
  -e, --encoding string       Binary encoding (hex|b64|btc) (default "hex")
      --home string           directory for config and data (default "/Users/bitmain/.cetcli")
  -o, --output string         Output format (text|json) (default "text")
      --swagger-host string   Default host of swagger API
      --trace                 print out full stack trace on errors

```

首先，我们在构造一笔交易的时候，可以指定--generate-only来组装交易但不进行签名：

```BASH
cetcli tx donate 100cet --from coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv --gas=30000 --fees=600000cet  --generate-only >unsigned.json

```

查看组装好的交易：

```BASH
cat unsigned.json 
{"type":"cosmos-sdk/StdTx","value":{"msg":[{"type":"distrx/MsgDonateToCommunityPool","value":{"from_addr":"coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv","amount":[{"denom":"cet","amount":"100"}]}}],"fee":{"amount":[{"denom":"cet","amount":"600000"}],"gas":"30000"},"signatures":null,"memo":""}}
```

对组装好的交易进行签名：

```bash
cetcli tx sign unsigned.json --from your_key_name --signature-only --chain-id=coinexdex
Password to sign with 'your_key_name':
{"pub_key":{"type":"tendermint/PubKeySecp256k1","value":"AkjG1xCjYnMSr/JAfSqzqkMMoLlRuqVCqfTQeBGM/31V"},"signature":"jdYgO2BRNKkro2Q+ofQ84JfdOS8VCDrcHi1uLhll83JyM6EIAdcDcLmnilzUSJwrpDv24H6XLTQowVtf0bEwBA=="}

```

或者在不加--signature-only的情况下，将输出整个组装好的交易：

```BASH
cetcli tx sign unsigned.json --from your_key_name  --chain-id=coinedex >signed.json
Password to sign with 'your_key_name':

cat signed.json
{"type":"cosmos-sdk/StdTx","value":{"msg":[{"type":"distrx/MsgDonateToCommunityPool","value":{"from_addr":"coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv","amount":[{"denom":"cet","amount":"100"}]}}],"fee":{"amount":[{"denom":"cet","amount":"600000"}],"gas":"30000"},"signatures":[{"pub_key":{"type":"tendermint/PubKeySecp256k1","value":"AkjG1xCjYnMSr/JAfSqzqkMMoLlRuqVCqfTQeBGM/31V"},"signature":"jdYgO2BRNKkro2Q+ofQ84JfdOS8VCDrcHi1uLhll83JyM6EIAdcDcLmnilzUSJwrpDv24H6XLTQowVtf0bEwBA=="}],"memo":""}}
```

使用--validate-signatures来验证签名是否足够：

```BASH
cetcli tx sign signed.json --validate-signatures --from jy --chain-id=coinexdex
Signers:
  0: coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv

Signatures:
  0: coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv			[OK]

```

关于--multisig & multisign 命令的使用可以参考[coinexdex-test网络上发送一笔2-of-4多签交易](https://github.com/coinexchain/research/blob/master/dex/coinexdex-testnet-2of4-multisigTx.md)。

## broadcast

```bash
cetcli tx broadcast signed.json

height: 0
txhash: DD9E03518C93128AB523BA02BCA8967B2FD3C3D0BA9140B9C2FE9422D9137F06
code: 0
data: ""
rawlog: '[{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"action","value":"donate_to_community_pool"}]}]}]'
logs:
- msgindex: 0
  success: true
  log: ""
  events:
  - type: message
    attributes:
    - key: action
      value: donate_to_community_pool
info: ""
gaswanted: 0
gasused: 0
codespace: ""
tx: null
timestamp: ""
events: []

```

使用broadcast命令来广播前面组装好的交易，这里未指定--broadcast-mode，默认为sync，即只对交易做基本的检查，检查成功后即返回。可以通过指定--broadcast-mode=block来等待交易被打包进区块，最终返回交易的执行结果。

## encode

```BASH
cetcli tx encode -h
Encode transactions created with the --generate-only flag and signed with the sign command.
Read a transaction from <file>, serialize it to the Amino wire protocol, and output it as base64.
If you supply a dash (-) argument in place of an input filename, the command reads from standard input.

Usage:
  cetcli tx encode [file] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10cet
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10cet)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for encode
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string       Chain ID of tendermint node
      --default-http          Use Http as default schema
  -e, --encoding string       Binary encoding (hex|b64|btc) (default "hex")
      --home string           directory for config and data (default "/Users/bitmain/.cetcli")
  -o, --output string         Output format (text|json) (default "text")
      --swagger-host string   Default host of swagger API
      --trace                 print out full stack trace on errors
```

该命令用来对已经组装好的（包含签名的）json格式的交易进行序列化和base64编码。

```bash
cetcli tx encode signed.json

rQEoKBapCiYgOjsaChRhN716RHNVFmgMukFGWo0c9J3tsxIKCgNjZXQSAzEwMBITCg0KA2NldBIGNjAwMDAwELDqARpqCibrWumHIQJIxtcQo2JzEq/yQH0qs6pDDKC5UbqlQqn00HgRjP99VRJAjdYgO2BRNKkro2Q+ofQ84JfdOS8VCDrcHi1uLhll83JyM6EIAdcDcLmnilzUSJwrpDv24H6XLTQowVtf0bEwBA==
```



