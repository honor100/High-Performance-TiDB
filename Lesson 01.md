## Lesson 01 编译源码部署环境

### 环境准备

- 系统包

`yum install gcc-c++ libstdc++-devel cmake3 zlib-devel –y`

`ln -s -f /usr/bin/cmake3 /usr/bin/cmake`
 
 
- golang 环境

`wget https://golang.org/dl/go1.13.15.linux-arm64.tar.gz`

`tar -xf go1.13.15.linux-arm64.tar.gz`

`export PATH=/home/ops/go/bin:$PATH`

- rust 环境

`curl https://sh.rustup.rs -sSf | sh -s -- -y`

`source $HOME/.cargo/env`



### 编译 pd
`git clone -b v4.0.4 https://github.com/tikv/pd.git`

`cd pd`

`make`

### 编译 tidb
`git clone -b v4.0.4 https://github.com/pingcap/tidb.git`

修改代码，实现启动事务时打印日志
![](static/img/Lesson%2001.png)

`cd tidb`

`make`


### 编译 tikv
`git clone -b v4.0.4 https://github.com/tikv/tikv.git`

`cd tikv`

`cat rust-toolchain | xargs rustup override set`

`ROCKSDB_SYS_SSE=0 make dist_release`

备注: ROCKSDB_SYS_SSE pingcap 自定义的环境变量，表示是否用 SSE 指令集，arm 架构不支持这个指令集。

### 启动验证：

- 将编译后的文件拷贝到统一的目录：

- 启动

`./pd-server --data-dir=pd --log-file=pd.log &`

`./tikv-server --pd='127.0.0.1:2379' --data-dir=tikv --log-file=tikv.log &`

`./tidb-server --store=tikv --path='127.0.0.1:2379' --log-file=tidb.log &`

- 验证

通过 mysql 客户端工具连接，正常连接则代表功能正常

`mysql -h 127.0.0.1 -u root -P 4000`
