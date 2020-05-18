## Java SDK

We provide Java version of the SDK library to make it easier for Java developers to use  functions of  dex chain, and the SDK provides support for all the basic and extended APIs. Combined with [polarbear](https://github.com/coinexchain/polarbear/) library, developers can perform the functions that wallet and browser applications rely on, such as secret key management, transaction assembly, signature, and send transaction to chain.

### Prerequisite

Set the WalletCaller class with dynamic library of polarbear using [JNA](https://github.com/java-native-access/jna)

```Java
public class WalletCaller {
    public interface WalletLib extends Library {
        String libPath = "/{Absolute Path}/wallet_mac.so";
        WalletLib INSTANCE = (WalletLib) Native.load(libPath, WalletLib.class);

        void BearInit(String root);
        String CreateKey(String name, String password, String bip39Passphrase, int account, int index);
        String DeleteKey(String name, String password);
        String RecoverKey(String name, String mnemonic, String password, String bip39Passphrase, int account, int index);
        String AddKey(String name, String armor, String passphrase);
        String ExportKey(String name, String decryptPassphrase, String encryptPassphrase);
        String ListKeys();
        String ResetPassword(String name, String password, String newPassword);
        String GetAddress(String name);
        String GetSigner(String signInfo);
        String GetPubKey(String name);
        String GetAddressFromWIF(String wif);

        String Sign(String name, String password, String tx);
        String SignStdTx(String name, String password, String tx, String chainId, long accountNum, long sequence);

        String SignAndBuildBroadcast(String name, String password, String tx, String chainId, String mode, long accountNum, long sequence);

    }
    // other codes...
}
```

### Usage

Initialize the WalletLib and call its function

```Java
WalletLib.INSTANCE.BearInit("tmp");
WalletLib.INSTANCE.CreateKey(keyName, password, bip39Password, 0, 0);
WalletLib.INSTANCE.ListKeys();
// ...
```

Initialize the SDK context

```Java
private static BaseReq createBaseReq(String address, List<Coin> fees) {
    BaseReq req = new BaseReq();


    String memo = "";
    String chainId = "coinexdex-test1";
    String accountNumber = "0";
    String sequence = "0";
    String gas = "30000";
    String gasAdjustment = "1.1";
    Boolean simulate = false;

    req.setFrom(address);
    req.setMemo(memo);
    req.setChainId(chainId);
    req.setAccountNumber(accountNumber);
    req.setSequence(sequence);
    req.setGas(gas);
    req.setGasAdjustment(gasAdjustment);
    req.setSimulate(simulate);
    req.setFees(fees);

    return req;
}

String toAddress = "coinex10c22dwn7hxps77tnkpj5pzu9zcpq5zf76xms55";
String name = "alice";
String password = "12345678";

WalletCaller.WalletLib.INSTANCE.BearInit("./tmp");
String fromAddress = WalletCaller.WalletLib.INSTANCE.GetAddress(name);

Coin coin = new Coin();
coin.setDenom("cet");
coin.setAmount("600000");
List<Coin> fees = new ArrayList<>();
fees.add(coin);


BaseReq baseReq = createBaseReq(fromAddress, fees);
ApiContext context = new ApiContext(fromAddress, name, password, baseReq);
context.refreshAccNumAndSeq();
```


Assembly transaction parameters

Take a normal transfer as an example, provide the transfer target address, transfer quantity, transfer currency and unlock time

```Java
Coin sendCoin = new Coin();
sendCoin.setDenom("cet");
sendCoin.setAmount("1000000");
List<Coin> sendCoins = new ArrayList<>();
sendCoins.add(sendCoin);

Account account = new Account();
account.setBaseReq(context.getBaseReq());
account.setAmount(sendCoins);
account.setUnlockTime("0");

BankApi bankApi = new BankApi();
StdTx stdTx = bankApi.sendCoins(toAddress, account);
```

Sign and send transaction to chain

```Java
String stdTxStr = context.getJson().serializeWithNull(stdTx);
BroadcastTxCommitResult broadcastTxCommitResult = context.signAndBroadcast(stdTxStr);
```

### Details

Repository: https://github.com/coinexchain/dex-api-java

List of supported APIs: 

[Basic APIs](./api/rest-api.html) 

[Extended Query APIs](./api/tradeserver-rest-api.html)

[Extended Subscribe APIs](./ext-watcher/websocket-subscription.md)

