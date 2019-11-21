# Bank命令

bank模块用于账户间的资产转移

#### 转移指定资产

该命令用于从`from`账户转移指定数量的某种token到`to`账户，可以指定这笔钱到账后在未来什么时刻之后生效，改设置可选。

```
cetcli tx send -h
Create and sign a send tx
Usage:
  cetcli tx send [to_address] [amount] [flags]
```

**参数解释**

- `to_address`: 目的地址
- `amount`: 转移token数量
- `--unlock-time`: 锁定时间，unix时间戳，单位秒，区块时间大于该时间后，该资产自动在目的地址账户内解锁。

**举例**

从地址`coinex10x5hpff75438mpdz4k28yq6n35qa78h9sqj4yt`转移`100000000cet`（命令行中凡是涉及到token数量的要在使用者本意上再添加八个0，这是因为系统内部按照八位精度存储和处理大数）到`coinex1vg35xer0e34pfszhtktggvyhjfhnqhzpd67d9y`，这笔钱到达目的账户后，需要等到unix时间`1574245028`之后才可以被自动解锁，解锁之前的token不能参与任何链上经济活动

> $cetcli tx send coinex1vg35xer0e34pfszhtktggvyhjfhnqhzpd67d9y 10000000000000000cet --unlock-time=1574245028 --from=coinex10x5hpff75438mpdz4k28yq6n35qa78h9sqj4yt --fees=2000000000cet --gas=6000000 --trust-node --chain-id=coinexdex-test1

#### supervised转账

该命令同样用于从`form`转移指定数量的某种token到`to`账户，与上面的命令不同的是，该命令还有第三个参与者，即`supervisor`地址，该地址作为这笔交易的中间人起到监督仲裁的作用，他有权发起两类交易：提前解锁发给`to`账户的token以及归还token给`from`账户。

另外还有一点不同是：`from`账户可以提前解锁发给`to`账户的token。

归纳下这个命令的四种用法：

- 地址A发送数量M的token给地址B，指定解锁时间为未来的时间点T3，中间人设置为地址M
- 地址A在时间点T0发起提前解锁，T0 < T3
- 地址M在时间点T1发起归还操作，T1 < T3
- 地址M在时间点T2发起提前解锁定操作，T2 < T3

**命令概览与详解如下**

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
  cetcli tx send supervised-tx [amount] [flags]
```

**参数解释**

- --supervisor: 监督人地址，该地址拥有者有权利发起参数--operation为1（归还token给from账户）和3（提前解锁发给to账户的token）的交易
- --sender：from地址，在operation为1和3时需要填写
- --unlock-time：同上面的转移指定资产命令
- --reward： 该参数指定的token用于赠送给supevisor
- --operation：可选参数为0，1，2，3四个数字；0代表创建supervisor转账交易；1代表supervisor将token归还给from地址，这时交易的签名者为supervisor；2代表from地址提前解锁token给to地址；3代表supervisor提前解锁token给to地址。

**举例**

- 创建supervised转账交易

```
 cetcli tx send supervised-tx coinex1ke3qq22zvzlcdh3j8nenlrjxmvnrna7z426n0x 10000000cet \
        --supervisor=coinex1qga320mdvfhr62hcjn78n6pjl3z3vsvgtz2w8t \
        --unlock-time=1600000000 \
        --reward=100000000 \
        --operation=0 \
        --from=sender_user \
        --fees=2000000000cet --gas=6000000 --trust-node --chain-id=coinexdex-test1
```

- supervisor创建提前解锁定交易

```
 cetcli tx send supervised-tx coinex1ke3qq22zvzlcdh3j8nenlrjxmvnrna7z426n0x 10000000cet \
        --sender=coinex1hckjvduhckfaxq2tuythfd270cex94c0hv5hs7 \
        --unlock-time=1600000000 \
        --reward=100000000 \
        --operation=1 \
        --from=supervisor_user \
        --fees=2000000000cet --gas=6000000 --trust-node --chain-id=coinexdex-test1
```

#### 设置接受转账时改地址是否需要memo

如果指定地址设置了需要memo，那么给该地址的转账都需要设置memo

```
cetcli tx require-memo -h

Mark if memo is required to receive coins
Usage:
  cetcli tx require-memo <bool> [flags]
```

**举例**

设置地址bob需要设置memo，后续转移token到这个地址的交易都需要添加memo，否则交易无效

> cetcli tx require-memo true --from=bob --fees=2000000000cet --gas=6000000 --memo="Sent with example memo" --chain-id=coinexdex-test1

