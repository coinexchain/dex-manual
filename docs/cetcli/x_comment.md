# Comment命令

`comment`命令用来对Token发出评论，以及对其他人的评论发表进一步的评论。基于它，可以实现一个链上的“股吧”。对这些评论的查询功能，需要其他相关工具（例如Trade-Server）来提供，cetd和cetcli并不支持。

## tx命令

用于发表评论，或者针对他人的评论发表进一步的评论。

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

### 添加新评论

可以使用`new-thread`子命令发表一个新评论。

用法：
```
cetcli tx comment new-thread -h
Post a comment under some token, which creates a new thread, instead of following any other comments.

Example: 
	 cetcli tx comment new-thread --token=cet --donation=200000000 
	 --title="I love cet." --content="CET to da moon!!!" --content-type=UTF8Text

Usage:
  cetcli tx comment new-thread [flags]
```

主要选项：

| 选项名              | 类型（取值范围） | 是否必填 | 默认值 | 说明                           |
| ------------------- | ---------------- | -------- | ------ | -------------------------------|
| --token             | string           | ✔        |        | 被评论的Token的名称            |
| --donation          | integer          | ✔        |        | 向community pool捐赠的金额     |
| --title             | string           | ✔        |        | 评论的标题                     |
| --content           | string           | ✔        |        | 评论的内容                     |
| --content-type      | string           | ✔        |        | 评论的内容的类型               |

示例：

```
cetcli tx comment new-thread --token=cet --donation=2 
	 --title="I love cet." --content="CET to da moon!!!" --content-type=UTF8Text
```

此命令在cet对应的贴吧下发表一个题为“I love cet.”的帖子，其内容为“CET to da moon!!!”，内容的类型为UTF8Text，即普通的UTF8字符串。内容的类型还可以是：
1. IPFS，即IPFS地址
2. Magnet，即磁力链地址
3. HTTP，即http或https的网址
4. ShortHanzi，即使用[shorthanzi库](https://github.com/coinexchain/shorthanzi)编码的UTF8字符串
5. RawBytes，即原始的字节串，供上传二进制文件时使用。

为了让这条评论可以排名比较靠前，这条命令还用donation选项向community pool当中捐赠了2个CET。


### 针对其他评论追加新评论

可以使用`follow-up`子命令来针对某条评论来追加自己对它的评论。

用法：
```
$cetcli tx comment follow-up -h
Post a comment to follow another comment in a thread.

Example: 
	 cetcli tx comment follow-up --token=cet --donation=0 --follow="10001;coinex1qw508d6qejxtdg4y5r2zarvary0c0xw9kv8f3t4;2;cet;like,favorite"
	 --title="I love cet too." --content="CET to da moon!!!" --content-type=UTF8Text

Usage:
  cetcli tx comment follow-up [flags]
```

主要选项：

| 选项名              | 类型（取值范围） | 是否必填 | 默认值 | 说明                                 |
| ------------------- | ---------------- | -------- | ------ | -------------------------------      |
| --token             | string           | ✔        |        | 被评论的Token的名称                  |
| --donation          | integer          | ✔        |        | 向community pool捐赠的金额           |
| --follow            | string           | ✔        |        | 针对某评论的态度和打赏               |
| --title             | string           | ✔        |        | 评论的标题                           |
| --content           | string           | ✔        |        | 评论的内容                           |
| --content-type      | string           | ✔        |        | 评论的内容的类型                     |

示例：

```
cetcli tx comment follow-up --token=cet --donation=0 --follow="10001;coinex1qw508d6qejxtdg4y5r2zarvary0c0xw9kv8f3t4;200000000;cet;like,favorite"
	 --title="I love cet too." --content="CET to da moon!!!" --content-type=UTF8Text

```

`follow-up`子命令的token、donation、title、content、content-type选项的含义同`new-thread`子命令一样。它独有的选项是follow，其取值是一个长的字符串，中间用英文分号分割开若干个字段，每个字段的含义是：

1. 10001，这是一个评论的ID，即被追加的对象
2. coinex1qw508d6qejxtdg4y5r2zarvary0c0xw9kv8f3t4，这是上述评论的作者
3. 200000000和cet，表示对此作者打赏的金额是2个CET
4. like和favorite，是对上述评论的态度

可选的态度包括：like, dislike, laugh, cry, angry, surprise, heart, sweat, speechless, favorite, condolences。

### 对若干评论进行打赏

子命令`reward-comments`可以对某个贴吧下的多条评论进行打赏。

用法：
```
$cetcli tx comment reward-comments -h
Reward the senders and some comments that you like, while showing why you like them individually.

Example: 
	 cetcli tx comment reward-comments --token=cet --reward-to="10001;coinex1qi598e62ejitdg4yur3zarvary0c5xw7kv8f3t4;2;cet;like,favorite" --reward-to="20021;coinex1qw508d6qejxtdg4y5r3zarvary0c5xw7kv8f3t4;1;cet;like"

Usage:
  cetcli tx comment reward-comments [flags]
```

主要选项：

| 选项名              | 类型（取值范围） | 是否必填 | 默认值 | 说明                                 |
| ------------------- | ---------------- | -------- | ------ | -------------------------------      |
| --token             | string           | ✔        |        | 被评论的Token的名称                  |
| --reward-to         | string           | ✔        |        | 针对某评论的态度和打赏               |

示例：

```
tx comment reward-comments --token=cet --reward-to="10001;coinex1qi598e62ejitdg4yur3zarvary0c5xw7kv8f3t4;2;cet;like,favorite" --reward-to="20021;coinex1qw508d6qejxtdg4y5r3zarvary0c5xw7kv8f3t4;1;cet;like"
```

上面的reward-to选项可以多次使用，每次出现都对一篇帖子表达自己的态度，同时（可选地）对帖子的作者进行打赏。reward-to选项后面跟着的字符串，同`follow-up`子命令的选项之后的字符串，具有一样的格式和含义。

