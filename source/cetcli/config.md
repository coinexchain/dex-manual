# config command



## Motivation

For some frequently-used sub-commands of cetcli, we want to write the common options into a configure file. Thus we can avoid to re-type them every time we use these commands: it saves our typing and reduces the chances of typos. For a specific option, cetcli fetches its value according to following priority:

1. If an option value is specified in command in command, then use this value.
2. Else, if it is specified in the configure file, then use this value.
3. Else, use its default value.

The configure file for cetcli is `$CETCLI_HOME/config/config.toml`, it's a plain text file, in [TOML](https://github.com/toml-lang/toml) format. The `config` sub command can be used to **edit** or **view** the content of this file.



## Usage

To see the hep:

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

Examples：

* Setting options：`$ ./cetcli config chain-id coinexdex`

* Query options：`$ ./cetcli config chain-id --get`

Parameters and options:

| Parameter/option | name  | type         | required        | default | Comment                                                      |
| ---------------- | ----- | ------------ | --------------- | ------- | ------------------------------------------------------------ |
| parameter 1      | key   | string       | ✔               |         | The name of the option (Please note there is no leading dash `-` |
| parameter 2      | value | string\|bool | ✔(when setting) |         | The value of the option, which can be a string or bool value |
| option 1         | --get | bool         |                 | false   | If this option is true, then read its value from configure file and print it to the console, or else the new value will be written to the configure file. |



## The keys that can be set

| name           | type                        | default               | comment                            |
| -------------- | --------------------------- | --------------------- | ---------------------------------- |
| chain-id       | string                      |                       | corresponding to `--chain-id`      |
| output         | string (text\|json)         | text                  | corresponding to`--output`         |
| node           | string                      | tcp://localhost:26657 | corresponding to`--node`           |
| broadcast-mode | string (sync\|async\|block) | sync                  | corresponding to`--broadcast-mode` |
| trace          | bool                        | false                 | corresponding to`--trace`          |
| trust-node     | bool                        | false                 | corresponding to`--trust-node`     |
| indent         | bool                        | false                 | corresponding to`--indent`         |

