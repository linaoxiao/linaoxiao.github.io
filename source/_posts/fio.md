---
title: fio
categories:
 - work
 - 性能测试
---

### 测试方法
<!--more-->
```
1、安装相关软件包 ：sudo apt-get install libaio1 libaio-dev
2、解压 fio工具包：
# tar -xzvf fio-3.20.tar.gz
3、进入fio解压后目录进行编译：
# cd fio-3.20
# sudo ./configure
# sudo make && make install
4、执行以下命令，清除缓存后执行测试：
# sudo su
# sync
# echo 3 > /proc/sys/vm/drop_caches
#fio -filename=/fio_test -ioengine=libaio -bs=4K -direct=1 -size=1024M -iodepth 32 -thread -rw=write -runtime=600 -numjobs=$CPUN -group_reporting -name=**.result >> **.result -runtime=600
// 将-bs参数设置为4K、128K、1M，将-rw设置为read、write、randread、randwrite，依次进行组合测试；
// **.result为测试结果文件,自定义文件名即可。
```

### Attention！
#### 测试需使用稳定盘
```
注意下盘是否稳定，不稳定的盘测试出的结果无法做比较。
可以使用dd命令对盘进行读写测试，然后用iostat查看数据是否
稳定即可。
例：使用dd命令对盘进行写测试
dd if=/dev/zero of=/root/iot bs=1M count=20000 oflag=direct
这里只写了20g，观察iostat -xmd 1 命令的wMB/s项的结果。
如果都稳定的话，这个盘应该是稳定的
```

#### 目前不是稳定盘的机器
```
联想昭阳N4620Z、曙光W330-H35A1、浪潮英政CP300L、
```

### 一些优化
#### 修改预置条件
```
1、准备fio-3.20测试工具包。
2、二选一：将调度器bfq的空闲时间设为0，或者切换调度器为mq-deadline：
（1）# echo bfq > /sys/block/sda/queue/scheduler
# echo 0 > /sys/block/xxx/queue/iosched/slice_idle
（2）# echo mq-deadline > /sys/block/sda/queue/scheduler
```