## TiDB Sysbench 性能测试报告

### 数据准备

- 创建数据库

    `create database sbtest;`
    
- 导入数据之前先设置为乐观模式

    `set global tidb_disable_txn_auto_retry=0;`
    
    `set global tidb_txn_mode='optimistic';`
    
- 导入数据

    `sysbench --config-file=config.cnf oltp_point_select.lua --tables=32 --table-size=1000000 prepare`
    
- 导入结束后再设置回悲观模式

    `set global tidb_disable_txn_auto_retry=1;`
    
    `set global tidb_txn_mode='pessimistic';`
    
### 压测
`sysbench --config-file=config.cnf oltp_point_select.lua --threads=16 --tables=32 --table-size=1000000 run`

`sysbench --config-file=config.cnf oltp_point_select.lua --threads=32 --tables=32 --table-size=1000000 run`

`sysbench --config-file=config.cnf oltp_point_select.lua --threads=64 --tables=32 --table-size=1000000 run`
### 测试结果

Point Select 性能

| Threads | 优化前 QPS | 优化前 95% latency (ms) | 优化后 QPS | 优化后 95% latency (ms) |
| :------: | :------: | :------: | :------: | :------: |
| 16 | 9493.60 | 1.68 | 9410.84 | 1.70 |
| 32 | 12639.68 | 2.53 | 14520.86 | 2.20 |
| 64 | 11924.38 | 5.37 | 16490.16 | 3.88 |

![](static/img/Lesson%2002%20point_select.png)
![](static/img/Lesson%2002%20point_select_TiDB_summary.png)
![](static/img/Lesson%2002%20point_select%20TiKV-QPS.png)
![](static/img/Lesson%2002%20point_select%20TiKV-gRPC.png)

### 优化项

TiDB v4.0 参数配置

`prepared-plan-cache.enabled: true`



### 总结

由tikv detail 面板分析，压测期间grpc duration稍高，大致分析 tikv 的性能是整个系统瓶颈。