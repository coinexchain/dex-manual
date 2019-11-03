# Validator node monitor metrics
- Monitor latest height (`latest_block_height`):

    > /home/ubuntu/cetcli status --home=/home/ubuntu/.cetcli_watchdog_query
    
    ```
    
    {"node_info":{"protocol_version":{"p2p":"7","block":"10","app":"0"},"id":"b71a1fbfd2aeaad55eec6d2e61c4d2b431a20e09","listen_addr":"tcp://3.132.21.89:26656",
    "network":"coinexdex-test2004","version":"0.32.2","channels":"4020212223303800","moniker":"Lauren-sentry2","other":{"tx_index":"on","rpc_address":"tcp://0.0.0.0:26657"}},"sync_info":{"latest_block_hash":"4802068193D3DD728CF75E6682BB024051E72A43139695633F0AD789913C0971","latest_app_hash":"F4FAC23AC50B23CFAA44EB3BF9EBDCEEDEC73CCDDF42D520820B55490AE80797",
    "latest_block_height":"67344","latest_block_time":"2019-10-23T02:44:24.003982451Z","catching_up":false},"validator_info":{"address":"9339BCBBC0D7D1AB53CA88F472E5411DF17A08A3","pub_key":{"type":"tendermint/PubKeyEd25519","value":"7wKfJ9kgbZ/Za8TImKvsHHkOwiqkfeGOCoudg2QWDXg="},"voting_power":"0"}}
    
    ```

- Monitor consensus activities
[Is my validator still online and participating consensus process](../docs/12_pubkeys_and_addresses.md)


# prometheus
- Set `prometheus = true` in config.toml 
    - default file location: ~/.cetd/config/config.toml
- Metrics will be provided at: http://localhost:26660/metrics

```
# HELP go_gc_duration_seconds A summary of the GC invocation durations.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 1.5995e-05
go_gc_duration_seconds{quantile="0.25"} 1.8698e-05
go_gc_duration_seconds{quantile="0.5"} 2.5378e-05
go_gc_duration_seconds{quantile="0.75"} 3.8401e-05
go_gc_duration_seconds{quantile="1"} 0.00034557
go_gc_duration_seconds_sum 0.000444042
go_gc_duration_seconds_count 5
# HELP go_goroutines Number of goroutines that currently exist.
# TYPE go_goroutines gauge
go_goroutines 57
# HELP go_info Information about the Go environment.
# TYPE go_info gauge
go_info{version="go1.13.1"} 1
# HELP go_memstats_alloc_bytes Number of bytes allocated and still in use.
# TYPE go_memstats_alloc_bytes gauge
go_memstats_alloc_bytes 4.1781096e+07
# HELP go_memstats_alloc_bytes_total Total number of bytes allocated, even if freed.
# TYPE go_memstats_alloc_bytes_total counter
go_memstats_alloc_bytes_total 4.9153984e+07
# HELP go_memstats_buck_hash_sys_bytes Number of bytes used by the profiling bucket hash table.
# TYPE go_memstats_buck_hash_sys_bytes gauge
go_memstats_buck_hash_sys_bytes 1.454263e+06
# HELP go_memstats_frees_total Total number of frees.
# TYPE go_memstats_frees_total counter
go_memstats_frees_total 64817
# HELP go_memstats_gc_cpu_fraction The fraction of this program's available CPU time used by the GC since the program started.
# TYPE go_memstats_gc_cpu_fraction gauge
go_memstats_gc_cpu_fraction 0.0048864180722613774
# HELP go_memstats_gc_sys_bytes Number of bytes used for garbage collection system metadata.
# TYPE go_memstats_gc_sys_bytes gauge
go_memstats_gc_sys_bytes 2.38592e+06
# HELP go_memstats_heap_alloc_bytes Number of heap bytes allocated and still in use.
# TYPE go_memstats_heap_alloc_bytes gauge
go_memstats_heap_alloc_bytes 4.1781096e+07
# HELP go_memstats_heap_idle_bytes Number of heap bytes waiting to be used.
# TYPE go_memstats_heap_idle_bytes gauge
go_memstats_heap_idle_bytes 2.2126592e+07
# HELP go_memstats_heap_inuse_bytes Number of heap bytes that are in use.
# TYPE go_memstats_heap_inuse_bytes gauge
go_memstats_heap_inuse_bytes 4.3835392e+07
# HELP go_memstats_heap_objects Number of allocated objects.
# TYPE go_memstats_heap_objects gauge
go_memstats_heap_objects 146154
# HELP go_memstats_heap_released_bytes Number of heap bytes released to OS.
# TYPE go_memstats_heap_released_bytes gauge
go_memstats_heap_released_bytes 2.2126592e+07
# HELP go_memstats_heap_sys_bytes Number of heap bytes obtained from system.
# TYPE go_memstats_heap_sys_bytes gauge
go_memstats_heap_sys_bytes 6.5961984e+07
# HELP go_memstats_last_gc_time_seconds Number of seconds since 1970 of last garbage collection.
# TYPE go_memstats_last_gc_time_seconds gauge
go_memstats_last_gc_time_seconds 1.570602094854841e+09
# HELP go_memstats_lookups_total Total number of pointer lookups.
# TYPE go_memstats_lookups_total counter
go_memstats_lookups_total 0
# HELP go_memstats_mallocs_total Total number of mallocs.
# TYPE go_memstats_mallocs_total counter
go_memstats_mallocs_total 210971
# HELP go_memstats_mcache_inuse_bytes Number of bytes in use by mcache structures.
# TYPE go_memstats_mcache_inuse_bytes gauge
go_memstats_mcache_inuse_bytes 13888
# HELP go_memstats_mcache_sys_bytes Number of bytes used for mcache structures obtained from system.
# TYPE go_memstats_mcache_sys_bytes gauge
go_memstats_mcache_sys_bytes 16384
# HELP go_memstats_mspan_inuse_bytes Number of bytes in use by mspan structures.
# TYPE go_memstats_mspan_inuse_bytes gauge
go_memstats_mspan_inuse_bytes 188496
# HELP go_memstats_mspan_sys_bytes Number of bytes used for mspan structures obtained from system.
# TYPE go_memstats_mspan_sys_bytes gauge
go_memstats_mspan_sys_bytes 196608
# HELP go_memstats_next_gc_bytes Number of heap bytes when next garbage collection will take place.
# TYPE go_memstats_next_gc_bytes gauge
go_memstats_next_gc_bytes 5.2807136e+07
# HELP go_memstats_other_sys_bytes Number of bytes used for other system allocations.
# TYPE go_memstats_other_sys_bytes gauge
go_memstats_other_sys_bytes 1.386561e+06
# HELP go_memstats_stack_inuse_bytes Number of bytes in use by the stack allocator.
# TYPE go_memstats_stack_inuse_bytes gauge
go_memstats_stack_inuse_bytes 1.14688e+06
# HELP go_memstats_stack_sys_bytes Number of bytes obtained from system for stack allocator.
# TYPE go_memstats_stack_sys_bytes gauge
go_memstats_stack_sys_bytes 1.14688e+06
# HELP go_memstats_sys_bytes Number of bytes obtained from system.
# TYPE go_memstats_sys_bytes gauge
go_memstats_sys_bytes 7.25486e+07
# HELP go_threads Number of OS threads created.
# TYPE go_threads gauge
go_threads 19
# HELP promhttp_metric_handler_requests_in_flight Current number of scrapes being served.
# TYPE promhttp_metric_handler_requests_in_flight gauge
promhttp_metric_handler_requests_in_flight 1
# HELP promhttp_metric_handler_requests_total Total number of scrapes by HTTP status code.
# TYPE promhttp_metric_handler_requests_total counter
promhttp_metric_handler_requests_total{code="200"} 2
promhttp_metric_handler_requests_total{code="500"} 0
promhttp_metric_handler_requests_total{code="503"} 0
# HELP tendermint_consensus_block_interval_seconds Time between this and the last block.
# TYPE tendermint_consensus_block_interval_seconds gauge
tendermint_consensus_block_interval_seconds{chain_id="coinexdex-test1"} 2.143899
# HELP tendermint_consensus_block_size_bytes Size of the block.
# TYPE tendermint_consensus_block_size_bytes gauge
tendermint_consensus_block_size_bytes{chain_id="coinexdex-test1"} 567
# HELP tendermint_consensus_byzantine_validators Number of validators who tried to double sign.
# TYPE tendermint_consensus_byzantine_validators gauge
tendermint_consensus_byzantine_validators{chain_id="coinexdex-test1"} 0
# HELP tendermint_consensus_byzantine_validators_power Total power of the byzantine validators.
# TYPE tendermint_consensus_byzantine_validators_power gauge
tendermint_consensus_byzantine_validators_power{chain_id="coinexdex-test1"} 0
# HELP tendermint_consensus_height Height of the chain.
# TYPE tendermint_consensus_height gauge
tendermint_consensus_height{chain_id="coinexdex-test1"} 13
# HELP tendermint_consensus_latest_block_height The latest block height.
# TYPE tendermint_consensus_latest_block_height gauge
tendermint_consensus_latest_block_height{chain_id="coinexdex-test1"} 12
# HELP tendermint_consensus_missing_validators Number of validators who did not sign.
# TYPE tendermint_consensus_missing_validators gauge
tendermint_consensus_missing_validators{chain_id="coinexdex-test1"} 0
# HELP tendermint_consensus_missing_validators_power Total power of the missing validators.
# TYPE tendermint_consensus_missing_validators_power gauge
tendermint_consensus_missing_validators_power{chain_id="coinexdex-test1"} 0
# HELP tendermint_consensus_num_txs Number of transactions.
# TYPE tendermint_consensus_num_txs gauge
tendermint_consensus_num_txs{chain_id="coinexdex-test1"} 0
# HELP tendermint_consensus_rounds Number of rounds.
# TYPE tendermint_consensus_rounds gauge
tendermint_consensus_rounds{chain_id="coinexdex-test1"} 0
# HELP tendermint_consensus_total_txs Total number of transactions.
# TYPE tendermint_consensus_total_txs gauge
tendermint_consensus_total_txs{chain_id="coinexdex-test1"} 0
# HELP tendermint_consensus_validators Number of validators.
# TYPE tendermint_consensus_validators gauge
tendermint_consensus_validators{chain_id="coinexdex-test1"} 1
# HELP tendermint_consensus_validators_power Total power of all validators.
# TYPE tendermint_consensus_validators_power gauge
tendermint_consensus_validators_power{chain_id="coinexdex-test1"} 5e+08
# HELP tendermint_mempool_size Size of the mempool (number of uncommitted transactions).
# TYPE tendermint_mempool_size gauge
tendermint_mempool_size{chain_id="coinexdex-test1"} 0
# HELP tendermint_state_block_processing_time Time between BeginBlock and EndBlock in ms.
# TYPE tendermint_state_block_processing_time histogram
tendermint_state_block_processing_time_bucket{chain_id="coinexdex-test1",le="1"} 5
tendermint_state_block_processing_time_bucket{chain_id="coinexdex-test1",le="11"} 12
tendermint_state_block_processing_time_bucket{chain_id="coinexdex-test1",le="21"} 12
tendermint_state_block_processing_time_bucket{chain_id="coinexdex-test1",le="31"} 12
tendermint_state_block_processing_time_bucket{chain_id="coinexdex-test1",le="41"} 12
tendermint_state_block_processing_time_bucket{chain_id="coinexdex-test1",le="51"} 12
tendermint_state_block_processing_time_bucket{chain_id="coinexdex-test1",le="61"} 12
tendermint_state_block_processing_time_bucket{chain_id="coinexdex-test1",le="71"} 12
tendermint_state_block_processing_time_bucket{chain_id="coinexdex-test1",le="81"} 12
tendermint_state_block_processing_time_bucket{chain_id="coinexdex-test1",le="91"} 12
tendermint_state_block_processing_time_bucket{chain_id="coinexdex-test1",le="+Inf"} 12
tendermint_state_block_processing_time_sum{chain_id="coinexdex-test1"} 14.956
tendermint_state_block_processing_time_count{chain_id="coinexdex-test1"} 12
```


