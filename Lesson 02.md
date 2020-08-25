## Lesson 02 TiDB Sysbench 性能测试


### 测试环境

- 硬件配置

| 服务类型 | 硬件配置 | 实例数 |
| :------ | ------: | :------: |
| PD | 2c4g | 3 |
| TiKV | 4c8g | 4 |
| TiDB | 4c8g | 2 |

- 软件版本

| 服务类型 | 软件版本 |
| :------ | ------: |
| PD | v4.0.4 |
| TiKV | v4.0.4 |
| TiDB | v4.0.4 |
| Sysbench | 1.0.17 |

- 拓扑信息

| 服务类型 | 硬件配置 | 实例数 | IP |
| :------ | :------: | :------: | :------: |
| PD | 2c4g | 3 | 172.16.249.161 <br /> 172.16.249.162 <br /> 172.16.249.163 |
| TiKV | 4c8g 100g(ssd) * 1 | 4 | 172.16.249.142 <br /> 172.16.249.143 <br /> 172.16.249.166 |
| TiDB | 4c8g | 2 | 172.16.249.164 <br /> 172.16.249.165 <br /> |

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

### 测试报告
    
- [TiDB Sysbench 性能测试报告](Lesson%2002-1.md)
- [TiDB Sysbench 性能测试报告](Lesson%2002-1.md)
- [TiDB Sysbench 性能测试报告](Lesson%2002-1.md)