# 消息结构说明

目前支持的消息类型：

| 模块    | 消息                                         | 用途             | 备注 |
| ------- | ------------------------------------------- | ---------------- | ------- |
|         | "cosmos-sdk/MsgDelegate"                    | 委托             |  |
|         | "cosmos-sdk/MsgUndelegate"                  | 取消委托         |  |
|         | "cosmos-sdk/MsgBeginRedelegate"             | 转移委托         |  |
|         | "cosmos-sdk/MsgWithdrawValidatorCommission" | 提取佣金         |  |
|         | "cosmos-sdk/MsgWithdrawDelegationReward"    | 提取委托收益     |  |
|         | "cosmos-sdk/MsgModifyWithdrawAddress"       | 修改收益取回地址 |  |
|         | "cosmos-sdk/MsgCreateValidator"             | 创建验证人       |  |
|         | "cosmos-sdk/MsgEditValidator"               | 修改验证人       |  |
|         | "cosmos-sdk/MsgVerifyInvariant"             | 不变量检查       |  |
|         | "cosmos-sdk/MsgSubmitProposal"              | 发起提案 |  |
|         | "cosmos-sdk/MsgDeposit"                     | 给提案存押金 |  |
|         | "cosmos-sdk/MsgVote"                        | 提案投票 |  |
|         | "cosmos-sdk/MsgUnjail"                      | 赦免            |  |
| bankx   | "bankx/MsgSend"                             | 发送             |  |
|         | "bankx/MsgMultiSend"                        | 多重发送         |  |
|         | "bankx/MsgSetMemoRequired"                  | 设置memo是否必须 |  |
|         | "bankx/MsgSupervisedSend"                   | 发送担保交易     |  |
| authx   | "authx/MsgSetReferee"                       | 设置介绍人       | DEX2新增 |
| distrx  | "distrx/MsgDonateToCommunityPool"           | 向community pool捐赠 |  |
| alias   | "alias/MsgAliasUpdate"                      | 设置别名         |  |
| asset   | "asset/MsgIssueToken"                       | 发行token|  |
|         | "asset/MsgTransferOwnership"                | 转移token的owner|  |
|         | "asset/MsgMintToken"                        | token增发|  |
|         | "asset/MsgBurnToken"                        | token燃烧|  |
|         | "asset/MsgForbidToken"                      | 冻结token|  |
|         | "asset/MsgUnForbidToken"                    | 取消冻结token|  |
|         | "asset/MsgAddTokenWhitelist"                | 加入token白名单|  |
|         | "asset/MsgRemoveTokenWhitelist"             | 从token白名单移除|  |
|         | "asset/MsgForbidAddr"                       | 冻结地址token|  |
|         | "asset/MsgUnForbidAddr"                     | 取消冻结地址token|  |
|         | "asset/MsgModifyTokenInfo"                  | 修改token信息|  |
| market  | "market/MsgCreateTradingPair"               | 创建交易对|  |
|         | "market/MsgCancelTradingPair"               | 取消交易对|  |
|         | "market/MsgCreateOrder"                     | 创建订单|  |
|         | "market/MsgCancelOrder"                     | 取消订单|  |
|         | "market/MsgModifyPricePrecision"            | 修改价格精度|  |
| bancor  | "bancorlite/MsgBancorInit"                  | 创建bancor|  |
|         | "bancorlite/MsgBancorCancel"                | 取消bancor|  |
|         | "bancorlite/MsgBancorTrade"                 | 和bancor交易|  |
| comment | "comment/MsgCommentToken"                   | 对token发表评论|  |



coin结构

| 字段名 | 类型   | 是否必须 | 描述 |
| ------ | ------ | -------- | ---- |
| amount | string | required | 数额 |
| denom  | string | required | 币种 |

说明：POST接口返回的是未签名交易，POST接口参数参考[swagger文档](https://dex-api.coinex.org/swagger/)。



# 转账

## 发送

```http 
POST /bank/accounts/{address}/transfers
```

消息类型：bankx/MsgSend

消息结构

| 字段名       | 类型   | 是否必须 | 描述                                                         |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| from_address | string | required | 发送地址                                                     |
| to_address   | string | required | 接收地址                                                     |
| amount       | coin[] | required | 金额                                                         |
| unlock_time  | string | required | 非转帐锁定填0。非0时表示用户转帐的钱需要在指定时间T后才能再次流通 (the number of seconds elapsed since January 1, 1970 UTC) |

示例

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



## 多重发送

消息类型：bankx/MsgMultiSend



## 设置memo是否必须

```http
POST /bank/accounts/memo
```

消息类型：bankx/MsgSetMemoRequired

消息结构

| 字段名   | 类型   | 是否必须 | 描述     |
| -------- | ------ | -------- | -------- |
| address  | string | required | 地址     |
| required | bool   | required | 是否必须 |

示例

```json
{
    "type": "bankx/MsgSetMemoRequired",
    "value": {
        "address": "coinex1w2d73ml30gnfvdvrh3xmtnrampuznczc0vzaey",
        "required": true
    }
}
```



## 发送担保交易

```http 
POST /bank/accounts/{address}/supervised_transfers
```

消息类型：bankx/MsgSupervisedSend

消息结构

| 字段名       | 类型   | 是否必须 | 描述                                                         |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| from_address | string | required | 发送地址                                                     |
| supervisor   | string | optional | 监督人地址                                                   |
| to_address   | string | required | 接收地址                                                     |
| amount       | coin   | required | 金额                                                         |
| unlock_time  | string | required | 非转帐锁定填0。非0时表示用户转帐的钱需要在指定时间T后才能再次流通 (the number of seconds elapsed since January 1, 1970 UTC) |
| reward       | string | optional | 给监督者的奖励                                               |
| operation    | int    | required | 操作类型。0-创建交易，1-退给发送人，2-发送人提前解锁，3-监督人提前解锁 |

示例

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



## 设置别名

```http
POST /alias/update
```

消息类型：alias/MsgAliasUpdate

消息结构

| 字段名     | 类型   | 是否必须 | 描述               |
| ---------- | ------ | -------- | ------------------ |
| owner      | string | required | owner              |
| alias      | string | required | 别名               |
| is_add     | bool   | required | 设置或删除         |
| as_default | bool   | required | 设置是否为默认别名 |

示例

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

## 设置介绍人

```http
POST /auth/accounts/{address}/referee
```

消息类型：authx/MsgSetReferee

消息结构

| 字段名  | 类型           | 是否必须 | 描述       |
| ------- | -------------- | -------- | ---------- |
| sender  | sdk.AccAddress | required | 发送方地址 |
| referee | sdk.AccAddress | required | 介绍人地址 |

示例

```json
{
  	"type":"authx/MsgSetReferee",
  	"value"：{
  		"sender":"coinex10pd6h8rqsplgn696gw9shygj8c8n2y7hcyj8vg",
  		"referee":"coinex1gx8dy25uhrmxap37fe094f6ujhd2hq5r02jgrn"
		}
}
```



# Staking & 收益分配

## 委托

```http
POST /staking/delegators/{delegatorAddr}/delegations
```

消息类型：cosmos-sdk/MsgDelegate

消息结构

| 字段名            | 类型   | 是否必须 | 描述       |
| ----------------- | ------ | -------- | ---------- |
| delegator_address | string | required | 委托人地址 |
| validator_address | string | required | 验证人地址 |
| amount            | coin   | required | 委托金额   |

示例

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



## 取消委托

```http
POST /staking/delegators/{delegatorAddr}/unbonding_delegations
```

消息类型：cosmos-sdk/MsgUndelegate

消息结构

| 字段名            | 类型   | 是否必须 | 描述         |
| ----------------- | ------ | -------- | ------------ |
| delegator_address | string | required | 委托人地址   |
| validator_address | string | required | 验证人地址   |
| amount            | coin   | required | 取消委托金额 |


示例

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



## 转移委托

```http
POST /staking/delegators/{delegatorAddr}/redelegations
```

消息类型：cosmos-sdk/MsgBeginRedelegate

消息结构

| 字段名                | 类型   | 是否必须 | 描述         |
| --------------------- | ------ | -------- | ------------ |
| delegator_address     | string | required | 委托人地址   |
| validator_src_address | string | required | 原验证人地址 |
| validator_dst_address | string | required | 新验证人地址 |
| amount                | coin   | required | 委托金额     |

示例

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



## 提取佣金

```http
POST /distribution/validators/{validatorAddr}/rewards  #委托收益也会取回
```

消息类型：cosmos-sdk/MsgWithdrawValidatorCommission

消息结构

| 字段名            | 类型   | 是否必须 | 描述       |
| ----------------- | ------ | -------- | ---------- |
| validator_address | string | required | 验证人地址 |

示例

```json
{
    "type":"cosmos-sdk/MsgWithdrawValidatorCommission",
    "value":{
        "validator_address":"coinexvaloper1ch57lj4pyfx6mw97nfrpqr6v0xcqshty93eca0"
    }
}
```



## 提取委托收益

```http
POST /distribution/delegators/{delegatorAddr}/rewards
```

消息类型：cosmos-sdk/MsgWithdrawDelegationReward

消息结构

| 字段名            | 类型   | 是否必须 | 描述       |
| ----------------- | ------ | -------- | ---------- |
| delegator_address | string | required | 委托人地址 |
| validator_address | string | required | 验证人地址 |

示例

```json
{
    "type": "cosmos-sdk/MsgWithdrawDelegationReward",
    "value": {
        "delegator_address": "coinex1eefc7unwgku2h6hlsv6yhmd6eavscykegt3y0p",
        "validator_address": "coinexvaloper1eefc7unwgku2h6hlsv6yhmd6eavscykenyjvp4"
    }
}
```



## 修改收益取回地址

```http
POST /distribution/delegators/{delegatorAddr}/withdraw_address
```

消息类型：cosmos-sdk/MsgModifyWithdrawAddress

消息结构

| 字段名            | 类型   | 是否必须 | 描述         |
| ----------------- | ------ | -------- | ------------ |
| delegator_address | string | required | 委托人地址   |
| withdraw_address  | string | required | 收益取回地址 |

示例

```json
{
    "type": "cosmos-sdk/MsgModifyWithdrawAddress",
    "value": {
        "delegator_address": "coinex1w3wndxggvx90t5cce0zuw7874048tqp5wuwxha",
        "withdraw_address": "coinex1cvh32v0pujf73sr5sqzgy43zkea7mptr6unav4"
    }
}
```



# 治理

## 创建验证人

消息类型：cosmos-sdk/MsgCreateValidator

消息结构

| 字段名              | 类型   | 是否必须 | 描述             |
| ------------------- | ------ | -------- | ---------------- |
| delegator_address   | string | required | 创建人地址       |
| validator_address   | string | required | 验证人运营地址   |
| pubkey              | string | required | 共识pubkey       |
| value               | coin   | required | 委托金额         |
| min_self_delegation | string | required | 自我委托最小金额 |
| description         | object | required | 描述             |
| commission          | object | required | 佣金设置         |

description结构

| 字段名           | 类型   | 是否必须 | 描述                          |
| ---------------- | ------ | -------- | ----------------------------- |
| moniker          | string | required | 代号                          |
| identity         | string | optional | identity signature(keybase等) |
| website          | string | optional | 网站                          |
| details          | string | optional | 详情                          |
| security_contact | string | optional | 紧急联系人（接口暂未支持）    |

commission结构

| 字段名          | 类型   | 是否必须 | 描述                     |
| --------------- | ------ | -------- | ------------------------ |
| rate            | string | required | 佣金率                   |
| max_rate        | string | required | 最大佣金率               |
| max_change_rate | string | required | 每天最多可以增加的佣金率 |

示例

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



## 修改验证人

消息类型：cosmos-sdk/MsgEditValidator

消息结构

| 字段名     | 类型   | 是否必须 | 描述       |
| --------- | ------ | -------- | ---------- |
| address | string | required | 地址 |
| commission_rate | string | optional | 佣金率 |
| min_self_delegation | string | optional | 自我委托最小金额 |
| Description | object | optional | 描述 |

description结构

| 字段名           | 类型   | 是否必须 | 描述                          |
| ---------------- | ------ | -------- | ----------------------------- |
| moniker          | string | required | 代号                          |
| identity         | string | optional | identity signature(keybase等) |
| website          | string | optional | 网站                          |
| details          | string | optional | 详情                          |
| security_contact | string | optional | 紧急联系人（接口暂未支持）    |

示例

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



## 不变量检查

消息类型：cosmos-sdk/MsgVerifyInvariant

消息结构

| 字段名                | 类型   | 是否必须 | 描述         |
| --------------------- | ------ | -------- | ------------ |
| sender                | string | required | 发送人       |
| invariant_module_name | string | required | 不变量模块名 |
| invariant_route       | string | required | 不变量路径   |

示例

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



## 发起提案

```http
POST /gov/proposals
POST /gov/proposals/param_change
POST /gov/proposals/community_pool_spend
```

消息类型：cosmos-sdk/MsgSubmitProposal

消息结构

| 字段名          | 类型   | 是否必须 | 描述       |
| --------------- | ------ | -------- | ---------- |
| proposer        | string | required | 提案发起人 |
| initial_deposit | coin[] | required | 初始押金   |
| content         | object | required | 内容       |

content结构

| 字段名 | 类型   | 是否必须 | 描述     |
| ------ | ------ | -------- | -------- |
| type   | string | required | 提案类型 |
| value  | object | required | 提案详情 |

value结构

| 字段名      | 类型     | 是否必须 | 描述                               |
| ----------- | -------- | -------- | ---------------------------------- |
| title       | string   | required | 标题                               |
| description | string   | required | 描述                               |
| changes     | object[] | optional | 变更参数（参数修改提案）           |
| recipient   | string   | optional | 接收人（Community Pool花费提案）   |
| amount      | coin     | optional | 接收金额（Community Pool花费提案） |

changes结构

| 字段名   | 类型   | 是否必须 | 描述       |
| -------- | ------ | -------- | ---------- |
| subspace | string | required | 模块       |
| key      | string | required | 参数key    |
| subkey   | string | optional | 参数subkey |
| value    | any    | required | 参数值     |

示例

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



## 给提案存押金

```http
POST /gov/proposals/{proposalId}/deposits
```

消息类型：cosmos-sdk/MsgDeposit

消息结构

| 字段名      | 类型   | 是否必须 | 描述       |
| ----------- | ------ | -------- | ---------- |
| depositor   | string | required | 提案发起人 |
| proposal_id | string | required | 提案ID     |
| amount      | coin[] | required | 存入金额   |

示例

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



## 提案投票

```http
POST /gov/proposals/{proposalId}/votes
```

消息类型：bankx/MsgSend

消息结构

| 字段名      | 类型   | 是否必须 | 描述                                                       |
| ----------- | ------ | -------- | ---------------------------------------------------------- |
| voter       | string | required | 投票人                                                     |
| proposal_id | string | required | 提案ID                                                     |
| option      | string | required | 投票选项有四种选项: `yes`, `no`, `no_with_veto`, `abstain` |

示例

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



## 捐赠

```http
POST /distribution/{accAddress}/donates
```

消息类型：distrx/MsgDonateToCommunityPool

消息结构

| 字段名    | 类型   | 是否必须 | 描述       |
| --------- | ------ | -------- | ---------- |
| from_addr | string | required | 捐献人地址 |
| amount    | coin[] | required | 捐赠金额   |

示例

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



# 惩罚

## 赦免

```http
POST /slashing/validators/{validatorAddr}/unjail
```

消息类型：asset/MsgUnjail

消息结构

| 字段名  | 类型   | 是否必须 | 描述       |
| ------- | ------ | -------- | ---------- |
| address | string | required | 验证人地址 |

示例

```json
{
    "type":"cosmos-sdk/MsgUnjail",
    "value":{
        "address":"coinexvaloper1800j6c8cqk59wnvjppgwk2n98gg3tdrfsvce5c"
    }
}
```



# Token

## 发行token

```http
POST /asset/tokens
```

消息类型：asset/MsgIssueToken

消息结构

| 字段名            | 类型   | 是否必须 | 描述                          |
| ----------------- | ------ | -------- | ----------------------------- |
| symbol            | string | required | 符号                          |
| name              | string | required | 名字                          |
| owner             | string | required | owner                         |
| total_supply      | string | required | 发行量                        |
| mintable          | bool   | required | 是否可增发                    |
| burnable          | bool   | required | 是否可燃烧                    |
| token_forbiddable | bool   | required | 是否可全局冻结                |
| addr_forbiddable  | bool   | required | 是否可根据地址冻结            |
| description       | string |          | 描述                          |
| url               | string |          | URL                           |
| identity          | string |          | identity signature(keybase等) |

示例

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



## 转移token的owner

```http
POST /asset/tokens/{symbol}/ownerships
```

消息类型：asset/MsgTransferOwnership

消息结构

| 字段名         | 类型   | 是否必须 | 描述    |
| -------------- | ------ | -------- | ------- |
| symbol         | string | required | 符号    |
| original_owner | string | required | 原owner |
| new_owner      | string | required | 新owner |

示例

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



## 增发token

```http
POST /asset/tokens/{symbol}/mints
```

消息类型：asset/MsgMintToken

消息结构

| 字段名        | 类型   | 是否必须 | 描述      |
| ------------- | ------ | -------- | --------- |
| symbol        | string | required | 符号      |
| owner_address | string | required | owner地址 |
| amount        | string | required | 数量      |

示例

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



## 燃烧token

```http
POST /asset/tokens/{symbol}/burns
```

消息类型：asset/MsgBurnToken

消息结构

| 字段名        | 类型   | 是否必须 | 描述      |
| ------------- | ------ | -------- | --------- |
| symbol        | string | required | 符号      |
| owner_address | string | required | owner地址 |
| amount        | string | required | 数量      |

示例

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



## 冻结token

```http
POST /asset/tokens/{symbol}/forbids
```

消息类型：asset/MsgForbidToken

消息结构

| 字段名        | 类型   | 是否必须 | 描述      |
| ------------- | ------ | -------- | --------- |
| symbol        | string | required | 符号      |
| owner_address | string | required | owner地址 |

示例

```json
{
    "type": "asset/MsgForbidToken",
    "value": {
        "owner_address": "coinex1k2qtl2lkr6yqhvraqsj385rl9axf8rewlfthcc",
        "symbol": "abc"
    }
}
```



## 取消冻结token

```http
POST /asset/tokens/{symbol}/unforbids
```

消息类型：asset/MsgUnForbidToken

消息结构

| 字段名        | 类型   | 是否必须 | 描述      |
| ------------- | ------ | -------- | --------- |
| symbol        | string | required | 符号      |
| owner_address | string | required | owner地址 |

示例

```json
{
    "type": "asset/MsgUnForbidToken",
    "value": {
        "owner_address": "coinex1k2qtl2lkr6yqhvraqsj385rl9axf8rewlfthcc",
        "symbol": "abc"
    }
}
```



## 加入token白名单

```http
POST /asset/tokens/{symbol}/forbidden/whitelist
```

消息类型：asset/MsgAddTokenWhitelist

消息结构

| 字段名        | 类型     | 是否必须 | 描述                |
| ------------- | -------- | -------- | ------------------- |
| symbol        | string   | required | 符号                |
| owner_address | string   | required | owner地址           |
| whitelist     | string[] | required | token白名单地址列表 |

示例

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



## 从token白名单移除

```http
POST /asset/tokens/{symbol}/unforbidden/whitelist
```

消息类型：asset/MsgRemoveTokenWhitelist

消息结构

| 字段名        | 类型     | 是否必须 | 描述                        |
| ------------- | -------- | -------- | --------------------------- |
| symbol        | string   | required | 符号                        |
| owner_address | string   | required | owner地址                   |
| whitelist     | string[] | required | 从token白名单移除的地址列表 |

示例

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



## 冻结地址token

```http
POST /asset/tokens/{symbol}/forbidden/addresses
```

消息类型：asset/MsgForbidAddr

消息结构

| 字段名        | 类型     | 是否必须 | 描述         |
| ------------- | -------- | -------- | ------------ |
| symbol        | string   | required | 符号         |
| owner_address | string   | required | owner地址    |
| addresses     | string[] | required | 冻结地址列表 |

示例

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



## 取消冻结地址token

```http
POST /asset/tokens/{symbol}/unforbidden/addresses
```

消息类型：asset/MsgUnForbidAddr

消息结构

| 字段名        | 类型     | 是否必须 | 描述             |
| ------------- | -------- | -------- | ---------------- |
| symbol        | string   | required | 符号             |
| owner_address | string   | required | owner地址        |
| addresses     | string[] | required | 取消冻结地址列表 |

示例

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



## 修改token信息

```http
POST /asset/tokens/{symbol}/infos
```

消息类型：asset/MsgModifyTokenInfo

消息结构

| 字段名        | 类型   | 是否必须 | 描述      |
| ------------- | ------ | -------- | --------- |
| symbol        | string | required | 符号      |
| owner_address | string | required | owner地址 |
| description   | string | optional | 描述      |
| url           | string | optional | URL       |

示例

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



## 对token发表评论

```http
POST /comment/new-thread
```


消息类型：comment/MsgCommentToken

消息结构

| 字段名       | 类型     | 是否必须 | 描述                                                         |
| ------------ | -------- | -------- | ------------------------------------------------------------ |
| token        | string   | required | 被评论的币种                                                 |
| sender       | string   | required | 发送者                                                       |
| title        | string   | required | 标题                                                         |
| content_type | integer  | required | 表示评论的内容是什么类型的，包括：0-IPFS、1-Magnet、2-HTTP、3-UTF8Text、4-ShortHanzi、5-ShortHanziLZ4、6-RawBytes |
| content      | string   | required | 评论的内容，字节数组，最长为16KByte                          |
| donation     | string   |          | 捐赠给Incentive Pool的金额，只能捐赠CET                      |
| references   | object[] | optional | 对另外一些Comment的引用                                      |

示例

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



# 交易

## 创建交易对

```http
POST /market/trading-pairs
```

消息类型：market/MsgCreateTradingPair

消息结构

| 字段名          | 类型    | 是否必须 | 描述                                                    |
| --------------- | ------- | -------- | ------------------------------------------------------- |
| creator         | string  | required | 创建者，是stock或money的发行者                          |
| money           | string  | required | money，必须是链上已经被创建的token                      |
| stock           | string  | required | stock，必须是链上已经被创建的token                      |
| price_precision | integer | required | 价格精度，取值为0~18，价格在小数点后保留多少位整数      |
| order_precision | integer | required | 下单精度，下单数量必须是10的order_precision次方的整数倍 |

示例

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



## 取消交易对

```http
POST /market/cancel-trading-pair
```

消息类型：market/MsgCancelTradingPair

消息结构

| 字段名         | 类型   | 是否必须 | 描述                    |
| -------------- | ------ | -------- | ----------------------- |
| sender         | string | required | 发送者                  |
| trading_pair   | string | required | 交易对                  |
| effective_time | string | required | 生效时间戳（单位:纳秒） |

示例

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



## 创建订单

```http
POST /market/gte-orders
POST /market/ioc-orders
```

消息类型：market/MsgCreateOrder

消息结构

| 字段名          | 类型    | 是否必须 | 描述                                                         |
| --------------- | ------- | -------- | ------------------------------------------------------------ |
| sender          | string  | required | 发送者                                                       |
| identify        | integer | required | 消息在交易内序号                                             |
| trading_pair    | string  | required | 交易对                                                       |
| order_type      | integer | required | 订单类型。2-限价单                                           |
| price_precision | integer | required | 价格精度，取值为0~18，价格在小数点后保留多少位整数。它的取值必须不大于交易对默认的价格精度。默认的价格精度由交易对的创建者所设定。 |
| price           | string  | required | 价格                                                         |
| quantity        | string  | required | 数量                                                         |
| side            | integer | required | 买卖方向：1-买，2-卖                                         |
| time_in_force   | string  | required | 有效时间类型：3-GTE，4-IOC                                   |
| exist_blocks    | string  | required | 有效区块数                                                   |

示例

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



## 取消订单

```http
POST /market/cancel-order
```

消息类型：market/MsgCancelOrder

消息结构

| 字段名   | 类型   | 是否必须 | 描述   |
| -------- | ------ | -------- | ------ |
| order_id | string | required | 订单ID |
| sender   | string | required | 发送者 |

示例

```json
{
    "type":"market/MsgCancelOrder",
    "value":{
        "sender":"coinex144u7kv7ehpdndvf42l3vyfg543c62w2h0ks500",
        "order_id":"coinex144u7kv7ehpdndvf42l3vyfg543c62w2h0ks500-1025"
    }
}
```



## 修改价格精度

```http
POST /market/price-precision
```

消息类型：market/MsgModifyPricePrecision

消息结构

| 字段名          | 类型    | 是否必须 | 描述                                               |
| --------------- | ------- | -------- | -------------------------------------------------- |
| trading_pair    | string  | required | 交易对                                             |
| sender          | string  | required | 发送者                                             |
| price_precision | integer | required | 价格精度，取值为0~18，价格在小数点后保留多少位整数 |

示例

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



## 创建bancor

```http
POST /bancorlite/bancor-init
```

消息类型：bancorlite/MsgBancorInit

消息结构

| 字段名               | 类型    | 是否必须 | 描述                                                    |
| -------------------- | ------- | -------- | ------------------------------------------------------- |
| owner                | string  | required | owner地址                                               |
| money                | string  | required | money                                                   |
| stock                | string  | required | stock                                                   |
| init_price           | string  | required | 初始价格                                                |
| max_price            | string  | required | 最大价格                                                |
| max_supply           | string  | required | 最大数量                                                |
| earliest_cancel_time | string  | required | 最早可以被取消的时间(戳)                                |
| stock_precision      | integer | required | 下单精度，下单数量必须是10的order_precision次方的整数倍 |

示例

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



## 取消bancor

```http
POST /bancorlite/bancor-cancel
```

消息类型：bancorlite/MsgBancorCancel

消息结构

| 字段名 | 类型   | 是否必须 | 描述      |
| ------ | ------ | -------- | --------- |
| owner  | string | required | owner地址 |
| money  | string | required | money     |
| stock  | string | required | stock     |

示例

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



## 和bancor交易

```http
POST /bancorlite/bancor-trade
```

消息类型：bancorlite/MsgBancorTrade

消息结构

| 字段名      | 类型   | 是否必须 | 描述      |
| ----------- | ------ | -------- | --------- |
| sender      | string | required | 发送者    |
| money       | string | required | money     |
| stock       | string | required | stock     |
| is_buy      | bool   | required | 买或卖    |
| amount      | string | required | 数量      |
| money_limit | string | required | money限制 |

示例

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

