## About validator consensus pubkey

- There are several `validator_address` in response of /blocks API
    - which one is my node?
    - Is my node still online and participates in the consensus?

- /blocks API example response:
<details> <summary> http://<REST_API_IP>:1317/blocks/67344?format=json&kind=block </summary>
<p>

    
```
{
    "block": {
        "data": {
            "txs": null
        },
        "evidence": {
            "evidence": null
        },
        "header": {
            "app_hash": "F4FAC23AC50B23CFAA44EB3BF9EBDCEEDEC73CCDDF42D520820B55490AE80797",
            "chain_id": "coinexdex-test2004",
            "consensus_hash": "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
            "data_hash": "",
            "evidence_hash": "",
            "height": "67344",
            "last_block_id": {
                "hash": "BD75016005EF14520382AD881E06908DCF94517AA9252D717112AAC13A79E2C8",
                "parts": {
                    "hash": "7B2EEEBD903991D1FDD9B1D431DBEB20BF6FE22FB25A0259B3FC8EFEAEBB2732",
                    "total": "1"
                }
            },
            "last_commit_hash": "49DACEE8099ADBB8A3FB07D36B6D118C7C8EFDD8D9D4D901FCCF60BAACC101D8",
            "last_results_hash": "",
            "next_validators_hash": "957264FCBD0C70F2267B8F75F16AA1D6DC3D0E3131823FB1395A93A4B62A1E65",
            "num_txs": "0",
            "proposer_address": "2716C82E471084F0D0C232565670CDB9BD990328",
            "time": "2019-10-23T02:44:24.003982451Z",
            "total_txs": "339",
            "validators_hash": "957264FCBD0C70F2267B8F75F16AA1D6DC3D0E3131823FB1395A93A4B62A1E65",
            "version": {
                "app": "0",
                "block": "10"
            }
        },
        "last_commit": {
            "block_id": {
                "hash": "BD75016005EF14520382AD881E06908DCF94517AA9252D717112AAC13A79E2C8",
                "parts": {
                    "hash": "7B2EEEBD903991D1FDD9B1D431DBEB20BF6FE22FB25A0259B3FC8EFEAEBB2732",
                    "total": "1"
                }
            },
            "precommits": [
                {
                    "block_id": {
                        "hash": "BD75016005EF14520382AD881E06908DCF94517AA9252D717112AAC13A79E2C8",
                        "parts": {
                            "hash": "7B2EEEBD903991D1FDD9B1D431DBEB20BF6FE22FB25A0259B3FC8EFEAEBB2732",
                            "total": "1"
                        }
                    },
                    "height": "67343",
                    "round": "0",
                    "signature": "P1oitdCZRW1n8jP0S1uHin+w/Y8vB0sgzk8iUxBScIksTsoLy+xfWQSbUV9L1CNoU0R0V6wt7uCuMea4W/X9CQ==",
                    "timestamp": "2019-10-23T02:44:24.003982451Z",
                    "type": 2,
                    "validator_address": "2716C82E471084F0D0C232565670CDB9BD990328",
                    "validator_index": "0"
                },
                {
                    "block_id": {
                        "hash": "",
                        "parts": {
                            "hash": "",
                            "total": "0"
                        }
                    },
                    "height": "67343",
                    "round": "0",
                    "signature": "XC00xwvLcOmwoXCDvQwbbTylp9RTMOMDjOR4cyl7Fk+Da/1qBf49/Bi3nF0uegGjQtcj6s7ZD4w2R+PEwK9XDw==",
                    "timestamp": "2019-10-23T02:44:24.190100112Z",
                    "type": 2,
                    "validator_address": "457CC3DA57D1DDD29AA2AAC2941DC8807A6800F7",
                    "validator_index": "1"
                },
                {
                    "block_id": {
                        "hash": "",
                        "parts": {
                            "hash": "",
                            "total": "0"
                        }
                    },
                    "height": "67343",
                    "round": "0",
                    "signature": "2VI6r/iMfRzTnVGjYVpZQBnYwigfaMNwxcyvkTGoRmAr30778NlmT0WmZ2HswokhLoz4dSNy3bycoUX4gSblCg==",
                    "timestamp": "2019-10-23T02:44:24.244571095Z",
                    "type": 2,
                    "validator_address": "728AC69E6CB054051D4E51C078355F41C339E4E9",
                    "validator_index": "2"
                },
                {
                    "block_id": {
                        "hash": "",
                        "parts": {
                            "hash": "",
                            "total": "0"
                        }
                    },
                    "height": "67343",
                    "round": "0",
                    "signature": "mZAQyc8kKLu2tZMaLMR5sAz8kdQRQ1sdGrksbStIru0I3K6OMsi2olmtSFWRgj/EZsvbDOV6mLeBcvDPl077Cw==",
                    "timestamp": "2019-10-23T02:44:24.18239523Z",
                    "type": 2,
                    "validator_address": "D28D32A69AFBCDA74885F9A689DE4A331220B237",
                    "validator_index": "3"
                }
            ]
        }
    },
    "block_meta": {
        "block_id": {
            "hash": "4802068193D3DD728CF75E6682BB024051E72A43139695633F0AD789913C0971",
            "parts": {
                "hash": "469AA3491D59AC0F004DC994324CF6B8C60BBD8DC941297811A610831B5951FB",
                "total": "1"
            }
        },
        "header": {
            "app_hash": "F4FAC23AC50B23CFAA44EB3BF9EBDCEEDEC73CCDDF42D520820B55490AE80797",
            "chain_id": "coinexdex-test2004",
            "consensus_hash": "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
            "data_hash": "",
            "evidence_hash": "",
            "height": "67344",
            "last_block_id": {
                "hash": "BD75016005EF14520382AD881E06908DCF94517AA9252D717112AAC13A79E2C8",
                "parts": {
                    "hash": "7B2EEEBD903991D1FDD9B1D431DBEB20BF6FE22FB25A0259B3FC8EFEAEBB2732",
                    "total": "1"
                }
            },
            "last_commit_hash": "49DACEE8099ADBB8A3FB07D36B6D118C7C8EFDD8D9D4D901FCCF60BAACC101D8",
            "last_results_hash": "",
            "next_validators_hash": "957264FCBD0C70F2267B8F75F16AA1D6DC3D0E3131823FB1395A93A4B62A1E65",
            "num_txs": "0",
            "proposer_address": "2716C82E471084F0D0C232565670CDB9BD990328",
            "time": "2019-10-23T02:44:24.003982451Z",
            "total_txs": "339",
            "validators_hash": "957264FCBD0C70F2267B8F75F16AA1D6DC3D0E3131823FB1395A93A4B62A1E65",
            "version": {
                "app": "0",
                "block": "10"
            }
        }
    }
}

```

</p>
</details>



### Get validator consensus pubkey in priv_validator_key.json
如果节点未使用tmkms的private_validator方案, 使用本地priv_validator_key.json进行签名时,
地址会在priv_validator_key.json中存在.

> grep address ~/.cetd/config/priv_validator_key.json
```
  "address": "2716C82E471084F0D0C232565670CDB9BD990328",
```

### Get validator consensus pubkey
```
ubuntu@ip-172-31-1-44:~$ ./cetd tendermint show-validator
cettestvalconspub1zcjduepqmt3aqqy3hcvm0wv8t3540cvdpy7h2258xamj7zscm3u875pgtg5qg2cpvv

j@j ~ $ cetcli keys parse cettestvalconspub1zcjduepqmt3aqqy3hcvm0wv8t3540cvdpy7h2258xamj7zscm3u875pgtg5qg2cpvv
human: cettestvalconspub
bytes: 1624DE6420DAE3D00091BE19B7B9875C6957E18D093D752A8737772F0A18DC787F50285A28
```

### Get hex address of pubkey
> cetdebug pubkey <bytes[10:]> —> tm.pubkey

```
j@j ~ $ cetdebug pubkey DAE3D00091BE19B7B9875C6957E18D093D752A8737772F0A18DC787F50285A28
Address: 2716C82E471084F0D0C232565670CDB9BD990328
Hex: DAE3D00091BE19B7B9875C6957E18D093D752A8737772F0A18DC787F50285A28
JSON (base64): {"type":"tendermint/PubKeyEd25519","value":"2uPQAJG+Gbe5h1xpV+GNCT11Koc3dy8KGNx4f1AoWig="}
Bech32 Acc: coinexpub1zcjduepqmt3aqqy3hcvm0wv8t3540cvdpy7h2258xamj7zscm3u875pgtg5qtv5lkc
Bech32 Validator Operator: coinexvaloperpub1zcjduepqmt3aqqy3hcvm0wv8t3540cvdpy7h2258xamj7zscm3u875pgtg5qlrd5l5
Bech32 Validator Consensus: coinexvalconspub1zcjduepqmt3aqqy3hcvm0wv8t3540cvdpy7h2258xamj7zscm3u875pgtg5qjf2jsu
```
