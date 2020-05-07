# Comment Command

`comment` command can be used to comment on tokens and other people's comment. Based on it, we can implement a on-chain forum. You need some third-part tools to show, search and sort the commetns, because cetd and cetcli do not suport these functions.

## tx

To comment about a token, or a comment from someone else.

```
~ $cetcli tx comment -h
comment transactions subcommands

Usage:
  cetcli tx comment [command]

Available Commands:
  new-thread      Create a new thread of comments under some token
  follow-up       Create a follow-up comment in a thread
  reward-comments Reward some comments that you like

Flags:
  -h, --help   help for comment

Global Flags:
      --chain-id string       Chain ID of tendermint node
      --default-http          Use Http as default schema
  -e, --encoding string       Binary encoding (hex|b64|btc) (default "hex")
      --home string           directory for config and data (default "/Users/a/.cetcli")
  -o, --output string         Output format (text|json) (default "text")
      --swagger-host string   Default host of swagger API
      --trace                 print out full stack trace on errors

Use "cetcli tx comment [command] --help" for more information about a command.
```

### Add new comment

Use the `new-thread` sub command to add a new comment

Usage:

```
cetcli tx comment new-thread -h
Post a comment under some token, which creates a new thread, instead of following any other comments.

Example: 
	 cetcli tx comment new-thread --token=cet --donation=200000000 
	 --title="I love cet." --content="CET to da moon!!!" --content-type=UTF8Text

Usage:
  cetcli tx comment new-thread [flags]
```

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------  |
| --token          | string           | ✔        |        | The name of the commented token       |
| --donation       | integer          | ✔        |        | The amount to be donated to community pool |
| --title          | string           | ✔        |        | The title of the comment    |
| --content        | string           | ✔        |        | The content of the comment    |
| --content-type   | string           | ✔        |        | The type of the content               |

The content type can be:
1. IPFS, an IPFS address
2. Magnet, a magnet address
3. HTTP, a http or https address
4. UTF8Text, a utf-8 string
5. ShortHanzi, a utf-8 string encoded with [shorthanzi library](https://github.com/coinexchain/shorthanzi)
6. RawBytes, raw bytes, used to present binary files

The third-party tool can sort the comments according to the donated tokens. So a comment which donates more tokens will be shown on the top.

Example：

```
cetcli tx comment new-thread --token=cet --donation=200000000
	 --title="I love cet." --content="CET to da moon!!!" --content-type=UTF8Text
```

This command comments on "cet" with a post with the title "I love cet.", whose content is "cet to da moon" and the content type is UTF8Text. It also donates 2 cet to community pool.

### Follow up an existing comment

`follow-up` sub comment follows up an existing comment with another comment.

Usage:

```
$cetcli tx comment follow-up -h
Post a comment to follow another comment in a thread.

Example: 
	 cetcli tx comment follow-up --token=cet --donation=0 --follow="10001;coinex1qw508d6qejxtdg4y5r2zarvary0c0xw9kv8f3t4;2;cet;like,favorite"
	 --title="I love cet too." --content="CET to da moon!!!" --content-type=UTF8Text

Usage:
  cetcli tx comment follow-up [flags]
```

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------  |
| --token          | string           | ✔        |        | The name of the commented token       |
| --donation       | integer          | ✔        |        | The amount to be donated to community pool |
| --follow         | string           | ✔        |        | See Below               |
| --title          | string           | ✔        |        | The title of the comment    |
| --content        | string           | ✔        |        | The content of the comment    |
| --content-type   | string           | ✔        |        | The type of the content               |

Example:

```
cetcli tx comment follow-up --token=cet --donation=0 --follow="10001;coinex1qw508d6qejxtdg4y5r2zarvary0c0xw9kv8f3t4;200000000;cet;like,favorite"
	 --title="I love cet too." --content="CET to da moon!!!" --content-type=UTF8Text

```

The value of `--follow` option is a long string, which is divided by `;` to several fields. We use the above example to explain these fields:

1. 10001, this is the comment's ID which is followed up.
2. coinex1qw508d6qejxtdg4y5r2zarvary0c0xw9kv8f3t4, this is the author of the comment whose ID is 10001.
3. 200000000和cet, this means ttthe author is rewarded with 2 cet.
4. like和favorite, this is the attitude towards the author.

Possible attitude includes: like, dislike, laugh, cry, angry, surprise, heart, sweat, speechless, favorite, condolences.

### Reward several comments

`reward-comments` rewards several comments' authors.

Usage:

```
$cetcli tx comment reward-comments -h
Reward the senders and some comments that you like, while showing why you like them individually.

Example: 
	 cetcli tx comment reward-comments --token=cet --reward-to="10001;coinex1qi598e62ejitdg4yur3zarvary0c5xw7kv8f3t4;2;cet;like,favorite" --reward-to="20021;coinex1qw508d6qejxtdg4y5r3zarvary0c5xw7kv8f3t4;1;cet;like"

Usage:
  cetcli tx comment reward-comments [flags]
```

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------  |
| --token          | string           | ✔        |        | The name of the commented token       |
| --reward-to      | string           | ✔        |        | See below |

The `--reward-to` option can be used multiple times. Each time it is used, you show your attitude towards a comment, and (optionally) reward the author. The value of `--reward-to` option has the same format and meaning as the `--follow` option in above example.

Example:

```
tx comment reward-comments --token=cet --reward-to="10001;coinex1qi598e62ejitdg4yur3zarvary0c5xw7kv8f3t4;2;cet;like,favorite" --reward-to="20021;coinex1qw508d6qejxtdg4y5r3zarvary0c5xw7kv8f3t4;1;cet;like"
```


