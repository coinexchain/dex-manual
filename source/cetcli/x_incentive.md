# Incentive 

## query

To query the parameters of the incentive module.

```BASH
Query incentive params

Usage:
  cetcli query incentive params [flags]
```

Example:

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

The returned result includes a `default_reward_per_block` parameter and a series of entries. Each entry contains a range between `[start_height, end_height)`, the reward for each block in this range and the total rewards in this range.

If a block's height is between any range specified in these entries, its reward will be the corresponding value. If the height does not fall in any of these ranges, and `incentive_addr` has enough CET, then the block's reward will take `default_reward_per_block`.

