## Node.js SDK

We provide Node.js version of the SDK library to make it easier for Node.js developers to use  functions of  dex chain, and the SDK provides support for all the basic and extended APIs. Combined with [polarbear](https://github.com/coinexchain/polarbear/) library, developers can perform the functions that wallet and browser applications rely on, such as secret key management, transaction assembly, signature, and send transaction to chain.

### Prerequisite

Set the WalletCaller class with dynamic library of polarbear using [ffi-napi](https://github.com/node-ffi-napi/node-ffi-napi)

```JavaScript
export default class Wallet {

    constructor(keyName, password, bip39Password, keyDirPath) {
        this.keyName = keyName;
        this.password = password;
        this.bip39Password = bip39Password;
        this.keyDirPath = keyDirPath;
        this.polarBear = ffi.Library("/{Absolute Path}/wallet_mac", {
            'BearInit': ['void', ['CString']],
            'CreateKey': ['CString', ['CString', 'CString', 'CString', 'uint32', 'uint32']],
            'DeleteKey': ['CString', ['CString', 'CString']],
            'RecoverKey': ['CString', ['CString', 'CString', 'CString', 'CString', 'uint32', 'uint32']],
            'AddKey': ['CString', ['CString', 'CString', 'CString']],
            'ExportKey': ['CString', ['CString', 'CString', 'CString']],
            'ListKeys': ['CString', []],
            'ResetPassword': ['CString', ['CString', 'CString', 'CString']],
            'GetAddress': ['CString', ['CString']],
            'GetSigner': ['CString', ['CString']],
            'GetPubKey': ['CString', ['CString']],
            'GetAddressFromWIF': ['CString', ['CString']],
            'Sign': ['CString', ['CString', 'CString', 'CString']],
            'SignStdTx': ['CString', ['CString', 'CString', 'CString', 'CString', 'uint64', 'uint64']],
            'SignAndBuildBroadcast': ['CString', ['CString', 'CString', 'CString', 'CString', 'CString', 'uint64', 'uint64']],
        });
    }

}
```

### Usage

Initialize the WalletLib and call its function

```JavaScript
const keyName = 'alice';
const password = '12345678';
const bip39Password = '11111111';
const keyDirPath = "./tmp";
const wallet = new Wallet(keyName, password, bip39Password, keyDirPath;
// Init Wallet
wallet.polarBear.BearInit(keyDirPath);

// Create Key
const key = wallet.polarBear.CreateKey(keyName, password, bip39Password, 0, 0);
console.log(key);
// ...
```

Initialize the SDK context

```JavaScript
function createBaseReq(fromAddress) {
    let baseReq = new BaseReq();
    const coin = new Coin('cet', '600000');
    let fees = [];
    fees.push(coin);
    baseReq['from'] = fromAddress;
    baseReq['memo'] = '';
    baseReq['chain_id'] = 'coinexdex-test1';
    baseReq['account_number'] = '0';
    baseReq['sequence'] = '0';
    baseReq['gas'] = '30000';
    baseReq['gas_adjustment'] = '1.1';
    baseReq['fees'] = fees;
    baseReq['simulate'] = false;
    return baseReq;
}

const apiClient = ApiClient;
const toAddress = 'coinex10c22dwn7hxps77tnkpj5pzu9zcpq5zf76xms55';
const name = 'alice';
const password = '12345678';
const bip39Password = '11111111';
const keyDirPath = "./tmp";

const wallet = new Wallet(name, password, bip39Password, keyDirPath);
wallet.polarBear.BearInit(keyDirPath);
const fromAddress = wallet.polarBear.GetAddress(name);
let baseReq = createBaseReq(fromAddress);
let apiContext = new ApiContext(apiClient, fromAddress, name, password, baseReq);
```


Assembly transaction parameters

Take a normal transfer as an example, provide the transfer target address, transfer quantity, transfer currency and unlock time

Then sign and send transaction to chain

```JavaScript
function refreshAccNumAndSeq() {
// Refresh account number and sequence
    const authApi = new AuthApi();
    return authApi.getAccount(fromAddress).then(function (value) {
        console.log('API called successfully. Returned data: ' + JSON.stringify(value));
        apiContext.baseReq['account_number'] = value['result']['account_number'];
        apiContext.baseReq['sequence'] = value['result']['sequence'];
    }, function (error) {
        console.error(error);
    });
}

function getStdTx() {
    const sendCoin = new Coin('cet', '1000000');
    let sendCoins = [];
    sendCoins.push(sendCoin);
    const account = new Account(apiContext.baseReq, sendCoins, '0');
    const bankApi = new BankApi();
    return bankApi.sendCoins(toAddress, account).then(function (value) {
        const stdTxStr = JSON.stringify(value);
        console.log('API called successfully. Returned data: ' + stdTxStr);
        return stdTxStr;
    }, function (err) {
        console.error(err);
    });
}

function signAndBroadcast(unsignedBytes) {
    const bytes = wallet.polarBear.SignAndBuildBroadcast(name, password,
        unsignedBytes, apiContext.baseReq.chain_id, 'block', apiContext.baseReq.account_number, apiContext.baseReq.sequence);
    const txBroadcast = JSON.parse(bytes);
    console.log(bytes);
    const transactionsApi = new TransactionsApi();
    return transactionsApi.broadcastTx(txBroadcast).then(function (result) {
        return result;
    }, function (error) {
        console.error(error);
    });

}

refreshAccNumAndSeq().then(function (value) {
    return getStdTx();
}).then(function (value) {
    return signAndBroadcast(value);
}).then(function (result) {
    console.log(result);
}).catch(function (error) {
    console.error(error);
});
```

### Details

Repository: https://github.com/coinexchain/dex-api-nodejs

List of supported APIs: 

[Basic APIs](./api/rest-api.html) 

[Extended Query APIs](./api/tradeserver-rest-api.html)

[Extended Subscribe APIs](./ext-watcher/websocket-subscription.md)

