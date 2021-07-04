# 简介
  ssh全称secure shell，基于网络（UDP）的安全加密的远程终端。分为客户端和服务端，通常使用openssh-client和openssh-server实现。
```shell
#使用root用户登录远程IP为172.16.8.8的服务器
ssh root@172.16.8.8
#将客户端/root文件夹下main.c传到远程IP为172.16.8.8的服务器的/root文件夹下（也可以反过来）
scp /root/main.c root@172.16.8.8:/root
#退出ssh远程登录
exit
```

# 服务安装
```shell
#检查是否安装openssh-client和openssh-server服务
sudo dpkg -l | grep ssh
```

## openssh-client
  Ubuntu预装了openssh-client，也可以使用如下命令手动安装：
```shell
#安装服务
sudo apt install openssh-client
```

## openssh-server
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

# 问题
* 启动sshd服务时报错：sshd: no hostkeys available -- exiting.
  检查/etc/ssh目录下是否ssh_host_key、ssh_host_rsa_key、ssh_host_dsa_key，使用如下命令生成hostkey：
```shell
ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
/usr/sbin/sshd
```
  最简单的方法其实是卸载openssh-server然后重装。