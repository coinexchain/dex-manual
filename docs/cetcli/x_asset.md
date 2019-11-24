# Asset命令

asset模块用于资产发行



## tx命令

```
cetcli tx asset -h
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

这些属性的修改对应了上面的几种子命令

#### 发行token

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

**参数解释**

- name：token的名字
- total-supply：token的总供应量
- mintable：是否可增发
- burnable：是否可燃烧
- addr-forbiddable：是否可以禁止指定地址上该token的流通
- token-forbiddable：是否可以禁止该token的流通
- url：token发行者可以添加一个url
- description：token的描述
- identity：token的identity号码，比如来自keybase的id

例子见上面的命令帮助

#### 转移token所有权

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

该命令转移token的所有权给new-owner参数指定的地址

#### 增发token

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

该命令增发`--amount`的`--symbol`类型token

#### 燃烧token

```
cetcli tx asset burn-token -h

Create and sign a burn token tx, broadcast to nodes.
Example:
$ cetcli tx asset burn-token --symbol="abc" \
	--amount=10000000000000000 \
	--from mykey

Usage:
  cetcli tx asset burn-token [flags]
```

该命令燃烧`--amount`的`--symbol`类型token

#### 禁止整个token的流通

```
cetcli tx asset forbid-token -h

Create and sign a forbid token tx, broadcast to nodes.
Example:
$ cetcli tx asset forbid-token --symbol="abc" \
	--from mykey

Usage:
  cetcli tx asset forbid-token [flags]
```

禁止`--symbol`的流通。

#### 撤销对某个token的流通性禁止

```
cetcli tx asset unforbid-token -h
Create and sign a unforbid token tx, broadcast to nodes.

Example:
$ cetcli tx asset unforbid-token --symbol="abc" \
	--from mykey

Usage:
  cetcli tx asset unforbid-token [flags]
```

撤销对`--symbol`的流通禁止，使其恢复流动性

#### 给token添加白名单

```
cetcli tx asset add-whitelist -h

Create and sign a add-whitelist tx, broadcast to nodes. Multiple addresses separated by commas.
Example:
$ cetcli tx asset add-whitelist --symbol="abc" \
	--whitelist=key,key,key \
	--from mykey

Usage:
  cetcli tx asset add-whitelist [flags]
```

给`--symbol`类型的token添加白名单`--whitelist`，白名单地址用逗号隔开，添加到白名单的地址不被上述的禁止整个token流通的操作限制其流动性。

#### 移除白名单地址

将地址从白名单中移除

```
etcli tx asset remove-whitelist -h

Create and sign a remove-whitelist tx, broadcast to nodes.
				Multiple addresses separated by commas.
Example:
$ cetcli tx asset remove-whitelist --symbol="abc" \
	--whitelist=key,key,key \
	--from mykey

Usage:
  cetcli tx asset remove-whitelist [flags]
```

在`--symbol`类型的token的白名单中去除`--whitelist`指定的地址，意味着这些地址不再享有白名单的特权。

#### 禁止指定地址的流动性

禁止指定地址的流动性，被限制的地址将不能花费它地址中的该token

```
cetcli tx asset forbid-addr -h
Create and sign a forbid-addr tx, broadcast to nodes.
				Multiple addresses separated by commas.

Example:
$ cetcli tx asset forbid-addr --symbol="abc" \
	--addresses=key,key,key \
	--from mykey

Usage:
  cetcli tx asset forbid-addr [flags]
```

限制`--addresses`指定的地址的流动性，无论该地址是否在白名单中，这个限制都是生效的

#### 取消对指定地址的流动性限制

取消对指定地址的流动性限制

```
cetcli tx asset unforbid-addr -h

Create and sign a unforbid-addr tx, broadcast to nodes.
				Multiple addresses separated by commas.
Example:
$ cetcli tx asset unforbid-addr --symbol="abc" \
	--addresses=key,key,key \
	--from mykey

Usage:
  cetcli tx asset unforbid-addr [flags]
```

取消`--addresses`指定地址的流动性限制，这些地址在该笔交易生效后将恢复`--symbol`类型token的流动性

#### 修改token的信息

该命令可以修改token的一些属性，这些属性详见下面的参数解释

```
cetcli tx asset modify-token-info -h

Create and sign a modify token info msg, broadcast to nodes.
Example:
$ cetcli tx asset modify-token-info --symbol="abc" \
	--url="www.abc.com" \
	--description="abc example description" \
	--identity="552A83BA62F9B1F8" \
	--from mykey

Usage:
  cetcli tx asset modify-token-info [flags]
```

**参数解释**

- `--url`: token的url
- `--description`: token的描述
- `--identity`：token的identity



## query命令

```
cetcli query asset -h

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

从上面的帮助中可以看到可供查询的项目，下面依次介绍

#### 查询asset模块参数

```
cetcli query asset params --trust-node=true
{
  "issue_token_fee": "1000000000000",
  "issue_rare_token_fee": "10000000000000"
}
```

参数描述了发币的费用，记得去掉八个零才是真实的花费数额

#### 查询指定token信息

```
cetcli query asset token cet --trust-node=true
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

#### 查询全部token信息

```
cetcli query asset tokens --trust-node=true
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

#### 查询指定token的白名单

```
cetcli query asset whitelist -h

Query whitelist for a token. You can find it by token symbol".
Example:
$ cetcli query asset whitelist abc

Usage:
  cetcli query asset whitelist [symbol] [flags]
```

#### 查询指定token的指定禁止流通地址名单

```
cetcli query asset forbidden-addresses -h

Query forbidden addresses for a token. You can find it by token symbol".
Example:
$ cetcli query asset forbidden-addresses abc

Usage:
  cetcli query asset forbidden-addresses [symbol] [flags]
```

#### 查询系统保留token列表

```
etcli query asset reserved-symbols --trust-node=true
"[btc, eth, xrp, bch, usdt, ltc, eos, bnb, bsv, xlm, trx, ada, xmr, leo, ht, link, xtz, neo, miota, atom, mkr, dash, etc, ont, usdc, cro, xem, bat, doge, vet, zec, pax, hedg, qtum, dcr, zrx, tusd, hot, btg, cennz, vsys, rvn, omg, zb, btm, nano, luna, rep, snx, abbc, algo, ekt, kcs, dai, btt, lsk, bcd, icx, dgb, sc, hc, qnt, kmd, waves, bts, bcn, theta, sxp, kbc, iost, mona, ftt, ae, mco, dx, xvg, maid, nexo, seele, zil, nrg, ardr, aoa, rlc, chz, enj, steem, rif, elf, snt, ren, npxs, gnt, xzc, crpt, new, hpt, ilc, solve, ode, zen, nex, lamb, gxc, etn, matic, xmx, mof, eurs, drg, win, bcv, ela, wtc, mana, strat, dgtx, nas, aion, etp, brd, grin, beam, pai, nuls, knc, tt, tnt, lrc, wicc, cvc, true, ppt, fct, ignis, rdd, dgd, ftm, fet, ark, rcn, gt, wan, fun, tomo, you, ant, r, hbar, eng, waxp, iotx, loom, lina, hyn, dcn, orbs, edc, bhp, busd, qash, man, powr, bnt, c20, dag, dent, storj, mtl, nxs, uni, san, gno, kan, cocos, rox, stream, grs, abt, bix, cs, cmt, edo, one, tel, gas, fx, medx, adn, celr, sys, wxt, erd, wgr, lend, mda, dpt, loki, iris, egt, ont, grin, gnt, atom, qtum, eth, btc, omg, ae, wings, btm, eosc, dcr, loom, olt, neo, icx, bat, etc, kmd, xzc, dash, iota, btt, vet, okb, xtz, link, stx, algo, hot, cnn, tusd, lsk, ltc, zil, xrp, zec, vsys, bch, ada, cmt, cody, nano, zrx, hydro, trx, cet, seele, doge, dot, ftt, bsv, hc, btu, iost, sai, xmr, dero, eos, spice, xvg, spok, ong, rvn, sys, usdh, xem, dgb, bnb, nnb, xlm, waves, ela, ardr, ult, usdt, usdc, lamb, sc, kan, ckb, ht, pax, ctxc, gusd, gram, trtl, vtho, akro, whc, lfc, tct, gas, egt, fcny, usd, all, dzd, ars, amd, aud, azn, bhd, bdt, byn, bmd, bob, bam, brl, bgn, khr, cad, clp, cny, cop, crc, hrk, cup, czk, dkk, dop, egp, eur, gel, ghs, gtq, hnl, hkd, huf, isk, inr, idr, irr, iqd, ils, jmd, jpy, jod, kzt, kes, kwd, kgs, lbp, mkd, myr, mur, mxn, mdl, mnt, mad, mmk, nad, npr, twd, nzd, nio, ngn, nok, omr, pkr, pab, pen, php, pln, gbp, qar, ron, rub, sar, rsd, sgd, zar, krw, ssp, ves, lkr, sek, chf, thb, ttd, tnd, try, ugx, uah, aed, uyu, uzs, vnd, xau, xag, xpt, xpd, libra, cet, rmb, dcep, coinex, viabtc, matrix, bitmain, ]"
```

