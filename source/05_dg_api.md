## APIs

CoinEx Chain is developed using tendermeint as the consensus engine. Tendermint provides a low-level API and CoinEx Chain inherits it. This API is usually served on port 26657.

This [low-level tendermint API](./api/tendermint-rest-api.html) is relateively hard to use. Currently only `cetcli` uses it.

It is recommended to use another feature-rich API provided by `cetd`. This API does not have a usually port. You can serve it at normal http ports (80, 8080) or any ports you like. 

This API is made up with two parts: the basic part and the extended part. The [basic part](./api/rest-api.html) can:

1. query the current status of the blockchain
2. construct unsigned transactions in json format, which can be signed by a wallet

And the extended part can:

1. [query](./api/tradeserver-rest-api.html) the historical status of the blockchain
2. [subscribe](./ext-watcher/websocket-subscription.md) for the status updates

These two parts are served at the same port. And the extended API is optional. Why is it optional? Because it is provided by an internal "watcher program" inside `cetd`, which needs extra CPU time, memory and hard disk to work. If the extended API is not necessary for your application, it's better NOT to run this  "watcher program" from beginning.

