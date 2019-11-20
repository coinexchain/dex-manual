# keys命令

`keys`命令用来维护本地keystore：

```
$ ./cetcli keys -h
Keys allows you to manage your local keystore for tendermint.

    These keys may be in any format supported by go-crypto and can be
    used by light-clients, full nodes, or any other application that
    needs to sign with a private key.

Usage:
  cetcli keys [command]

Available Commands:
  mnemonic    Compute the bip39 mnemonic for some input entropy
  add         Add an encrypted private key (either newly generated or recovered), encrypt it, and save to disk
  export      Export private keys
  import      Import private keys into the local keybase
  list        List all keys
  show        Show key info for the given name
              
  delete      Delete the given key
  update      Change the password used to protect private key
  parse       Parse address from hex to bech32 and vice versa
```



## 添加秘钥

可以使用`add`命令添加密钥：

```
$ ./cetcli keys add -h
Derive a new private key and encrypt to disk.
Optionally specify a BIP39 mnemonic, a BIP39 passphrase to further secure the mnemonic,
and a bip32 HD path to derive a specific account. The key will be stored under the given name
and encrypted with the given password. The only input that is required is the encryption password.

If run with -i, it will prompt the user for BIP44 path, BIP39 mnemonic, and passphrase.
The flag --recover allows one to recover a key from a seed passphrase.
If run with --dry-run, a key would be generated (or recovered) but not stored to the
local keystore.
Use the --pubkey flag to add arbitrary public keys to the keystore for constructing
multisig transactions.

You can add a multisig key by passing the list of key names you want the public
key to be composed of to the --multisig flag and the minimum number of signatures
required through --multisig-threshold. The keys are sorted by address, unless
the flag --nosort is set.

Usage:
  cetcli keys add <name> [flags]

Flags:
      --account uint32            Account number for HD derivation
      --dry-run                   Perform action, but don't add key to local keystore
  -h, --help                      help for add
      --indent                    Add indent to JSON response
      --index uint32              Address index number for HD derivation
  -i, --interactive               Interactively prompt user for BIP39 passphrase and mnemonic
      --ledger                    Store a local reference to a private key on a Ledger device
      --multisig strings          Construct and store a multisig public key (implies --pubkey)
      --multisig-threshold uint   K out of N required signatures. For use in conjunction with --multisig (default 1)
      --no-backup                 Don't print out seed phrase (if others are watching the terminal)
      --nosort                    Keys passed to --multisig are taken in the order they're supplied
      --pubkey string             Parse a public key in bech32 format and save it to disk
      --recover                   Provide seed phrase to recover existing key instead of creating

Global Flags: 省略
```

例1，添加名为bob的普通密钥：`$ ./cetcli keys add bob`

例2，添加名为t3的2-of-3多签密钥：`$ ./cetcli keys add t3 --multisig=bob,eve,jay --multisig-threshold=2 `

例3，根据bip39助记词恢复密钥：`./cetcli keys add mykey1 --recover`

例3，生成密钥，但是仅打印信息，不写入本地存储：`$ ./cetcli keys add mykey2 --dry-run`



## 更新密码

可以使用`update`命令更新密钥保护密码：

```
$ ./cetcli keys update -h
Change the password used to protect private key

Usage:
  cetcli keys update <name> [flags]

Flags:
  -h, --help   help for update

Global Flags: 省略
```

例：`$ ./cetcli keys update bob`



## 列出全部密钥

可以使用`list`列出全部密钥：

```
$ ./cetcli keys list -h
Return a list of all public keys stored by this key manager
along with their associated name and address.

Usage:
  cetcli keys list [flags]

Flags:
  -h, --help     help for list
      --indent   Add indent to JSON response

Global Flags: 省略
```

例：`$ ./cetcli keys list`



## 显示单个密钥

可以使用`show`命令显示单个密钥：

```
$ ./cetcli keys show -h
Return public details of a single local key. If multiple names are
provided, then an ephemeral multisig key will be created under the name "multi"
consisting of all the keys provided by name and multisig threshold.

Usage:
  cetcli keys show [name [name...]] [flags]

Flags:
  -a, --address                   Output the address only (overrides --output)
      --bech string               The Bech32 prefix encoding for a key (acc|val|cons) (default "acc")
  -d, --device                    Output the address in a ledger device
  -h, --help                      help for show
      --indent                    Add indent to JSON response
      --multisig-threshold uint   K out of N required signatures (default 1)
  -p, --pubkey                    Output the public key only (overrides --output)

Global Flags: 省略
```

例1，显示bob的密钥信息：`$ ./cetcli keys show bob`

例2，仅显示bob的地址：`$ ./cetcli keys show bob -a`

例3，仅显示bob的公钥：`$ ./cetcli keys show bob -p`



## 删除密钥

可以使用`delete`命令删除密钥：

```
$ ./cetcli keys delete -h
Delete a key from the store.

Note that removing offline or ledger keys will remove
only the public key references stored locally, i.e.
private keys stored in a ledger device cannot be deleted with the CLI.

Usage:
  cetcli keys delete <name> [flags]

Flags:
  -f, --force   Remove the key unconditionally without asking for the passphrase
  -h, --help    help for delete
  -y, --yes     Skip confirmation prompt when deleting offline or ledger key references

Global Flags: 省略
```

例，删除名为bob的密钥：`$ ./cetcli keys delete bob`



## 导出密钥 

可以使用`export`命令导出密钥：

```
$ ./cetcli keys export -h
Export a private key from the local keybase in ASCII-armored encrypted format.

Usage:
  cetcli keys export <name> [flags]

Flags:
  -h, --help   help for export

Global Flags: 省略
```

例，导出bob的密钥：`$ ./cetcli keys export bob`



## 导入密钥

可以使用`import`命令导人密钥：

```
$ ./cetcli keys import -h
Import a ASCII armored private key into the local keybase.

Usage:
  cetcli keys import <name> <keyfile> [flags]

Flags:
  -h, --help   help for import

Global Flags: 省略
```

例，导入bob的密钥：`$ ./cetcli keys import bob bob.key`



## 创建bip39助记词

可以使用`mnemonic`命令创建bip39助记词：

```
$ ./cetcli keys mnemonic -h
Create a bip39 mnemonic, sometimes called a seed phrase, by reading from the system entropy. To pass your own entropy, use --unsafe-entropy

Usage:
  cetcli keys mnemonic [flags]

Flags:
  -h, --help             help for mnemonic
      --unsafe-entropy   Prompt the user to supply their own entropy, instead of relying on the system

Global Flags: 省略
```

