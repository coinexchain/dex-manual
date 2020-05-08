# Rest API Query Examples （https/http）
About type definition, please refer to: swagger/swagger.yaml

- Query Block Time 

```bash
$ curl -k "https://localhost:8000/misc/block-times?height=246&count=2" 
$ curl "http://localhost:8000/misc/block-times?height=246&count=2"
[
  1566374464,
  1566374462
]
```

- Query Market Tickers 

```bash
$ curl -k "https://localhost:8000/market/tickers?market_list=abc/cet" 
$ curl "http://localhost:8000/market/tickers?market_list=abc/cet" 
[
  {
    "market": "abc/cet",
    "new": "5.299999998276284917",
    "old": "0.000000000000000000"
  }
]
```

- Query Market Depth 

```bash
$ curl -k "https://localhost:8000/market/depths?market=abc/cet&count=1" 
$ curl "http://localhost:8000/market/depths?market=abc/cet&count=1" 
{
  "sell": [
    {
      "p": "0.015200000000000000",
      "a": "10000000"
    }
  ],
  "buy": [
    {
      "p": "0.005200000000000000",
      "a": "10000000"
    }
  ]
}
```

- Query Market K-lines  
timespan=1min/1hour/1day

```bash
$ curl -k "https://localhost:8000/market/candle-sticks?market=abc/cet&timespan=1day&time=1567745901&count=1&sid=0"
$ curl "http://localhost:8000/market/candle-sticks?market=abc/cet&timespan=1day&time=1567745901&count=1&sid=0"
[
  {
    "open": "5.299999999273690910",
    "close": "5.299999998276284917",
    "high": "5.300000000000000000",
    "low": "4.799999997059165898",
    "total": "849734272994",
    "unix_time": 1566374439,
    "time_span": "1day",
    "market": "abc/cet"
  }
]
```

- Query Order-info 
tag=create/fill/cancel

```bash
$ curl -k  "https://localhost:8000/market/user-orders?account=coinex1x6rhu5m53fw8qgpwuljauaptvxyur57zym4jly&time=1567745901&count=3&sid=0"
$ curl "http://localhost:8000/market/user-orders?account=coinex1x6rhu5m53fw8qgpwuljauaptvxyur57zym4jly&time=1567745901&count=3&sid=0"
{
  "create_order_info": {
    "data": [
      {
        "order_id": "coinex18t4sp5kv07czmv2ar9ds04z3xx946kkkcwnfpx-5",
        "sender": "coinex18t4sp5kv07czmv2ar9ds04z3xx946kkkcwnfpx",
        "trading_pair": "abc/cet",
        "order_type": 2,
        "price": "1.000000000000000000",
        "quantity": 300000000,
        "side": 1,
        "time_in_force": 3,
        "feature_fee": 0,
        "height": 10,
        "frozen_fee": 1000000,
        "freeze": 300000000
      }
    ],
    "timesid": [
      1566371090,
      6
    ]
  },
  "fill_order_info": {
    "data": [
      {
        "order_id": "coinex18t4sp5kv07czmv2ar9ds04z3xx946kkkcwnfpx-5",
        "trading_pair": "abc/cet",
        "height": 12,
        "side": 1,
        "price": "1.000000000000000000",
        "left_stock": 0,
        "freeze": 30000000,
        "deal_stock": 300000000,
        "deal_money": 270000000,
        "curr_stock": 300000000,
        "curr_money": 270000000
      }
    ],
    "timesid": [
      1566371090,
      8
    ]
  },
  "cancel_order_info": {
    "data": [
      {
        "order_id": "coinex18t4sp5kv07czmv2ar9ds04z3xx946kkkcwnfpx-5",
        "trading_pair": "abc/cet",
        "height": 12,
        "side": 1,
        "price": "1.000000000000000000",
        "del_reason": "The order was fully filled",
        "used_commission": 1000000,
        "left_stock": 0,
        "remain_amount": 30000000,
        "deal_stock": 300000000,
        "deal_money": 270000000
      }
    ],
    "timesid": [
      1566371090,
      12
    ]
  }
}
```

- Query Market-deal 

```bash
$ curl -k "https://localhost:8000/market/deals?market=abc/cet&time=1567745901&count=2&sid=0" 
$ curl "http://localhost:8000/market/deals?market=abc/cet&time=1567745901&count=2&sid=0" 
{
  "data": [
    {
      "order_id": "coinex1yj66ancalgk7dz3383s6cyvdd0nd93q0tk4x0c-5916",
      "trading_pair": "abc/cet",
      "height": 234,
      "side": 2,
      "price": "4.500000000000000000",
      "left_stock": 0,
      "freeze": 0,
      "deal_stock": 522128053,
      "deal_money": 2767278680,
      "curr_stock": 522128053,
      "curr_money": 2767278680
    },
    {
      "order_id": "coinex1yj66ancalgk7dz3383s6cyvdd0nd93q0tk4x0c-5912",
      "trading_pair": "abc/cet",
      "height": 234,
      "side": 2,
      "price": "1.200000000000000000",
      "left_stock": 0,
      "freeze": 0,
      "deal_stock": 595775796,
      "deal_money": 3157611718,
      "curr_stock": 595775796,
      "curr_money": 3157611718
    }
  ],
  "timesid": [
    1566374439,
    105873,
    1566374439,
    105871
  ]
}
```

- Query Bancorlite-trade

```bash
$ curl -k "https://localhost:8000/bancorlite/trades?account=coinex1x6rhu5m53fw8qgpwuljauaptvxyur57zym4jly&time=1567745901&count=2&sid=0"
$ curl "http://localhost:8000/bancorlite/trades?account=coinex1x6rhu5m53fw8qgpwuljauaptvxyur57zym4jly&time=1567745901&count=2&sid=0"
{
  "data": [
    {
      "sender": "coinex1x6rhu5m53fw8qgpwuljauaptvxyur57zym4jly",
      "stock": "abc",
      "money": "cet",
      "amount": 60,
      "side": 1,
      "money_limit": 100,
      "transaction_price": "5.300000000000000000",
      "block_height": 300
    },
    {
      "sender": "coinex1x6rhu5m53fw8qgpwuljauaptvxyur57zym4jly",
      "stock": "abc",
      "money": "cet",
      "amount": 60,
      "side": 1,
      "money_limit": 100,
      "transaction_price": "5.300000000000000000",
      "block_height": 290
    }
  ],
  "timesid": [
    1566374447,
    105925,
    1566374447,
    105923
  ]
}
```

- Query Bancorlite-deal

```bash
$ curl -k "https://localhost:8000/bancorlite/deals?market=B:abc/cet&time=1567745901&count=1&sid=0"
$ curl "http://localhost:8000/bancorlite/deals?market=B:abc/cet&time=1567745901&count=2&sid=0" 
{
  "data": [
    {
      "sender": "coinex1yj66ancalgk7dz3383s6cyvdd0nd93q0tk4x0c",
      "stock": "abc",
      "money": "cet",
      "init_price": "1.000000000000000000",
      "max_supply": "10000000000000",
      "max_price": "500.000000000000000000",
      "price": "1.000000005988000000",
      "stock_in_pool": "9999999999880",
      "money_in_pool": "120",
      "earliest_cancel_time": 1917014400
    }
  ],
  "timesid": [
    1566374447,
    105926
  ]
}

```

- Query Bancorlite-info 

```bash
$ curl -k "https://localhost:8000/bancorlite/infos?market=abc/cet&time=1567745901&count=1&sid=0"
$ curl "http://localhost:8000/bancorlite/infos?market=abc/cet&time=1567745901&count=1&sid=0"
{
  "data": [
    {
      "sender": "coinex1yj66ancalgk7dz3383s6cyvdd0nd93q0tk4x0c",
      "stock": "abc",
      "money": "cet",
      "init_price": "1.000000000000000000",
      "max_supply": "10000000000000",
      "max_price": "500.000000000000000000",
      "price": "1.000000005988000000",
      "stock_in_pool": "9999999999880",
      "money_in_pool": "120",
      "earliest_cancel_time": 1917014400
    }
  ],
  "timesid": [
    1566374447,
    105926
  ]
}
```

- Query Redelegations

```bash
$ curl -k "https://localhost:8000/expiry/redelegations?account=coinex18rdsh78t4ds76p58kum34rye2pmrt3hj8z2ehg&time=1567745901&sid=0&count=1"
$ curl "http://localhost:8000/expiry/redelegations?account=coinex18rdsh78t4ds76p58kum34rye2pmrt3hj8z2ehg&time=1567745901&sid=0&count=1"
{
  "data": [
    {
      "delegator": "coinex18rdsh78t4ds76p58kum34rye2pmrt3hj8z2ehg",
      "src": "coinexvaloper1z6vr3s5nrn5d6fyxl5vmw77ehznme07w9dan6x",
      "dst": "coinexvaloper16pr4xqlsglwu6urkyt975nxzl65hlt2fw0n58d",
      "amount": "200000000000",
      "completion_time": "2019-08-21T16:00:50+08:00"
    }
  ],
  "timesid": [
    1566374450,
    105919
  ]
}
```

- Query Unbindings 

```bash
$ curl -k "https://localhost:8000/expiry/unbondings?account=coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw&time=1567745901&sid=0&count=1"
$ curl "http://localhost:8000/expiry/unbondings?account=coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw&time=1567745901&sid=0&count=1"
{
  "data": [
    {
      "delegator": "coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw",
      "validator": "coinexvaloper1yj66ancalgk7dz3383s6cyvdd0nd93q0sekwpv",
      "amount": "100000",
      "completion_time": "2019-08-21T08:00:49.505077Z"
    }
  ],
  "timesid": [
    1566374449,
    105927
  ]
}
```

- Query Unlocks 

```bash
$ curl -k "https://localhost:8000/expiry/unlocks?account=coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw&time=1567745901&sid=0&count=1" 
$ curl "http://localhost:8000/expiry/unlocks?account=coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw&time=1567745901&sid=0&count=1" 
{
  "data": [
    {
      "address": "coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw",
      "unlocked": [
        {
          "denom": "cet",
          "amount": "1000000"
        }
      ],
      "locked_coins": null,
      "frozen_coins": [
        {
          "denom": "abc",
          "amount": "796912961248"
        },
        {
          "denom": "cet",
          "amount": "1896049635319"
        }
      ],
      "coins": [
        {
          "denom": "abc",
          "amount": "24999230553270264"
        },
        {
          "denom": "cet",
          "amount": "24971271794985539"
        }
      ],
      "height": 669
    }
  ],
  "timesid": [
    1566374455,
    105949
  ]
}
```

- Query Locked Coins 

```bash
$ curl -k "https://localhost:8000/expiry/lockeds?account=coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw&time=1569862862&sid=0&count=1"
$ curl "http://localhost:8000/expiry/lockeds?account=coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw&time=1569862862&sid=0&count=1"
{
  "data": [
    {
      "from_address": "coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw",
      "to_address": "coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw",
      "amount": [
        {
          "denom": "cet",
          "amount": "1000000"
        }
      ],
      "unlock_time": "1569862861"
    }
  ],
  "timesid": [
    1566374466,
    13246
  ]
}
```
- Query Transactions Based on Hash 

```bash
$ curl -k "https://localhost:8000/tx/txs/2B6D7633C460DAABFCA47592B7F76A95CE95C52B515179C9E9BA49AA620377BA" 
$ curl "http://localhost:8000/tx/txs/2B6D7633C460DAABFCA47592B7F76A95CE95C52B515179C9E9BA49AA620377BA" 
{
  "signers": [
    "coinex1avmxlmztzxap20hawpc85h3uzj3277ja88wec2"
  ],
  "transfers": [
    {
      "sender": "coinex1avmxlmztzxap20hawpc85h3uzj3277ja88wec2",
      "recipient": "coinex1j0awxx9lf32y235esjkwvs8h36whqj7f699f8z",
      "amount": "600000000cet"
    }
  ],
  "serial_number": 13827,
  "msg_types": [
    "MsgSend"
  ],
  "tx_json": "{\"msg\":[{\"from_address\":\"coinex1avmxlmztzxap20hawpc85h3uzj3277ja88wec2\",\"to_address\":\"coinex1j0awxx9lf32y235esjkwvs8h36whqj7f699f8z\",\"amount\":[{\"denom\":\"cet\",\"amount\":\"600000000\"}],\"unlock_time\":0}],\"fee\":{\"amount\":[{\"denom\":\"cet\",\"amount\":\"200000000\"}],\"gas\":1000000},\"signatures\":[{\"pub_key\":[3,179,51,177,38,165,239,131,137,70,140,45,16,32,32,180,206,208,147,220,179,33,64,50,7,222,134,13,154,168,169,84,223],\"signature\":\"srnjer9+PRkcuZCQjGLQfcBEU+1wO/jPFy9h6sxJBvBwoZ3FuSSC00mvmPmPz/2R9nJ3gGG/KVzIZ128XgIk9w==\"}],\"memo\":\"send money from one account to another\"}",
  "height": 1928,
  "hash": ""
}
```
- Query Signed Transactions 

```bash
$ curl -k "https://localhost:8000/tx/txs?account=coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw&time=1567745901&count=2&sid=0"  
$ curl "http://localhost:8000/tx/txs?account=coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw&time=1567745901&count=2&sid=0" 
{
  "data": [
    {
      "signers": [
        "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge"
      ],
      "transfers": [
        {
          "sender": "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge",
          "recipient": "coinex1jv65s3grqf6v6jl3dp4t6c9t9rk99cd8vc4efa",
          "amount": "100000000cet"
        },
        {
          "sender": "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge",
          "recipient": "coinex1py9lss4nr0lm6ep4uwk3tclacw42a5nx0ra92r",
          "amount": "10cet"
        }
      ],
      "serial_number": 1,
      "msg_types": [
        "MsgCommentToken"
      ],
      "tx_json": "{\"msg\":[{\"sender\":\"coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge\",\"token\":\"cet\",\"donation\":100000000,\"title\":\"RE: I-Love-CET\",\"content\":\"WWVzLS1jZXQtdG8tdGhlLW1vb24=\",\"content_type\":3,\"references\":[{\"id\":0,\"reward_target\":\"coinex1py9lss4nr0lm6ep4uwk3tclacw42a5nx0ra92r\",\"reward_token\":\"cet\",\"reward_amount\":10,\"attitudes\":[50,56]}]}],\"fee\":{\"amount\":[{\"denom\":\"cet\",\"amount\":\"100000000\"}],\"gas\":200000},\"signatures\":[{\"pub_key\":[2,84,68,132,220,55,74,188,138,189,60,200,212,17,85,23,18,33,77,198,3,109,55,255,129,211,61,30,151,237,51,91,12],\"signature\":\"AvQcD0F+wj7ffTjI3fia6qybVvYK7thH/JPBHV2LoI5wU1rrU7LKgMQ7tqJFPuH6ZLpoXTIE4e9ELMmdH7jK/Q==\"}],\"memo\":\"Follow-up RE: “I-Love-CET” was commented by the user node1 on the CET Forum with an attached donation of 100,000,000 sato.CET. The details are as follows: \"}",
      "height": 16
    },
    {
      "signers": [
        "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge"
      ],
      "transfers": [
        {
          "sender": "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge",
          "recipient": "coinex1jv65s3grqf6v6jl3dp4t6c9t9rk99cd8vc4efa",
          "amount": "100000000cet"
        },
        {
          "sender": "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge",
          "recipient": "coinex1py9lss4nr0lm6ep4uwk3tclacw42a5nx0ra92r",
          "amount": "10cet"
        }
      ],
      "serial_number": 1,
      "msg_types": [
        "MsgCommentToken"
      ],
      "tx_json": "{\"msg\":[{\"sender\":\"coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge\",\"token\":\"cet\",\"donation\":100000000,\"title\":\"RE: I-Love-CET\",\"content\":\"WWVzLS1jZXQtdG8tdGhlLW1vb24=\",\"content_type\":3,\"references\":[{\"id\":0,\"reward_target\":\"coinex1py9lss4nr0lm6ep4uwk3tclacw42a5nx0ra92r\",\"reward_token\":\"cet\",\"reward_amount\":10,\"attitudes\":[50,56]}]}],\"fee\":{\"amount\":[{\"denom\":\"cet\",\"amount\":\"100000000\"}],\"gas\":200000},\"signatures\":[{\"pub_key\":[2,84,68,132,220,55,74,188,138,189,60,200,212,17,85,23,18,33,77,198,3,109,55,255,129,211,61,30,151,237,51,91,12],\"signature\":\"AvQcD0F+wj7ffTjI3fia6qybVvYK7thH/JPBHV2LoI5wU1rrU7LKgMQ7tqJFPuH6ZLpoXTIE4e9ELMmdH7jK/Q==\"}],\"memo\":\"Follow-up RE: “I-Love-CET” was commented by the user node1 on the CET Forum with an attached donation of 100,000,000 sato.CET. The details are as follows:\"}",
      "height": 16
    }
  ],
  "timesid": [
    1566374449,
    105929,
    1566374449,
    92659
  ]
}
```

- Query Tx Incomes

```bash
$ curl -k "https://localhost:8000/tx/incomes?account=coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw&time=1567745901&count=2&sid=0"
$ curl "http://localhost:8000/tx/incomes?account=coinex1tlegt4y40m3qu3dd4zddmjf6u3rswdqk8xxvzw&time=1567745901&count=2&sid=0"
{
  "data": [
    {
      "signers": [
        "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge"
      ],
      "transfers": [
        {
          "sender": "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge",
          "recipient": "coinex1jv65s3grqf6v6jl3dp4t6c9t9rk99cd8vc4efa",
          "amount": "100000000cet"
        },
        {
          "sender": "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge",
          "recipient": "coinex1py9lss4nr0lm6ep4uwk3tclacw42a5nx0ra92r",
          "amount": "10cet"
        }
      ],
      "serial_number": 1,
      "msg_types": [
        "MsgCommentToken"
      ],
      "tx_json": "{\"msg\":[{\"sender\":\"coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge\",\"token\":\"cet\",\"donation\":100000000,\"title\":\"RE: I-Love-CET\",\"content\":\"WWVzLS1jZXQtdG8tdGhlLW1vb24=\",\"content_type\":3,\"references\":[{\"id\":0,\"reward_target\":\"coinex1py9lss4nr0lm6ep4uwk3tclacw42a5nx0ra92r\",\"reward_token\":\"cet\",\"reward_amount\":10,\"attitudes\":[50,56]}]}],\"fee\":{\"amount\":[{\"denom\":\"cet\",\"amount\":\"100000000\"}],\"gas\":200000},\"signatures\":[{\"pub_key\":[2,84,68,132,220,55,74,188,138,189,60,200,212,17,85,23,18,33,77,198,3,109,55,255,129,211,61,30,151,237,51,91,12],\"signature\":\"AvQcD0F+wj7ffTjI3fia6qybVvYK7thH/JPBHV2LoI5wU1rrU7LKgMQ7tqJFPuH6ZLpoXTIE4e9ELMmdH7jK/Q==\"}],\"memo\":\"Follow-up RE: “I-Love-CET” was commented by the user node1 on the CET Forum with an attached donation of 100,000,000 sato.CET. The details are as follows: \"}",
      "height": 16
    },
    {
      "signers": [
        "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge"
      ],
      "transfers": [
        {
          "sender": "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge",
          "recipient": "coinex1jv65s3grqf6v6jl3dp4t6c9t9rk99cd8vc4efa",
          "amount": "100000000cet"
        },
        {
          "sender": "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge",
          "recipient": "coinex1py9lss4nr0lm6ep4uwk3tclacw42a5nx0ra92r",
          "amount": "10cet"
        }
      ],
      "serial_number": 1,
      "msg_types": [
        "MsgCommentToken"
      ],
      "tx_json": "{\"msg\":[{\"sender\":\"coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge\",\"token\":\"cet\",\"donation\":100000000,\"title\":\"RE: I-Love-CET\",\"content\":\"WWVzLS1jZXQtdG8tdGhlLW1vb24=\",\"content_type\":3,\"references\":[{\"id\":0,\"reward_target\":\"coinex1py9lss4nr0lm6ep4uwk3tclacw42a5nx0ra92r\",\"reward_token\":\"cet\",\"reward_amount\":10,\"attitudes\":[50,56]}]}],\"fee\":{\"amount\":[{\"denom\":\"cet\",\"amount\":\"100000000\"}],\"gas\":200000},\"signatures\":[{\"pub_key\":[2,84,68,132,220,55,74,188,138,189,60,200,212,17,85,23,18,33,77,198,3,109,55,255,129,211,61,30,151,237,51,91,12],\"signature\":\"AvQcD0F+wj7ffTjI3fia6qybVvYK7thH/JPBHV2LoI5wU1rrU7LKgMQ7tqJFPuH6ZLpoXTIE4e9ELMmdH7jK/Q==\"}],\"memo\":\"Follow-up RE: “I-Love-CET” was commented by the user node1 on the CET Forum with an attached donation of 100,000,000 sato.CET. The details are as follows: \"}",
      "height": 16
    }
  ],
  "timesid": [
    1566374449,
    105930,
    1566374449,
    92660
  ]
}
```

- Query Token's Forum 

```bash
$ curl -k "https://localhost:8000/comment/comments?token=cet&time=1567745901&count=1&sid=0"
$ curl "http://localhost:8000/comment/comments?token=cet&time=1567745901&count=1&sid=0"
{
  "data": [
    {
      "id": 2,
      "height": 19,
      "sender": "coinex1a28ertglt4rn9z8lkasq48fh7s7n484dfpflqt",
      "token": "cet",
      "donation": 0,
      "title": "",
      "content": "No-Content",
      "content_type": 3,
      "references": [
        {
          "id": 0,
          "reward_target": "coinex1py9lss4nr0lm6ep4uwk3tclacw42a5nx0ra92r",
          "reward_token": "cet",
          "reward_amount": 20,
          "attitudes": [
            50,
            59
          ]
        },
        {
          "id": 1,
          "reward_target": "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge",
          "reward_token": "cet",
          "reward_amount": 10,
          "attitudes": [
            50,
            56
          ]
        }
      ]
    }
  ],
  "timesid": [
    1566374451,
    105941
  ]
}
```

- Query Validator's Slash 

```bash
$ curl -k "https://localhost:8000/slash/slashings?time=1567745901&count=1&sid=0"
$ curl "http://localhost:8000/slash/slashings?time=1567745901&count=1&sid=0"
{
  "data": [
    {
      "validator": "coinexvalcons1qwztwxzzndpdc94tujv8fux9phfenqmvx296zw",
      "power": "1000000",
      "reason": "double_sign",
      "jailed": true
    }
  ],
  "timesid": [
    1566374442,
    105918
  ]
}
```

- Query Donations 

```bash
$ curl -k "https://localhost:8000/misc/donations?time=1569862862&sid=0&count=2"
$ curl "http://localhost:8000/misc/donations?time=1569862862&sid=0&count=2"
{
  "data": [
    {
      "sender": "coinex1k8ygwdfuagq0mg6d7zr5pgj92qa8532a3f7xge",
      "amount": "100000000"
    },
    {
      "sender": "coinex1py9lss4nr0lm6ep4uwk3tclacw42a5nx0ra92r",
      "amount": "200000000"
    }
  ],
  "timesid": [
    1566374451,
    13228,
    1566374451,
    13222
  ]
}
```

- Query Cancelled Trading Pairs

```bash
$ curl -k "https://localhost:8000//market/delist?market=lkk2/cet&time=1569862862&sid=0&count=2"
$ curl "http://localhost:8000/market/delist?market=lkk2/cet&time=1569862862&sid=0&count=2"
{
  "data": [
     1668700800
  ],
  "timesid": [
    1566374451,
    2
  ]
}
```


- Query Cancelled Trading Pair List 

```bash
$ curl -k "https://localhost:8000/market/delists?time=1569862862&sid=0&count=2"
$ curl "http://localhost:8000/market/delists?time=1569862862&sid=0&count=2"
{
    "data": [
        {
            "trading_pair": "lkp237/cet", 
            "cancel_time": 1668700800
        }, 
        {
            "trading_pair": "lkp37/cet", 
            "cancel_time": 1668700800
        }, 
        {
            "trading_pair": "lkp/cet", 
            "cancel_time": 1668700800
        }
    ], 
    "timesid": [
        1571633845, 
        13, 
        1571633838, 
        7, 
        1571633838, 
        4
    ]
}
```




