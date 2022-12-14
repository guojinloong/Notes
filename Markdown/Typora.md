简介
===
  [Typora](https://typora.io/)目前已经收费，官网国内上不了。

下载和安装
------
### Windows

### Linux
#### 仓库安装
```shell
# optional, but recommended
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE

# add Typora's repository
sudo add-apt-repository 'deb http://typora.io linux/'
sudo apt-get update

# install typora
sudo apt-get install typora
```

#### 压缩包安装
  点击[Typora Linux](https://www.jb51.net/softs/736377.html)下载压缩包Typora-linux-x64.tar.gz。
  在Linux下（以Ubuntu 18.04为例）打开终端，解压下载的压缩包，并复制到/opt路径下，执行看看是否报错。
```shell
tar zxvf Typora-linux-x64.tar.gz
cd bin
cp 
sudo cp -ar Typora-linux-x64 /opt
cd /opt/Typora-linux-x64
./Typora
```

  创建快捷方式。
```shell
cd /usr/share/applications
sudo vi Typora.desktop
```
  输入如下内容，这时就可以在应用程序列表中看到Typora了，可以打开并固定到桌面工具栏。
```shell
[Desktop Entry]
Name=Typora
Exec=/opt/Typora-linux-x64/Typora
Type=Application
Icon=/opt/Typora-linux-x64/resources/assets/icon/icon_512x512.png
```

使用
------

参考
===
* [linux下安装typora](https://www.jianshu.com/p/05546654685b)
* [Linux安装及美化Typora详细步骤](https://blog.csdn.net/y_universe/article/details/107184300)