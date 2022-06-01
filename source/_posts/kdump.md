---
title: kdump
categories:
 - work
 - 内核测试
---

### 测试方法
<!--more-->
```
1、环境搭建
ubuntu/kylinos桌面版本系统：
sudo apt install linux-crashdump
（默认同时还会安装kdump-tools、kexec-tools、makedumpfile，注意安装过程会出现两个选择界面，都选择Y）
sudo apt install crash
2、检查/boot/grub/grub.cfg文件
如果grub参数中没有crashkernel参数，则需要手动添加crashkernel=512M参数。
3、kdump状态检查(检查kdump环境是否搭建完成)
ubuntu/kylinos桌面版本系统： 
sudo kdump-config show 或 dmesg | grep crash
如果出现not ready，则要根据具体显示信息分析查看原因。
4、kdump验证测试
然后测试验证kdump，通过sysrq触发内核panic：
# echo 1 > /proc/sys/kernel/sysrq
# echo c > /proc/sysrq-trigger
等待1~2分钟机器自动重启后，在/var/crash目录下可以发现对应的dump文件
通过ls -hf命令查看这两个文件大小，如果文件大小为0则说明有问题，需要接串口查看详细打印信息。
```

#### 测试原理
``` 
kdump（内存转储工具）是基于kexec（内核快速启动工具）的内核崩溃转储机制 在内核崩溃时收集内核崩溃前的内存的运行状态和数据以便分析崩溃原因，收集完成后让系统自动重启。 
sysrq被称为”魔术组合键”， 是内建于Linux内核的调试工具。Sysrq实现的基本原理为：在键盘或串口驱动中(如果是/proc接口方式，则直接定义/proc的相关写入接口即可)，对按键进行判断过滤，然后根据不同的按键进行相应的中断处理。

内核空间的系统调用kexec_load。负责在生产内核启动时将捕获内核加载到指定地址。
用户空间的工具kexec-tools。将捕获内核的地址传递给生产内核，从而在系统崩溃的时候找到捕获内核的地址并运行。

当系统崩溃时，kdump使用kexec启动到第二个内核。第二个内核通常叫做捕获内核，以很小内存启动以捕获转储镜像。第一个内核保留了内存的一部分给第二个内核启动使用。
由于kdump利用kexec启动捕获内核，绕过了BIOS，所以第一个内核的内存得以保留。这是内存崩溃转储的本质。

kdump功能主要分为3部分：
1）kdump使能阶段：安装kdump，然后重启系统生效，系统会预留一段内存作为第二内核和日志存储用；kdump启动会使用kexec命令加载第二内核完毕，等待触发panic启动；
2）系统panic，进入第二内核，内核将出错信息保存在/proc/vmcore提供给用户；
3）日志转储完毕，重启系统，/var/crash分析日志；

# echo 1 > /proc/sys/kernel/sysrq //向sysrq文件中写入1是为了开启内核的SysRq功能，开启这个功能后，只要内核没有挂掉，它就会响应你要求的任何操作
# echo c > /proc/sysrq-trigger  //模拟输入c这个键触发SysRq功能。 c参数指故意让系统崩溃（Crashes the system without first unmounting file systems or syncing disks attached to the system）
```