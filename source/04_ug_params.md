### The parameters the can be changed by proposals



**Unit 1CET = 1_0000_0000sato.cet**

| Module    | Name                        | Meaning                                                      |       Current Value |
| --------- | --------------------------- | ------------------------------------------------------------ | ------------------: |
| auth      | MaxMemoCharacters           | Memo长度（字节数）上限                                       |                 512 |
|           | TxSigLimit                  | StdTx签名数量上限                                            |                   7 |
|           | TxSizeCostPerByte           | 交易每字节Gas消耗                                            |                  20 |
|           | SigVerifyCostED25519        | ED25519验签Gas消耗                                           |               11800 |
|           | SigVerifyCostSecp256k1      | Secp256k1验签Gas消耗                                         |               20000 |
| bank      | SendEnabled                 | 是否可以转账                                                 |                true |
| staking   | UnbondingTime               | Unbonding成熟时间（以纳秒为单位来设置）                      |                21天 |
|           | MaxValidators               | Validator数量上限                                            |                  42 |
|           | MaxEntries                  | undelegation/redelegation队列长度上限                        |                   7 |
|           | BondDenom                   | Bond币种                                                     |                 cet |
| slashing  | MaxEvidenceAge              | 双签证据的最长有效期（以纳秒为单位来设置）                   |                21天 |
|           | SignedBlocksWindow          | Validator投票区块统计滑动窗口                                |               10000 |
|           | MinSignedPerWindow          | 滑动窗口内最少投票区块数，小于这个参数Validator 就会因可用性差被惩罚 |                0.05 |
|           | DowntimeJailDuration        | 可用性差关监狱时间（以纳秒为单位来设置）                     |              10分钟 |
|           | SlashFractionDoubleSign     | 双签资金罚没比例                                             |                0.05 |
|           | SlashFractionDowntime       | 可用性差资金罚没比例                                         |              0.0001 |
| distr     | WithdrawAddrEnabled         | 提款地址是否允许重置                                         |                true |
|           | CommunityTax                | 社区税比例                                                   |                0.02 |
|           | BaseProposerReward          | 基本的proposer奖励                                           |                0.01 |
|           | BonusProposerReward         | 附加的proposer奖励                                           |                0.04 |
| gov       | MinDeposit                  | 提案最小充值，超过这个值提案才会进入投票阶段                 |           1_0000CET |
|           | MaxDepositPeriod            | 完成充值最长期限（以纳秒为单位来设置）                       |                14天 |
|           | VotingPeriod                | 投票期限（以纳秒为单位来设置）                               |                 7天 |
|           | Quorum                      | 有效提案需要达到的投票比例                                   |                 40% |
|           | Threshold                   | **赞成**票占非弃权票票数的比例超过该值提案才能**通过**       |                 50% |
|           | Veto                        | **强烈否决**票占总票数的比例超过该值则提案被**否决**         |               33.4% |
| crisis    | ConstantFee                 | 发起不变量检查交易需要支付的费用                             |         10_0000 CET |
| authx     | MinGasPriceLimit            | GasPrice下限                                                 |          20sato.cet |
|           | RefereeChangeMinInterval    | 更新介绍人的最短间隔（以纳秒为单位来设置）                   |                 7天 |
|           | RebateRatio                 | 返还介绍人的佣金比例（目前包括market和bancor交易），取值范围1~10000，基数为10000 | **2000**<br />即20% |
| bankx     | ActivationFee               | 激活账户费用                                                 |                1CET |
|           | LockCoinsFreeTime           | 锁定转账的免费时长（以纳秒为单位来设置）                     |                 7天 |
|           | LockCoinsFeePerDay          | 锁定时长超过LockCoinsFreeTime时，每多一天所需要支付的费用（按天数取整） |             0.01CET |
| stakingx  | MinSelfDelegation           | 节点最低自我委托CET数量                                      |         100_0000CET |
|           | MinMandatoryCommissionRate  | 节点最低佣金费率                                             |                 10% |
| asset     | IssueTokenFee               | 发行长度大于2的Token费用(dex1)<br />发行长度大于6的Token费用(dex2) |               50CET |
|           | IssueRareTokenFee           | 发行长度为2的Token费用                                       |           1_0000CET |
|           | Issue3CharTokenFee          | 发行长度为3的Token费用                                       |             1000CET |
|           | Issue4CharTokenFee          | 发行长度为4的Token费用                                       |              500CET |
|           | Issue5CharTokenFee          | 发行长度为5的Token费用                                       |              200CET |
|           | Issue6CharTokenFee          | 发行长度为6的Token费用                                       |              100CET |
| market    | CreateMarketFee             | 创建交易对市场的费用                                         |              100CET |
|           | GTEOrderFeatureFeeByBlocks  | 对于GTE类型的订单： 当订单在dex上存储的时间为[0,GTEOrderLifetime]区块时，不收取功能费； GTE Order在超过`GTEOrderLifetime`个区块之后, 超出的每个区块收取 `GTEOrderFeatureFeeByBlocks`. |         10 sato.CET |
|           | GTEOrderLifetime            | 订单在dex上免费存储的时间长度；                              |        200000(区块) |
|           | MaxExecutedPriceChangeRatio | 计算执行价格时的参考比例                                     |                 25% |
|           | MarketFeeRate               | 订单的费率                                                   |                  10 |
|           | MarketFeeRatePrecision      | 费率的基数                                                   |      10<sup>4<sup/> |
|           | MarketFeeMin                | 每笔最低需满足的手续费(来控制订单交易token的最小数量)        |             0.01CET |
|           | FeeForZeroDeal              | 订单从链上删除时，无任何成交情况下，收取的手续费             |             0.01CET |
|           | MarketMinExpiredTime        | 发起交易对下线请求时，最快的下线时间间隔（以纳秒为单位来设置） |                 7天 |
| alias     | FeeForAliasLength2          | 当设置一个长度为2字符的别名时所收取的功能费                  |           1_0000CET |
|           | FeeForAliasLength3          | 当设置一个长度为3字符的别名时所收取的功能费                  |             5000CET |
|           | FeeForAliasLength4          | 当设置一个长度为4字符的别名时所收取的功能费                  |             2000CET |
|           | FeeForAliasLength5          | 当设置一个长度为5字符的别名时所收取的功能费                  |             1000CET |
|           | FeeForAliasLength6          | 当设置一个长度为6字符的别名时所收取的功能费                  |              100CET |
|           | FeeForAliasLength7OrHigher  | 当设置一个长度为7字符或更长的别名时所收取的功能费            |               10CET |
|           | MaxAliasCount               | 一个账户所能拥有的别名数量的上限                             |                   5 |
| bancor    | CreateBancorFee             | 创建Bancor所收取的功能费                                     |              100CET |
|           | CancelBancorFee             | 取消Bancor所收取的功能费                                     |              100CET |
|           | TradeFeeRate                | Bancor交易的费率                                             |                  10 |
| incentive | DefaultRewardPerBlock       | 当高度不在激励计划中时的出块激励                             |                2CET |
|           | Plans                       | 激励计划                                                     |                     |



使用如下脚本可以得到正在运行的节点内部的参数：

```bash
export rest=http://120.79.253.17:1317
echo "curl $rest/auth/parameters        " ; curl $rest/auth/parameters
echo "curl $rest/bank/parameters        " ; curl $rest/bank/parameters
echo "curl $rest/staking/parameters     " ; curl $rest/staking/parameters
echo "curl $rest/slashing/parameters    " ; curl $rest/slashing/parameters
echo "curl $rest/gov/parameters/deposit " ; curl $rest/gov/parameters/deposit
echo "curl $rest/gov/parameters/tallying" ; curl $rest/gov/parameters/tallying
echo "curl $rest/gov/parameters/voting  " ; curl $rest/gov/parameters/voting
echo "curl $rest/distribution/parameters" ; curl $rest/distribution/parameters
echo "curl $rest/asset/parameters       " ; curl $rest/asset/parameters
echo "curl $rest/market/parameters      " ; curl $rest/market/parameters
echo "curl $rest/alias/parameters       " ; curl $rest/alias/parameters
echo "curl $rest/bancorlite/parameters  " ; curl $rest/bancorlite/parameters
echo "curl $rest/incentive/parameters   " ; curl $rest/incentive/parameters

```



