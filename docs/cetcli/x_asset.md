# Asset命令

Asset模块用于资产发行和管理。



## tx命令

用于发起资产相关交易，用法：

```
$ cetcli tx asset -h
Asset transactions subcommands

Usage:
  cetcli tx asset [command]

Available Commands:
  issue-token        Create and sign a issue-token tx
  transfer-ownership Create and sign a transfer-ownership tx
  mint-token         Create and sign a mint token tx
  burn-token         Create and sign a burn token tx
  forbid-token       Create and sign a forbid token tx
  unforbid-token     Create and sign a unforbid token tx
  add-whitelist      Create and sign a add-whitelist tx
  remove-whitelist   Create and sign a remove-whitelist tx
  forbid-addr        Create and sign a forbid-addr tx
  unforbid-addr      Create and sign a unforbid-addr tx
  modify-token-info  Modify token info
```

资产具有如下几个属性：

- mintable: 是否可增发
- burnable：是否可燃烧
- addrForbiddable：是否可以禁止指定地址
- tokenForbiddable：是否可以禁止整个token的流动

这些属性的修改对应了上面的几种子命令。



### 发行token

发行新的token，用法：

```
$ cetcli tx asset issue-token -h

Create and sign a issue-token tx, broadcast to nodes.
Example:
$ cetcli tx asset issue-token --name="ABC Token" \
	--symbol="abc" \
	--total-supply=2100000000000000 \
	--mintable=false \
	--burnable=true \
	--addr-forbiddable=false \
	--token-forbiddable=false \
	--url="www.abc.org" \
	--description="token abc is a example token" \
	--identity="552A83BA62F9B1F8" \
	--from mykey

Usage:
  cetcli tx asset issue-token [flags]
```

主要选项：

| 选项名              | 类型（取值范围） | 是否必填 | 默认值 | 说明                                     |
| ------------------- | ---------------- | -------- | ------ | ---------------------------------------- |
| --name              | string           | ✔        |        | token的名字                              |
| --symbol            | string           | ✔        |        | token的symbol                            |
| --total-supply      | int              | ✔        |        | token的总供应量                          |
| --mintable          | bool             | ✔        |        | 是否可增发                               |
| --burnable          | bool             | ✔        |        | 是否可燃烧                               |
| --addr-forbiddable  | bool             | ✔        |        | 是否可以禁止指定地址上该token的流通      |
| --token-forbiddable | bool             | ✔        |        | 是否可以禁止该token的流通                |
| --url               | string           | ✔        |        | token发行者可以添加一个url               |
| --description       | string           | ✔        |        | token的描述                              |
| --identity          | string           | ✔        |        | token的identity号码，比如来自keybase的id |

例子见上面的命令帮助。



### 转移token所有权

该命令转移token的所有权给new-owner参数指定的地址，用法：

```
$ cetcli tx asset transfer-ownership -h

Create and sign a transfer-ownership tx, broadcast to nodes.
Example:
$ cetcli tx asset transfer-ownership --symbol="abc" \
	--new-owner=newkey \
	--from mykey

Usage:
  cetcli tx asset transfer-ownership [flags]
```

主要选项：

| 选项名      | 类型（取值范围） | 是否必填 | 默认值 | 说明          |
| ----------- | ---------------- | -------- | ------ | ------------- |
| --symbol    | string           | ✔        |        | token的symbol |
| --new-owner | string           | ✔        |        | 新owner的地址 |

例子见上面的命令帮助。



### 增发token

增加token的总发行量，用法：

```
$ cetcli tx asset mint-token -h

Create and sign a mint token tx, broadcast to nodes.
Example:
$ cetcli tx asset mint-token --symbol="abc" \
	--amount=10000000000000000 \
	--from mykey

Usage:
  cetcli tx asset mint-token [flags]
```

主要选项：

| 选项名   | 类型（取值范围） | 是否必填 | 默认值 | 说明          |
| -------- | ---------------- | -------- | ------ | ------------- |
| --symbol | string           | ✔        |        | token的symbol |
| --amount | int              | ✔        |        | 增加的发行量  |

例子见上面的命令帮助。



### 燃烧token

将一定数量的token燃烧掉，用法：

```
$ cetcli tx asset burn-token -h

Create and sign a burn token tx, broadcast to nodes.
Example:
$ cetcli tx asset burn-token --symbol="abc" \
	--amount=10000000000000000 \
	--from mykey

Usage:
  cetcli tx asset burn-token [flags]
```

主要选项：

| 选项名   | 类型（取值范围） | 是否必填 | 默认值 | 说明          |
| -------- | ---------------- | -------- | ------ | ------------- |
| --symbol | string           | ✔        |        | token的symbol |
| --amount | int              | ✔        |        | 本次燃烧量    |

例子见上面的命令帮助。



### 禁止token流通

禁止整个token的流通，用法：

```
$ cetcli tx asset forbid-token -h

Create and sign a forbid token tx, broadcast to nodes.
Example:
$ cetcli tx asset forbid-token --symbol="abc" \
	--from mykey

Usage:
  cetcli tx asset forbid-token [flags]
```

主要选项：

| 选项名   | 类型（取值范围） | 是否必填 | 默认值 | 说明          |
| -------- | ---------------- | -------- | ------ | ------------- |
| --symbol | string           | ✔        |        | token的symbol |

例子见上面的命令帮助。



### 允许token流通

撤销对某个token的流通性禁止，用法：

```
$ cetcli tx asset unforbid-token -h
Create and sign a unforbid token tx, broadcast to nodes.

Example:
$ cetcli tx asset unforbid-token --symbol="abc" \
	--from mykey

Usage:
  cetcli tx asset unforbid-token [flags]
```

主要选项：

| 选项名   | 类型（取值范围） | 是否必填 | 默认值 | 说明          |
| -------- | ---------------- | -------- | ------ | ------------- |
| --symbol | string           | ✔        |        | token的symbol |

例子见上面的命令帮助。



### 添加流动性白名单地址

如果token被全局禁止流通，可通过将地址添加到白名单中将该地址设置为例外。用法：

```
$ cetcli tx asset add-whitelist -h

Create and sign a add-whitelist tx, broadcast to nodes. Multiple addresses separated by commas.
Example:
$ cetcli tx asset add-whitelist --symbol="abc" \
	--whitelist=key,key,key \
	--from mykey

Usage:
  cetcli tx asset add-whitelist [flags]
```

主要选项：

| 选项名      | 类型（取值范围） | 是否必填 | 默认值 | 说明                                   |
| ----------- | ---------------- | -------- | ------ | -------------------------------------- |
| --symbol    | string           | ✔        |        | token的symbol                          |
| --whitelist | string           | ✔        |        | 要添加到白名单中的地址列表，用逗号隔开 |

例子见上面的命令帮助。



### 移除流动性白名单地址

将地址从全局禁止流通的白名单中移除，此后这些地址不再享有白名单的特权。用法：

```
$ etcli tx asset remove-whitelist -h

Create and sign a remove-whitelist tx, broadcast to nodes.
				Multiple addresses separated by commas.
Example:
$ cetcli tx asset remove-whitelist --symbol="abc" \
	--whitelist=key,key,key \
	--from mykey

Usage:
  cetcli tx asset remove-whitelist [flags]
```

主要选项：

| 选项名      | 类型（取值范围） | 是否必填 | 默认值 | 说明                                   |
| ----------- | ---------------- | -------- | ------ | -------------------------------------- |
| --symbol    | string           | ✔        |        | token的symbol                          |
| --whitelist | string           | ✔        |        | 要从白名单中删除的地址列表，用逗号隔开 |

例子见上面的命令帮助。



### 禁止地址的流动性

禁止指定地址的流动性，被限制的地址将不能花费它地址中的该token（无论该地址是否在白名单中，这个限制都是生效的）。用法：

```
$ cetcli tx asset forbid-addr -h
Create and sign a forbid-addr tx, broadcast to nodes.
				Multiple addresses separated by commas.

Example:
$ cetcli tx asset forbid-addr --symbol="abc" \
	--addresses=key,key,key \
	--from mykey

Usage:
  cetcli tx asset forbid-addr [flags]
```

主要选项：

| 选项名      | 类型（取值范围） | 是否必填 | 默认值 | 说明                             |
| ----------- | ---------------- | -------- | ------ | -------------------------------- |
| --symbol    | string           | ✔        |        | token的symbol                    |
| --addresses | string           | ✔        |        | 将要被禁止的地址列表，用逗号隔开 |

例子见上面的命令帮助。



### 恢复地址的流动性

取消对指定地址的流动性限制，这些地址在该笔交易生效后将恢复token流动性。用法：

```
$ cetcli tx asset unforbid-addr -h

Create and sign a unforbid-addr tx, broadcast to nodes.
				Multiple addresses separated by commas.
Example:
$ cetcli tx asset unforbid-addr --symbol="abc" \
	--addresses=key,key,key \
	--from mykey

Usage:
  cetcli tx asset unforbid-addr [flags]
```

主要选项：

| 选项名      | 类型（取值范围） | 是否必填 | 默认值 | 说明                                 |
| ----------- | ---------------- | -------- | ------ | ------------------------------------ |
| --symbol    | string           | ✔        |        | token的symbol                        |
| --addresses | string           | ✔        |        | 将要被取消禁止的地址列表，用逗号隔开 |

例子见上面的命令帮助。



### 修改token的信息

该命令可以修改token的一些属性，这些属性详见下面的参数解释。用法：

```
$ cetcli tx asset modify-token-info -h

Create and sign a modify token info msg, broadcast to nodes.
Example:
$ cetcli tx asset modify-token-info --symbol="abc" \
	--name="ABC Token" \
	--total-supply=2100000000000000 \
	--mintable=false \
	--burnable=true \
	--addr-forbiddable=false \
	--token-forbiddable=false \
	--url="www.abc.com" \
	--description="abc example description" \
	--identity="552A83BA62F9B1F8" \
	--from mykey

Usage:
  cetcli tx asset modify-token-info [flags]
```

主要选项：

| 选项名              | 类型（取值范围） | 是否必填 | 默认值            | 说明                                | 何时可修改               |
| ------------------- | ---------------- | -------- | ----------------- | ----------------------------------- | ------------------------ |
| --symbol            | string           | ✔        |                   | token的symbol                       | 不可修改                 |
| --name              | string           |          | "[do-not-modify]" | 新的token名字                       | 扩散前                   |
| --total-supply      | string           |          | "[do-not-modify]" | 新的token的总供应量                 | 扩散前                   |
| --mintable          | string           |          | "[do-not-modify]" | 是否可增发                          | 扩散前；扩散后有条件修改 |
| --burnable          | string           |          | "[do-not-modify]" | 是否可燃烧                          | 扩散前；扩散后有条件修改 |
| --addr-forbiddable  | string           |          | "[do-not-modify]" | 是否可以禁止指定地址上该token的流通 | 扩散前；扩散后有条件修改 |
| --token-forbiddable | string           |          | "[do-not-modify]" | 是否可以禁止该token的流通           | 扩散前；扩散后有条件修改 |
| --url               | string           |          | "[do-not-modify]" | 新的url                             | 总是                     |
| --description       | string           |          | "[do-not-modify]" | 重新设置token的描述                 | 总是                     |
| --identity          | string           |          | "[do-not-modify]" | 重新设置token的identity号码         | 总是                     |

例子见上面的命令帮助。特别说明，如果taken已经扩散，则：

* mintable只能由true变为false；且token的TotalMint状态将被清零。
* burnable只能由fasle变为true；且token的TotalBurn状态将被清零。
* addr-forbiddable只能由true变为false；且token的禁止流通黑名单将被情况。
* token-forbiddable只能由true变为false；且token的IsForbidden状态将被重置为false。



## query命令

用于查询已发行资产的相关状态，用法：

```
$ cetcli query asset -h

Querying commands for the asset module
Usage:
  cetcli query asset [command]

Available Commands:
  params              Query asset params
  token               Query token info
  tokens              Query all token
  whitelist           Query whitelist
  forbidden-addresses Query forbidden addresses
  reserved-symbols    Query reserved symbols
```

从上面的帮助中可以看到可供查询的项目，下面依次介绍。



### 查询asset模块参数

用法：

```
$ cetcli query asset params -h
Query asset params

Usage:
  cetcli query asset params [flags]
```

示例：

```
$ cetcli query asset params --trust-node=true
{
  "issue_token_fee": "1000000000000",
  "issue_rare_token_fee": "10000000000000"
}
```

参数描述了发币的费用，记得去掉八个零才是真实的花费数额。



### 查询全部token信息

用法：

```
$ cetcli query asset tokens -h
Query details for all tokens. 
Example:
$ cetcli query asset tokens

Usage:
  cetcli query asset tokens  [flags]
```

示例：

```
$ cetcli query asset tokens --trust-node=true
[
  {
    "type": "asset/BaseToken",
    "value": {
      "name": "CoinEx Chain Native Token",
      "symbol": "cet",
      "total_supply": "586884903761317189",
      "send_lock": "0",
      "owner": "coinex1n6jzlyd48p9wpfuta6dwlxc6w3a4h6q39nkcar",
      "mintable": false,
      "burnable": true,
      "addr_forbiddable": false,
      "token_forbiddable": false,
      "total_burn": "413115096238682811",
      "total_mint": "0",
      "is_forbidden": false,
      "url": "www.coinex.org",
      "description": "A public chain built for the decentralized exchange",
      "identity": "552A83BA62F9B1F8"
    }
  }
]
```



### 查询指定token信息

用法：

```
$ cetcli query asset token -h
Query details for a token. You can find the token by token symbol".

Example:
$ cetcli query asset token abc

Usage:
  cetcli query asset token [symbol] [flags]
```

参数：

| 参数  | 类型   | 是否必填 | 默认值 | 说明   |
| ----- | ------ | -------- | ------ | ------ |
| 参数1 | string | ✔        |        | 交易对 |

示例：

```
$ cetcli query asset token cet --trust-node=true
{
  "type": "asset/BaseToken",
  "value": {
    "name": "CoinEx Chain Native Token",
    "symbol": "cet",
    "total_supply": "586884903761317189",
    "send_lock": "0",
    "owner": "coinex1n6jzlyd48p9wpfuta6dwlxc6w3a4h6q39nkcar",
    "mintable": false,
    "burnable": true,
    "addr_forbiddable": false,
    "token_forbiddable": false,
    "total_burn": "413115096238682811",
    "total_mint": "0",
    "is_forbidden": false,
    "url": "www.coinex.org",
    "description": "A public chain built for the decentralized exchange",
    "identity": "552A83BA62F9B1F8"
  }
}
```

返回的字段对应上面tx命令中发行token命令的字段。



### 查询指定token的白名单

用法：

```
$ cetcli query asset whitelist -h
Query whitelist for a token. You can find it by token symbol".

Example:
$ cetcli query asset whitelist abc

Usage:
  cetcli query asset whitelist [symbol] [flags]
```

参数：

| 参数  | 类型   | 是否必填 | 默认值 | 说明   |
| ----- | ------ | -------- | ------ | ------ |
| 参数1 | string | ✔        |        | 交易对 |

示例：

```
$ cetcli query asset whitelist -h

Query whitelist for a token. You can find it by token symbol".
Example:
$ cetcli query asset whitelist abc

Usage:
  cetcli query asset whitelist [symbol] [flags]
```



### 查询指定token的黑名单

用法：

```
$ cetcli query asset forbidden-addresses -h
Query forbidden addresses for a token. You can find it by token symbol".

Example:
$ cetcli query asset forbidden-addresses abc

Usage:
  cetcli query asset forbidden-addresses [symbol] [flags]
```

参数：

| 参数  | 类型   | 是否必填 | 默认值 | 说明   |
| ----- | ------ | -------- | ------ | ------ |
| 参数1 | string | ✔        |        | 交易对 |

示例：

```
$ cetcli query asset forbidden-addresses -h

Query forbidden addresses for a token. You can find it by token symbol".
Example:
$ cetcli query asset forbidden-addresses abc

Usage:
  cetcli query asset forbidden-addresses [symbol] [flags]
```



### 查询系统保留token列表

用法：

```
$ cetcli query asset reserved-symbols -h
Query reserved symbols list".

Example:
$ cetcli query asset reserved-symbols

Usage:
  cetcli query asset reserved-symbols [flags]
```

示例：

```
$ etcli query asset reserved-symbols --trust-node=true
"[btc, eth, xrp, bch, usdt, ltc, eos, bnb, bsv, xlm, trx, ada, xmr, leo, ht, link, xtz, neo, miota, atom, mkr, dash, etc, ont, usdc, cro, xem, bat, doge, vet, zec, pax, hedg, qtum, dcr, zrx, tusd, hot, btg, cennz, vsys, rvn, omg, zb, btm, nano, luna, rep, snx, abbc, algo, ekt, kcs, dai, btt, lsk, bcd, icx, dgb, sc, hc, qnt, kmd, waves, bts, bcn, theta, sxp, kbc, iost, mona, ftt, ae, mco, dx, xvg, maid, nexo, seele, zil, nrg, ardr, aoa, rlc, chz, enj, steem, rif, elf, snt, ren, npxs, gnt, xzc, crpt, new, hpt, ilc, solve, ode, zen, nex, lamb, gxc, etn, matic, xmx, mof, eurs, drg, win, bcv, ela, wtc, mana, strat, dgtx, nas, aion, etp, brd, grin, beam, pai, nuls, knc, tt, tnt, lrc, wicc, cvc, true, ppt, fct, ignis, rdd, dgd, ftm, fet, ark, rcn, gt, wan, fun, tomo, you, ant, r, hbar, eng, waxp, iotx, loom, lina, hyn, dcn, orbs, edc, bhp, busd, qash, man, powr, bnt, c20, dag, dent, storj, mtl, nxs, uni, san, gno, kan, cocos, rox, stream, grs, abt, bix, cs, cmt, edo, one, tel, gas, fx, medx, adn, celr, sys, wxt, erd, wgr, lend, mda, dpt, loki, iris, egt, ont, grin, gnt, atom, qtum, eth, btc, omg, ae, wings, btm, eosc, dcr, loom, olt, neo, icx, bat, etc, kmd, xzc, dash, iota, btt, vet, okb, xtz, link, stx, algo, hot, cnn, tusd, lsk, ltc, zil, xrp, zec, vsys, bch, ada, cmt, cody, nano, zrx, hydro, trx, cet, seele, doge, dot, ftt, bsv, hc, btu, iost, sai, xmr, dero, eos, spice, xvg, spok, ong, rvn, sys, usdh, xem, dgb, bnb, nnb, xlm, waves, ela, ardr, ult, usdt, usdc, lamb, sc, kan, ckb, ht, pax, ctxc, gusd, gram, trtl, vtho, akro, whc, lfc, tct, gas, egt, fcny, usd, all, dzd, ars, amd, aud, azn, bhd, bdt, byn, bmd, bob, bam, brl, bgn, khr, cad, clp, cny, cop, crc, hrk, cup, czk, dkk, dop, egp, eur, gel, ghs, gtq, hnl, hkd, huf, isk, inr, idr, irr, iqd, ils, jmd, jpy, jod, kzt, kes, kwd, kgs, lbp, mkd, myr, mur, mxn, mdl, mnt, mad, mmk, nad, npr, twd, nzd, nio, ngn, nok, omr, pkr, pab, pen, php, pln, gbp, qar, ron, rub, sar, rsd, sgd, zar, krw, ssp, ves, lkr, sek, chf, thb, ttd, tnd, try, ugx, uah, aed, uyu, uzs, vnd, xau, xag, xpt, xpd, libra, cet, rmb, dcep, coinex, viabtc, matrix, bitmain, ]"
```

