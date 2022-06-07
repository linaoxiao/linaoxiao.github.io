---
title: speccpu
categories:
 - work
 - 性能测试
---

### 测试方法
<!--more-->
```
1. 打开终端，安装依赖包：apt-get install gcc g++ gfortran
2.解压缩speccpu2006测试工具包，并附执行权限
# tar xvf speccpu2006-v1.0.1-newest.tar -C /home
# chmod -R a+x /home/speccpu2006-v1.0.1
3.修改./tools/src/make-3.80/glob/glob.c文件54行”==”为“>=”
4.在./tools/src/buildtools文件312行上一行添加“export PERLFLAGS="-A libs=-lm -A libs=-ldl"”
5.安装speccpu2006：
arm和x86架构执行：
# ./install.sh    //在弹出的提示信息中，选择yes回车
mips架构执行：
# mv ./tools/src/buildtools ./tools/src/buildtools.bak
# mv ./tools/src/buildtools.mips.bak ./tools/src/buildtools
# ./install.sh    //在弹出的提示信息中，选择yes回车
3A5000架构执行：
# cp -r loongarch64-linux /home/speccpu2006-v1.0.1/tools/bin
# cp loongarch-fix.cfg /home/speccpu2006-v1.0.1/config
# ./install.sh    //在弹出的提示信息中，选择yes回车
加载环境变量：
#. ./shrc
6.检查SPEC CPU2006是否安装成功
# runspec -V
7.选择相应配置文件进行测试：
arm平台：
# runspec –c arm64.cfg -n 1 all
# runspec –c arm64.cfg -r N -n 1 all    //N和CPU核数一致，根据机器的核数去指定多线程N值；-n 指定测试轮数;all代表浮点和整型参数都测试
x86_64平台：
# runspec –c x86.cfg -n 1 all
# runspec –c x86.cfg -r N -n 1 all        
mips平台：
# runspec -c mips.cfg -n 1 all
# runspec -c mips.cfg -r N -n 1 all
3A5000平台：
# runspec -c loongarch-fix.cfg -n 1 all
#runspec -c loongarch-fix.cfg -r N -n 1 all
```

### 单跑某一项
```
# runspec –c x86.cfg –iterations=1 –noreportable 435.gromacs
```

### 运行前需更改测试路径
`
/home目录改为/data
`

### 一些报错
```
1、安装报错：error running URI-1.35 test suite
解决办法：将./tools/src/URI-1.35/t/heuristic.t的48、56行注释掉，使其跳过检查即可
```