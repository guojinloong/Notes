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
* [markdown的标题设置自动添加序号](https://blog.csdn.net/jyn15159/article/details/122978472)
* [为markdown目录标题添加序号](https://blog.csdn.net/weixin_44080380/article/details/120811270?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-120811270-blog-122978472.pc_relevant_paycolumn_v3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-120811270-blog-122978472.pc_relevant_paycolumn_v3&utm_relevant_index=2)
* [markdown 标题自动添加序号，添加目录](https://zhuanlan.zhihu.com/p/476091445)
* [MarkDown公式指导手册](https://zhuanlan.zhihu.com/p/548594654?utm_id=0)