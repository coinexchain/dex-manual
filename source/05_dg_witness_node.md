### How to run a witness node



# trade-server部署说明

```
   +----------+ produce  +-----+ consume  +--------------+
   | DEX Node | -------> | dir | -------> | trade-server | 
   +----------+          +-----+          +--------------+
```

## 2. dex-node部署及配置

节点部署参考 https://github.com/coinexchain/testnets/blob/master/coinexdex-test/testnet-guide.md

部署完成后，需要修改配置文件``app.toml`` (默认在~/.cetd/config/app.toml)，添加以下配置

```toml
feature-toggle = true
subscribe-modules = "comment,authx,bankx,market,bancorlite,distribution"
brokers = [
    "dir:/home/data"                # 指定节点为trade-server数据存储的目录
]
```

brokers配置按实际存储的数据目录填写，修改完后启动节点。

## 3. trade-server部署

### 编译

需要安装Go (推荐12及以上版本) https://golang.org/doc/

进入工程目录，执行以下命令进行编译

```shell
GO111MODULE=on go build github.com/coinexchain/trade-server
```

### 配置文件说明

工程根目录提供了默认配置模板文件`config.toml.default`，可在此基础上拷贝修改

```toml
# 监听端口
port = 8000

# 是否代理DEX节点的REST API
proxy = false 

# DEX节点LCD地址
lcd = "http://localhost:1317"
lcdv0 = "http://<?>:1317"   # 老链的REST接口地址

# LevelDB数据目录
data-dir = "data"

# Log文件目录
log-dir = "log"

# Log级别: debug | info | warn | error
log-level = "info"

# Log格式: plain(普通文本格式) | json (json格式)
log-format = "plain"

# dir-mode

dir-mode = true             # 启用目录模式
dir = "/home/data"          # 指定dex-node 中为trade-server数据存储的目录；

# monitorinterval

# 设置看门狗的监控的时间间隔，单位为秒。当它取非零值的时候，会有一个goroutine定期检查是否发生了新的写KV数据库的行为
# 如果没有，意味着Trade-Server已经卡死，这时整个Trade-Server就会Panic掉，进程退出。
# 建议外部用initd等工具守护进程来监控Trade-Server，如果它的进程退出来，就自动启动一个新的进程
# 不设置此参数时，默认不启动看门狗监控

monitorinterval = 10       

# upgrade chain

initChainHeight = 0         
```


**initChainHeight** : 配置链的初始高度

*   设置该值的目的：
    *   v0.2.0的`cetd`升级时，允许延续旧链的高度；当在测试网中升级时，为测试方便，抛弃旧链的历史数据；
    *   trade-server添加了防止跳块的功能，当接收的块高度大于trade-server记录的高度时，不允许启动； 
    *   当升级后，如果抛弃旧链数据，且`cetd`延续高度的情况下，会导致接收区块大于trade-server记录的块高度，无法启动.
    *   此时，配置`initChainHeight`为新链的`genesis`高度.
*   正式环境中，推荐保存旧链的历史记录，且`initChainHeight`一直配置为0
    *   历史记录存在于两处：
        *   cetd 推送的数据目录，即`dir-mode`下`dir`的配置.
        *   trade-server自身的数据目录，即`data-dir`的配置.     


### 启动

启动时通过-c参数指定配置文件路径

```bash
nohup trade-server -c config.toml &
```


## 注意

1. dex-node 节点配置文件中 `brokers`下指定的`dir`的值 必须与 trade-server 配置文件中 `dir`的值一致。
2. **初次**启用节点的`brokers`的`dir`模式时，节点必须从genesis高度开始，重新同步数据。
3. 启用节点的`brokers`的`dir`模式后，当运行一段时间重启节点时，修改了节点配置文件中`dir`的值，节点必须从genesis高度开始，重新同步数据。
3. 启用节点的`brokers`的`dir`模式后，当运行一段时间重启节点时，配置文件的`dir`值未修改，直接启动节点即可。