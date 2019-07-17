## DEX Applications

CoinEx Chain only provides the basic functionality for a DEX. To build a full user experience of DEX, you need a back-end server to provide the depth information and k-lines, etc; and you need a front-end application to display depth graphs, k-line graphs, etc.

A full node of CoinEx Chain is lean and only contains the essential functions, so it does not directly provide information of depth and k-line. Instead, it dumps necessary messages to Kafka, and a dedicated program will process these messages and acts as the back-end server for the front-end applications.

### JSON Messages

A CoinEx Chain's full node can be configured to dump json messages to Kafka. These json messages can be divided into two kinds. 

The first kind of messages are constructed from Tendermint's "events". After a transaction's execution, it will generate some events and return them to Tendermint. These events carry important information, so we convert them into json format and dump them to Kafka. 

The second kind of messages are only sent to Kafka and Tendermint can not get them. These json strings are generated using Golang's built-in `json` package from Golang's structs. These structs are described as below.

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

Whenever a user sends a comment transaction successfully, the following `TokenComment` message is produced.
```go
type TokenComment struct {
	ID          uint64         `json:"id"`
	Sender      sdk.AccAddress `json:"sender"`
	Token       string         `json:"token"`
	Donation    int64          `json:"donation"`
	Title       string         `json:"title"`
	Content     string         `json:"content"`
	ContentType int8           `json:"content_type"`
	References  []CommentRef   `json:"references"`
}

type CommentRef struct {
	ID           uint64         `json:"id"`
	RewardTarget sdk.AccAddress `json:"reward_target"`
	RewardToken  string         `json:"reward_token"`
	RewardAmount int64          `json:"reward_amount"`
	Attitudes    []int32        `json:"attitudes"`
}
```

A comment has its Title and Content, both of which are plain texts.  ContentType is an integer to indiciate the type of the content, and its possible values include: **0** for IPFS hash id, **1** for magnet link, **2** for http link, **3** for plain utf8 text, **6** for base64-encoded byte string.

ID is a unique number assigned to each comment. Sender is the an account who sent this comment, whose presentation is a bech32 address. Token is the symbol of the token which is commented about. Donation is the amount of CET which will be donated to community pool. The sender can promote her comment's weight by donating more coins to community pool.  

A comment can reference other comments and the entry in a reference include: `ID`, the referenced comment's ID number; `RewardTarget`, the comment's sender which can be rewarded; `RewardToken` and `RewardAmount`, the coins that are rewarded to the comment's sender; `Attitudes`, a list of integer values showing the attitudes to this referenced comment. Valid attitudes are: **50** for "Like", **51** for "Dislike", **52** for "Laugh", **53** for "Cry", **54** for "Angry", **55** for "Surprise", **56** for "Heart", **57** for "Sweat", **58** for "Speechless", **59** for "Favorite", **60** for "Condolences".

When there is no reference, the comment opens a new thread. When there is only one reference, the comment should be displayed as a follow-up comment in an existing thread. 

When there are two or more references, this comment's title and content are ignored, and its only usage is to reward the comments' sender that it references.

