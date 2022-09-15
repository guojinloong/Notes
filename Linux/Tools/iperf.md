简介
===
  [iperf](https://iperf.fr)

## 使用
  iperf3的基本使用方法如下：
```shell
iperf3 [-s|-c host] [options]
```

### 服务端
```shell
iperf3 -s
```

### 客户端
```shell
iperf3 -c ip
```

### 参数
|参数|说明|
|---|---|
|-d|测试|
|-i \<interval\>|显示间隔|
|-t \<time\>|测试时间|
|-n \<times\>|测试次数|
|-u|UDP通信|
|-l \<length\>|缓冲区大小，默认128K|
|-P \<process\>|测试进程数|
|-b \<bandwidth\>|测试带宽|
|-w \<window\>|窗口大小|
|-M \<MSS\>|MSS大小，比MTU小40|

示例：
客户端上测试在8Mbps带宽情况下的网络质量：
```shell
iperf3 -u -c 172.17.0.5 -b 8M -i 1 -w 1M -t 10
```

客户端起20个进程，每个进程 100k带宽，测试网络质量：
```shell
iperf3 -u -c 172.17.0.5 -b 100k -i 1 -w 1M -t 30 -P20
```

参考
===
* [zynq操作系统：iperf的安装和使用](https://blog.csdn.net/qq_42330920/article/details/124015599)
* [iperf网络质量测试工具测试带宽](https://blog.51cto.com/u_15127538/4038333)