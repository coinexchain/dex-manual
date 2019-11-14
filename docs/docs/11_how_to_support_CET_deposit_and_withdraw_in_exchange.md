### 1. Create deposit address

<details>
<summary>cetcli keys add deposit_addr</summary>

```
j@j ~ $ cetcli keys add deposit_addr
Enter a passphrase to encrypt your key to disk:
Repeat the passphrase:

- name: deposit_addr
  type: local
  address: cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y
  pubkey: cettestpub1addwnpepqtpmprxg4udznyxugfp76fkhu8un45fe6rc9rtgajaxzwj553p3wkvj4au8
  mnemonic: ""
  threshold: 0
  pubkeys: []


**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

loan oil sign camera leave tail stem fabric rival addict elite latin portion loyal paper shrimp trim man over fun bright cruise volume sign
j@j ~ $
```

</details>

### 2. Query deposit address status

<details>
<summary>cetcli q account cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y --chain-id=coinexdex-test2004</summary>


```
j@j ~ $ cetcli q account cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y --chain-id=coinexdex-test2004
{
  "address": "cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y",
  "coins": [
    {
      "denom": "cet",
      "amount": "499900000000"
    }
  ],
  "locked_coins": null,
  "frozen_coins": [],
  "public_key": null,
  "account_number": "25",
  "sequence": "0",
  "memo_required": false
}
```

</details>


### 3. Set deposit address only receive transfer with memo, to identity user in accounting system

<details>
<summary>cetcli tx require-memo true --from deposit_addr --chain-id=coinexdex-test2004 --gas=600000 --fees=12000000cet</summary>


```
j@j ~ $ cetcli tx require-memo true --from deposit_addr --chain-id=coinexdex-test2004 --gas=600000 --fees=12000000cet
{"chain_id":"coinexdex-test2004","account_number":"25","sequence":"0","fee":{"amount":[{"denom":"cet","amount":"12000000"}],"gas":"600000"},"msgs":[{"type":"bankx/MsgSetMemoRequired","value":{"address":"cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y","required":true}}],"memo":""}

confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'deposit_addr':
height: 0
txhash: 6B225990C22D06B9A59CADDB055C0AD9264921629CF714869D3913E278D0D589
code: 0
data: ""
rawlog: '[{"msg_index":0,"success":true,"log":""}]'



# deposit without memo, failed.
j@j ~ $ cetcli tx send cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y 50000000000000cet --from coinex_foundation --chain-id=coinexdex-test2004 --fees=100000000cet
{"chain_id":"coinexdex-test2004","account_number":"2","sequence":"3","fee":{"amount":[{"denom":"cet","amount":"100000000"}],"gas":"200000"},"msgs":[{"type":"bankx/MsgSend","value":{"from_address":"cettest1gr3tmchnapaxazkjvzdea3c3ljneyc9adj6338","to_address":"cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y","amount":[{"denom":"cet","amount":"50000000000000"}],"unlock_time":"0"}}],"memo":""}

confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'coinex_foundation':
height: 0
txhash: D4C71801EA1DA86F89287903C1CBBA1CCD848F4B8B98D1D7FABD0D68CAE7D7EE
code: 301
data: ""
rawlog: '{"codespace":"bankx","code":301,"message":"memo is empty"}'


# deposit with user id as memo, succeeded
j@j ~ $ cetcli tx send cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y 50000000000000cet --memo="uid_12345678" --from coinex_foundation --chain-id=coinexdex-test2004 --fees=100000000cet
{"chain_id":"coinexdex-test2004","account_number":"2","sequence":"3","fee":{"amount":[{"denom":"cet","amount":"100000000"}],"gas":"200000"},"msgs":[{"type":"bankx/MsgSend","value":{"from_address":"cettest1gr3tmchnapaxazkjvzdea3c3ljneyc9adj6338","to_address":"cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y","amount":[{"denom":"cet","amount":"50000000000000"}],"unlock_time":"0"}}],"memo":"uid_12345678"}

confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'coinex_foundation':
height: 0
txhash: F66699CB93C363D42D88EC0D08973B43195BA7439E7F0BE5A2B53535B1D3A3B3
code: 0
data: ""
rawlog: '[{"msg_index":0,"success":true,"log":""}]'

```

</details>


### 4. Query deposit transactions by REST API

<details>
<summary>curl -X GET "http://18.190.80.148:1317/txs?transfer.recipient=cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y&message.action=send&page=1&limit=100" -H  "accept: application/json"</summary>

```
curl -X GET "http://18.190.80.148:1317/txs?transfer.recipient=cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y&message.action=send&page=1&limit=100" -H  "accept: application/json" | jq
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  2705    0  2705    0     0   2877      0 --:--:-- --:--:-- --:--:--  2874
{
  "total_count": "2",
  "count": "2",
  "page_number": "1",
  "page_total": "1",
  "limit": "100",
  "txs": [
    {
      "height": "38041",
      "txhash": "9646DB5C5E17E094D59C9F00D287024CCEF38470A20784109297B9F5A0EB94D0",
      "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\"}]",
      "logs": [
        {
          "msg_index": 0,
          "success": true,
          "log": ""
        }
      ],
      "gas_wanted": "100000",
      "gas_used": "67646",
      "events": [
        {
          "type": "message",
          "attributes": [
            {
              "key": "action",
              "value": "send"
            },
            {
              "key": "sender",
              "value": "cettest197l42dv2d4xt9dvuxstk5jw0pmvhlggyc9hhu4"
            },
            {
              "key": "module",
              "value": "bankx"
            },
            {
              "key": "sender",
              "value": "cettest197l42dv2d4xt9dvuxstk5jw0pmvhlggyc9hhu4"
            }
          ]
        },
        {
          "type": "transfer",
          "attributes": [
            {
              "key": "recipient",
              "value": "cettest17xpfvakm2amg962yls6f84z3kell8c5lyj9pgw"
            },
            {
              "key": "amount",
              "value": "100000000cet"
            },
            {
              "key": "recipient",
              "value": "cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y"
            },
            {
              "key": "amount",
              "value": "499900000000cet"
            }
          ]
        }
      ],
      "tx": {
        "type": "cosmos-sdk/StdTx",
        "value": {
          "msg": [
            {
              "type": "bankx/MsgSend",
              "value": {
                "from_address": "cettest197l42dv2d4xt9dvuxstk5jw0pmvhlggyc9hhu4",
                "to_address": "cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y",
                "amount": [
                  {
                    "denom": "cet",
                    "amount": "500000000000"
                  }
                ],
                "unlock_time": "0"
              }
            }
          ],
          "fee": {
            "amount": [
              {
                "denom": "cet",
                "amount": "2000000"
              }
            ],
            "gas": "100000"
          },
          "signatures": [
            {
              "pub_key": {
                "type": "tendermint/PubKeySecp256k1",
                "value": "Aik6Wqowg4bhpUKc6dIOSFY5LchK58Tu1Dnsao3kHGbB"
              },
              "signature": "nOInyy4NrLNvpWgyRMV2UhASw3xi5BW6+1JVoFD1ZcZf2KyIqc4nnpxvdug0zyzsU5xG9FhagftWSoUxsTU92g=="
            }
          ],
          "memo": ""
        }
      },
      "timestamp": "2019-10-22T06:46:39Z"
    },
```

</details>

### 5. Subscribe deposit txs in websocket, to update accounting system


<details>
<summary>5.1 subscribe python code</summary>

```python
import websocket
try:
    import thread
except ImportError:
    import _thread as thread
import time

def on_message(ws, message):
    print(message)

def on_error(ws, error):
    print(error)

def on_close(ws):
    print("### closed ###")

def on_open(ws):
    def run(*args):
        req = '''{
    "jsonrpc": "2.0",
    "method": "subscribe",
    "id": "1",
    "params": {
        "query": "tm.event = 'Tx' AND message.action = 'send' AND transfer.recipient = 'cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y'"
    }
}
        '''
        ws.send(req)
        time.sleep(6000)
        ws.close()
        print("thread terminating...")
    thread.start_new_thread(run, ())


if __name__ == "__main__":
    websocket.enableTrace(True)
    ws = websocket.WebSocketApp("ws://18.190.80.148:26657/websocket",
                              on_message = on_message,
                              on_error = on_error,
                              on_close = on_close)
    ws.on_open = on_open
    ws.run_forever()


```

</details>


<details>
<summary>5.2 send with memo when websocket connected and subscribed</summary>

```
j@j ~ $ cetcli tx send cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y 50000000000000cet --memo="uid_12345678" --from coinex_foundation --chain-id=coinexdex-test2004 --fees=100000000cet
{"chain_id":"coinexdex-test2004","account_number":"2","sequence":"5","fee":{"amount":[{"denom":"cet","amount":"100000000"}],"gas":"200000"},"msgs":[{"type":"bankx/MsgSend","value":{"from_address":"cettest1gr3tmchnapaxazkjvzdea3c3ljneyc9adj6338","to_address":"cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y","amount":[{"denom":"cet","amount":"50000000000000"}],"unlock_time":"0"}}],"memo":"uid_12345678"}

confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'coinex_foundation':
ERROR: Error reading passphrase: password must be at least 8 characters
j@j ~ $ cetcli tx send cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y 50000000000000cet --memo="uid_12345678" --from coinex_foundation --chain-id=coinexdex-test2004 --fees=100000000cet
{"chain_id":"coinexdex-test2004","account_number":"2","sequence":"5","fee":{"amount":[{"denom":"cet","amount":"100000000"}],"gas":"200000"},"msgs":[{"type":"bankx/MsgSend","value":{"from_address":"cettest1gr3tmchnapaxazkjvzdea3c3ljneyc9adj6338","to_address":"cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y","amount":[{"denom":"cet","amount":"50000000000000"}],"unlock_time":"0"}}],"memo":"uid_12345678"}

confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'coinex_foundation':
height: 0
txhash: 4AF7952343B10DC936B5572037BD5770E645ABFDFB560CD8380CE40639F99949
code: 0
data: ""
rawlog: '[{"msg_index":0,"success":true,"log":""}]'
```

</details>


<details>
<summary>5.3 Subscribed tx notifications from websocket</summary>

```
j@j ~ $ python3.6 ws2.py
--- request header ---
GET /websocket HTTP/1.1
Upgrade: websocket
Connection: Upgrade
Host: 18.190.80.148:26657
Origin: http://18.190.80.148:26657
Sec-WebSocket-Key: j51akR9gxFhXE7em16pZvA==
Sec-WebSocket-Version: 13


-----------------------
--- response header ---
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: WENmq9M3hEbTWHX5aEnTcU7dz/Y=
-----------------------
send: b'\x81\xfe\x00\xebA\xb5\xfe\x8d:\xbf\xde\xada\x95\xdc\xe72\xda\x90\xff1\xd6\xdc\xb7a\x97\xcc\xa3q\x97\xd2\x87a\x95\xde\xadc\xd8\x9b\xf9)\xda\x9a\xaf{\x95\xdc\xfe4\xd7\x8d\xee3\xdc\x9c\xe8c\x99\xf4\xada\x95\xde\xaf(\xd1\xdc\xb7a\x97\xcf\xafm\xbf\xde\xada\x95\xdc\xfd \xc7\x9f\xe02\x97\xc4\xad:\xbf\xde\xada\x95\xde\xada\x95\xdc\xfc4\xd0\x8c\xf4c\x8f\xde\xaf5\xd8\xd0\xe87\xd0\x90\xf9a\x88\xde\xaa\x15\xcd\xd9\xad\x00\xfb\xba\xad,\xd0\x8d\xfe \xd2\x9b\xa3 \xd6\x8a\xe4.\xdb\xde\xb0a\x92\x8d\xe8/\xd1\xd9\xad\x00\xfb\xba\xad5\xc7\x9f\xe32\xd3\x9b\xffo\xc7\x9b\xee(\xc5\x97\xe8/\xc1\xde\xb0a\x92\x9d\xe85\xc1\x9b\xfe5\x84\x99\xbd%\xd4\xca\xebr\xd9\xca\xe37\x83\x9f\xe7%\x86\xc8\xec-\xcc\x94\xe5$\x87\xcd\xe95\xc4\xcc\xb8/\xd1\x9b\xf8 \x8c\xc8\xf4f\x97\xf4\xada\x95\xde\xf0K\xc8\xf4\xada\x95\xde\xada\x95\xde'
{
  "jsonrpc": "2.0",
  "id": "1",
  "result": {}
}
{
  "jsonrpc": "2.0",
  "id": "1#event",
  "result": {
    "query": "tm.event = 'Tx' AND message.action = 'send' AND transfer.recipient = 'cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y'",
    "data": {
      "type": "tendermint/event/Tx",
      "value": {
        "TxResult": {
          "height": "46519",
          "index": 0,
          "tx": "3wEoKBapCkeON4ydChRA4r3i8+h6borSYJuexxH8p5JgvRIUQ9vapj+s2a7Jsdd+SV8qi1YFUm0aFQoDY2V0Eg41MDAwMDAwMDAwMDAwMBIWChAKA2NldBIJMTAwMDAwMDAwEMCaDBpqCibrWumHIQJ2sJOKNXv32TBXQ2X52tt9R3WsSYAt3qGR4RK0DFgbZRJArfgyeVI+VMXslfN6dJVHjcuQgdDLHh912gSO9dK03hglrYWqKV6HgmztNVl4rQsn2O4GvkoCFxyEmJ1cGd5c8SIMdWlkXzEyMzQ1Njc4",
          "result": {
            "log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\"}]",
            "gas_wanted": "200000",
            "gas_used": "51932",
            "events": [
              {
                "type": "message",
                "attributes": [
                  {
                    "key": "YWN0aW9u",
                    "value": "c2VuZA=="
                  }
                ]
              },
              {
                "type": "message",
                "attributes": [
                  {
                    "key": "bW9kdWxl",
                    "value": "YmFua3g="
                  }
                ]
              },
              {
                "type": "transfer",
                "attributes": [
                  {
                    "key": "cmVjaXBpZW50",
                    "value": "Y2V0dGVzdDFnMGRhNGYzbDRudjZhamQzNmFseWpoZTIzZHRxMjVuZGV1YTk2eQ=="
                  },
                  {
                    "key": "YW1vdW50",
                    "value": "NTAwMDAwMDAwMDAwMDBjZXQ="
                  }
                ]
              },
              {
                "type": "message",
                "attributes": [
                  {
                    "key": "c2VuZGVy",
                    "value": "Y2V0dGVzdDFncjN0bWNobmFwYXhhemtqdnpkZWEzYzNsam5leWM5YWRqNjMzOA=="
                  }
                ]
              }
            ]
          }
        }
      }
    },
    "events": {
      "transfer.recipient": [
        "cettest1g0da4f3l4nv6ajd36alyjhe23dtq25ndeua96y"
      ],
      "transfer.amount": [
        "50000000000000cet"
      ],
      "message.sender": [
        "cettest1gr3tmchnapaxazkjvzdea3c3ljneyc9adj6338"
      ],
      "tm.event": [
        "Tx"
      ],
      "tx.hash": [
        "4AF7952343B10DC936B5572037BD5770E645ABFDFB560CD8380CE40639F99949"
      ],
      "tx.height": [
        "46519"
      ],
      "message.action": [
        "send"
      ],
      "message.module": [
        "bankx"
      ]
    }
  }
}
^C
send: b'\x88\x82Oe\xca\x06L\x8d'
### closed ###
```

</details>


### 6. Withdraw user's tokens from your exchange

#### 6.1. construct tx send transaction

<details> <summary> Request REST API to construct unsigned transfer tx </summary>
<p>

```
---> POST http://localhost:27000/bank/accounts/coinex1yza205m7tkfchhalu2jk5hghe3dvh7dgmzz3pz/transfers

{'auth': None,
 'cookies': None,
 'data': '{"base_req": {"from": '
         '"coinex1k2qtl2lkr6yqhvraqsj385rl9axf8rewlfthcc", "memo": '
         '"\\u7528\\u6237node0\\u5411\\u7528\\u6237node1\\u8f6c\\u8d26500_0000_0000\\u6570\\u91cf\\u7684abc", '
         '"chain_id": "coinex-integrationtestxbMbuu", "gas": "200000", '
         '"gas_adjustment": "1", "fees": [{"denom": "cet", "amount": '
         '"100000000"}], "simulate": false}, "amount": [{"denom": "abc", '
         '"amount": "50000000000"}], "unlock_time": "0"}',
 'files': [],
 'headers': {'Content-Type': 'application/json'},
 'hooks': {'response': []},
 'json': None,
 'method': 'POST',
 'params': {},
 'url': 'http://localhost:27000/bank/accounts/coinex1yza205m7tkfchhalu2jk5hghe3dvh7dgmzz3pz/transfers'}
```
</p>
</details>
<details> <summary> Response with unsigned tx </summary>
<p>

```
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "fee": {
            "amount": [
                {
                    "amount": "100000000",
                    "denom": "cet"
                }
            ],
            "gas": "200000"
        },
        "memo": "\u7528\u6237node0\u5411\u7528\u6237node1\u8f6c\u8d26500_0000_0000\u6570\u91cf\u7684abc",
        "msg": [
            {
                "type": "bankx/MsgSend",
                "value": {
                    "amount": [
                        {
                            "amount": "50000000000",
                            "denom": "abc"
                        }
                    ],
                    "from_address": "coinex1k2qtl2lkr6yqhvraqsj385rl9axf8rewlfthcc",
                    "to_address": "coinex1yza205m7tkfchhalu2jk5hghe3dvh7dgmzz3pz",
                    "unlock_time": "0"
                }
            }
        ],
        "signatures": null
    }
}
```
</p>
</details>


#### 6.2.1 sign transaction by cetcli

```
echo 12345678 > /tmp/pass
cetcli tx sign --chain-id=coinexdex-test2004 --from from_addr /cetd/unsigned_tx_file < /tmp/pass"
```

#### 6.2.2 sign with golang / python SDK

please refer link for golang/python SDK details:
- [Golang SDK](https://github.com/coinexchain/polarbear/blob/master/doc/api.md)
- [Compile Golang SDK into .so for python user](https://github.com/coinexchain/polarbear)
- [Python demo](https://github.com/coinexchain/polarbear/blob/master/sdkforpython/demo.py)
- [Specify private keystore location](https://github.com/coinexchain/polarbear/blob/master/doc/manual.md)



### 7. deposit status can be double checked in REST API


<details> <summary> success deposit case:  </summary>
<p>

```
https://dex-api.coinex.org/txs/157CCB6BC47341F2A30DD2C16FB6EC6A0F555945CA8C7F4007FCFEE6C5FC23CB
{
    "events": [
        {
            "attributes": [
                {
                    "key": "sender",
                    "value": "coinex1kggcdxug9z4t83rfmck7zm4pg0hmqp3hzuku5q"
                },
                {
                    "key": "module",
                    "value": "bankx"
                },
                {
                    "key": "sender",
                    "value": "coinex1kggcdxug9z4t83rfmck7zm4pg0hmqp3hzuku5q"
                },
                {
                    "key": "action",
                    "value": "send"
                }
            ],
            "type": "message"
        },
        {
            "attributes": [
                {
                    "key": "recipient",
                    "value": "coinex17xpfvakm2amg962yls6f84z3kell8c5lm7j9tl"
                },
                {
                    "key": "amount",
                    "value": "100000000cet"
                },
                {
                    "key": "recipient",
                    "value": "coinex1q5prhf2wrwammvdl8xezyr4zkfdlwxxa9v0szc"
                },
                {
                    "key": "amount",
                    "value": "32999900000000cet"
                },
                {
                    "key": "sender",
                    "value": "coinex1kggcdxug9z4t83rfmck7zm4pg0hmqp3hzuku5q"
                }
            ],
            "type": "transfer"
        }
    ],
    "gas_used": "24160",
    "gas_wanted": "29040",
    "height": "83637",
    "logs": [
        {
            "events": [
                {
                    "attributes": [
                        {
                            "key": "sender",
                            "value": "coinex1kggcdxug9z4t83rfmck7zm4pg0hmqp3hzuku5q"
                        },
                        {
                            "key": "module",
                            "value": "bankx"
                        },
                        {
                            "key": "sender",
                            "value": "coinex1kggcdxug9z4t83rfmck7zm4pg0hmqp3hzuku5q"
                        },
                        {
                            "key": "action",
                            "value": "send"
                        }
                    ],
                    "type": "message"
                },
                {
                    "attributes": [
                        {
                            "key": "recipient",
                            "value": "coinex17xpfvakm2amg962yls6f84z3kell8c5lm7j9tl"
                        },
                        {
                            "key": "amount",
                            "value": "100000000cet"
                        },
                        {
                            "key": "recipient",
                            "value": "coinex1q5prhf2wrwammvdl8xezyr4zkfdlwxxa9v0szc"
                        },
                        {
                            "key": "amount",
                            "value": "32999900000000cet"
                        },
                        {
                            "key": "sender",
                            "value": "coinex1kggcdxug9z4t83rfmck7zm4pg0hmqp3hzuku5q"
                        }
                    ],
                    "type": "transfer"
                }
            ],
            "log": "",
            "msg_index": 0,
            "success": true
        }
    ],
    "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"sender\",\"value\":\"coinex1kggcdxug9z4t83rfmck7zm4pg0hmqp3hzuku5q\"},{\"key\":\"module\",\"value\":\"bankx\"},{\"key\":\"sender\",\"value\":\"coinex1kggcdxug9z4t83rfmck7zm4pg0hmqp3hzuku5q\"},{\"key\":\"action\",\"value\":\"send\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"coinex17xpfvakm2amg962yls6f84z3kell8c5lm7j9tl\"},{\"key\":\"amount\",\"value\":\"100000000cet\"},{\"key\":\"recipient\",\"value\":\"coinex1q5prhf2wrwammvdl8xezyr4zkfdlwxxa9v0szc\"},{\"key\":\"amount\",\"value\":\"32999900000000cet\"},{\"key\":\"sender\",\"value\":\"coinex1kggcdxug9z4t83rfmck7zm4pg0hmqp3hzuku5q\"}]}]}]",
    "timestamp": "2019-11-13T14:15:01Z",
    "tx": {
        "type": "cosmos-sdk/StdTx",
        "value": {
            "fee": {
                "amount": [
                    {
                        "amount": "580800",
                        "denom": "cet"
                    }
                ],
                "gas": "29040"
            },
            "memo": "",
            "msg": [
                {
                    "type": "bankx/MsgSend",
                    "value": {
                        "amount": [
                            {
                                "amount": "33000000000000",
                                "denom": "cet"
                            }
                        ],
                        "from_address": "coinex1kggcdxug9z4t83rfmck7zm4pg0hmqp3hzuku5q",
                        "to_address": "coinex1q5prhf2wrwammvdl8xezyr4zkfdlwxxa9v0szc",
                        "unlock_time": "0"
                    }
                }
            ],
            "signatures": [
                {
                    "pub_key": {
                        "type": "tendermint/PubKeySecp256k1",
                        "value": "AhAu40Tb4Jugm6srorATWJmGzCtzdZAYrcpLhqf1tzlT"
                    },
                    "signature": "oKjQYvokN18iQuA9WD1d5c279iA9oFE1/UeHgPywS8Uaz6zvL6hoAyzhmKYKRqmH1BKTA8fm8QsZbr742Kif6A=="
                }
            ]
        }
    },
    "txhash": "157CCB6BC47341F2A30DD2C16FB6EC6A0F555945CA8C7F4007FCFEE6C5FC23CB"
}

```
</p>
</details>








<details> <summary> success deposit case:  </summary>
<p>

```

https://dex-api.coinex.org/txs/0B19A39DD934FFFD0F9DC93A69679790ABDF3FA49E4E88AEDEE456918287D9E9

{
    "code": 608,
    "events": [
        {
            "attributes": [
                {
                    "key": "action",
                    "value": "create_order"
                }
            ],
            "type": "message"
        }
    ],
    "gas_used": "24060",
    "gas_wanted": "200000",
    "height": "81796",
    "logs": [
        {
            "events": [
                {
                    "attributes": [
                        {
                            "key": "action",
                            "value": "create_order"
                        }
                    ],
                    "type": "message"
                }
            ],
            "log": "{\"codespace\":\"market\",\"code\":608,\"message\":\"Insufficient coin\"}",
            "msg_index": 0,
            "success": false
        }
    ],
    "raw_log": "[{\"msg_index\":0,\"success\":false,\"log\":\"{\\\"codespace\\\":\\\"market\\\",\\\"code\\\":608,\\\"message\\\":\\\"Insufficient coin\\\"}\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"create_order\"}]}]}]",
    "timestamp": "2019-11-13T12:54:28Z",
    "tx": {
        "type": "cosmos-sdk/StdTx",
        "value": {
            "fee": {
                "amount": [
                    {
                        "amount": "4000000",
                        "denom": "cet"
                    }
                ],
                "gas": "200000"
            },
            "memo": "ifwallet",
            "msg": [
                {
                    "type": "market/MsgCreateOrder",
                    "value": {
                        "exist_blocks": "10000",
                        "identify": 0,
                        "order_type": 2,
                        "price": "12",
                        "price_precision": 3,
                        "quantity": "1310900000000",
                        "sender": "coinex1wkgdutvxaln0a3k4fsthqvqpqdjs85jcwngngk",
                        "side": 1,
                        "time_in_force": "3",
                        "trading_pair": "ift/cet"
                    }
                }
            ],
            "signatures": [
                {
                    "pub_key": {
                        "type": "tendermint/PubKeySecp256k1",
                        "value": "Aml8GUpFlG4AIY92j2bnnA/8If5MLnFDFEQ3bpLYkU1k"
                    },
                    "signature": "px4EtpaDCK93cjT5Hvp0vXwoKedTplWVuJNXmM8U685ZlI6d+MmKORh7Yn2eeThpYa0ims1BoUVrNFl7OhTcQw=="
                }
            ]
        }
    },
    "txhash": "0B19A39DD934FFFD0F9DC93A69679790ABDF3FA49E4E88AEDEE456918287D9E9"
}


```
</p>
</details>



#### notes

* [\#4990](https://github.com/cosmos/cosmos-sdk/issues/4990) Add `Events` to the `ABCIMessageLog` to
provide context and grouping of events based on the messages they correspond to. The `Events` field
in `TxResponse` is deprecated and will be removed in the next major release.
