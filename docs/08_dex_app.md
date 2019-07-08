## DEX Applications

CoinEx Chain only provides the basic functionality for a DEX. To build a full user experience of DEX, you need a back-end server to provide the depth information and k-lines, etc; and you need a front-end application to display depth graphs, k-line graphs, etc.

A full node of CoinEx Chain is lean and only contains the essential functions, so it does not directly provide information of depth and k-line. Instead, it dumps necessary messages to Kafka, and a dedicated program will process these messages and acts as the back-end server for the front-end applications.

### JSON Messages

A CoinEx Chain's full node can be configured to dump json messages to Kafka. These json strings are generated using Golang's built-in `json` package from Golang's structs. These structs are described as below.

Whenever a new block is committed, the following message is produced.
```go
type NewHeightInfo struct {
	Height    int64 `json:"height"`
	Timestamp int64 `json:"timestamp"`
}
```

If the coins owned by some account is changed in a block,  the following message is produced.
```go
type AccountCoinsChangedInfo struct {
	Account string  `json:"account"`
	ChangedCoins Coins `json:"changed_coins"`
}
```

Whenever a new market (trading pair) is created,  the following message is produced.
```go
type CreateMarketInfo struct {
	Stock          string `json:"stock"`
	Money          string `json:"money"`
	PricePrecision byte   `json:"price_precision"`

	Creator      string `json:"creator"`
	CreateHeight int64  `json:"create_height"`
}
```

When a market is canceled, i.e., a trading pair is delisted, the following message is produced.
```go
type CancelMarketInfo struct {
	Stock string `json:"stock"`
	Money string `json:"money"`
	Deleter string `json:"deleter"`
	DelTime int64  `json:"del_time"`
}
```

Whenever an account creates a new order, the following message is produced.
```go
type CreateOrderInfo struct {
	OrderID     string `json:"order_id"`
	Sender      string `json:"sender"`
	Symbol      string `json:"symbol"`
	OrderType   byte   `json:"order_type"` //must be 2 for LIMIT order
	Price       string `json:"price"`
	Quantity    int64  `json:"quantity"`
	Side        byte   `json:"side"`  // 1 for buy and 2 for sell
	TimeInForce int    `json:"time_in_force"` // 3 for GTE and 4 for IOC
	Height      int64  `json:"height"`
    FeatureFee  int64  `json:"feature_fee"` //the feature fee paid to lengthen GTE order' life time.
	FrozenFee   int64  `json:"frozen_fee"` //the frozen feature fee according to the quantity of stock
	Freeze      int64  `json:"freeze"` //the frozen stock or money tokens
}
```

Whenever an order is filled or partial-filled, the following message is produced.
```go
type FillOrderInfo struct {
	OrderID string `json:"order_id"`
	Height  int64  `json:"height"`

	// These fields will change when order was filled/canceled.
	LeftStock int64  `json:"left_stock"`
	Freeze    int64  `json:"freeze"`
	DealStock int64  `json:"deal_stock"`
	DealMoney int64  `json:"deal_money"`
	CurrStock int64  `json:"curr_stock"` //Amount of the dealt stock
	Price     string `json:"price"`
}
```

When an order is deleted from order book, because of user's manual operation, expiration of fully-filled, the following message is produced.
```go
type CancelOrderInfo struct {
	OrderID string `json:"order_id"`

	// Del infos
	DelReason string `json:"del_reason"`
	DelHeight int64  `json:"del_height"`

	// Fields of amount
	UsedCommission       int64 `json:"used_commission"`
	LeftStock    int64 `json:"left_stock"`
	RemainAmount int64 `json:"remain_amount"`
	DealStock    int64 `json:"deal_stock"`
	DealMoney    int64 `json:"deal_money"`
}
```

Whenever some coins are sent from an address to another address, the following message is produced.
```go
type MsgSend struct {
	FromAddress sdk.AccAddress `json:"from_address"`
	ToAddress   sdk.AccAddress `json:"to_address"`
	Amount      sdk.Coins      `json:"amount"`
	UnlockTime  int64          `json:"unlock_time"` // it's a non-zero value for a locked transfer
}
```

Whenever an account change's its "Memo-Required" option, the following message is produced.
```go
type MsgSetMemoRequired struct {
	Address  sdk.AccAddress `json:"address"`
	Required bool           `json:"required"`
}
```



