# CET-Lite for CoinEx Chain
A REST interface for state queries, transaction generation and broadcasting.

## Version: 3.0

### Security
**kms**  

|basic|*Basic*|
|---|---|

### /version

#### GET
##### Summary:

Version of CET-lite

##### Description:

Get the version of cetcli

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Plaintext version i.e. "v0.25.0" |

### /node_version

#### GET
##### Summary:

Version of the connected node

##### Description:

Get the version of the SDK running on the connected node to compare against expected

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Plaintext version i.e. "v0.25.0" |
| 500 | failed to query node version |

### /node_info

#### GET
##### Summary:

The properties of the connected node

##### Description:

Information about the connected node

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Node status | object |
| 500 | Failed to query node status |  |

### /syncing

#### GET
##### Summary:

Syncing state of node

##### Description:

Get if the node is currently syning with other nodes

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | "true" or "false" |
| 500 | Server internal error |

### /blocks/latest

#### GET
##### Summary:

Get the latest block

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | The latest block | [BlockQuery](#blockquery) |
| 500 | Server internal error |  |

### /blocks/{height}

#### GET
##### Summary:

Get a block at a certain height

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| height | path | Block height | Yes | number |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | The block at a specific height | [BlockQuery](#blockquery) |
| 400 | Invalid height |  |
| 404 | Request block height doesn't |  |
| 500 | Server internal error |  |

### /validatorsets/latest

#### GET
##### Summary:

Get the latest validator set

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | The validator set at the latest block height | object |
| 500 | Server internal error |  |

### /validatorsets/{height}

#### GET
##### Summary:

Get a validator set a certain height

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| height | path | Block height | Yes | number |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | The validator set at a specific block height | object |
| 400 | Invalid height |  |
| 404 | Block at height not available |  |
| 500 | Server internal error |  |

### /txs/{hash}

#### GET
##### Summary:

Get a Tx by hash

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| hash | path | Tx hash | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Tx with the provided hash | [TxQuery](#txquery) |
| 500 | Internal Server Error |  |

### /txs

#### GET
##### Summary:

Search transactions

##### Description:

Search transactions by tag(s).

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| tag | query | transaction tags such as 'action=submit-proposal' and 'proposer=coinex1g9ahr6xhht5rmqven628nklxluzyv8z9jqjcmc' which results in the following endpoint: 'GET /txs?action=submit-proposal&proposer=coinex1g9ahr6xhht5rmqven628nklxluzyv8z9jqjcmc' | Yes | string |
| page | query | Page number | No | integer |
| limit | query | Maximum number of items per page | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | All txs matching the provided tags | [ [TxQuery](#txquery) ] |
| 400 | Invalid search tags |  |
| 500 | Internal Server Error |  |

#### POST
##### Summary:

Broadcast a signed tx

##### Description:

Broadcast a signed tx to a full node

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| txBroadcast | body | The tx must be a signed StdTx. The supported broadcast modes include `"block"`(return after tx commit), `"sync"`(return afer CheckTx) and `"async"`(return right away). | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Tx broadcasting result | [BroadcastTxCommitResult](#broadcasttxcommitresult) |
| 500 | Internal Server Error |  |

### /txs/encode

#### POST
##### Summary:

Encode a transaction to the Amino wire format

##### Description:

Encode a transaction (signed or not) from JSON to base64-encoded Amino serialized bytes

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| tx | body | The tx to encode | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | The tx was successfully decoded and re-encoded | object |
| 400 | The tx was malformated |  |
| 500 | Server internal error |  |

### /bank/balances/{address}

#### GET
##### Summary:

Get the account balances

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| address | path | Account address in bech32 format | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Account balances | object |
| 204 | There is no data for the requested account |  |
| 500 | Server internal error |  |

### /bank/accounts/{address}/transfers

#### POST
##### Summary:

Send coins from one account to another

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| address | path | Account address in bech32 format | Yes | string |
| account | body | The sender and tx information | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Tx was succesfully generated | [StdTx](#stdtx) |
| 400 | Invalid request |  |
| 500 | Server internal error |  |

### /bank/accounts/memo

#### POST
##### Summary:

Mark if memo is required to receive coins

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| account | body | The mark | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 202 | Tx was succesfully generated | [StdTx](#stdtx) |
| 400 | Invalid request |  |
| 500 | Server internal error |  |

### /market/gte-orders

#### POST
##### Summary:

Create GTE order in blockchain

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| orderInfo | body | create order tx | Yes | [Order](#order) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Tx was succesfully generated | [StdTx](#stdtx) |
| 400 | Invalid request |  |
| 500 | Server internal error |  |

### /market/ioc-orders

#### POST
##### Summary:

Create IOC order in blockchain

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| orderInfo | body | create order tx | Yes | [Order](#order) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Tx was succesfully generated | [StdTx](#stdtx) |
| 400 | Invalid request |  |
| 500 | Server internal error |  |

### /market/trading-pairs

#### POST
##### Summary:

Create trading-pair in blockchain

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| info | body | Create trading-pair | Yes | [BaseMarket](#basemarket) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Tx was succesfully generated | [StdTx](#stdtx) |
| 400 | Invalid request |  |
| 500 | Server internal error |  |

### /market/trading-pairs/{stock}/{money}

#### GET
##### Summary:

Query trading-pair info

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| stock | path | stock symbol | Yes | string |
| money | path | money symbol | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | trading-pair info of the specified symbol | [MarketInfo](#marketinfo) |
| 400 | Invalid symbol |  |
| 500 | Server internal error |  |

### /market/orders/{order-id}

#### GET
##### Summary:

Query order info

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| order-id | path | The order id | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Order info of the specified id | [OrderInfo](#orderinfo) |
| 400 | Invalid order-id |  |
| 500 | Server internal error |  |

### /market/orders/account/{address}

#### GET
##### Summary:

Query user order-id list

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| address | path | The user address | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [Order-Ids](#order-ids) |
| 400 | Invalid address |  |
| 500 | Server internal error |  |

### /market/cancel-order

#### POST
##### Summary:

Cancel the order

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| orderInfo | body | cancel order tx | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Tx was succesfully generated | [StdTx](#stdtx) |
| 400 | Invalid request |  |
| 500 | Server internal error |  |

### /market/cancel-trading-pair

#### POST
##### Summary:

Cancel the trading-pair

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| info | body | cancel trading-pair in dex | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Tx was succesfully generated | [StdTx](#stdtx) |
| 400 | Invalid request |  |
| 500 | Server internal error |  |

### /auth/accounts/{address}

#### GET
##### Summary:

Get the account information on blockchain

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| address | path | Account address | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Account information on the blockchain | object |
| 204 | No content about this account address |  |
| 500 | Server internel error |  |

### /staking/delegators/{delegatorAddr}/delegations

#### GET
##### Summary:

Get all delegations from a delegator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Delegation](#delegation) ] |
| 400 | Invalid delegator address |  |
| 500 | Internal Server Error |  |

#### POST
##### Summary:

Submit delegation

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |
| delegation | body | submit delegation to provided validator | No | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [BroadcastTxCommitResult](#broadcasttxcommitresult) |
| 400 | Invalid delegator address or delegation request body |  |
| 401 | Key password is wrong |  |
| 500 | Internal Server Error |  |

### /staking/delegators/{delegatorAddr}/delegations/{validatorAddr}

#### GET
##### Summary:

Query the current delegation between a delegator and a validator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |
| validatorAddr | path | Bech32 OperatorAddress of validator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [Delegation](#delegation) |
| 400 | Invalid delegator address or validator address |  |
| 500 | Internal Server Error |  |

### /staking/delegators/{delegatorAddr}/unbonding_delegations

#### GET
##### Summary:

Get all unbonding delegations from a delegator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [UnbondingDelegation](#unbondingdelegation) ] |
| 400 | Invalid delegator address |  |
| 500 | Internal Server Error |  |

#### POST
##### Summary:

Submit an unbonding delegation

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |
| delegation | body | The password of the account to remove from the KMS | No | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [BroadcastTxCommitResult](#broadcasttxcommitresult) |
| 400 | Invalid delegator address or unbonding delegation request body |  |
| 401 | Key password is wrong |  |
| 500 | Internal Server Error |  |

### /staking/delegators/{delegatorAddr}/unbonding_delegations/{validatorAddr}

#### GET
##### Summary:

Query all unbonding delegations between a delegator and a validator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |
| validatorAddr | path | Bech32 OperatorAddress of validator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [UnbondingDelegation](#unbondingdelegation) ] |
| 400 | Invalid delegator address or validator address |  |
| 500 | Internal Server Error |  |

### /staking/redelegations

#### GET
##### Summary:

Get all redelegations (filter by query params)

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegator | query | Bech32 AccAddress of Delegator | No | string |
| validator_from | query | Bech32 ValAddress of SrcValidator | No | string |
| validator_to | query | Bech32 ValAddress of DstValidator | No | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Redelegation](#redelegation) ] |
| 500 | Internal Server Error |  |

### /staking/delegators/{delegatorAddr}/redelegations

#### POST
##### Summary:

Submit a redelegation

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |
| delegation | body | The sender and tx information | No | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Tx was succesfully generated | [StdTx](#stdtx) |
| 400 | Invalid delegator address or redelegation request body |  |
| 500 | Internal Server Error |  |

### /staking/delegators/{delegatorAddr}/validators

#### GET
##### Summary:

Query all validators that a delegator is bonded to

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Validator](#validator) ] |
| 400 | Invalid delegator address |  |
| 500 | Internal Server Error |  |

### /staking/delegators/{delegatorAddr}/validators/{validatorAddr}

#### GET
##### Summary:

Query a validator that a delegator is bonded to

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |
| validatorAddr | path | Bech32 ValAddress of Delegator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [Validator](#validator) |
| 400 | Invalid delegator address or validator address |  |
| 500 | Internal Server Error |  |

### /staking/delegators/{delegatorAddr}/txs

#### GET
##### Summary:

Get all staking txs (i.e msgs) from a delegator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [TxQuery](#txquery) ] |
| 204 | No staking transaction about this delegator address |  |
| 400 | Invalid delegator address |  |
| 500 | Internal Server Error |  |

### /staking/validators

#### GET
##### Summary:

Get all validator candidates. By default it returns only the bonded validators.

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| status | query | The validator bond status. Must be either 'bonded', 'unbonded', or 'unbonding'. | No | string |
| page | query | The page number. | No | integer |
| limit | query | The maximum number of items per page. | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Validator](#validator) ] |
| 500 | Internal Server Error |  |

### /staking/validators/{validatorAddr}

#### GET
##### Summary:

Query the information from a single validator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| validatorAddr | path | Bech32 OperatorAddress of validator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [Validator](#validator) |
| 400 | Invalid validator address |  |
| 500 | Internal Server Error |  |

### /staking/validators/{validatorAddr}/delegations

#### GET
##### Summary:

Get all delegations from a validator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| validatorAddr | path | Bech32 OperatorAddress of validator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Delegation](#delegation) ] |
| 400 | Invalid validator address |  |
| 500 | Internal Server Error |  |

### /staking/validators/{validatorAddr}/unbonding_delegations

#### GET
##### Summary:

Get all unbonding delegations from a validator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| validatorAddr | path | Bech32 OperatorAddress of validator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [UnbondingDelegation](#unbondingdelegation) ] |
| 400 | Invalid validator address |  |
| 500 | Internal Server Error |  |

### /staking/pool

#### GET
##### Summary:

Get the current state of the staking pool

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | object |
| 500 | Internal Server Error |  |

### /staking/parameters

#### GET
##### Summary:

Get the current staking parameter values

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | object |
| 500 | Internal Server Error |  |

### /slashing/validators/{validatorPubKey}/signing_info

#### GET
##### Summary:

Get sign info of given validator

##### Description:

Get sign info of given validator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| validatorPubKey | path | Bech32 validator public key | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [SigningInfo](#signinginfo) |
| 204 | No sign info of this validator |  |
| 400 | Invalid validator public key |  |
| 500 | Internal Server Error |  |

### /slashing/signing_infos

#### GET
##### Summary:

Get sign info of given all validators

##### Description:

Get sign info of all validators

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| page | query | Page number | Yes | integer |
| limit | query | Maximum number of items per page | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [SigningInfo](#signinginfo) ] |
| 204 | No validators with sign info |  |
| 400 | Invalid validator public key for one of the validators |  |
| 500 | Internal Server Error |  |

### /slashing/validators/{validatorAddr}/unjail

#### POST
##### Summary:

Unjail a jailed validator

##### Description:

Send transaction to unjail a jailed validator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| validatorAddr | path | Bech32 validator address | Yes | string |
| UnjailBody | body |  | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Tx was succesfully generated | [BroadcastTxCommitResult](#broadcasttxcommitresult) |
| 400 | Invalid validator address or base_req |  |
| 500 | Internal Server Error |  |

### /slashing/parameters

#### GET
##### Summary:

Get the current slashing parameters

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | object |
| 500 | Internal Server Error |  |

### /gov/proposals

#### POST
##### Summary:

Submit a proposal

##### Description:

Send transaction to submit a proposal

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| post_proposal_body | body | valid value of `"proposal_type"` can be `"text"`, `"parameter_change"`, `"software_upgrade"` | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Tx was succesfully generated | [StdTx](#stdtx) |
| 400 | Invalid proposal body |  |
| 500 | Internal Server Error |  |

#### GET
##### Summary:

Query proposals

##### Description:

Query proposals information with parameters

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| voter | query | voter address | No | string |
| depositor | query | depositor address | No | string |
| status | query | proposal status, valid values can be `"deposit_period"`, `"voting_period"`, `"passed"`, `"rejected"` | No | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [TextProposal](#textproposal) ] |
| 400 | Invalid query parameters |  |
| 500 | Internal Server Error |  |

### /gov/proposals/{proposalId}

#### GET
##### Summary:

Query a proposal

##### Description:

Query a proposal by id

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| proposalId | path |  | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [TextProposal](#textproposal) |
| 400 | Invalid proposal id |  |
| 500 | Internal Server Error |  |

### /gov/proposals/{proposalId}/proposer

#### GET
##### Summary:

Query proposer

##### Description:

Query for the proposer for a proposal

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| proposalId | path |  | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [Proposer](#proposer) |
| 400 | Invalid proposal ID |  |
| 500 | Internal Server Error |  |

### /gov/proposals/{proposalId}/deposits

#### GET
##### Summary:

Query deposits

##### Description:

Query deposits by proposalId

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| proposalId | path |  | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Deposit](#deposit) ] |
| 400 | Invalid proposal id |  |
| 500 | Internal Server Error |  |

#### POST
##### Summary:

Deposit tokens to a proposal

##### Description:

Send transaction to deposit tokens to a proposal

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| proposalId | path | proposal id | Yes | string |
| post_deposit_body | body |  | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [BroadcastTxCommitResult](#broadcasttxcommitresult) |
| 400 | Invalid proposal id or deposit body |  |
| 401 | Key password is wrong |  |
| 500 | Internal Server Error |  |

### /gov/proposals/{proposalId}/deposits/{depositor}

#### GET
##### Summary:

Query deposit

##### Description:

Query deposit by proposalId and depositor address

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| proposalId | path | proposal id | Yes | string |
| depositor | path | Bech32 depositor address | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [Deposit](#deposit) |
| 400 | Invalid proposal id or depositor address |  |
| 404 | Found no deposit |  |
| 500 | Internal Server Error |  |

### /gov/proposals/{proposalId}/votes

#### GET
##### Summary:

Query voters

##### Description:

Query voters information by proposalId

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| proposalId | path | proposal id | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Vote](#vote) ] |
| 400 | Invalid proposal id |  |
| 500 | Internal Server Error |  |

#### POST
##### Summary:

Vote a proposal

##### Description:

Send transaction to vote a proposal

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| proposalId | path | proposal id | Yes | string |
| post_vote_body | body | valid value of `"option"` field can be `"yes"`, `"no"`, `"no_with_veto"` and `"abstain"` | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [BroadcastTxCommitResult](#broadcasttxcommitresult) |
| 400 | Invalid proposal id or vote body |  |
| 401 | Key password is wrong |  |
| 500 | Internal Server Error |  |

### /gov/proposals/{proposalId}/votes/{voter}

#### GET
##### Summary:

Query vote

##### Description:

Query vote information by proposal Id and voter address

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| proposalId | path | proposal id | Yes | string |
| voter | path | Bech32 voter address | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [Vote](#vote) |
| 400 | Invalid proposal id or voter address |  |
| 404 | Found no vote |  |
| 500 | Internal Server Error |  |

### /gov/proposals/{proposalId}/tally

#### GET
##### Summary:

Get a proposal's tally result at the current time

##### Description:

Gets a proposal's tally result at the current time. If the proposal is pending deposits (i.e status 'DepositPeriod') it returns an empty tally result.

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| proposalId | path | proposal id | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [TallyResult](#tallyresult) |
| 400 | Invalid proposal id |  |
| 500 | Internal Server Error |  |

### /gov/parameters/deposit

#### GET
##### Summary:

Query governance deposit parameters

##### Description:

Query governance deposit parameters. The max_deposit_period units are in nanoseconds.

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | object |
| 400 | <other_path> is not a valid query request path |  |
| 404 | Found no deposit parameters |  |
| 500 | Internal Server Error |  |

### /gov/parameters/tallying

#### GET
##### Summary:

Query governance tally parameters

##### Description:

Query governance tally parameters

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK |  |
| 400 | <other_path> is not a valid query request path |  |
| 404 | Found no tally parameters |  |
| 500 | Internal Server Error |  |

### /gov/parameters/voting

#### GET
##### Summary:

Query governance voting parameters

##### Description:

Query governance voting parameters. The voting_period units are in nanoseconds.

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK |  |
| 400 | <other_path> is not a valid query request path |  |
| 404 | Found no voting parameters |  |
| 500 | Internal Server Error |  |

### /distribution/delegators/{delegatorAddr}/rewards

#### GET
##### Summary:

Get the total rewards balance from all delegations

##### Description:

Get the sum of all the rewards earned by delegations by a single delegator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Coin](#coin) ] |
| 400 | Invalid delegator address |  |
| 500 | Internal Server Error |  |

#### POST
##### Summary:

Withdraw all the delegator's delegation rewards

##### Description:

Withdraw all the delegator's delegation rewards

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |
| Withdraw request body | body |  | No |  |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [BroadcastTxCommitResult](#broadcasttxcommitresult) |
| 400 | Invalid delegator address |  |
| 401 | Key password is wrong |  |
| 500 | Internal Server Error |  |

### /distribution/delegators/{delegatorAddr}/rewards/{validatorAddr}

#### GET
##### Summary:

Query a delegation reward

##### Description:

Query a single delegation reward by a delegator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |
| validatorAddr | path | Bech32 OperatorAddress of validator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Coin](#coin) ] |
| 400 | Invalid delegator address |  |
| 500 | Internal Server Error |  |

#### POST
##### Summary:

Withdraw a delegation reward

##### Description:

Withdraw a delegator's delegation reward from a single validator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |
| validatorAddr | path | Bech32 OperatorAddress of validator | Yes | string |
| Withdraw request body | body |  | No |  |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [BroadcastTxCommitResult](#broadcasttxcommitresult) |
| 400 | Invalid delegator address or delegation body |  |
| 401 | Key password is wrong |  |
| 500 | Internal Server Error |  |

### /distribution/delegators/{delegatorAddr}/withdraw_address

#### GET
##### Summary:

Get the rewards withdrawal address

##### Description:

Get the delegations' rewards withdrawal address. This is the address in which the user will receive the reward funds

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [Address](#address) |
| 400 | Invalid delegator address |  |
| 500 | Internal Server Error |  |

#### POST
##### Summary:

Replace the rewards withdrawal address

##### Description:

Replace the delegations' rewards withdrawal address for a new one.

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| delegatorAddr | path | Bech32 AccAddress of Delegator | Yes | string |
| Withdraw request body | body |  | No |  |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [BroadcastTxCommitResult](#broadcasttxcommitresult) |
| 400 | Invalid delegator or withdraw address |  |
| 401 | Key password is wrong |  |
| 500 | Internal Server Error |  |

### /distribution/validators/{validatorAddr}

#### GET
##### Summary:

Validator distribution information

##### Description:

Query the distribution information of a single validator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| validatorAddr | path | Bech32 OperatorAddress of validator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ValidatorDistInfo](#validatordistinfo) |
| 400 | Invalid validator address |  |
| 500 | Internal Server Error |  |

### /distribution/validators/{validatorAddr}/outstanding_rewards

#### GET
##### Summary:

Fee distribution outstanding rewards of a single validator

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| validatorAddr | path | Bech32 OperatorAddress of validator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Coin](#coin) ] |
| 500 | Internal Server Error |  |

### /distribution/validators/{validatorAddr}/rewards

#### GET
##### Summary:

Commission and self-delegation rewards of a single validator

##### Description:

Query the commission and self-delegation rewards of validator.

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| validatorAddr | path | Bech32 OperatorAddress of validator | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Coin](#coin) ] |
| 400 | Invalid validator address |  |
| 500 | Internal Server Error |  |

#### POST
##### Summary:

Withdraw the validator's rewards

##### Description:

Withdraw the validator's self-delegation and commissions rewards

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| validatorAddr | path | Bech32 OperatorAddress of validator | Yes | string |
| Withdraw request body | body |  | No |  |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [BroadcastTxCommitResult](#broadcasttxcommitresult) |
| 400 | Invalid validator address |  |
| 401 | Key password is wrong |  |
| 500 | Internal Server Error |  |

### /distribution/{accAddress}/donates

#### POST
##### Summary:

Donate to the community pool

##### Description:

Donate some amount of cet to the community pool

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| accAddress | path | Account address of the user | Yes | string |
| amount | body | Amount of cet to donate | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Donate tx result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /distribution/community_pool

#### GET
##### Summary:

Community pool parameters

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [ [Coin](#coin) ] |
| 500 | Internal Server Error |  |

### /distribution/parameters

#### GET
##### Summary:

Fee distribution parameters

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK |  |
| 500 | Internal Server Error |  |

### /asset/tokens

#### POST
##### Summary:

Issue token

##### Description:

Issue a new Token

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| tokenInfo | body | the detail info about the Token to issue | Yes | [TokenBasicInfo](#tokenbasicinfo) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Token issuance result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

#### GET
##### Summary:

List tokens

##### Description:

List all existing tokens

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | All existing tokens | [ [Token](#token) ] |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}

#### GET
##### Summary:

queryToken

##### Description:

Get token with provided `symbol`

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | token with provided symbol | [Token](#token) |
| 400 | Invalid Request |  |
| 404 | Tokens not found |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}/ownerships

#### POST
##### Summary:

Transfer ownership

##### Description:

Transfer token owner ship

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |
| newOwner | body | transfer ownership to new owner | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Token transfer ownership result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}/mints

#### POST
##### Summary:

Mint token

##### Description:

Mint mintable token

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |
| amount | body | mint token amount | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Mint token result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}/burns

#### POST
##### Summary:

Burn token

##### Description:

Burn burnable token

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |
| amount | body | burn token amount | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Burn token result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}/forbids

#### POST
##### Summary:

Forbid token

##### Description:

Forbid forbiddable token

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |
| baseReq | body | base req | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Forbid token result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}/unforbids

#### POST
##### Summary:

UnForbid token

##### Description:

UnForbid forbiddable token

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |
| baseReq | body | base req | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | UnForbid token result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}/forbidden/whitelist

#### GET
##### Summary:

queryWhitelist

##### Description:

Get token whitelist with provided `symbol`

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | whitelist with provided symbol | [ [Address](#address) ] |
| 404 | Token not found |  |
| 500 | Internal Server Error |  |

#### POST
##### Summary:

Add forbid whitelist

##### Description:

Add forbiddable token whitelist addr

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |
| whitelist | body | token whitelist addr | Yes | [AddrList](#addrlist) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Add forbid whitelist result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}/unforbidden/whitelist

#### POST
##### Summary:

Remove forbid whitelist

##### Description:

Remove forbiddable token whitelist addr

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |
| whitelist | body | token whitelist addr | Yes | [AddrList](#addrlist) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Remove forbid whitelist result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}/forbidden/addresses

#### GET
##### Summary:

query forbidden addr

##### Description:

Get forbidden addresses with provided `symbol`

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | forbidden addresses with provided symbol | [ [Address](#address) ] |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

#### POST
##### Summary:

Forbid address

##### Description:

Add forbidden address

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |
| forbiddenAddr | body | forbidden addr | Yes | [AddrList](#addrlist) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Forbid address result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}/unforbidden/addresses

#### POST
##### Summary:

UnForbid address

##### Description:

Remove forbidden address

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |
| forbiddenAddr | body | forbidden addr | Yes | [AddrList](#addrlist) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | UnForbid address result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/reserved/symbols

#### GET
##### Summary:

List reserved symbols

##### Description:

List all reserved symbols

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | All reserved symbols | string |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}/urls

#### POST
##### Summary:

Modify token url

##### Description:

Modify token url

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |
| url | body | new token url | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Modify token url result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### /asset/tokens/{symbol}/descriptions

#### POST
##### Summary:

Modify token description

##### Description:

Modify token description

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | token symbol | Yes | string |
| description | body | new token description | Yes | object |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Modify token description result | [StdTx](#stdtx) |
| 400 | Invalid Request |  |
| 500 | Internal Server Error |  |

### Models


#### BaseMarket

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| stock | string |  | No |
| money | string |  | No |
| price_precision | string |  | No |

#### MarketInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| MarketInfo | object |  |  |

#### Order

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| order_type | string |  | No |
| symbol | string |  | No |
| price_precision | string |  | No |
| price | string |  | No |
| quantity | string |  | No |
| side | string |  | No |

#### OrderInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| OrderInfo | object |  |  |

#### Order-Ids

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| Order-Ids | array |  |  |

#### CheckTxResult

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | integer |  | No |
| data | string |  | No |
| gas_used | integer |  | No |
| gas_wanted | integer |  | No |
| info | string |  | No |
| log | string |  | No |
| tags | [ [KVPair](#kvpair) ] |  | No |

#### DeliverTxResult

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | integer |  | No |
| data | string |  | No |
| gas_used | integer |  | No |
| gas_wanted | integer |  | No |
| info | string |  | No |
| log | string |  | No |
| tags | [ [KVPair](#kvpair) ] |  | No |

#### BroadcastTxCommitResult

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| check_tx | [CheckTxResult](#checktxresult) |  | No |
| deliver_tx | [DeliverTxResult](#delivertxresult) |  | No |
| hash | [Hash](#hash) |  | No |
| height | integer |  | No |

#### KVPair

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| key | string |  | No |
| value | string |  | No |

#### Msg

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| Msg | string |  |  |

#### Address

bech32 encoded address

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| Address | string | bech32 encoded address |  |

#### PublicKey

public key information

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| type | string |  | No |
| value | string |  | No |

#### ValidatorAddress

bech32 encoded address

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| ValidatorAddress | string | bech32 encoded address |  |

#### Coin

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| denom | string |  | No |
| amount | string |  | No |

#### LockedCoin

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| coin | [Coin](#coin) |  | No |
| unlock_time | string |  | No |

#### Hash

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| Hash | string |  |  |

#### TxQuery

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| hash | string |  | No |
| height | number |  | No |
| tx | [StdTx](#stdtx) |  | No |
| result | object |  | No |

#### StdTx

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| msg | [ [Msg](#msg) ] |  | No |
| fee | object |  | No |
| memo | string |  | No |
| signature | object |  | No |

#### KeyOutput

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| name | string |  | No |
| address | string |  | No |
| pub_key | string |  | No |
| type | string |  | No |
| seed | string |  | No |

#### BlockID

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| hash | [Hash](#hash) |  | No |
| parts | object |  | No |

#### BlockHeader

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| chain_id | string |  | No |
| height | number |  | No |
| time | string |  | No |
| num_txs | number |  | No |
| last_block_id | [BlockID](#blockid) |  | No |
| total_txs | number |  | No |
| last_commit_hash | [Hash](#hash) |  | No |
| data_hash | [Hash](#hash) |  | No |
| validators_hash | [Hash](#hash) |  | No |
| next_validators_hash | [Hash](#hash) |  | No |
| consensus_hash | [Hash](#hash) |  | No |
| app_hash | [Hash](#hash) |  | No |
| last_results_hash | [Hash](#hash) |  | No |
| evidence_hash | [Hash](#hash) |  | No |
| proposer_address | [Address](#address) |  | No |
| version | object |  | No |

#### Block

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| header | [BlockHeader](#blockheader) |  | No |
| txs | [ string ] |  | No |
| evidence | object |  | No |
| last_commit | object |  | No |

#### BlockQuery

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| block_meta | object |  | No |
| block | [Block](#block) |  | No |

#### BaseReq

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| from | string | Sender address or Keybase name to generate a transaction | No |
| memo | string |  | No |
| chain_id | string |  | No |
| account_number | string |  | No |
| sequence | string |  | No |
| gas | string |  | No |
| gas_adjustment | string |  | No |
| fees | [ [Coin](#coin) ] |  | No |
| simulate | boolean | Estimate gas for a transaction (cannot be used in conjunction with generate_only) | No |

#### TendermintValidator

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| address | [ValidatorAddress](#validatoraddress) |  | No |
| pub_key | string |  | No |
| voting_power | string |  | No |
| proposer_priority | string |  | No |

#### TextProposal

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| proposal_id | string |  | No |
| title | string |  | No |
| description | string |  | No |
| proposal_type | string |  | No |
| proposal_status | string |  | No |
| final_tally_result | [TallyResult](#tallyresult) |  | No |
| submit_time | string |  | No |
| total_deposit | [ [Coin](#coin) ] |  | No |
| voting_start_time | string |  | No |

#### Proposer

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| proposal_id | string |  | No |
| proposer | string |  | No |

#### Deposit

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| amount | [ [Coin](#coin) ] |  | No |
| proposal_id | string |  | No |
| depositor | [Address](#address) |  | No |

#### TallyResult

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| yes | string |  | No |
| abstain | string |  | No |
| no | string |  | No |
| no_with_veto | string |  | No |

#### Vote

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| voter | string |  | No |
| proposal_id | string |  | No |
| option | string |  | No |

#### Validator

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| operator_address | [ValidatorAddress](#validatoraddress) |  | No |
| consensus_pubkey | string |  | No |
| jailed | boolean |  | No |
| status | integer |  | No |
| tokens | string |  | No |
| delegator_shares | string |  | No |
| description | object |  | No |
| bond_height | string |  | No |
| bond_intra_tx_counter | integer |  | No |
| unbonding_height | string |  | No |
| unbonding_time | string |  | No |
| commission | object |  | No |

#### Delegation

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| delegator_address | string |  | No |
| validator_address | string |  | No |
| shares | string |  | No |
| height | integer |  | No |

#### UnbondingDelegation

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| delegator_address | string |  | No |
| validator_address | string |  | No |
| initial_balance | string |  | No |
| balance | string |  | No |
| creation_height | integer |  | No |
| min_time | integer |  | No |

#### Redelegation

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| delegator_address | string |  | No |
| validator_src_address | string |  | No |
| validator_dst_address | string |  | No |
| creation_height | integer |  | No |
| min_time | integer |  | No |
| initial_balance | string |  | No |
| balance | string |  | No |
| shares_src | string |  | No |
| shares_dst | string |  | No |

#### ValidatorDistInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| operator_address | [ValidatorAddress](#validatoraddress) |  | No |
| self_bond_rewards | [ [Coin](#coin) ] |  | No |
| val_commission | [ [Coin](#coin) ] |  | No |

#### SigningInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| start_height | string |  | No |
| index_offset | string |  | No |
| jailed_until | string |  | No |
| missed_blocks_counter | string |  | No |

#### TokenBasicInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| name | string |  | No |
| symbol | string |  | No |
| total_supply | string |  | No |
| owner | [Address](#address) |  | No |
| mintable | boolean |  | No |
| burnable | boolean |  | No |
| addr_forbiddable | boolean |  | No |
| token_forbiddable | boolean |  | No |
| url | string |  | No |
| description | string |  | No |

#### Token

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| Token | object |  |  |

#### AddrList

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| base_req | [BaseReq](#basereq) |  | No |
| addr_list | [ [Address](#address) ] |  | No |