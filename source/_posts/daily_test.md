---
title: 性能测试注意事项
categories:
 - work
 - 性能测试
tags: 
 - work_srt
password: kk123123
abstract: 有暗号才可以戳戳我
message: 暗号对了才可以看哦~
theme: up
wrong_pass_message: 不对啦！
wrong_hash_message: 不对不对！
---

### 注意事项
<!--more-->
``` 
1.连接电源【使用电池的情况下电源管理会更改调频策略】
2.控制面板设置电源计划为性能模式
  如果控制面板-系统-电源-使用电源时的模式选择没有性能模式则需要手动设置【v10sp1 0820-1 版本暂时没有性能模式选项】
$ gsettings set org.ukui.power-manager power-policy-ac 0
3.控制面板-系统-电源-此时间段后关闭显示器 选为从不
4.控制面板-系统-电源-此时间段后进入睡眠   选为从不
5.控制面板-个性化-屏保-此时间段后开启屏保 选为从不
```

### 确认是否设置成功：
``` 
$ cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor 
// 确认是否为performance
// 例如八核，检查所有cpu0~7设置是否成功
```

### 硬件环境
#### USB
```
测试USB传输相关性能时，未明确要求时尽量使用USB3.0口（usb3.0有9pin，通常为蓝色，标志带有“SS”）；明确要求USB3.0时，不要插入USB2.0口进行测试。
```
#### 网络
```
测试网络相关性能时，请检查使用的网口速度是否正确。方法：
（1）ifconfig确认网口名称；
（2）ethtool xx|grep Speed，其中xx代表网口名。
若插的千兆，则Speed应为“1000Mb/s”，如果不是请排查问题并重新连接后再测试。
```

### 设置显示器为常亮
```
# echo 80 > /sys/class/backlight/loongson-gpu/brightness（亮度最大值默认是100）
# chmod 444 /sys/class/backlight/loongson-gpu/brightness（防止亮度自动改变可以将文件的写权限关闭）
```

### 访问内网dns
```
或者在/etc/hosts里面加入以下内容：
172.17.66.192	pm.kylin.com
172.20.185.184	gerrit.kylin.com
172.20.191.4	launchpad.dev
172.20.191.3	builder.kylin.com
172.20.191.29	oauth.kylin.com
172.20.191.3	distro.kylin.com
172.20.191.4    archive.launchpad.dev
172.20.191.21   gitlab.kylin.com
172.20.191.4    ppa.launchpad.dev
172.20.185.203  kernel.kylin.com
172.20.191.4    dev.kylinos.cn
172.20.191.4    code.dev.kylinos.cn
172.20.191.4    bugs.dev.kylinos.cn
172.20.191.4    answers.dev.kylinos.cn
172.20.191.4    blueprints.dev.kylinos.cn
172.20.191.4    translations.dev.kylinos.cn
172.20.191.5    nas.kylin.com
172.20.191.22   zsk.kylin.com
```

### 关闭动态调频
```
# gsettings set org.ukui.power-manager power-policy-ac 0
# gsettings set org.ukui.power-manager power-policy-battery 0
```

### 下载离线包
```
$ sudo apt-get install package-name
然后到/var/cache/apt里面就有包了
```

### 串口日志文档
#### 对于导日志的机器 
```
第一步
   下载串口通信工具minicom。使用命令 sudo apt install minicom进行下载。
第二步
    进行串口配置。使用命令sudo minicom -s，，使用上下键选择Serial port setup，回车。
需要改4个地方。分别是Serial Device,Bps/Par/Bits,Hardware Flow Control，Soft Flow Control 
Serial Device 设置
  按A键可以编辑端口，一般来说都是/dev/ttyUSB0,可以通过命令 ls /dev | grep USB查看。根据实际得结果修改。
Bps/Pay/Bits波特率设置
  按E键进入波特率设置页面，一般我们波特率是115200，再次按E即可设置为115200，按enter保存就好。
Hardware Flow Control ,Soft Flow Control
   都设置为NO，分别按F,G即可。设置完成之后再次按enter键来到设置页面，选择save setup as dfl，在enter键保存为默认值，就不用每次打开进行重新设置。按exit退出设置界面。
minicom配置好了，minicom还有很多妙用，执行命令sudo minicom -C file，可以定义日志保存路径
```
#### 对于被测机
```
首先终端输入sudo ls  /proc/tty/driver/
查看通过那个端口输出，一般的话ARM架构里面有ttyAMA，Mips架构和X86架构里面只有serial。因此默认Arm架构为ttyAMA0输出。Mips和X86架构为ttyS0。
（具体看文档 https://docs.qq.com/doc/DTUJhYXJldmJJRFB6）
```

### 蓝牙传输文件
#### 查看传输速率
```
$ obexctl
```
#### 查看服务是否存在
```
$ ps -aux ｜ grep bluetoothService
```
