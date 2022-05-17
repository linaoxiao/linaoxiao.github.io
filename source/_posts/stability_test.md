---
title: 稳定性测试注意事项
categories:
 - work
 - 稳定性测试
tags: 
 - work_srt
password: kk123123
abstract: 有暗号才可以戳戳我
message: 暗号对了才可以看哦~
theme: up
wrong_pass_message: 不对啦！
wrong_hash_message: 不对不对！
---

### LTP接口测试
#### 3a5000架构
```
3a5000需更改脚本：
loogarch64) ./configure --build=mips --with-realtime-testsuite --with-open-posix-testsuite > ${SCRIPT_DIR}/results/ltp_log/${ARG}/${ARG}_conf.log 2>&1 ;;
```

### 图形稳定性
#### 视频无法正常播放
```
先检查是否安装mpv
更改脚本：将-vo=x11 删掉
```

### LTP
#### 若中途失败或其他因素需重跑
```
reboot后删除/otp/ltp
```
#### 设置优先级范围以及改变优先级调度策略
```
终端执行如下命令：
$ cd /opt/ltp/testcases/bin/
$ ./sched_setparam01-05 //设置优先级范围
$ ./sched_setscheduler01-03 //改变优先级调度策略
sched_setparam()系统调用才能改变进程的静态优先级。
sched_setscheduler()函数将pid所指定进程的调度策略和调度参数分别设置为param指向的sched_param结构中指定的policy和参数。
```