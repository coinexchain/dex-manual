### "亚特兰提斯"硬分叉升级指南

#### 0. 概述

CoinEx Chain将于2020年3月30日进行代号为“亚特兰提斯”的硬分叉升级。此次升级，将基于genesis.json来进行数据的迁移，旧链所生成数据目录（其中存储着历史区块、最新状态等信息）将不再有用，新链将使用新的数据目录，并且有新的Chain-ID。

升级的大致流程是：

1. 当旧链到达预先约定的高度的时候（或者超过这个高度若干个块的时候），停止旧链的cetd进程执行
2. 使用旧链的cetd导出预先约定的高度下链的状态，生成genesis.exported.json文件
3. 使用新链的cetd对上述genesis.exported.json文件进行处理，得到新链所使用的genesis.json文件
4. 使用新链的genesis.json来启动新链的cetd

本文将详细介绍整个升级流程，文中将包含一些现成的命令，供运维人员“拷贝粘贴”之用。需要特别注意的是，本文随时可能进行更新，所以在拷贝粘贴之前，请确保浏览器中所展示的本文的页面，是最新的，必要时，请刷新浏览器页面。



#### 1. 停止旧链和备份数据

当旧链到达预先约定的高度的时候（或者超过这个高度若干个块的时候），请停止旧链的cetd的执行。用如下命令可以查看目前执行到了哪个高度：

```
/path/to/old/cetcli status --indent
```

注意上述cetcli是老链的二进制文件。

各个Validator所使用的运维工具不同，但一般大家都会使用systemctl或者supervisor等工具来运行cetd。因此，请使用这些运维工具来停止cetd的执行，而不是简单地用`kill`命令来杀死cetd的进程。

虽然升级后的新链不再需要使用老链的数据目录，但为了保险起见，**请务必将老链的数据目录打包压缩之后，保存到本地的电脑或移动硬盘上，以确保万无一失。另外，短期内，切勿删除服务器上的老链的数据目录。**

如果之前没有特别的设置，那么老链的数据目录会存在于：$HOME/.cetd/

Validator投票时所使用的ED25519私钥文件，重要性尤其的高，所以务必采用多个物理设备（移动硬盘、U盘）对它进行保存，如果之前没有特别的设置，那么这个私钥文件将位于：$HOME/.cetd/config/priv_validator_key.json 

各个节点找到自己的私钥文件后，请再次确认它里面包含的地址，的确是自己节点的地址，所有节点的地址可以在这里查看：https://docs.google.com/spreadsheets/d/1oZOa1PLzilOdUbCIxkXy7nQbus5qd5pjn81DCCf5-CY/edit#gid=0 

#### 2. 生成新链所使用的genesis.json文件（可选）

本步骤并不是必须的，因为您也可选择直接使用其他节点所生成的genesis文件，免去自己生成的麻烦。但是为了避免其他节点传给您的genesis文件是错误的，建议您最好还是自己生成genesis文件来使用，或者，将自己生成的genesis文件同其他节点所生成的genesis文件进行比对、确认无误后，再使用它。

使用旧链的cetd导出预先约定的高度下链的状态，生成genesis.exported.json文件：

```bash
/path/to/old/cetd export --height=<predefined-height> --for-zero-height > genesis.exported.json
```

使用新链的cetd对上述genesis.exported.json文件进行处理，得到新链所使用的genesis.json文件：

```bash
/path/to/new/cetd migrate genesis.exported.json --genesis-block-height=<predefined-height> --output genesis.json 

```

如果希望对比自己生成的genesis.json.myself和其他节点所生成的genesis.json.others，请使用如下命令：

```bash
jq -S . genesis.json.myself > 1
jq -S . genesis.json.others > 2
diff 1 2
```

这里使用了jq对json文件进行排序，以便其输出是可读的。



#### 3. 使用genesis.json启动新链

##### 3.1. 下载新版本的二进制文件

首先定义若干环境变量：
```bash
export ARTIFACTS_BASE_URL=https://raw.githubusercontent.com/coinexchain/testnets/master/coinexdex-test-upgrade
export CETD_URL=${ARTIFACTS_BASE_URL}/linux_x86_64/cetd
export CETCLI_URL=${ARTIFACTS_BASE_URL}/linux_x86_64/cetcli
export CHECK_SH=${ARTIFACTS_BASE_URL}/dex2_check.sh
export GENESIS_URL=${ARTIFACTS_BASE_URL}/genesis.json
export SHA256_CHECKSUM_URL=${ARTIFACTS_BASE_URL}/sha256.sum
export PUBLIC_IP=～～123.36.28.137～～
export RUN_DIR=～～/home/ubuntu/data-cetd-dex2～～
```
注意，上面的`PUBLIC_IP`和`RUN_DIR`的取值仅仅用作示意，请根据自己的情况来设置它们。

接下来下载文件：
```bash
mkdir ${RUN_DIR}
cd ${RUN_DIR}
curl ${CETD_URL} >  cetd
curl ${CETCLI_URL} > cetcli
curl ${CHECK_SH} > dex2_check.sh
curl ${GENESIS_URL} > genesis.json
chmod a+x cetd cetcli
```
注意，上面步骤中所下载的genesis.json就是由其他节点生成完毕后，上传到网上的genesis文件。



#### 3.2. 创建新的数据目录

1. `${RUN_DIR}/cetd init moniker --chain-id=coinexdex2 --home=${RUN_DIR}/.cetd`
2. 拷贝下载的genesis.json 到数据目录: `cp genesis.json ${RUN_DIR}/.cetd/config`
3. 如果是验证者节点：

    *   拷贝原节点数据目录的ED25519私钥文件`priv_validator_key.json` 至新数据目录，该文件应该在的位置：`${RUN_DIR}/.cetd/config`

4. 设置节点的对外IP

   *	`ansible localhost -m ini_file -a "path=${RUN_DIR}/.cetd/config/config.toml section=p2p option=external_address value='\"tcp://${PUBLIC_IP}:26656\"' backup=true"`
   *   这里使用了ansible，如果您没有ansible，请安装它：[ansible安装文档](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu)

5. 验证可执行程序、genesis.json等：
   *  `bash dex2_check.sh`

    
#### 3.3. 用nohup启动新节点

首先定义seeds节点的列表，seeds节点是硬分叉升级时首先启动的若干个节点，具体是哪些，需要等到硬分叉的时刻才能得知：


```bash
export CHAIN_SEEDS=<the-chain-seeds-which-would-be-known-then>
```

接下来用nohup来启动cetd：

```bash
nohup ${RUN_DIR}/cetd start --home=${RUN_DIR}/.cetd --minimum-gas-prices=20.0cet --p2p.seeds=${CHAIN_SEEDS} &> cetd.log &
```



#### 3.4. 使用systemctl或supervisor等工具来配置新链的cetd的自动运行

等待cetd稳定运行1～2个小时之后，可以把cetd进程kill掉，转而使用systemctl或supervisor等工具来配置新链cetd的自动运行。

首先把节点seeds配置加入到config.toml文件中：

```bash
ansible localhost -m ini_file -a "path=${RUN_DIR}/.cetd/config/config.toml section=p2p option=seeds value='\"${CHAIN_SEEDS}\"' backup=true"
```

之后，即可按照您的习惯，选择使用systemctl或supervisor等工具来配置新链cetd的自动运行。这里的配置方式各不相同，本文不再一一介绍。