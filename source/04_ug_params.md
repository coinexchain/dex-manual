### The parameters the can be changed by proposals



**单位：1CET = 1_0000_0000sato.cet**

| 模块      | 名称                        | 含义                                                         |       DEX1取值 |                                          DEX2取值 |
| --------- | --------------------------- | ------------------------------------------------------------ | -------------: | ------------------------------------------------: |
| auth      | MaxMemoCharacters           | Memo长度（字节数）上限                                       |            512 |                                          （不变） |
|           | TxSigLimit                  | StdTx签名数量上限                                            |              7 |                                          （不变） |
|           | TxSizeCostPerByte           | 交易每字节Gas消耗                                            |             20 |                                          （不变） |
|           | SigVerifyCostED25519        | ED25519验签Gas消耗                                           |          11800 |                                          （不变） |
|           | SigVerifyCostSecp256k1      | Secp256k1验签Gas消耗                                         |          20000 |                                          （不变） |
| bank      | SendEnabled                 | 是否可以转账                                                 |           true |                                          （不变） |
| staking   | UnbondingTime               | Unbonding成熟时间（以纳秒为单位来设置）                      |           21天 |                                          （不变） |
|           | MaxValidators               | Validator数量上限                                            |             42 |                                          （不变） |
|           | MaxEntries                  | undelegation/redelegation队列长度上限                        |              7 |                                          （不变） |
|           | BondDenom                   | Bond币种                                                     |            cet |                                          （不变） |
| slashing  | MaxEvidenceAge              | 双签证据的最长有效期（以纳秒为单位来设置）                   |           21天 |                                          （不变） |
|           | SignedBlocksWindow          | Validator投票区块统计滑动窗口                                |          10000 |                                          （不变） |
|           | MinSignedPerWindow          | 滑动窗口内最少投票区块数，小于这个参数Validator 就会因可用性差被惩罚 |           0.05 |                                          （不变） |
|           | DowntimeJailDuration        | 可用性差关监狱时间（以纳秒为单位来设置）                     |         10分钟 |                                          （不变） |
|           | SlashFractionDoubleSign     | 双签资金罚没比例                                             |           0.05 |                                          （不变） |
|           | SlashFractionDowntime       | 可用性差资金罚没比例                                         |         0.0001 |                                          （不变） |
| distr     | WithdrawAddrEnabled         | 提款地址是否允许重置                                         |           true |                                          （不变） |
|           | CommunityTax                | 社区税比例                                                   |           0.02 |                                          （不变） |
|           | BaseProposerReward          | 基本的proposer奖励                                           |           0.01 |                                          （不变） |
|           | BonusProposerReward         | 附加的proposer奖励                                           |           0.04 |                                          （不变） |
| gov       | MinDeposit                  | 提案最小充值，超过这个值提案才会进入投票阶段                 |      1_0000CET |                                          （不变） |
|           | MaxDepositPeriod            | 完成充值最长期限（以纳秒为单位来设置）                       |           14天 |                                          （不变） |
|           | VotingPeriod                | 投票期限（以纳秒为单位来设置）                               |           14天 |                   **7天**<br />尊重链上投票的结果 |
|           | Quorum                      | 有效提案需要达到的投票比例                                   |            40% |                                          （不变） |
|           | Threshold                   | **赞成**票占非弃权票票数的比例超过该值提案才能**通过**       |            50% |                                          （不变） |
|           | Veto                        | **强烈否决**票占总票数的比例超过该值则提案被**否决**         |          33.4% |                                          （不变） |
| crisis    | ConstantFee                 | 发起不变量检查交易需要支付的费用                             |    10_0000 CET |                                          （不变） |
| authx     | MinGasPriceLimit            | GasPrice下限                                                 |     20sato.cet |                                          （不变） |
|           | RefereeChangeMinInterval    | 更新介绍人的最短间隔（以纳秒为单位来设置）                   |            N/A |                                           **7天** |
|           | RebateRatio                 | 返还介绍人的佣金比例（目前包括market和bancor交易），取值范围1~10000，基数为10000 |            N/A |                               **2000**<br />即20% |
| bankx     | ActivationFee               | 激活账户费用                                                 |           1CET |                                          （不变） |
|           | LockCoinsFreeTime           | 锁定转账的免费时长（以纳秒为单位来设置）                     |            7天 |                                          （不变） |
|           | LockCoinsFeePerDay          | 锁定时长超过LockCoinsFreeTime时，每多一天所需要支付的费用（按天数取整） |        0.01CET |                                          （不变） |
| stakingx  | MinSelfDelegation           | 节点最低自我委托CET数量                                      |    500_0000CET | **100_0000CET**<br />链上投票28日截止，应该会通过 |
|           | MinMandatoryCommissionRate  | 节点最低佣金费率                                             |            10% |                                          （不变） |
| asset     | IssueTokenFee               | 发行长度大于2的Token费用(dex1)<br />发行长度大于6的Token费用(dex2) |      1_0000CET |                                         **50CET** |
|           | IssueRareTokenFee           | 发行长度为2的Token费用                                       |     10_0000CET |             **1_0000CET**<br />尊重链上投票的结果 |
|           | Issue3CharTokenFee          | 发行长度为3的Token费用                                       |            N/A |                                      **1_000CET** |
|           | Issue4CharTokenFee          | 发行长度为4的Token费用                                       |            N/A |                                        **500CET** |
|           | Issue5CharTokenFee          | 发行长度为5的Token费用                                       |            N/A |                                        **200CET** |
|           | Issue6CharTokenFee          | 发行长度为6的Token费用                                       |            N/A |                                        **100CET** |
| market    | CreateMarketFee             | 创建交易对市场的费用                                         |      1_0000CET |                **100CET**<br />尊重链上投票的结果 |
|           | GTEOrderFeatureFeeByBlocks  | 对于GTE类型的订单： 当订单在dex上存储的时间为[0,GTEOrderLifetime]区块时，不收取功能费； GTE Order在超过`GTEOrderLifetime`个区块之后, 超出的每个区块收取 `GTEOrderFeatureFeeByBlocks`. |    10 sato.CET |                                          （不变） |
|           | GTEOrderLifetime            | 订单在dex上免费存储的时间长度；                              |   100000(区块) |                                  **200000(区块)** |
|           | MaxExecutedPriceChangeRatio | 计算执行价格时的参考比例                                     |            25% |                                          （不变） |
|           | MarketFeeRate               | 订单的费率                                                   |             10 |                                          （不变） |
|           | MarketFeeRatePrecision      | 费率的基数                                                   | 10<sup>4<sup/> |                                          （不变） |
|           | MarketFeeMin                | 每笔最低需满足的手续费(来控制订单交易token的最小数量)        |        0.01CET |                                          （不变） |
|           | FeeForZeroDeal              | 订单从链上删除时，无任何成交情况下，收取的手续费             |        0.01CET |                                          （不变） |
|           | MarketMinExpiredTime        | 发起交易对下线请求时，最快的下线时间间隔（以纳秒为单位来设置） |            7天 |                                          （不变） |
|           | FixedTradeFee               | 当订单创建时，订单中的stock与cet的交易对无历史成交价时，收取的固定费率 |        0.01CET |                                      **已经去掉** |
| alias     | FeeForAliasLength2          | 当设置一个长度为2字符的别名时所收取的功能费                  |      1_0000CET |                                          （不变） |
|           | FeeForAliasLength3          | 当设置一个长度为3字符的别名时所收取的功能费                  |        5000CET |                                          （不变） |
|           | FeeForAliasLength4          | 当设置一个长度为4字符的别名时所收取的功能费                  |        2000CET |                                          （不变） |
|           | FeeForAliasLength5          | 当设置一个长度为5字符的别名时所收取的功能费                  |        1000CET |                                          （不变） |
|           | FeeForAliasLength6          | 当设置一个长度为6字符的别名时所收取的功能费                  |         100CET |                                          （不变） |
|           | FeeForAliasLength7OrHigher  | 当设置一个长度为7字符或更长的别名时所收取的功能费            |          10CET |                                          （不变） |
|           | MaxAliasCount               | 一个账户所能拥有的别名数量的上限                             |              5 |                                          （不变） |
| bancor    | CreateBancorFee             | 创建Bancor所收取的功能费                                     |         100CET |                                          （不变） |
|           | CancelBancorFee             | 取消Bancor所收取的功能费                                     |         100CET |                                          （不变） |
|           | TradeFeeRate                | Bancor交易的费率                                             |             10 |                                          （不变） |
| incentive | DefaultRewardPerBlock       | 当高度不在激励计划中时的出块激励                             |           2CET |                                          （不变） |
|           | Plans                       | 激励计划                                                     |                |                                          （不变） |



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





# 区块收益分发

### 区块收益:

- GAS
- 功能费
  - 上币费
  - 上交易对费
  - 交易撮合佣金
  - 锁定转账费
  - 帐户激活费
  - 地址别名收取功能费
  - Bancor创建及取消费用
  - Bancor交易佣金
  - Invariant检查费用(crisis)
- [出块激励](https://gitlab.com/-/ide/project/cetchain/docs/blob/master/-/meta/meetings/Requirements_Clarification_3_20190618/incentive_plan.md)(Incentive Pool初始激励总量为3亿)

### CommunityPool:

- Community Pool收入
  - Slash金额不会燃烧会存入CommunityPool
    - Double Sign
    - Pool Availability
  - Governance模块的deposit
    - reject掉的proposal, deposit会被存入CommunityPool
  - 区块整体收益会收取CommunityTax(2%) -> CommunityPool
  - 区块收益分发时产生的零头及communityTax(2%)归入CommunityPool
- Community Pool花费
  - 通过CommunityPoolSpendProposal做为开发者激励
  - 通过CommunityPoolSpendProposal做为运营者或社区参与者的激励    
  - 通过CommunityPoolSpendProposal转入incentive_pool 做为区块激励

### 区块收益分发:
- 会在下一个块处理的开始阶段, 进行上一个块收益的分发.

- 1%+4%(max): 出块节点会拿到base(1%) 及 最大(4%)的bonus
  - bonus取决于多少power对这个上一个块进行了precommit, 如果所有节点都对这个块投了precommit, 则会拿满额外的4%
  - `出块节点拿到的base(1%) 及 bonus后, 会分发给出块节点代理的delegators, 分发时按delegator在当前出块节点的占比分发.`

- 2%: 另外区块收益的2%(是社区参数)会分发到CommunityPool帐户, 后期可通过`CommunityPoolSpendProposal`按社区提案分发

- 剩余的金额, 会分发给对上一个块进行Precommit的节点 (按这些节点的VotingPower比例分发):
  - 也就是说, 去掉出块节点奖励 及 communityTax后剩余的部分, 按power比例分发给Validators
  - 分发到节点后, 节点内部再按delegator在当前节点的占比分发.
  - proposerMultiplier = baseProposerReward(1%) + sumPreviousPrecommitPower / totalPreviousPower * bonusProposerReward(4%)
  - proposerReward = feesCollected * proposerMultiplier
  - remaining = feesCollected - proposerReward
  - voteMultiplier = 1 - proposerMultiplier - communityTax(2%)
  - for each validator
  - powerFraction = ValidatorPower / totalPreviousPower
  - reward = feesCollected * voteMultiplier * powerFraction
  - remaining = remaining - reward
  
- 按18位精度计算产生的零头及communityTax(2%)归入CommunityPool
  - feePool.CommunityPool += remaining