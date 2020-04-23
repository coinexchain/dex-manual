# incentive命令



## query

该命令用来查询incentive模块相关参数。用法：

```BASH
Query incentive params

Usage:
  cetcli query incentive params [flags]
```

示例：

```BASH
cetcli q incentive params --chain-id=coinexdex
{
  "default_reward_per_block": "200000000",
  "plans": [
    {
      "start_height": "0",
      "end_height": "10512000",
      "reward_per_block": "1000000000",
      "total_incentive": "10512000000000000"
    },
    {
      "start_height": "10512000",
      "end_height": "21024000",
      "reward_per_block": "800000000",
      "total_incentive": "8409600000000000"
    },
    {
      "start_height": "21024000",
      "end_height": "31536000",
      "reward_per_block": "600000000",
      "total_incentive": "6307200000000000"
    },
    {
      "start_height": "31536000",
      "end_height": "42048000",
      "reward_per_block": "400000000",
      "total_incentive": "4204800000000000"
    },
    {
      "start_height": "42048000",
      "end_height": "52560000",
      "reward_per_block": "200000000",
      "total_incentive": "2102400000000000"
    }
  ]
}

```

**参数解释**：

| 参数                       | 含义                                                         |
| -------------------------- | ------------------------------------------------------------ |
| `default_reward_per_block` | 在当前高度不在任何激励计划内时，按当前值进行每块的激励分发   |
| `plans`                    | 当区块高度在start_height到end_height之间的每块的激励以及总激励 |



