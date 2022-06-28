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
GRAPHIC_TESTS = defined   //测试2D、3D才需要
修改第50行为：
GL_LIBS = -lGL -lXext -lX11 -lm   //需要系统提供x11perf命令gl_glibs库
4、在解压后的目录中打开终端执行以下命令，编译、清除缓存然后执行测试：
# make
# export vblank_mode=0
# sync
# echo 3 > /proc/sys/vm/drop_caches
# ./Run graphics
```

### 一些优化
#### 挂载到内存
```
解压前执行：
# sudo mkdir -p /home/Ub
# sudo mount -t tmpfs -o size=4096M tmpfs /home/Ub

较默认方法预计提升：单线10%左右，多线20%左右。
```

#### 修改编译参数
```
解压后进入Unixbench目录：
修改Makefile文件74行，将“OPTON = -O2 -fomit-frame-pointer -fforce-addr -ffast-math -Wall”改为“OPTON = -O3 -fomit-frame-pointer -fforce-addr -ffast-math -Wall -static -flto”
然后编译

较默认方法预计提升：单线15%~20%左右，多线15%~20%左右。
```

#### 修改运行参数
```
解压后进入Unixbench目录：
修改Run文件251行syscall项的option，将“"options" => "10",”改为“"options" => "10 getpid",”
然后编译

较默认方法预计提升：单线20%左右，多线20%左右。
```

#### 安装优化包
```
安装优化包（需额外申请）：kylin-perf-utils_1.3.2-1-20_arm64/x86.deb

较默认方法预计提升：单线40%左右，多线55%左右。
```

#### 挂载到内存 & 修改编译参数
```
较默认方法预计提升：单线25%~35%左右，多线35%~50%左右。
```

#### 挂载到内存 & 修改编译参数 & 修改运行参数
```
较默认方法预计提升：单线55%左右，多线65%~80%左右。
```

#### 挂载到内存 & 修改编译参数 & 修改运行参数 & 优化包
```
较默认方法预计提升：单线115%左右，多线150%~180%左右。
```


