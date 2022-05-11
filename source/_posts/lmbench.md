---
title: lmbench
categories:
 - work
 - 性能测试
---

### 测试方法
<!--more-->
```
1、在终端中输入：
$sudo su
# tar xzvf tools/lmbench-3.0-a9-change.tar.gz -C /opt
# cd /opt/lmbench-3.0-a9
2、如果架构为X86跳过此步
# sed -i 's/arm/aarch/' scripts/gnu-os
3、拷贝config-run、lmbench.config.desktop配置文件至 scripts/，并将lmbench.config.desktop改名为lmbench.config，并赋执行权限；
龙芯3a5000执行该步骤，其他架构跳过该步骤：将os拷贝至lmbench/scripts目录下。
4、进行测试：
# make results，选项除mail选NO外均选择默认
5、导出测试结果：
# cd ./results/***-linux-gnu/（***为架构名，根据实际情况进入目录）
# sed -i '/\/dev\/tty: No such device or address/d' `ls ./`
# sed -i '/\[net:/d' `ls ./`
# sed -i '/\[if:/d' `ls ./`
# sed -i '/\[mount:/d' `ls ./`
# cd ../..
# make see
删除无用的信息，得到有用的结果文件summary.out
```

### attention！
```
1、测试的内存，默认2/3，超过16g的固定测的10g
2、换核需要make clean一下
```

```
若没有结果且为3a5000架构，看以下两条命令结果是否一致：
$ arch
$ uname -i
然后修改脚本
```