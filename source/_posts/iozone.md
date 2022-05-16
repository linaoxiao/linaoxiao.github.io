---
title: iozone
categories:
 - work
 - 性能测试
---

### 测试方法
<!--more-->
```
1、解压iozone工具包：
# tar xvf iozone3_430.tar
2、进入到解压目录编译工具：
# cd iozone3_430/src/current
arm架构执行： # make linux-arm
x86架构执行： # make linux-AMD64
龙芯架构执行： # make linux-ia64
3、分别设置 iozone 块大小为内存的2倍、1倍、1/2倍大小，命令分别为:
# sync
# echo 3 > /proc/sys/vm/drop_caches
# ./iozone -i 0 -i 1 -i 2 -f /iozonetest -r 16M -s 4G -Rb iozone_4G.xls
# sync
# echo 3 > /proc/sys/vm/drop_caches
# ./iozone -i 0 -i 1 -i 2 -f /iozonetest -r 16M -s 8G
-Rb iozone_8G.xls
# sync
# echo 3 > /proc/sys/vm/drop_caches
# ./iozone -i 0 -i 1 -i 2 -f /iozonetest -r 16M -s 16G
-Rb iozone_16G.xls
// 命令中假设机器内存为 8G，测试时请根据实际情况修改-s 参数。
```

### 调优
#### vm.dirty_ratio 
```
vm.dirty_ratio用于调节脏数据刷新率，即脏数据达到百分之几的时候回写，即理论上调高此值，写性能提高
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

#### 如需设置sys vm参数
```
修改方法：
# sysctl -w vm.dirty_background_ratio=35
# sysctl -w vm.dirty_ratio=60
修改好后可以在sysctl -a | grep vm.dirty 看下是否修改完成
```

#### 修改预读
```
# echo 4069 > /sys/block/sd**/queue/read_ahead_kb 
sd** 换成要测的盘
```

#### 修改bfq空闭
```
# echo 0 > /sys/block/sd**/queue/iosched/slice_idle
sd** 换成要测的盘
```

#### 一些报错
##### 当前设备/目录下可用空间不足16，测试中止
```
方法一：
修改test_opt.h脚本：
function iozone_opt() {
    IOZONE_VERSION="iozone3_430.tar"
    IOZONE_TESTPATH="/"
    IOZONE_TESTNUM=8
    IOZONE_TEST_R="16M"
    IOZONE_TEST_S=""
}

方法二：
更换至/data目录下
```