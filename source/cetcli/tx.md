# tx command

The sub commands related to transactions are all listed below the `tx` command:

```
$ cetcli tx -h
Transactions subcommands

Usage:
  cetcli tx [command]

Available Commands:
  send         Create and sign a send tx
  require-memo Mark if memo is required to receive coins
  donate       Donate to community pool
              
  sign         Sign transactions generated offline
  multisign    Generate multisig signatures for transactions generated offline
              
  broadcast    Broadcast transactions generated offline
  encode       Encode transactions generated offline
              
  alias        alias transactions subcommands
  asset        Asset transactions subcommands
  bancorlite   bancorlite transactions subcommands
  comment      comment transactions subcommands
  market       market transactions subcommands
  set-referee  Set referee address to earn trading fees
  staking      Staking transaction subcommands
  crisis       Crisis transactions subcommands
  gov          Governance transactions subcommands
  slashing     Slashing transactions subcommands
  distribution Distribution transactions subcommands

Flags:
  -h, --help   help for tx

Global Flags:
      --chain-id string       Chain ID of tendermint node
      --default-http          Use Http as default schema
  -e, --encoding string       Binary encoding (hex|b64|btc) (default "hex")
      --home string           directory for config and data (default "/Users/zxh/.cetcli")
  -o, --output string         Output format (text|json) (default "text")
      --swagger-host string   Default host of swagger API
      --trace                 print out full stack trace on errors

Use "cetcli tx [command] --help" for more information about a command.
```

Some detailed `tx` sub commands are related to the different modules, which are described in the modules' command manual. Here are the sub commands which are not covered by the modules' command manual.



## send

Send coins to other accounts:

```
$ cetcli tx send -h
Create and sign a send tx

Usage:
  cetcli tx send [to_address] [amount] [flags]
  cetcli tx send [command]

Available Commands:
  supervised-tx Create and sign a supervised tx

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
      --generate-unsigned-tx    Generate a unsigned tx
  -h, --help                    help for send
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
      --unlock-time int         The unix timestamp when tokens can transfer again
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: ...
```

Parameters:

| Parameter   | Name       | Type   | Required | Default Value | Comment                    |
| ------------| ---------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | to_address | string | ✔        |               | The account to receive coins |
| Parameter 2 | amount     | string | ✔        |               | Amount of transferred coins |

Example：

```
$ cetcli tx send $(cetcli keys show -a alice) 200000000cet \
	--from bob --gas=30000 --fees=600000cet --chain-id=coinexdex  
```


## require-memo

Set the account's “require-memo” attribute to true or false:

```
$ cetcli tx require-memo -h
Mark if memo is required to receive coins

Usage:
  cetcli tx require-memo <bool> [flags]

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
  -h, --help                    help for require-memo
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | memo-required   | bool   | ✔        |               | The value of memo-required |

Example:

```
$ cetcli tx require-memo true --from your_key_name \
	--gas=30000 --fees=600000cet --chain-id=coinexdex  
```



## donate

This command donates coins to the community pool of CoinEx Chain. Currently only cet is supported.

```
$ cetcli tx donate -h
Donate to community pool

Usage:
  cetcli tx donate [amount] [flags]

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
      --generate-unsigned-tx    Generate a unsigned tx
  -h, --help                    help for donate
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: 省略
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | amount          | string | ✔        |               | The amount of donated coins |


Example：

```BASH
$ cetcli tx donate 100cet --from your_key_name \
	--gas=30000 --fees=600000cet --chain-id=coinexdex  
```



## sign

This command signs a transactions which is generated but not signed:

```BASH
$ cetcli tx sign -h
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

Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | file            | string | ✔        |               | path of the file containing unsigned transaction |


Firstly, when you construct a transaction, you can use the `--generate-only` option to generated an unsigned transaction.

```BASH
$ cetcli tx donate 100cet --from coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv \
	--gas=30000 --fees=600000cet  --generate-only >unsigned.json
```

You can view the generated transaction:

```BASH
$ cat unsigned.json 
{"type":"cosmos-sdk/StdTx","value":{"msg":[{"type":"distrx/MsgDonateToCommunityPool","value":{"from_addr":"coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv","amount":[{"denom":"cet","amount":"100"}]}}],"fee":{"amount":[{"denom":"cet","amount":"600000"}],"gas":"30000"},"signatures":null,"memo":""}}
```

Now sign the generated transaction. The signatures will be printed to console by cetcli:

```bash
$ cetcli tx sign unsigned.json --from your_key_name --signature-only --chain-id=coinexdex
Password to sign with 'your_key_name':
{"pub_key":{"type":"tendermint/PubKeySecp256k1","value":"AkjG1xCjYnMSr/JAfSqzqkMMoLlRuqVCqfTQeBGM/31V"},"signature":"jdYgO2BRNKkro2Q+ofQ84JfdOS8VCDrcHi1uLhll83JyM6EIAdcDcLmnilzUSJwrpDv24H6XLTQowVtf0bEwBA=="}

```

Or you can make cetcli to print all the signed transaction to console by removing the `--signature-only` option.

```BASH
$ cetcli tx sign unsigned.json --from your_key_name  --chain-id=coinedex >signed.json
Password to sign with 'your_key_name':

$ cat signed.json
{"type":"cosmos-sdk/StdTx","value":{"msg":[{"type":"distrx/MsgDonateToCommunityPool","value":{"from_addr":"coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv","amount":[{"denom":"cet","amount":"100"}]}}],"fee":{"amount":[{"denom":"cet","amount":"600000"}],"gas":"30000"},"signatures":[{"pub_key":{"type":"tendermint/PubKeySecp256k1","value":"AkjG1xCjYnMSr/JAfSqzqkMMoLlRuqVCqfTQeBGM/31V"},"signature":"jdYgO2BRNKkro2Q+ofQ84JfdOS8VCDrcHi1uLhll83JyM6EIAdcDcLmnilzUSJwrpDv24H6XLTQowVtf0bEwBA=="}],"memo":""}}
```

Now use `--validate-signatures` to verify the signature.

```BASH
cetcli tx sign signed.json --validate-signatures --from jy --chain-id=coinexdex
Signers:
  0: coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv

Signatures:
  0: coinex1vymm67jywd23v6qvhfq5vk5drn6fmmdngad4kv			[OK]

```


## broadcast

This command broadcast the signed transaction to the P2P network of CoinEx Chain.

```
$ cetcli tx broadcast -h
Broadcast transactions created with the --generate-only
flag and signed with the sign command. Read a transaction from [file_path] and
broadcast it to a node. If you supply a dash (-) argument in place of an input
filename, the command reads from standard input.

$ <appcli> tx broadcast ./mytxn.json

Usage:
  cetcli tx broadcast [file_path] [flags]

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
  -h, --help                    help for broadcast
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | file            | string | ✔        |               | path of the file containing the signed transaction |


Example:

```bash
$ cetcli tx broadcast signed.json

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

In this example, `--broadcast-mode` is not specified, so its default value `sync` is used, which means only basic checks are performed (including format sanity check and signature check) before it returns. You can also use `--broadcast-mode=block` to wait for this transaction to be packed into a block and get executed.


## encode

This command performs serialization and base64 encoding for a signed transaction (in json format):

```BASH
$ cetcli tx encode -h
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

Global Flags: ...
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | file            | string | ✔        |               | path of the file containing the signed transaction |

Example:

```bash
$ cetcli tx encode signed.json

rQEoKBapCiYgOjsaChRhN716RHNVFmgMukFGWo0c9J3tsxIKCgNjZXQSAzEwMBITCg0KA2NldBIGNjAwMDAwELDqARpqCibrWumHIQJIxtcQo2JzEq/yQH0qs6pDDKC5UbqlQqn00HgRjP99VRJAjdYgO2BRNKkro2Q+ofQ84JfdOS8VCDrcHi1uLhll83JyM6EIAdcDcLmnilzUSJwrpDv24H6XLTQowVtf0bEwBA==
```


