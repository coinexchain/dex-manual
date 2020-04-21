# config命令



## 作用

对于一些常用的CETCLI子命令，可以把经常指定的选项写入一个配置文件。这样就可以避免每次指定相同的选项，即节约敲命令的时间，也避免了填写错误的可能。对于某个选项，CETCLI按照下面的优先级获取选项值：

1. 如果在命令行中显示指定了选项值，那么使用这个值
2. 如果在配置文件中配置了选项值，那么使用这个值
3. 否则使用选项的默认值

CETCLI的配置文件路径是`$CETCLI_HOME/config/config.toml`，这是个普通的文本文件，采用[TOML](https://github.com/toml-lang/toml)格式。`config`命令的主要作用就是**编辑**或**查看**这个文件的内容。



## 用法

查看帮助：

```
$ ./cetcli config -h
Create or query a CoinEx Chain CLI configuration file

Usage:
  cetcli config <key> [value] [flags]

Flags:
      --get    print configuration value or its default if unset
  -h, --help   help for config

Global Flags: 省略
```

例子：

* 设置：`$ ./cetcli config chain-id coinexdex`

* 查询：`$ ./cetcli config chain-id --get`

参数和选项：

| 参数/选项 | 参数名/选项名 | 类型         | 是否必填    | 默认值 | 说明                                                         |
| --------- | ------------- | ------------ | ----------- | ------ | ------------------------------------------------------------ |
| 参数1     | key           | string       | ✔           |        | 选项名（注意没有前导减号）                                   |
| 参数2     | value         | string\|bool | ✔（设置时） |        | 选项值，可以是字符串或布尔值                                 |
| 选项1     | --get         | bool         |             | false  | 如果该选项是true，则从配置文件读取值并打印到控制台；否则将新的值写入配置文件 |



## 可以设置的key

| name           | 类型（可选值）              | 默认值                | 说明                       |
| -------------- | --------------------------- | --------------------- | -------------------------- |
| chain-id       | string                      |                       | 对应`--chain-id`选项       |
| output         | string (text\|json)         | text                  | 对应`--output`选项         |
| node           | string                      | tcp://localhost:26657 | 对应`--node`选项           |
| broadcast-mode | string (sync\|async\|block) | sync                  | 对应`--broadcast-mode`选项 |
| trace          | bool                        | false                 | 对应`--trace`选项          |
| trust-node     | bool                        | false                 | 对应`--trust-node`选项     |
| indent         | bool                        | false                 | 对应`--indent`选项         |

