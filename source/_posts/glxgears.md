---
title: glxgears
categories:
 - work
 - 性能测试
---

### 测试方法
<!--more-->
```
1、预置条件:	
安装mesa-utils包：
# sudo apt-get install mesa-utils
2、清除缓存：
# sync
# echo 3 > /proc/sys/vm/drop_caches
3、执行测试： 
# export vblank_mode=0 && timeout 55 glxgears | tee -a glxgears.txt
```

### 一些优化
#### 测试开始后，立马按“Ctrl+Alt+F4”打开一个终端，不要做任何其他操作