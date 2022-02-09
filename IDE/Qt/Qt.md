# 概述
  Qt包括开源版本和商业版本，

# 下载和安装
|名称|说明|
|---|---|
|Source SPackages||
|Offline Installers||
|Qt Creator||
|Visual Studio Add-in||
## 下载

## 安装
  不管是在线安装还是离线安装，都需要登陆Qt账号，因此先注册一个Qt账号。
  双击安装程序（qt-opensource-windows-x86-5.12.11.exe），联网并登录Qt账号，选择组件安装。

## 更新组件
  找到Qt安装目录下的MaintenanceTool.exe程序，双击运行，添加、删除或更新组件。

# 开发
## 创建第一个应用
### Qt Widgets
  Qt Widgets库提供了一些UI控件，可以创建经典的桌面风格的用户界面。

### Qt Quik
  Qt Quik库提供了一些类和函数，可以创建时尚、流畅、生动的UI。

[Getting Started with Qt](https://doc.qt.io/qt-6/gettingstarted.html)

## 基础
  Qt程序的基本结构如下：
```C++
#include <QApplication>
#include <QLabel>

int main(int argc, char *argv[])
{
  QApplication app(argc, argv);
  QLabel *label = new QLable("Hello Qt!");
  label->show();
  return app.exec();
}
```

  对于每个Qt类，都有一个与之同名且大写的头文件，其中包含了对该类的定义，使用之前必须包含其头文件。
  QApplication类用来管理整个应用程序所用到的资源，包含两个参数（argc和argv），支持Qt自己的一些命令行参数。
  窗口部件（Widget，Window gadget缩写）有按钮QButton、标签QLabel、滚动条QSlider、菜单栏QMenuBar、工具栏QToolBar、状态栏QStatusBar、框架QMainWindow、对话框QDialog等，窗口部件也可以包含其他窗口部件，绝大多数应用程序都会使用一个QMainWidnow或QDialog作为窗口，其实任意一个窗口部件都可以作为应用程序的窗口。


### 信号和槽
### 布局

## 中级
## 高级

# 参考
* [C++ GUI Programming with Qt4, 2nd Edition](https://www.informit.com/store/c-plus-plus-gui-programming-with-qt4-9780132354165)
* [QT5的程序打包发布（打包成exe可执行程序）](https://blog.csdn.net/kangshuaibing/article/details/84951619)
* [QT5版本添加icon图标](https://www.cnblogs.com/it-tsz/p/10741114.html)