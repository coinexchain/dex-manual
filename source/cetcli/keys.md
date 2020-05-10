# keys command

`keys` sub command manages the local keystone, its usage:

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



## Add new private key

Please use `add` command to add a new private key:

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

Global Flags: ...
```

Example 1: Add a plain private key with the name bob: `$ ./cetcli keys add bob`

Example 2: Add a 2-of-3 multi-sig private key with the name t3 (the 3 keys are owned by bob, eve and jay): `$ ./cetcli keys add t3 --multisig=bob,eve,jay --multisig-threshold=2 `

Example 3: Recover a private key according to bip39 mnemonic: `./cetcli keys add mykey1 --recover`

Example 4: Generate a private key but only print its information to console, instead of writting to local disk: `$ ./cetcli keys add mykey2 --dry-run`

Plase note the names used here (bob, eve, jay) are different from alias, which are on-chain and with consensus. The names use here are totally local to your computer.


## Update the passphrase

Please use the `update` sub-command to update the passphrase which is used to protect your private key:

```
$ ./cetcli keys update -h
Change the password used to protect private key

Usage:
  cetcli keys update <name> [flags]

Flags:
  -h, --help   help for update

Global Flags: ...
```

Example: `$ ./cetcli keys update bob`



## List all the private keys

Please use `list` to list all the private keys:

```
$ ./cetcli keys list -h
Return a list of all public keys stored by this key manager
along with their associated name and address.

Usage:
  cetcli keys list [flags]

Flags:
  -h, --help     help for list
      --indent   Add indent to JSON response

Global Flags: ...
```

Example: `$ ./cetcli keys list`



## Show information about a particular key

Please use the `show` sub command to show information about a particular key:

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

Global Flags: ...
```

Example 1: Show the information about bob's key: `$ ./cetcli keys show bob`

Example 2: Only show the address about bob's key: `$ ./cetcli keys show bob -a`

Example 3: Only show the public key about bob's key: `$ ./cetcli keys show bob -p`



## Delete a private key

Please use `delete` sub command to delete a private key:

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

Global Flags: ...
```

Example: Delete bob's key: `$ ./cetcli keys delete bob`



## Export private key

Please use `export` sub command to export a private key:

```
$ ./cetcli keys export -h
Export a private key from the local keybase in ASCII-armored encrypted format.

Usage:
  cetcli keys export <name> [flags]

Flags:
  -h, --help   help for export

Global Flags: ...
```

Example: export the private key of bob: `$ ./cetcli keys export bob`



## Import private key

Please use `import` sub command to import a private key:

```
$ ./cetcli keys import -h
Import a ASCII armored private key into the local keybase.

Usage:
  cetcli keys import <name> <keyfile> [flags]

Flags:
  -h, --help   help for import

Global Flags: ...
```

Example: Import a private key with the name bob: `$ ./cetcli keys import bob bob.key`



## Generate bip39 mnemonics

Please use `mnemonic` sub command to generate bip39 mnemonics:

```
$ ./cetcli keys mnemonic -h
Create a bip39 mnemonic, sometimes called a seed phrase, by reading from the system entropy. To pass your own entropy, use --unsafe-entropy

Usage:
  cetcli keys mnemonic [flags]

Flags:
  -h, --help             help for mnemonic
      --unsafe-entropy   Prompt the user to supply their own entropy, instead of relying on the system

Global Flags: ...
```

