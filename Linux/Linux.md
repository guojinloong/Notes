简介
===

系统
===
## 命令
### lscpu
### uname
### hostname
### ifup
### mount
```shell
mount -a
```

## 设备
### tty
### can
### eth
### mtd

### mdev
```shell
mdev -s
```

## 文件系统
### proc
  proc是进程和系统文件系统。

### tmpfs
  tmpfs是临时文件系统,一种基于内存的文件系统。

### devpts
  devpts远程虚拟终端文件设备,文件夹里面一般是一些字符设备文件。

### sysfs
  sysfs基于内存的文件系统,将内核信息以文件方式提供给用户使用。

移植
===

开发
===
## 线程同步

## 内存映射
mmap()

参考
===
* [Linux sysfs 介绍](https://www.xiexianbin.cn/linux/basic/linux-sysfs/index.html)
* [mmap()的原理与应用](https://www.jianshu.com/p/59dad2d290a1)
* [原来 mmap 这么简单](https://zhuanlan.zhihu.com/p/429455424)