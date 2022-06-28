---
title: stream
categories:
 - work
 - 性能测试
---

### 测试方法
<!--more-->
```
1、解压stream工具包：
# tar xvf stream-5.9-1.tar.bz2
2、进入到解压目录执行编译：
# cd stream-5.9-1
# make clean
# make SET=TRUE N=80000000 NTIMES=100
3、清除缓存：
# sync
# echo 3 > /proc/sys/vm/drop_caches
4、测试单线程和多线程测试：
# ./Run.sh -n 1 -n N
// 若CPU核数为4，则N为4
```

#### 开透明大页的影响
```
修改透明大页配置 TRANSPARENT_HUGEPAGE_ALWAYS=y，stream单项会有提升，但是stream满线会下降，只能二选一，只要不比UOS低就行。
需要多次测试取区间作对比。
```

### 一些优化
#### 用Os测Copy会比O3高，添加-ftree-vectorize会让O2单线Add和Triad分值提高
```
1、解压stream工具包：
# tar xvf stream-5.9-1.tar.bz2
2、进入到解压目录执行编译：
# cd stream-5.9-1
# make clean
# make SET=TRUE N=80000000 NTIMES=100
3、清除缓存：
# sudo su 
# sync
# echo 3 > /proc/sys/vm/drop_caches
4、测试单线程和多线程测试（假设机器为8核）：
# export OMP_NUM_THREADS=8
# export GOMP_CPU_AFFINITY=0-7
# ./Run.sh -n 1 -n 8
```

