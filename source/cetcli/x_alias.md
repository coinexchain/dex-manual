# alias command

The alias module manages the relationship between accounts and aliases.

## tx command

It's used to add or remove alias.

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

One account can have at most `MaxAliasCount` aliases. The parameter `MaxAliasCount` has default value 5, which can be changed through voting process. Among these aliases, you can pick one as default alias, or none as default alias.

### add alias

Please use `add` sub command to add an alias:

```
$./cetcli tx alias add -h
Add an alias for current account.

Example: 
	 cetcli tx alias add super_super_boy --from local_user_1 --as-default yes

Usage:
  cetcli tx alias add [alias] [flags]

```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | alias           | string | ✔        |               | The alias to be added |

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------  |
| --as-default     | bool             | ✔        |               |  Whether to use this alias as default |


Example:

```
cetcli tx alias add superman --as-default yes --from coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
```

This command add an alias "superman" to the address coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg, and use it as the default alias. If you change the value of "--as-default" to "no", then it is set as non-default alias. When you set a different alias as default alias, the old default alias (if any) will become a non-default alias.

Option "--as-default" not only can control a new alias to be default or non-default, but also can switch an existing alias between default and non-default. After above command, if you use the following command:

```
cetcli tx alias add superman --as-default no --from coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
```

You can change the default alias "superman" to a non-default alias.


### Remove alias

Please use `remove` sub command to remove an alias:

```
$./cetcli tx alias remove -h
Remove an alias for current account.

Example: 
	 cetcli tx alias remove super_super_boy --from local_user_1

Usage:
  cetcli tx alias remove [alias] [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | alias           | string | ✔        |               | The alias to be removed |

Example:

```
cetcli tx alias remove superman --from coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
```

After execution of this command, "superman" is no longer an alias of coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg.

## query command

This command is used to query about aliases.

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

### Query the module parameters about the alias module

Usage:

```
./cetcli query alias params -h
Query alias params

Usage:
  cetcli query alias params [flags]
```

Example:

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

Parameter `fee_for_alias_length_*` describes the feature fees for issuing tokens with different lengths (from 2 to 7 or longer). Remember to divide the value by 1e8 to get the real amount.
Parameter `max_alias_count` describes the maximum count of aliases that an account can have.


### Given alias, query account

The sub command `address-of-alias` queries the unique account which an alias corresponds to.

Usage:

```
$./cetcli query alias address-of-alias -h
query the corresponding address of an alias. 

Example : 
	cetcli query alias address-of-alias super_super_boy

Usage:
  cetcli query alias address-of-alias [alias] [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | alias           | string | ✔        |               | The alias to be queried |

Example:

```
~ $cetcli query alias address-of-alias coinex --chain-id=coinexdex
[
  "coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg"
]
```

This command tells you that, the alias "coinex" belongs to the account coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg.

## Given account, query aliases

The sub command `aliases-of-address` queries the aliases which belongs to a given account.

Usage:

```
$./cetcli query alias aliases-of-address -h
query the aliases of an address. 

Example : 
	cetcli query alias aliases-of-address coinex1qw508d6qejxtdg4y5r3zarvary0c5xw7kv8f3t4

Usage:
  cetcli query alias aliases-of-address [address] [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | address         | string | ✔        |               | A bech32 address |

Example:

```
~ $cetcli query alias aliases-of-address coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg --chain-id=coinexdex
[
  "coinex",
  "coinex.com",
  "coinex.org"
]
```

The above command tells you that the account coinex1e06xh60m7juwl3mglkjvxgzfyrcphcp7ylh4wg has three aliases: "coinex", "coinex.com" and "coinex.org". Among these alias, "coinex" is the default alias. The default alias is always returned at the first position of the list. If there is no default alias, the first position of list will be an empty string.

