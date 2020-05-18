## Python SDK

We provide python version of the SDK library to make it easier for python developers to use  functions of  dex chain, and the SDK provides support for all the basic and extended apis. Combined with [polarbear](https://github.com/coinexchain/polarbear/) library as a dynamic link library, developers can perform the functions that wallet and browser applications rely on, such as secret key management, transaction assembly, signature, and send transaction to chain.

### Usage

Initialize the `keybase` used for signature and get the transaction sender address `from`

```Python
keybase = ctypes.CDLL('./wallet_mac.so')
keybase.BearInit('tmp'.encode("utf-8"))
get_address = keybase.GetAddress
get_address.restype = ctypes.c_char_p
from_addr_bytes = get_address(key_name)
```

Initialize the SDK context

```Python
base_req = {
        'account_number': "0",
        'chain_id': "coinexdex-test1",
        'fees': fee,
        'from': from_addr_str,
        'gas': gas,
        'gas_adjustment': "1.1",
        'memo': "",
        'sequence': "0",
        'simulate': False,
}
ctx = ApiContext(from_addr_str, key_name, password, base_req, lib)
```

Set the chain ID, sender address,  sender's key_name, password and so on in the context.

initalize node url

```Python
config = Configuration()
config.host = "127.0.0.1:1317"
```

Assembly transaction parameters

Take a normal transfer as an example, provide the transfer target address, transfer quantity, transfer currency and unlock time

```Python
coins = [
        {
            'denom': 'cet',
            'amount': "1000000000"
        }
]
to_addr = "coinex10c22dwn7hxps77tnkpj5pzu9zcpq5zf76xms55"
unlock_time = "0"
account = Account(base_req, coins, unlock_time)
```

Sign and send transaction to chain

```Python
client = ApiClient(configuration=config)
(res, res_str) = BankApi(client).send_coins(to_addr, account)
ctx.sign_and_broadcast(res_str.data)
```

### Details

Repository: https://github.com/coinexchain/dex-api-python

List of supported APIs: 

[Basic APIs](./api/rest-api.html) 

[Extended Query APIs](./api/tradeserver-rest-api.html)

[Extended Subscribe APIs](./ext-watcher/websocket-subscription.md)

