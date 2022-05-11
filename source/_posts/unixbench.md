---
title: unixbench
categories:
 - work
 - 性能测试
---

### 测试方法
<!--more-->
#### base
```
1、解压UnixBench工具包：
# tar xvf unixbench5.1.3-new.tar
2、进入Unixbench解压后的目录下，编译：
# cd UnixBench
# make clean && make
3、清除缓存：
# sync
# echo 3 > /proc/sys/vm/drop_caches
4、测试单线程和多线程性能，执行命令：
# sudo ./Run -c 1 -c N 
其中N代表cpu核数
```

#### graphics
```
1、解压unixbench测试包，并进入文件夹：
# tar xvf unixbench5.1.3-new.tar
# cd Unixbench
2、修改Run脚本中第1218行，注释如下内容：
# If it failed, bomb out.
#       if (defined($res->{'ERROR'})) {
#           my $name = $params->{'logmsg'};
#           abortRun("\"$name\": " . $res->{'ERROR'});
#       }
3、修改Makefile文件，打开第47行注释，修改为：
GRAPHIC_TESTS = defined
修改第50行为：
GL_LIBS = -lGL -lXext -lX11 -lm
4、在解压后的目录中打开终端执行以下命令，编译、清除缓存然后执行测试：
# make
# export vblank_mode=0
# sync
# echo 3 > /proc/sys/vm/drop_caches
# ./Run graphics
```