# Commands provided by bank module

The bank module provides several sub commands for transferring tokens between accounts. These sub commands are listed under the 'tx' command.

## tx

#### Send given asset

该命令用于从`from`账户转移指定数量的某种token到`to`账户，可以指定这笔钱到账后在未来什么时刻之后生效，改设置可选。
These command is used to send given amount of some token from `from` account to the `to` account. This transfer can take effect immediately, or be locked util a specified time in the future if an unlock time is specified. The locked asset is unlocked automatically when the block's timestamp is larger than the specified time.


```
cetcli tx send -h
Create and sign a send tx
Usage:
  cetcli tx send [to_address] [amount] [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | to\_address     | string | ✔        |               | The receiver's address     |
| Parameter 2 | amount          | string | ✔        |               | The amount of tokens to be sent |

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------  |
| --unlock-time    | int              | ✔        |               | Unix timestamp (in second). If zero, the transfer take effect immediately |


Example: Send `100000000cet` to `coinex10x5hpff75438mpdz4k28yq6n35qa78h9sqj4yt`, and these coins will be locked until unix time `1574245028`.

```
$cetcli tx send coinex1vg35xer0e34pfszhtktggvyhjfhnqhzpd67d9y 10000000000000000cet --unlock-time=1574245028 --from=coinex10x5hpff75438mpdz4k28yq6n35qa78h9sqj4yt --fees=2000000000cet --gas=6000000 --trust-node --chain-id=coinexdex-test1
```

Please note in the command line, we must type 10000000000000000cet, because internally 1cet is represented as 1e8.

#### supervised send

This command can also send tokens from one account to another. Which makes it different from the `send` command is that it introduced the third participator, the supervisor, into the transaction. The supervisor can arbitrate this transaction by initiating two kinds of transaction: to unlock the locked tokens immediately and send them to the receiver, or to cancel the transfer and return the locked tokens to the sender.

In a supervised send transaction, the `from` account can also unlock the locked tokens immediately and send them to the receiver.

The supervisor can never take the tokens as hers.

Suppose that address A sends M tokens to address B and specifies the unlock time is T3, supervisor is address M. Then this transfer has four possible endings:

1. The tokens are unlocked and given to B at T3, automatically.
2. Address A unlocks the tokens and gives them to B, at T0 (T0 < T3)
3. Address M unlocks the tokens and gives them to B, at T1 (T1 < T3)
4. Address M returns the tokens back to A, at T2 (T2 < T3)

Usage:

```
$ cetcli tx send supervised-tx -h

Create and sign a supervised tx.
Example:
    cetcli tx send supervised-tx coinex1ke3qq22zvzlcdh3j8nenlrjxmvnrna7z426n0x 10000000cet \
        --supervisor=coinex1qga320mdvfhr62hcjn78n6pjl3z3vsvgtz2w8t \
        --unlock-time=1600000000 \
        --reward=100000000 \
        --operation=0 \
        --from=sender_user

    cetcli tx send supervised-tx coinex1ke3qq22zvzlcdh3j8nenlrjxmvnrna7z426n0x 10000000cet \
        --sender=coinex1hckjvduhckfaxq2tuythfd270cex94c0hv5hs7 \
        --unlock-time=1600000000 \
        --reward=100000000 \
        --operation=1 \
        --from=supervisor_user
Usage:
  cetcli tx send supervised-tx [to_address] [amount] [flags]
```

**参数解释**

- --supervisor: 监督人地址，该地址拥有者有权利发起参数--operation为1（归还token给from账户）和3（提前解锁发给to账户的token）的交易
- --sender：from地址，在operation为1和3时需要填写
- --unlock-time：同上面的转移指定资产命令
- --reward： 该参数指定的token用于赠送给supevisor
- --operation：可选参数为0，1，2，3四个数字；0代表创建supervisor转账交易；1代表supervisor将token归还给from地址，这时交易的签名者为supervisor；2代表from地址提前解锁token给to地址；3代表supervisor提前解锁token给to地址。

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | to\_address     | string | ✔        |               | The receiver's address     |
| Parameter 2 | amount          | string | ✔        |               | The amount of tokens to be sent |

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------  |
| --unlock-time    | int              | ✔        |               | Unix timestamp (in second). If zero, the transfer take effect immediately |
| --supervisor     | string           | ✔        |               | The supervisor's address |
| --sender         | string           | ✔        |               | The sender's address |
| --reward         | string           | ✔        |               | The reward that would be sent to supervisor (instead of the receiver), required when operation is 0 |
| --operation      | string           | ✔        |               | See below |

The valid values for operation are:

- 0: initiate a supervised send transaction
- 1: supervisor return the tokens to the sender (supervisor should sign the transaction)
- 2: the sender unlocks the tokens early and sends them to the receiver (sender should sign the transaction)
- 3: the supervisor unlocks the tokens early and sends them to the receiver (supervisor should sign the transaction)


Example 1: initiate a supervised send transaction

```
 cetcli tx send supervised-tx coinex1ke3qq22zvzlcdh3j8nenlrjxmvnrna7z426n0x 10000000cet \
        --supervisor=coinex1qga320mdvfhr62hcjn78n6pjl3z3vsvgtz2w8t \
        --unlock-time=1600000000 \
        --reward=100000000 \
        --operation=0 \
        --from=sender_user \
        --fees=2000000000cet --gas=6000000 --trust-node --chain-id=coinexdex-test1
```

Example 2: supervisor return the tokens to the sender

```
 cetcli tx send supervised-tx coinex1ke3qq22zvzlcdh3j8nenlrjxmvnrna7z426n0x 10000000cet \
        --sender=coinex1hckjvduhckfaxq2tuythfd270cex94c0hv5hs7 \
        --unlock-time=1600000000 \
        --reward=100000000 \
        --operation=1 \
        --from=supervisor_user \
        --fees=2000000000cet --gas=6000000 --trust-node --chain-id=coinexdex-test1
```

#### Change the memo-required attribute of an account

If an account set its memo-required attribute to true, then all the transactions taking it as a receiver must have non-empty memo.

```
cetcli tx require-memo -h

Mark if memo is required to receive coins
Usage:
  cetcli tx require-memo <bool> [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | memo-required   | bool   | ✔        |               | Whether memo is required   |

Example: Set the memo-required to true for the account bob.

```
cetcli tx require-memo true --from=bob --fees=2000000000cet --gas=6000000 --memo="Sent with example memo" --chain-id=coinexdex-test1
```



## query command

```
cetcli query bank -h

Querying commands for the bank module
Usage:
  cetcli query bank [command]

Available Commands:
  params      Query bank params
  balances    Query account balance
```

#### Query the parameters of bank module

```
cetcli query bank params --trust-node
{
  "activation_fee": "100000000",
  "lock_coins_free_time": "604800000000000",
  "lock_coins_fee_per_day": "1000000"
}
```

Meaning of the parameters.

- `activation_fee`: The feature fee used to activate a nonexistent address (when sending coins to a nonexistent address)
- `lock_coins_free_time`: If the lock time is shorter than its value, then the transaction is free of feature fees.
- `lock_coins_fee_per_day`: If the lock time is shorter than its value, then feature fees must be paid according to the number of locked days.

#### Query the balances of an account

Usage:

```
cetcli query bank balances -h

Query account balance
Usage:
  cetcli query bank balances [address] [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | address         | string | ✔        |               | The account's address to be required   |

