# Asset Command

The asset module is used to issue and manage tokens.


## tx command

The sub commands under `tx` are used to issue and manage tokens:

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

An asset has following permissions (attributes):

- mintable: whether new tokens can be minted.
- burnable: whether existing tokens can be burned.
- addrForbiddable: whether the token owner can forbid accounts.
- tokenForbiddable: whether the token owner can forbid this token (all transferring are disallowd).

The permissions determine whether some of the above commands can be used.


### Issue a new token

To issue a new token:

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

Major Options：

| Option              | Type             | Required | Default Value | Comment                       |
| ------------------- | ---------------- | -------- | --------------| --------------------------------------- |
| --name              | string           | ✔        |             | token's name                              |
| --symbol            | string           | ✔        |             | token's symbol                            |
| --total-supply      | int              | ✔        |             | token's total supply                      |
| --mintable          | bool             | ✔        |             | token's mintable attribute                |
| --burnable          | bool             | ✔        |             | token's burnable attribute                |
| --addr-forbiddable  | bool             | ✔        |             | token's addrForbiddable attribute      |
| --token-forbiddable | bool             | ✔        |             | token's tokenForbiddable attribute      |
| --url               | string           | ✔        |             | A url introducing this token             |
| --description       | string           | ✔        |             | token's short description               |
| --identity          | string           | ✔        |             | token's logo, which is a keybase id     |


### Transfer the ownership of a token

This command transfers the ownership of a token to the address specified by the "new-owner" parameter:

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

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------ |
| --symbol         | string           | ✔        |               | token's symbol                |
| --new-owner      | string           | ✔        |               | the address of token's owner | 


### Mint token

Mint new coins to increase the total supply of some asset:

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

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------ |
| --symbol         | string           | ✔        |               | token's symbol                |
| --amount         | int              | ✔        |               | amount of newly-mint tokens   | 



### Burn token

Burn (destroy) given amount of coins:

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

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------ |
| --symbol         | string           | ✔        |               | token's symbol                |
| --amount         | int              | ✔        |               | amount of burned tokens   | 



### Forbid a token (make it has no liquidity)

To forbid a given token:

```
$ cetcli tx asset forbid-token -h

Create and sign a forbid token tx, broadcast to nodes.
Example:
$ cetcli tx asset forbid-token --symbol="abc" \
	--from mykey

Usage:
  cetcli tx asset forbid-token [flags]
```

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------ |
| --symbol         | string           | ✔        |               | token's symbol                |



### Unforbid a token (undo a former forbid operation)

To cancel the forbidden status of a token:

```
$ cetcli tx asset unforbid-token -h
Create and sign a unforbid token tx, broadcast to nodes.

Example:
$ cetcli tx asset unforbid-token --symbol="abc" \
	--from mykey

Usage:
  cetcli tx asset unforbid-token [flags]
```

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------ |
| --symbol         | string           | ✔        |               | token's symbol                |


### Add an address to whitelist (when a token is forbidden)

When a token is forbidden, the address(es) in the white list can still send this token.

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

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------ |
| --symbol         | string           | ✔        |               | token's symbol                |
| --whitelist      | string           | ✔        |               | Several addresses which will be added to whitelist, seperated by commas |



### Remove an address from whitelist (when a token is forbidden)

To remove addresses from white list, such that they can no longer send a foridden token.

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

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------ |
| --symbol         | string           | ✔        |               | token's symbol                |
| --whitelist      | string           | ✔        |               | Several addresses which will be added to whitelist, seperated by commas |



### Forbid addresses to send a given token (lost their liquidity)

A token owner can forbid an account to send the tokens in it. When this account is forbidden, even if it is in the whitelist, it still can not send the token.

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

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------ |
| --symbol         | string           | ✔        |               | token's symbol                |
| --addresses      | string           | ✔        |               | Several addresses which will be added to whitelist, seperated by commas |



### Unforbid addresses to send a given token (cancel forbid operation)

Move addresses out from the forbidden list, such that they can send the token.

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

Major options:

| Option           | Type             | Required | Default Value | Comment                       |
| ---------------- | ---------------- | -------- | ------------- | ------------------------------ |
| --symbol         | string           | ✔        |               | token's symbol                |
| --addresses      | string           | ✔        |               | Several addresses which will be added to whitelist, seperated by commas |


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
| --symbol            | string           | ✔        |                   |  token's symbol                     | Can not modify           |
| --name              | string           |          | "[do-not-modify]" |  token's name                       | Can only be modified before dispersing   |
| --total-supply      | string           |          | "[do-not-modify]" |  token's total supply               | Can only be modified before dispersing   |
| --mintable          | string           |          | "[do-not-modify]" |  token's mintable attribute         | See below |
| --burnable          | string           |          | "[do-not-modify]" |  token's burnable attribute         | See below |
| --addr-forbiddable  | string           |          | "[do-not-modify]" |  token's addrForbiddable attribute  | See below |
| --token-forbiddable | string           |          | "[do-not-modify]" |  token's tokenForbiddable attribute | See below |
| --url               | string           |          | "[do-not-modify]" |  A url introducing this token       | Can always be modified   |
| --description       | string           |          | "[do-not-modify]" |  token's short description          | Can always be modified   |
| --identity          | string           |          | "[do-not-modify]" |  token's logo, which is a keybase id| Can always be modified   |

The four attributes can be freely modified if the token is not dispersed (all the supply is owned by the token owner). But if the token is dispersed, the attributes can only modified in a limited way, as is shown below:

* mintable can only be changed from true to false; when it's changed, token's TotalMint will be cleared to zero.
* burnable can only be changed from fasle to true; when it's changed, token's TotalBurn will be cleared to zero.
* addr-forbiddable can only be changed from true to false; when it's changed, token's forbidden list will be cleared to empty.
* token-forbiddable can only be changed from true to false; when it's changed, token's IsForbidden status will be reset to false.


## query command

To query the status of a token:

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


### Query the parameters of the asset module

Usage:

```
$ cetcli query asset params -h
Query asset params

Usage:
  cetcli query asset params [flags]
```

Example:

```
$ cetcli query asset params --trust-node=true
{
  "issue_rare_token_fee ": "1000000000000",
  "issue_3char_token_fee": "100000000000",
  "issue_4char_token_fee": "50000000000",
  "issue_5char_token_fee": "20000000000",
  "issue_6char_token_fee": "10000000000",
  "issue_token_fee ": "5000000000"
}
```

Feature fees for issuing token symbol length:

| Parameter Name      | Meaning                    | Value   |
| ------------------- | -------------------------- | ---------- |
| IssueRareTokenFee    | Feature fee for issuing a token with length=2 | 1_0000CET |
| Issue3CharTokenFee   | Feature fee for issuing a token with length=3 | 1000CET  |
| Issue4CharTokenFee   | Feature fee for issuing a token with length=4 | 500CET    |
| Issue5CharTokenFee   | Feature fee for issuing a token with length=5 | 200CET    |
| Issue6CharTokenFee   | Feature fee for issuing a token with length=6 | 100CET    |
| IssueNonRareTokenFee | Feature fee for issuing a token with length>=7 | 50CET     |




### Query all the tokens

Usage:

```
$ cetcli query asset tokens -h
Query details for all tokens. 
Example:
$ cetcli query asset tokens

Usage:
  cetcli query asset tokens  [flags]
```

Example:

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



### Query information about a token

Usage:

```
$ cetcli query asset token -h
Query details for a token. You can find the token by token symbol".

Example:
$ cetcli query asset token abc

Usage:
  cetcli query asset token [symbol] [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | token name      | string | ✔        |               | The token to be queried |


Example:

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



### Query the whitelist of a given token

Usage：

```
$ cetcli query asset whitelist -h
Query whitelist for a token. You can find it by token symbol".

Example:
$ cetcli query asset whitelist abc

Usage:
  cetcli query asset whitelist [symbol] [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | token name      | string | ✔        |               | The token to be queried |

Examples:

```
$ cetcli query asset whitelist -h

Query whitelist for a token. You can find it by token symbol".
Example:
$ cetcli query asset whitelist abc

Usage:
  cetcli query asset whitelist [symbol] [flags]
```



### Query the forbidden list of a given token

Usage:

```
$ cetcli query asset forbidden-addresses -h
Query forbidden addresses for a token. You can find it by token symbol".

Example:
$ cetcli query asset forbidden-addresses abc

Usage:
  cetcli query asset forbidden-addresses [symbol] [flags]
```

Parameter:

| Parameter   | Name            | Type   | Required | Default Value | Comment                    |
| ------------| --------------- | ------ | -------- | ------------  | -------------------------- |
| Parameter 1 | token name      | string | ✔        |               | The token to be queried |

Examples:

```
$ cetcli query asset forbidden-addresses -h

Query forbidden addresses for a token. You can find it by token symbol".
Example:
$ cetcli query asset forbidden-addresses abc

Usage:
  cetcli query asset forbidden-addresses [symbol] [flags]
```



### Query the reserved token list

Some tokens are reserved. Only the owner of cet can issue them.

```
$ cetcli query asset reserved-symbols -h
Query reserved symbols list".

Example:
$ cetcli query asset reserved-symbols

Usage:
  cetcli query asset reserved-symbols [flags]
```

Example:

```
$ etcli query asset reserved-symbols --trust-node=true
"[btc, eth, xrp, bch, usdt, ltc, eos, bnb, bsv, xlm, trx, ada, xmr, leo, ht, link, xtz, neo, miota, atom, mkr, dash, etc, ont, usdc, cro, xem, bat, doge, vet, zec, pax, hedg, qtum, dcr, zrx, tusd, hot, btg, cennz, vsys, rvn, omg, zb, btm, nano, luna, rep, snx, abbc, algo, ekt, kcs, dai, btt, lsk, bcd, icx, dgb, sc, hc, qnt, kmd, waves, bts, bcn, theta, sxp, kbc, iost, mona, ftt, ae, mco, dx, xvg, maid, nexo, seele, zil, nrg, ardr, aoa, rlc, chz, enj, steem, rif, elf, snt, ren, npxs, gnt, xzc, crpt, new, hpt, ilc, solve, ode, zen, nex, lamb, gxc, etn, matic, xmx, mof, eurs, drg, win, bcv, ela, wtc, mana, strat, dgtx, nas, aion, etp, brd, grin, beam, pai, nuls, knc, tt, tnt, lrc, wicc, cvc, true, ppt, fct, ignis, rdd, dgd, ftm, fet, ark, rcn, gt, wan, fun, tomo, you, ant, r, hbar, eng, waxp, iotx, loom, lina, hyn, dcn, orbs, edc, bhp, busd, qash, man, powr, bnt, c20, dag, dent, storj, mtl, nxs, uni, san, gno, kan, cocos, rox, stream, grs, abt, bix, cs, cmt, edo, one, tel, gas, fx, medx, adn, celr, sys, wxt, erd, wgr, lend, mda, dpt, loki, iris, egt, ont, grin, gnt, atom, qtum, eth, btc, omg, ae, wings, btm, eosc, dcr, loom, olt, neo, icx, bat, etc, kmd, xzc, dash, iota, btt, vet, okb, xtz, link, stx, algo, hot, cnn, tusd, lsk, ltc, zil, xrp, zec, vsys, bch, ada, cmt, cody, nano, zrx, hydro, trx, cet, seele, doge, dot, ftt, bsv, hc, btu, iost, sai, xmr, dero, eos, spice, xvg, spok, ong, rvn, sys, usdh, xem, dgb, bnb, nnb, xlm, waves, ela, ardr, ult, usdt, usdc, lamb, sc, kan, ckb, ht, pax, ctxc, gusd, gram, trtl, vtho, akro, whc, lfc, tct, gas, egt, fcny, usd, all, dzd, ars, amd, aud, azn, bhd, bdt, byn, bmd, bob, bam, brl, bgn, khr, cad, clp, cny, cop, crc, hrk, cup, czk, dkk, dop, egp, eur, gel, ghs, gtq, hnl, hkd, huf, isk, inr, idr, irr, iqd, ils, jmd, jpy, jod, kzt, kes, kwd, kgs, lbp, mkd, myr, mur, mxn, mdl, mnt, mad, mmk, nad, npr, twd, nzd, nio, ngn, nok, omr, pkr, pab, pen, php, pln, gbp, qar, ron, rub, sar, rsd, sgd, zar, krw, ssp, ves, lkr, sek, chf, thb, ttd, tnd, try, ugx, uah, aed, uyu, uzs, vnd, xau, xag, xpt, xpd, libra, cet, rmb, dcep, coinex, viabtc, matrix, bitmain, ]"
```

