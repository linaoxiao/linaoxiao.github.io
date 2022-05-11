---
title: glmark2
categories:
 - work
 - 性能测试
---

### 测试方法
<!--more-->
```
1、安装glmark2包：# sudo apt-get install glmark2
2、清除缓存：
# sync
# echo 3 > /proc/sys/vm/drop_caches
3、执行测试： 
# glmark2 > glmark2.txt
```

### Attention！
```
若机器架构为龙芯，运行时报段错误的话，尝试运行以下代码：
# GALLIUM_DRIVER=softpipe glmark2
```

```
若需在X环境下运行，则需要另外一台电脑远程输入：
$ sudo systemctl stop lightdm
$ sudo X
$ export DISPLAY=:0
```