## Frequently Asked Questions

#### What's the difference between CoinEx DEX chain and previous DEXs based on smart contracts?

There have been various decentralized exchanges based on smart contracts, such as [WhaleEx](http://www.whaleex.com/), [Newdex](http://newdex.io) and [IDEX](http://idex.market), etc. Their performance is limited by the underlying public chain's capacity, which is shared among many Dapps. Further more, interpreting the smart contracts' byte codes is slower than executing native-supported transactions. 

CoinEx DEX chain is an application-specific chain. It is built dedicatedly for DEX, which means its capacity is fully devoted to DEX and it only natively supports the essential functions needed by DEX. So it can rival centralized exchanges in smooth and responsive user experience.



#### Is CoinEx Chain operated by CoinEx.com?

No, CoinEx Chain is operated by 42 validators elected by the community, who are responsible to propose and vote for new blocks. And to ensure its decentralization, CoinEx.com will not intervene the election or run for a validator.



#### Who can run as a validator for CoinEx Chain?

Anyone can become a validator candidate by sending the CreateValidator transaction. After the number of validators reaches 42, validators will be sorted by the amount of staked CET, and 42 validators with the highest staking quantity will be selected.  CET owners can delegate, undelegate or re-delegate their CET to different validators to change the staked quantities of them. 

For more information, please refer to XXX.



#### Why are there 42 validators?

Because it is a good trade-off between performance and decentralization, and, as you may know, 42 is also [Answer to the Ultimate Question of Life, The Universe, and Everything](https://en.wikipedia.org/wiki/Phrases_from_The_Hitchhiker's_Guide_to_the_Galaxy#Answer_to_the_Ultimate_Question_of_Life,_the_Universe,_and_Everything_(42)).



#### Will CET be inflated?

CoinEx will honor its previous promise not to issue additional CET and will not create new tokens by inflation. However, block incentive is crucial to community participation. Therefore, after the mainnet is launched, CoinEx Foundation will allocate about 315 million CET as incentives to the initial validators and staking participants.



#### Will CET still be repurchased and burnt by CoinEx.com?

The promised repurchase plan of CET will still be executed on time. 



#### What is the Fee Structure?

CoinEx Chain charges transaction fees and only CET is accepted. Transaction fees include two parts: the usual gas fee (like Ethereum) and feature fee. Gas is calculated according to the size of transaction, the signature count, the read/write count to persistent storage and the length of read/write data. Feature fee is an extra fee charged for some particular operations, for example, issuing a new asset, listing a new trading pair, activating an account and transferring tokens with a lock time. The matched orders are charged according to the dealt amount with a configurable rate (currently both 0.1% for maker and taker), which also falls in the category of feature fees. 



#### What kind of asset can be created?

Currently CoinEx Chain supports ERC20-like fungible tokens. Tokens' precision are fixed at 8 decimal digits and their total quantity must be no larger than 90 billion.  Some options can be turned on when issuing a token, which can not be changed thereafter. They are:  1) mintable; 2) burnable; 3) addresses can be blacklisted; 4) token can be globally forbidden with exceptions in a white list. With these options, stable coins and security tokens can be implemented.



#### How to list/delist a trading pair?

A token owner can list and/or delist her token against any other token, by paying a small amount of feature fee (10000 CET currently). The process is totally permissionless without any approval flow. The only requirement is that the first trading pair created for a token must be the one between it and CET.



#### How to register on CoinEx Chain?

You control your account with only the private key, so there is no need to register. But before an account can be operated,  it must be activated. A CET transfer to the new account can activate it and one CET will be deducted from the receivable tokens as feature fee.



#### How can I get back my account if the private key is stolen or forgot?

Sorry, you cannot. You take full responsibility for the private key protection, as you do in other block chains.



#### What kind of orders are supported on CoinEx Chain?

Currently only limit orders are supported. Market orders have potential security issues and will not be supported in the near future. Limited orders are divided into two categories: Good Till Expiry (GTE) and Immediate Or Cancel (IOC).  A GTE order expires at 00:00 UTC after the predefined life time, which can be lengthened by paying more feature fee; While a IOC expires at the next block after entering the order book (i.e., there is only one chance to be matched).



#### Are there limits on notional value of an order?

The notional value must be large enough that the feature fee is above a lower bound when the order is fully filled. Currently the fee rate is 0.1% for both makers and takers.



#### Where can I see my assets and trades?

You can use the official wallet which is a command line tool. User-friendly mobile wallets and desktop wallets  are under development and will soon be available. Chain explorers can also help check balances and transactions.



#### How can I deposit and withdraw tokens?

Inter-chain relay is the only fully-decentralized mechanism for transfer tokens from its native chain to another chain, which is under heavy development in Cosmos-SDK. When it is mature enough, it will be adopted such that users can transfer coins between CoinEx Chain and other chains, such as Bitcoin or Ehtereum network directly. 

Before that,  CoinEx.com will serve as a bridge to trade across tokens between CoinEx Chain and other chains. "Pegged Token" will be issued on CoinEx Chain for  trading digital asset from other block chains.  Users can deposit and withdraw these pegged tokens via CoinEx.com.

The pegged tokens are 100% backed by the native coin in reserve. The reserve addresses will be published for anyone to audit. They are as secure as the customers' assets in CoinEx.com.



#### Can I see orders/balances of others or can other people see my orders/balances?

Yes, all the on-chain data are open, including orders and balances. 

