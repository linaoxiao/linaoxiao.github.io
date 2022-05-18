---
title: optimize
categories:
 - work
 - 性能测试
---


### 系统优化
#### 关闭系统安全
<!--more-->
```
以下两种方法均可：
1、开机时在grub界面按“e”键进入编辑界面，将“security=enable”修改为“security=”，然后按“Ctrl+X”进入系统。该方法重启失效。
2、修改/boot/下的grub.cfg文件（不同架构路径不同），修改文件中启动参数中“security=enable”修改为“security=”，保存后重启。该方法永久生效。

设置完后使用:# getstatus确认是否设置成功，关闭成功会打印“KySec status: disabled”。
```

#### cpu频率固定为最高，并关闭动态调频
```
1、# sudo cpufreq-set -g performance
2、# sudo gsettings set org.ukui.power-manager power-policy-ac 0
   # sudo gsettings set org.ukui.power-manager power-policy-battery 0
3、# sudo echo performance > /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

设置完后使用：# cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor，查看是否均为“performance”。
```

#### 内核启动参数修改
```
以下两种方法均可：
1、开机时在grub界面按“e”键进入编辑界面，在“linux /vmlinuzxxx”行中添加“loglevel=0 cgroup_disable=memory audit=0 mitigations=off kpti=off ssbd=force-off security=none”，然后按“Ctrl+X”进入系统。改方法重启失效。
2、修改/boot/下的grub.cfg文件（不同架构路径不同），在文件中启动参数行添加“loglevel=0 cgroup_disable=memory audit=0 mitigations=off kpti=off ssbd=force-off security=none”，保存后重启。该方法永久生效。

设置完后进入系统使用# cat /proc/cmdline确认是否设置成功，成功时会打印信息中会包含“mitigations=off kpti=off ssbd=force-off”。
```

#### 关闭组调度
```
以下三种方法均可：
1、kernel.sched_autogroup_enabled = 0添加到/etc/sysctl.conf中，然后update-grub
2、把“echo 0 > /proc/sys/kernel/sched_autogroup_enabled”写到开机/etc/profile.d/下的脚本里
3、执行echo 0 > /proc/sys/kernel/sched_autogroup_enabled（该步骤每次开机都必须重复设置）

设置完后cat /proc/sys/kernel/sched_autogroup_enabled查看是否为0。
```

#### 关闭自启动应用
```
在开始菜单-设置-系统-开机启动中，关闭“天气”等其他自启动应用。
```

#### 关闭服务
```
以下两种方法均可：
1、（该方法每次重启后都必须重复操作）# systemctl stop syslog.socket
   # systemctl stop auditd.service
2、# systemctl disable auditd.service

设置完后查看服务状态是否修改成功。
```

#### 卸载不需要的应用
```
1、卸载软件商店：# sudo dpkg -P kylin-software-center
2、卸载ukui-search：# sudo dpkg -P ukui-search
3、卸载奇安信杀毒：# sudo dpkg -P qaxsafe
4、卸载安卓兼容相关包：# sudo dpkg -P containerd
5、卸载麒麟更新管理器：# sudo apt purge kylin-update-manager
6、更新电源管理器ukui-power-manager 2.1.29-rc1
```

#### 关闭不需要的进程
```
测试前，用top命令是否有进程占用CPU，如果有，等CPU空闲或杀死/卸载相关应用进程，可能占用CPU的有麒麟打印（kylin-printer）。
```

#### 升级应用层跑分补丁包
```
需额外申请。
```

#### 升级跑分biv内核
```
需额外申请。
```

#### 安装跑分版本系统
```
需额外申请。3A5000平台选择用XFS文件系统。
```