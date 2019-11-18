# source code for dex-docs
[Docs for CoinEx Chain Dex](https://coinexchain.github.io/dex-docs/index.html)

### inistall mkdocs
> sudo apt install mkdocs

### create new site
```
$ mkdocs new dex-docs
INFO    -  Creating project directory: dex-docs
INFO    -  Writing config file: dex-docs/mkdocs.yml
INFO    -  Writing initial docs: dex-docs/docs/index.md

$ tree dex-docs
dex-docs
├── docs
│   └── index.md
└── mkdocs.yml
```

### server for live editing
```
$ mkdocs serve -a 0.0.0.0:9999
INFO    -  Building documentation...
INFO    -  Cleaning site directory
[I 191103 13:31:07 server:283] Serving on http://0.0.0.0:9999
[I 191103 13:31:07 handlers:60] Start watching changes
[I 191103 13:31:13 handlers:133] Browser Connected: http://127.0.0.1:9999/
```

### build
> mkdocs build

### publish
push contents under `site/` into https://github.com/coinexchain/dex-docs
