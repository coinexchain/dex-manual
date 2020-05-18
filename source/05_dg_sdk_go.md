## Goland SDK

We provide goland version of the SDK library to make it easier for goland developers to use  functions of  dex chain, and the SDK provides support for all the basic and extended apis. Combined with [polarbear](https://github.com/coinexchain/polarbear/) library, developers can perform the functions that wallet and browser applications rely on, such as secret key management, transaction assembly, signature, and send transaction to chain.

### Usage

Initialize the `keybase` used for signature and get the transaction sender address `from`

```Go
import bear "github.com/coinexchain/polarbear"
bear.BearInit("keybase_path")
from := bear.GetAddress("key_name")
```

Initialize the SDK context

```Go
ctx := context.DefaultApiContext()
ctx.SetChainID("coinexdex2")
ctx.SetNodeUrl("127.0.0.1:1317")
ctx.SetFromAddress(from)
ctx.SetName("key_name")
ctx.SetPassword("12345678")
```

Set the chain ID, node url, sender address,  sender's key_name, password and so on in the context.

You can also set transaction fees and gas:

```Go
func (a *ApiContext) SetFees(amount string)
func (a *ApiContext) SetGas(amount string)
```

Assembly transaction parameters

Take a normal transfer as an example, provide the transfer target address, transfer quantity, transfer currency and unlock time

```Go
unlockTime := "0"
toAddress := "coinex1h6favnlytw3lgpy8cm6lcv530z0ctj6rplwt06"
transferredAmount := "400000000"
//refresh new account number and sequence first
ctx.RefreshAccNumAndSeq()
param := &bank.SendCoinsParams{
   Account: bank.SendCoinsBody{
      Amount: []*models.Coin{
         {
            Amount: &transferredAmount,
            Denom:  &context.DefaultDenom,
         },
      },
      BaseReq:    &ctx.BaseReq,
      UnlockTime: &unlockTime,
   },
   Address: toAddress,
}
param.SetTimeout(context.DefaultTimeOut)
```

Sign and send transaction to chain

```Go
unsignedResp, err := ctx.Client.Bank.SendCoins(param)
if err == nil {
   bz, _ := unsignedResp.GetPayload().MarshalBinary()
   resp, err := ctx.SignAndBroadcast(bz)
}
```

### Details

Repository: https://github.com/coinexchain/dex-api-go

List of supported APIs: 

[Basic APIs](./api/rest-api.html) 

[Extended Query APIs](./api/tradeserver-rest-api.html)

[Extended Subscribe APIs](./ext-watcher/websocket-subscription.md)

