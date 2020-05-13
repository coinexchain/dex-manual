# Message Compositions

The message list:

| Module  | Message                                     | Usage                | Note         |
| ------- | ------------------------------------------- | -------------------- | ------------ |
|         | "cosmos-sdk/MsgDelegate"                    | 委托                 |              |
|         | "cosmos-sdk/MsgUndelegate"                  | 取消委托             |              |
|         | "cosmos-sdk/MsgBeginRedelegate"             | 转移委托             |              |
|         | "cosmos-sdk/MsgWithdrawValidatorCommission" | 提取佣金             |              |
|         | "cosmos-sdk/MsgWithdrawDelegationReward"    | 提取委托收益         |              |
|         | "cosmos-sdk/MsgModifyWithdrawAddress"       | 修改收益取回地址     |              |
|         | "cosmos-sdk/MsgCreateValidator"             | 创建验证人           |              |
|         | "cosmos-sdk/MsgEditValidator"               | 修改验证人           |              |
|         | "cosmos-sdk/MsgVerifyInvariant"             | 不变量检查           |              |
|         | "cosmos-sdk/MsgSubmitProposal"              | 发起提案             |              |
|         | "cosmos-sdk/MsgDeposit"                     | 给提案存押金         |              |
|         | "cosmos-sdk/MsgVote"                        | 提案投票             |              |
|         | "cosmos-sdk/MsgUnjail"                      | 赦免                 |              |
| bankx   | "bankx/MsgSend"                             | 发送                 |              |
|         | "bankx/MsgMultiSend"                        | 多重发送             |              |
|         | "bankx/MsgSetMemoRequired"                  | 设置memo是否必须     |              |
|         | "bankx/MsgSupervisedSend"                   | 发送担保交易         |              |
| authx   | "authx/MsgSetReferee"                       | 设置介绍人           | Added in DEX2 |
| distrx  | "distrx/MsgDonateToCommunityPool"           | 向community pool捐赠 |              |
| alias   | "alias/MsgAliasUpdate"                      | 设置别名             |              |
| asset   | "asset/MsgIssueToken"                       | 发行token            |              |
|         | "asset/MsgTransferOwnership"                | 转移token的owner     |              |
|         | "asset/MsgMintToken"                        | token增发            |              |
|         | "asset/MsgBurnToken"                        | token燃烧            |              |
|         | "asset/MsgForbidToken"                      | 冻结token            |              |
|         | "asset/MsgUnForbidToken"                    | 取消冻结token        |              |
|         | "asset/MsgAddTokenWhitelist"                | 加入token白名单      |              |
|         | "asset/MsgRemoveTokenWhitelist"             | 从token白名单移除    |              |
|         | "asset/MsgForbidAddr"                       | 冻结地址token        |              |
|         | "asset/MsgUnForbidAddr"                     | 取消冻结地址token    |              |
|         | "asset/MsgModifyTokenInfo"                  | 修改token信息        | Add New Field in DEX2 |
| market  | "market/MsgCreateTradingPair"               | 创建交易对           |              |
|         | "market/MsgCancelTradingPair"               | 取消交易对           |              |
|         | "market/MsgCreateOrder"                     | 创建订单             |              |
|         | "market/MsgCancelOrder"                     | 取消订单             |              |
|         | "market/MsgModifyPricePrecision"            | 修改价格精度         |              |
| bancor  | "bancorlite/MsgBancorInit"                  | 创建bancor           | Add New Field in DEX2 |
|         | "bancorlite/MsgBancorCancel"                | 取消bancor           |              |
|         | "bancorlite/MsgBancorTrade"                 | 和bancor交易         |              |
| comment | "comment/MsgCommentToken"                   | 对token发表评论      |              |



coin struct

| Field  | Type   | Required | Description |
| ------ | ------ | -------- | ---- |
| amount | string | Yes      | 数额 |
| denom  | string | Yes      | 币种 |

说明：POST接口返回的是未签名交易，POST接口参数参考[swagger文档](https://dex-api.coinex.org/swagger/)。



# Transferring Tokens

## Send

```http 
POST /bank/accounts/{address}/transfers
```

Message Type: bankx/MsgSend

Message Composition

| Field Name   | Type   | Required | Description                                                         |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| from_address | string | Yes      | 发送地址                                                     |
| to_address   | string | Yes      | 接收地址                                                     |
| amount       | coin[] | Yes      | 金额                                                         |
| unlock_time  | string | Yes      | 非转帐锁定填0。非0时表示用户转帐的钱需要在指定时间T后才能再次流通 (the number of seconds elapsed since January 1, 1970 UTC) |

Example:

```json
{
    "type":"bankx/MsgSend",
    "value":{
        "from_address":"coinex14vfsqdh5gef0nya4mxczpddyy3r9ch0lynd462",
        "to_address":"coinex1lgeel9ulwpn3sepne5t7qwjp05kr579h4eyapm",
        "amount":[
            {
                "denom":"abc",
                "amount":"50000000000"
            }
        ],
        "unlock_time":"0"
    }
}
```



## Multiple-Sender-Reciever Send

Message Type: bankx/MsgMultiSend



## Set the attribue of memo-required

```http
POST /bank/accounts/memo
```

Message Type: bankx/MsgSetMemoRequired

Message Composition

| Field    | Type   | Required | Description     |
| -------- | ------ | -------- | -------- |
| address  | string | Yes      | 地址     |
| required | bool   | Yes      | Required |

Example:

```json
{
    "type": "bankx/MsgSetMemoRequired",
    "value": {
        "address": "coinex1w2d73ml30gnfvdvrh3xmtnrampuznczc0vzaey",
        "required": true
    }
}
```



## Send locked coins with a supervisor

```http 
POST /bank/accounts/{address}/supervised_transfers
```

Message Type: bankx/MsgSupervisedSend

Message Composition

| Field Name   | Type   | Required | Description                                                         |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| from_address | string | Yes      | 发送地址                                                     |
| supervisor   | string | optional | 监督人地址                                                   |
| to_address   | string | Yes      | 接收地址                                                     |
| amount       | coin   | Yes      | 金额                                                         |
| unlock_time  | string | Yes      | 非转帐锁定填0。非0时表示用户转帐的钱需要在指定时间T后才能再次流通 (the number of seconds elapsed since January 1, 1970 UTC) |
| reward       | string | optional | 给监督者的奖励                                               |
| operation    | int    | Yes      | 操作类型。0-创建交易，1-退给发送人，2-发送人提前解锁，3-监督人提前解锁 |

Example:

```json
{
    "type":"bankx/MsgSupervisedSend",
    "value":{
        "from_address":"coinex18067tdfjraw3nv0a2her6hdd4vdqlpk5hhg5l8",
        "supervisor":"coinex1v0e3g64xj0cy09zmvj46dh77fz2lfc7ngsyqka",
        "to_address":"coinex1nyrnvaw3a5klq5lpx5mg2jyvkke3jpzcfcgxq9",
        "amount":{
            "denom":"cet",
            "amount":"200000000"
        },
        "unlock_time":"1574334969",
        "reward":"100000000",
        "operation":0
    }
}
```



## Update Aliases

```http
POST /alias/update
```

Message Type: alias/MsgAliasUpdate

Message Composition

| Field Name | Type   | Required | Description               |
| ---------- | ------ | -------- | ------------------ |
| owner      | string | Yes      | owner              |
| alias      | string | Yes      | 别名               |
| is_add     | bool   | Yes      | 设置或删除         |
| as_default | bool   | Yes      | 设置是否为默认别名 |

Example:

```json
{
    "type": "alias/MsgAliasUpdate",
    "value": {
        "alias": "zero",
        "as_default": false,
        "is_add": true,
        "owner": "coinex1gx8dy25uhrmxap37fe094f6ujhd2hq5r02jgrn"
    }
}
```

## Set a Referee (Added in DEX2)

```http
POST /auth/accounts/{address}/referee
```

Message Type: authx/MsgSetReferee

Message Composition

| Field   | Type           | Required | Description       |
| ------- | -------------- | -------- | ---------- |
| sender  | sdk.AccAddress | Yes      | 发送方地址 |
| referee | sdk.AccAddress | Yes      | 介绍人地址 |

Example:

```json
{
  	"type":"authx/MsgSetReferee",
  	"value"：{
  		"sender":"coinex10pd6h8rqsplgn696gw9shygj8c8n2y7hcyj8vg",
  		"referee":"coinex1gx8dy25uhrmxap37fe094f6ujhd2hq5r02jgrn"
		}
}
```



# Staking & Reward-distribution

## Delegate

```http
POST /staking/delegators/{delegatorAddr}/delegations
```

Message Type: cosmos-sdk/MsgDelegate

Message Composition

| Field Name        | Type   | Required | Description       |
| ----------------- | ------ | -------- | ---------- |
| delegator_address | string | Yes      | 委托人地址 |
| validator_address | string | Yes      | 验证人地址 |
| amount            | coin   | Yes      | 委托金额   |

Example:

```json
{
    "type": "cosmos-sdk/MsgDelegate",
    "value": {
        "amount": {
            "amount": "2000000000000",
            "denom": "cet"
        },
        "delegator_address": "coinex10pd6h8rqsplgn696gw9shygj8c8n2y7hcyj8vg",
        "validator_address": "coinexvaloper1lh67wjwx22sdvnupnrprcujgewvx65t7rmfvyz"
    }
}
```



## Unbond

```http
POST /staking/delegators/{delegatorAddr}/unbonding_delegations
```

Message Type: cosmos-sdk/MsgUndelegate

Message Composition

| Field Name        | Type   | Required | Description         |
| ----------------- | ------ | -------- | ------------ |
| delegator_address | string | Yes      | 委托人地址   |
| validator_address | string | Yes      | 验证人地址   |
| amount            | coin   | Yes      | 取消委托金额 |


Example:

```json
{
    "type": "cosmos-sdk/MsgUndelegate",
    "value": {
        "amount": {
            "amount": "500000000000",
            "denom": "cet"
        },
        "delegator_address": "coinex1flnc53efpy6t9qt6fwl4zu03rg76r4fxx77wam",
        "validator_address": "coinexvaloper1pdcf8tdahyvk8h6y7g4hmh9ewhgf5pwrd67g9a"
    }
}
```



## Redelegate

```http
POST /staking/delegators/{delegatorAddr}/redelegations
```

Message Type: cosmos-sdk/MsgBeginRedelegate

Message Composition

| Field Name            | Type   | Required | Description         |
| --------------------- | ------ | -------- | ------------ |
| delegator_address     | string | Yes      | 委托人地址   |
| validator_src_address | string | Yes      | 原验证人地址 |
| validator_dst_address | string | Yes      | 新验证人地址 |
| amount                | coin   | Yes      | 委托金额     |

Example:

```json
{
    "type": "cosmos-sdk/MsgBeginRedelegate",
    "value": {
        "amount": {
            "amount": "500000000000",
            "denom": "cet"
        },
        "delegator_address": "coinex1pmvhd4gl5g4awkgpflr88kjewsd3u4d5tqlchl",
        "validator_dst_address": "coinexvaloper1jkty8qrtrxz7ltw09qp96x6ypcyy6gtemd74t3",
        "validator_src_address": "coinexvaloper1pdcf8tdahyvk8h6y7g4hmh9ewhgf5pwrd67g9a"
    }
}
```



## Withdraw Validator Commission (and rewards for self-delegation)

```http
POST /distribution/validators/{validatorAddr}/rewards
```

Message Type: cosmos-sdk/MsgWithdrawValidatorCommission

Message Composition

| Field Name        | Type   | Required | Description       |
| ----------------- | ------ | -------- | ---------- |
| validator_address | string | Yes      | 验证人地址 |

Example:

```json
{
    "type":"cosmos-sdk/MsgWithdrawValidatorCommission",
    "value":{
        "validator_address":"coinexvaloper1ch57lj4pyfx6mw97nfrpqr6v0xcqshty93eca0"
    }
}
```



## Withdraw rewards for self-delegation

```http
POST /distribution/delegators/{delegatorAddr}/rewards
```

Message Type: cosmos-sdk/MsgWithdrawDelegationReward

Message Composition

| Field Name        | Type   | Required | Description       |
| ----------------- | ------ | -------- | ---------- |
| delegator_address | string | Yes      | 委托人地址 |
| validator_address | string | Yes      | 验证人地址 |

Example:

```json
{
    "type": "cosmos-sdk/MsgWithdrawDelegationReward",
    "value": {
        "delegator_address": "coinex1eefc7unwgku2h6hlsv6yhmd6eavscykegt3y0p",
        "validator_address": "coinexvaloper1eefc7unwgku2h6hlsv6yhmd6eavscykenyjvp4"
    }
}
```



## Modify the address where the rewards go

```http
POST /distribution/delegators/{delegatorAddr}/withdraw_address
```

Message Type: cosmos-sdk/MsgModifyWithdrawAddress

Message Composition

| Field Name        | Type   | Required | Description         |
| ----------------- | ------ | -------- | ------------ |
| delegator_address | string | Yes      | 委托人地址   |
| withdraw_address  | string | Yes      | 收益取回地址 |

Example:

```json
{
    "type": "cosmos-sdk/MsgModifyWithdrawAddress",
    "value": {
        "delegator_address": "coinex1w3wndxggvx90t5cce0zuw7874048tqp5wuwxha",
        "withdraw_address": "coinex1cvh32v0pujf73sr5sqzgy43zkea7mptr6unav4"
    }
}
```



# Valiators

## Create a validator

Message Type: cosmos-sdk/MsgCreateValidator

Message Composition

| Field Name          | Type   | Required | Description             |
| ------------------- | ------ | -------- | ---------------- |
| delegator_address   | string | Yes      | 创建人地址       |
| validator_address   | string | Yes      | 验证人运营地址   |
| pubkey              | string | Yes      | 共识pubkey       |
| value               | coin   | Yes      | 委托金额         |
| min_self_delegation | string | Yes      | 自我委托最小金额 |
| description         | object | Yes      | 描述             |
| commission          | object | Yes      | 佣金设置         |

description struct

| Field Name       | Type   | Required | Description                          |
| ---------------- | ------ | -------- | ----------------------------- |
| moniker          | string | Yes      | 代号                          |
| identity         | string | optional | identity signature(keybase等) |
| website          | string | optional | 网站                          |
| details          | string | optional | 详情                          |
| security_contact | string | optional | 紧急联系人（接口暂未支持）    |

commission struct

| Field Name      | Type   | Required | Description                     |
| --------------- | ------ | -------- | ------------------------ |
| rate            | string | Yes      | 佣金率                   |
| max_rate        | string | Yes      | 最大佣金率               |
| max_change_rate | string | Yes      | 每天最多可以增加的佣金率 |

Example:

```json
{
    "type":"cosmos-sdk/MsgCreateValidator",
    "value":{
        "description":{
            "moniker":"Tester",
            "identity":"CED8F48D8CA69A50",
            "website":"hello.com",
            "details":"world"
        },
        "commission":{
            "rate":"0.100000000000000000",
            "max_rate":"0.200000000000000000",
            "max_change_rate":"0.010000000000000000"
        },
        "min_self_delegation":"50000000000",
        "delegator_address":"coinex127yea8hjqslwm4vzhhnjrzst0a7zqnu9khyvup",
        "validator_address":"coinexvaloper127yea8hjqslwm4vzhhnjrzst0a7zqnu9dc8yj4",
        "pubkey":"coinexvalconspub1zcjduepqqs0mevapk69z3hsvnu96rsxm7drypsfxku3wn2",
        "value":{
            "denom":"cet",
            "amount":"50000000000"
        }
    }
}
```



## Edit some information of a validator

Message Type: cosmos-sdk/MsgEditValidator

Message Composition

| Field Name          | Type   | Required | Description             |
| ------------------- | ------ | -------- | ---------------- |
| address             | string | Yes      | 地址             |
| commission_rate     | string | optional | 佣金率           |
| min_self_delegation | string | optional | 自我委托最小金额 |
| Description         | object | optional | 描述             |

description struct

| Field Name       | Type   | Required | Description                          |
| ---------------- | ------ | -------- | ----------------------------- |
| moniker          | string | Yes      | 代号                          |
| identity         | string | optional | identity signature(keybase等) |
| website          | string | optional | 网站                          |
| details          | string | optional | 详情                          |
| security_contact | string | optional | 紧急联系人（接口暂未支持）    |

Example:

```json
{
    "type":"cosmos-sdk/MsgEditValidator",
    "value":{
        "Description":{
            "moniker":"nodeAAA",
            "identity":"CF1FAAA36A78BE02",
            "website":"www.example.com",
            "details":"DETAILS"
        },
        "address":"coinexvaloper1wguwd8qz74zzqw8v2gltpyt9y04lny205fj7q0",
        "commission_rate":null,
        "min_self_delegation":null
    }
}
```



## Invariant Check

Message Type: cosmos-sdk/MsgVerifyInvariant

Message Composition

| Field Name            | Type   | Required | Description         |
| --------------------- | ------ | -------- | ------------ |
| sender                | string | Yes      | 发送人       |
| invariant_module_name | string | Yes      | 不变量模块名 |
| invariant_route       | string | Yes      | 不变量路径   |

Example:

```json
{
    "type":"cosmos-sdk/MsgVerifyInvariant",
    "value":{
        "sender":"cettest1kft8jenpsnytsk2yz7fnk04ztushzg3ftwe3pg",
        "invariant_module_name":"staking",
        "invariant_route":"module-accounts"
    }
}
```



## Create new proposal

```http
POST /gov/proposals
POST /gov/proposals/param_change
POST /gov/proposals/community_pool_spend
```

Message Type: cosmos-sdk/MsgSubmitProposal

Message Composition

| Field Name      | Type   | Required | Description       |
| --------------- | ------ | -------- | ---------- |
| proposer        | string | Yes      | 提案发起人 |
| initial_deposit | coin[] | Yes      | 初始押金   |
| content         | object | Yes      | 内容       |

content struct

| Field  | Type   | Required | Description     |
| ------ | ------ | -------- | -------- |
| type   | string | Yes      | 提案类型 |
| value  | object | Yes      | 提案详情 |

value struct

| Field       | Type     | Required | Description                               |
| ----------- | -------- | -------- | ---------------------------------- |
| title       | string   | Yes      | 标题                               |
| description | string   | Yes      | 描述                               |
| changes     | object[] | optional | 变更参数（参数修改提案）           |
| recipient   | string   | optional | 接收人（Community Pool花费提案）   |
| amount      | coin     | optional | 接收金额（Community Pool花费提案） |

changes struct

| Field    | Type   | Required | Description       |
| -------- | ------ | -------- | ---------- |
| subspace | string | Yes      | 模块       |
| key      | string | Yes      | 参数key    |
| subkey   | string | optional | 参数subkey |
| value    | any    | Yes      | 参数值     |

Example:

```json
{
    "type":"cosmos-sdk/MsgSubmitProposal",
    "value":{
        "content":{
            "type":"cosmos-sdk/ParameterChangeProposal",
            "value":{
                "title":"ChangeParam",
                "description":"Veto=50%",
                "changes":[
                    {
                        "subspace":"staking",
                        "key":"UnbondingTime",
                        "value":"60000000000"
                    }
                ]
            }
        },
        "initial_deposit":[
            {
                "denom":"cet",
                "amount":"2000000000000"
            }
        ],
        "proposer":"coinex1kx6wdlztzp43x8ayppz0pnlle8whmyu6ah529x"
    }
}
{
    "type":"cosmos-sdk/MsgSubmitProposal",
    "value":{
        "content":{
            "type":"cosmos-sdk/CommunityPoolSpendProposal",
            "value":{
                "title":"CommunityPoolSpend",
                "description":"pay cet",
                "recipient":"coinex1n30sm2qzpu4xssmates9g67kp4hqmew56s098y",
                "amount":[
                    {
                        "denom":"cet",
                        "amount":"300000000000"
                    }
                ]
            }
        },
        "initial_deposit":[
            {
                "denom":"cet",
                "amount":"2000000000000"
            }
        ],
        "proposer":"coinex1gz0fjl26q8k0rccgf2cynx3y69s007x22gu44m"
    }
}
```



## Deposit for a proposal

```http
POST /gov/proposals/{proposalId}/deposits
```

Message Type: cosmos-sdk/MsgDeposit

Message Composition

| Field Name  | Type   | Required | Description       |
| ----------- | ------ | -------- | ---------- |
| depositor   | string | Yes      | 提案发起人 |
| proposal_id | string | Yes      | 提案ID     |
| amount      | coin[] | Yes      | 存入金额   |

Example:

```json
{
    "type":"cosmos-sdk/MsgDeposit",
    "value":{
        "proposal_id":"1",
        "depositor":"coinex1le56qeftjm0f58fychjk3n5xm9st45x6kkcx5y",
        "amount":[
            {
                "denom":"cet",
                "amount":"1100000000000"
            }
        ]
    }
}
```



## Vote for a proposal

```http
POST /gov/proposals/{proposalId}/votes
```

Message Type: bankx/MsgSend

Message Composition

| Field Name  | Type   | Required | Description                                                       |
| ----------- | ------ | -------- | ---------------------------------------------------------- |
| voter       | string | Yes      | 投票人                                                     |
| proposal_id | string | Yes      | 提案ID                                                     |
| option      | string | Yes      | 投票选项有四种选项: `yes`, `no`, `no_with_veto`, `abstain` |

Example:

```json
{
    "type": "cosmos-sdk/MsgVote",
    "value": {
        "option": "Yes",
        "proposal_id": "1",
        "voter": "coinex1k9d4m8nweugvkuldll5xe60zqhdsev47smy0a2"
    }
}
```



## Donate to community pool

```http
POST /distribution/{accAddress}/donates
```

Message Type: distrx/MsgDonateToCommunityPool

Message Composition

| Field     | Type   | Required | Description       |
| --------- | ------ | -------- | ---------- |
| from_addr | string | Yes      | 捐献人地址 |
| amount    | coin[] | Yes      | 捐赠金额   |

Example:

```json
{
    "type": "distrx/MsgDonateToCommunityPool",
    "value": {
        "amount": [
            {
                "amount": "1000000000",
                "denom": "cet"
            }
        ],
        "from_addr": "coinex1mzzy82tr7fc873l2steksfa5sqtmaccljlh5k0"
    }
}
```



# Slash

## Unjail

```http
POST /slashing/validators/{validatorAddr}/unjail
```

Message Type: asset/MsgUnjail

Message Composition

| Field   | Type   | Required | Description       |
| ------- | ------ | -------- | ---------- |
| address | string | Yes      | 验证人地址 |

Example:

```json
{
    "type":"cosmos-sdk/MsgUnjail",
    "value":{
        "address":"coinexvaloper1800j6c8cqk59wnvjppgwk2n98gg3tdrfsvce5c"
    }
}
```



# Token

## Issue tokens

```http
POST /asset/tokens
```

Message Type: asset/MsgIssueToken

Message Composition

| Field Name        | Type   | Required | Description                          |
| ----------------- | ------ | -------- | ----------------------------- |
| symbol            | string | Yes      | 符号                          |
| name              | string | Yes      | 名字                          |
| owner             | string | Yes      | owner                         |
| total_supply      | string | Yes      | 发行量                        |
| mintable          | bool   | Yes      | 是否可增发                    |
| burnable          | bool   | Yes      | 是否可燃烧                    |
| token_forbiddable | bool   | Yes      | 是否可全局冻结                |
| addr_forbiddable  | bool   | Yes      | 是否可根据地址冻结            |
| description       | string |          | 描述                          |
| url               | string |          | URL                           |
| identity          | string |          | identity signature(keybase等) |

Example:

```json
{
    "type": "asset/MsgIssueToken",
    "value": {
        "addr_forbiddable": true,
        "burnable": true,
        "description": "",
        "identity": "",
        "mintable": true,
        "name": "token ABC",
        "owner": "coinex18n93knz2nxqzeftadc70r5t5vvgh7h22h6w9sl",
        "symbol": "abc",
        "token_forbiddable": true,
        "total_supply": "200000000000",
        "url": ""
    }
}
```



## Transfer the ownership of a token

```http
POST /asset/tokens/{symbol}/ownerships
```

Message Type: asset/MsgTransferOwnership

Message Composition

| Field Name     | Type   | Required | Description    |
| -------------- | ------ | -------- | ------- |
| symbol         | string | Yes      | 符号    |
| original_owner | string | Yes      | 原owner |
| new_owner      | string | Yes      | 新owner |

Example:

```json
{
    "type": "asset/MsgTransferOwnership",
    "value": {
        "new_owner": "coinex1z7txtump8gs2326z95x9vz9vyvnx8cqtws0egx",
        "original_owner": "coinex1xw2vagw58stukfcnmjywcj7y8hpfesqqmxtqvq",
        "symbol": "abc"
    }
}
```



## Mint tokens

```http
POST /asset/tokens/{symbol}/mints
```

Message Type: asset/MsgMintToken

Message Composition

| Field Name    | Type   | Required | Description      |
| ------------- | ------ | -------- | --------- |
| symbol        | string | Yes      | 符号      |
| owner_address | string | Yes      | owner地址 |
| amount        | string | Yes      | 数量      |

Example:

```json
{
    "type": "asset/MsgMintToken",
    "value": {
        "amount": "10000000000",
        "owner_address": "coinex1xw2vagw58stukfcnmjywcj7y8hpfesqqmxtqvq",
        "symbol": "abc"
    }
}
```



## Burn tokens

```http
POST /asset/tokens/{symbol}/burns
```

Message Type: asset/MsgBurnToken

Message Composition

| Field Name    | Type   | Required | Description      |
| ------------- | ------ | -------- | --------- |
| symbol        | string | Yes      | 符号      |
| owner_address | string | Yes      | owner地址 |
| amount        | string | Yes      | 数量      |

Example:

```json
{
    "type": "asset/MsgBurnToken",
    "value": {
        "amount": "10000000000",
        "owner_address": "coinex18n93knz2nxqzeftadc70r5t5vvgh7h22h6w9sl",
        "symbol": "token1"
    }
}
```



## Forbid tokens

```http
POST /asset/tokens/{symbol}/forbids
```

Message Type: asset/MsgForbidToken

Message Composition

| Field Name    | Type   | Required | Description      |
| ------------- | ------ | -------- | --------- |
| symbol        | string | Yes      | 符号      |
| owner_address | string | Yes      | owner地址 |

Example:

```json
{
    "type": "asset/MsgForbidToken",
    "value": {
        "owner_address": "coinex1k2qtl2lkr6yqhvraqsj385rl9axf8rewlfthcc",
        "symbol": "abc"
    }
}
```



## Unforbid tokens

```http
POST /asset/tokens/{symbol}/unforbids
```

Message Type: asset/MsgUnForbidToken

Message Composition

| Field Name    | Type   | Required | Description      |
| ------------- | ------ | -------- | --------- |
| symbol        | string | Yes      | 符号      |
| owner_address | string | Yes      | owner地址 |

Example:

```json
{
    "type": "asset/MsgUnForbidToken",
    "value": {
        "owner_address": "coinex1k2qtl2lkr6yqhvraqsj385rl9axf8rewlfthcc",
        "symbol": "abc"
    }
}
```



## Add addresses to whilelist

```http
POST /asset/tokens/{symbol}/forbidden/whitelist
```

Message Type: asset/MsgAddTokenWhitelist

Message Composition

| Field Name    | Type     | Required | Description                |
| ------------- | -------- | -------- | ------------------- |
| symbol        | string   | Yes      | 符号                |
| owner_address | string   | Yes      | owner地址           |
| whitelist     | string[] | Yes      | token白名单地址列表 |

Example:

```json
{
    "type": "asset/MsgAddTokenWhitelist",
    "value": {
        "owner_address": "coinex1fkeczvxf6aydy7cjp6d38nu0vndp60f696l3px",
        "symbol": "abc",
        "whitelist": [
            "coinex1gq9tsf2acfkm89gag5usvs7aqr62f00nf4t0vz"
        ]
    }
}
```



## Remove addresses from whitelist

```http
POST /asset/tokens/{symbol}/unforbidden/whitelist
```

Message Type: asset/MsgRemoveTokenWhitelist

Message Composition

| Field Name    | Type     | Required | Description                        |
| ------------- | -------- | -------- | --------------------------- |
| symbol        | string   | Yes      | 符号                        |
| owner_address | string   | Yes      | owner地址                   |
| whitelist     | string[] | Yes      | 从token白名单移除的地址列表 |

Example:

```json
{
    "type": "asset/MsgRemoveTokenWhitelist",
    "value": {
        "owner_address": "coinex1z7txtump8gs2326z95x9vz9vyvnx8cqtws0egx",
        "symbol": "abc",
        "whitelist": [
            "coinex1xw2vagw58stukfcnmjywcj7y8hpfesqqmxtqvq"
        ]
    }
}
```



## Forbid tokens in an account

```http
POST /asset/tokens/{symbol}/forbidden/addresses
```

Message Type: asset/MsgForbidAddr

Message Composition

| Field Name    | Type     | Required | Description         |
| ------------- | -------- | -------- | ------------ |
| symbol        | string   | Yes      | 符号         |
| owner_address | string   | Yes      | owner地址    |
| addresses     | string[] | Yes      | 冻结地址列表 |

Example:

```json
{
    "type": "asset/MsgForbidAddr",
    "value": {
        "addresses": [
            "coinex1xw2vagw58stukfcnmjywcj7y8hpfesqqmxtqvq",
            "coinex1mxwkeysk743q8dwn45eje9t04gpja6zvew7p62"
        ],
        "owner_address": "coinex1z7txtump8gs2326z95x9vz9vyvnx8cqtws0egx",
        "symbol": "abc"
    }
}
```



## Unforbid tokens in an account

```http
POST /asset/tokens/{symbol}/unforbidden/addresses
```

Message Type: asset/MsgUnForbidAddr

Message Composition

| Field Name    | Type     | Required | Description             |
| ------------- | -------- | -------- | ---------------- |
| symbol        | string   | Yes      | 符号             |
| owner_address | string   | Yes      | owner地址        |
| addresses     | string[] | Yes      | 取消冻结地址列表 |

Example:

```json
{
    "type": "asset/MsgUnForbidAddr",
    "value": {
        "addresses": [
            "coinex1xw2vagw58stukfcnmjywcj7y8hpfesqqmxtqvq"
        ],
        "owner_address": "coinex1z7txtump8gs2326z95x9vz9vyvnx8cqtws0egx",
        "symbol": "abc"
    }
}
```



## Modify some information of a token

```http
POST /asset/tokens/{symbol}/infos
```

Message Type: asset/MsgModifyTokenInfo

Message Composition

| Field Name        | Type   | Required | Description      | Note     |
| ----------------- | ------ | -------- | ---------------- | -------- |
| symbol            | string | Yes      | 符号             |          |
| owner_address     | string | Yes      | owner地址        |          |
| description       | string | optional | 描述             |          |
| url               | string | optional | URL              |          |
| name              | string | optional | 名称             | Added in DEX2 |
| total_supply      | string | optional | 总发行量         | Added in DEX2 |
| mintable          | bool   | optional | 可否增发         | Added in DEX2 |
| burnable          | bool   | optional | 可否燃烧         | Added in DEX2 |
| addr_forbiddable  | bool   | optional | 可否根据地址冻结 | Added in DEX2 |
| token_forbiddable | bool   | optional | 可否全局冻结     | Added in DEX2 |

Example:

```json
{
    "type": "asset/MsgModifyTokenInfo",
    "value": {
        "description": "[do-not-modify]",
        "owner_address": "coinex1z7txtump8gs2326z95x9vz9vyvnx8cqtws0egx",
        "symbol": "abc",
        "url": "www.abc.com"
    }
}
```



## Comment on tokens

```http
POST /comment/new-thread
```


Message Type: comment/MsgCommentToken

Message Composition

| Field Name   | Type     | Required | Description                                                         |
| ------------ | -------- | -------- | ------------------------------------------------------------ |
| token        | string   | Yes      | 被评论的币种                                                 |
| sender       | string   | Yes      | 发送者                                                       |
| title        | string   | Yes      | 标题                                                         |
| content_type | integer  | Yes      | 表示评论的内容是什么类型的，包括：0-IPFS、1-Magnet、2-HTTP、3-UTF8Text、4-ShortHanzi、5-ShortHanziLZ4、6-RawBytes |
| content      | string   | Yes      | 评论的内容，字节数组，最长为16KByte                          |
| donation     | string   |          | 捐赠给Incentive Pool的金额，只能捐赠CET                      |
| references   | object[] | optional | 对另外一些Comment的引用                                      |

Example:

```json
{
    "type": "comment/MsgCommentToken",
    "value": {
        "content": "Y2V0LXRvLXRoZS1tb29u",
        "content_type": 3,
        "donation": "200000000",
        "references": null,
        "sender": "coinex17t0jp03fh5fzxl7qg3ndpuu3rvjjvdq65eq9vs",
        "title": "I-Love-CET",
        "token": "cet"
    }
}
```



# Markets (Trading Pairs)

## Create trading pair

```http
POST /market/trading-pairs
```

Message Type: market/MsgCreateTradingPair

Message Composition

| Field Name      | Type    | Required | Description                                                    |
| --------------- | ------- | -------- | ------------------------------------------------------- |
| creator         | string  | Yes      | 创建者，是stock或money的发行者                          |
| money           | string  | Yes      | money，必须是链上已经被创建的token                      |
| stock           | string  | Yes      | stock，必须是链上已经被创建的token                      |
| price_precision | integer | Yes      | 价格精度，取值为0~18，价格在小数点后保留多少位整数      |
| order_precision | integer | Yes      | 下单精度，下单数量必须是10的order_precision次方的整数倍 |

Example:

```json
{
    "type":"market/MsgCreateTradingPair",
    "value":{
        "stock":"abc",
        "money":"cet",
        "creator":"coinex144u7kv7ehpdndvf42l3vyfg543c62w2h0ks500",
        "price_precision":10,
        "order_precision":0
    }
}
```



## Cancel a trading pair

```http
POST /market/cancel-trading-pair
```

Message Type: market/MsgCancelTradingPair

Message Composition

| Field Name     | Type   | Required | Description                    |
| -------------- | ------ | -------- | ----------------------- |
| sender         | string | Yes      | 发送者                  |
| trading_pair   | string | Yes      | 交易对                  |
| effective_time | string | Yes      | 生效时间戳（单位:纳秒） |

Example:

```json
{
    "type":"market/MsgCancelTradingPair",
    "value":{
        "sender":"coinex144u7kv7ehpdndvf42l3vyfg543c62w2h0ks500",
        "trading_pair":"abc/cet",
        "effective_time":"1575015087000000000"
    }
}
```



## Create new orders

```http
POST /market/gte-orders
POST /market/ioc-orders
```

Message Type: market/MsgCreateOrder

Message Composition

| Field Name      | Type    | Required | Description                                                         |
| --------------- | ------- | -------- | ------------------------------------------------------------ |
| sender          | string  | Yes      | 发送者                                                       |
| identify        | integer | Yes      | 消息在交易内序号                                             |
| trading_pair    | string  | Yes      | 交易对                                                       |
| order_type      | integer | Yes      | 订单类型。2-限价单                                           |
| price_precision | integer | Yes      | 价格精度，取值为0~18，价格在小数点后保留多少位整数。它的取值必须不大于交易对默认的价格精度。默认的价格精度由交易对的创建者所设定。 |
| price           | string  | Yes      | 价格                                                         |
| quantity        | string  | Yes      | 数量                                                         |
| side            | integer | Yes      | 买卖方向：1-买，2-卖                                         |
| time_in_force   | string  | Yes      | 有效时间类型：3-GTE，4-IOC                                   |
| exist_blocks    | string  | Yes      | 有效区块数                                                   |

Example:

```json
{
    "type":"market/MsgCreateOrder",
    "value":{
        "sender":"coinex144u7kv7ehpdndvf42l3vyfg543c62w2h0ks500",
        "identify":1,
        "trading_pair":"abc/cet",
        "order_type":2,
        "price_precision":10,
        "price":"100",
        "quantity":"100000000",
        "side":2,
        "time_in_force":"3",
        "exist_blocks":"100"
    }
}
```



## Cancel orders

```http
POST /market/cancel-order
```

Message Type: market/MsgCancelOrder

Message Composition

| Field    | Type   | Required | Description   |
| -------- | ------ | -------- | ------ |
| order_id | string | Yes      | 订单ID |
| sender   | string | Yes      | 发送者 |

Example:

```json
{
    "type":"market/MsgCancelOrder",
    "value":{
        "sender":"coinex144u7kv7ehpdndvf42l3vyfg543c62w2h0ks500",
        "order_id":"coinex144u7kv7ehpdndvf42l3vyfg543c62w2h0ks500-1025"
    }
}
```



## Change price precision of a trading pair

```http
POST /market/price-precision
```

Message Type: market/MsgModifyPricePrecision

Message Composition

| Field Name      | Type    | Required | Description                                               |
| --------------- | ------- | -------- | -------------------------------------------------- |
| trading_pair    | string  | Yes      | 交易对                                             |
| sender          | string  | Yes      | 发送者                                             |
| price_precision | integer | Yes      | 价格精度，取值为0~18，价格在小数点后保留多少位整数 |

Example:

```json
{
    "type": "market/MsgModifyPricePrecision",
    "value": {
        "price_precision": 12,
        "sender": "coinex143yslcw429y4esa7cwzz2edrz5ulcf5clqzqu7",
        "trading_pair": "abc/cet"
    }
}
```



## Create bancor market

```http
POST /bancorlite/bancor-init
```

Message Type: bancorlite/MsgBancorInit

Message Composition

| Field Name           | Type    | Required | Description                                             | Note     |
| -------------------- | ------- | -------- | ------------------------------------------------------- | -------- |
| owner                | string  | Yes      | owner地址                                               |          |
| money                | string  | Yes      | money                                                   |          |
| stock                | string  | Yes      | stock                                                   |          |
| init_price           | string  | Yes      | 初始价格                                                |          |
| max_price            | string  | Yes      | 最大价格                                                |          |
| max_supply           | string  | Yes      | 最大数量                                                |          |
| earliest_cancel_time | string  | Yes      | 最早可以被取消的时间(戳)                                |          |
| stock_precision      | integer | Yes      | 下单精度，下单数量必须是10的order_precision次方的整数倍 |          |
| max_money            | string  | optional |                                                         | Added in DEX2 |

Example:

```json
{
    "type":"bancorlite/MsgBancorInit",
    "value":{
        "owner":"coinex1ny40lyl8ad209nhy5uuhfsk4gsl0xnspsrmg49",
        "stock":"abc",
        "money":"cet",
        "init_price":"100000000",
        "max_supply":"1000",
        "max_price":"1100000000",
        "stock_precision":2,
        "earliest_cancel_time":"1565687747"
    }
}
```



## Cancel bancor market

```http
POST /bancorlite/bancor-cancel
```

Message Type: bancorlite/MsgBancorCancel

Message Composition

| Field  | Type   | Required | Description      |
| ------ | ------ | -------- | --------- |
| owner  | string | Yes      | owner地址 |
| money  | string | Yes      | money     |
| stock  | string | Yes      | stock     |

Example:

```json
{
    "type": "bancorlite/MsgBancorCancel",
    "value": {
        "money": "cet",
        "owner": "coinex1hl8zmfw8cm6wnxyffpejvjurme55x7u8jq0vdg",
        "stock": "abc"
    }
}
```



## Trade in bancor market

```http
POST /bancorlite/bancor-trade
```

Message Type: bancorlite/MsgBancorTrade

Message Composition

| Field Name  | Type   | Required | Description      |
| ----------- | ------ | -------- | --------- |
| sender      | string | Yes      | 发送者    |
| money       | string | Yes      | money     |
| stock       | string | Yes      | stock     |
| is_buy      | bool   | Yes      | 买或卖    |
| amount      | string | Yes      | 数量      |
| money_limit | string | Yes      | money限制 |

Example:

```json
{
    "type": "bancorlite/MsgBancorTrade",
    "value": {
        "amount": "1",
        "is_buy": true,
        "money": "cet",
        "money_limit": "10000000000",
        "sender": "coinex1hdzf8j29njm57sdkqh53qmuqtwrt3fznyxezsj",
        "stock": "abc"
    }
}
```
