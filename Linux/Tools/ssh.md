简介
===
  SSH全称Secure Shell，基于网络（UDP）的安全加密的远程终端。
```shell
#使用root用户登录远程IP为172.16.8.8的服务器
ssh root@172.16.8.8
#将客户端/root文件夹下main.c传到远程IP为172.16.8.8的服务器的/root文件夹下（也可以反过来）
scp /root/main.c root@172.16.8.8:/root
#退出ssh远程登录
exit
```

安装
===
## OpenSSH
  OpenSSH包括客户端openssh-client和服务端openssh-server。
```shell
#检查是否安装openssh-client和openssh-server服务
sudo dpkg -l | grep ssh
```

### openssh-client
  Ubuntu预装了openssh-client，也可以使用如下命令手动安装：
```shell
#安装服务
sudo apt install openssh-client
```

### openssh-server
```shell
#安装服务
sudo apt install openssh-server
#修改配置文件
sudo vim /etc/ssh/ssd_config
PermitRootLogin yes #使能管理员登录
PasswordAuthentication yes #使能用户密码登录
#启动服务
sudo /etc/init.d/ssh start
#检查服务是否启动
sudo ps -e | grep sshd
```

## Dropbear
  Dropbear是一个轻量级SSH服务端与客户端，由Matt Johnston开发，与OpendSSH相比更简洁小巧，对于每个普通用户登录，OpenSSH会开两个sshd进程，而Dropbear只开一个进程，因此占用内存较小，在嵌入式系统中有望取代OpenSSH。

问题
===
* 启动sshd服务时报错：sshd: no hostkeys available -- exiting.
  检查/etc/ssh目录下是否ssh_host_key、ssh_host_rsa_key、ssh_host_dsa_key，使用如下命令生成hostkey：
```shell
ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
/usr/sbin/sshd
```
  最简单的方法其实是卸载openssh-server然后重装。