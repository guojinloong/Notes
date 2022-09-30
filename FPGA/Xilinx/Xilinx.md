简介
===
## IBUFG
  输入全局缓冲，与专用全局时钟输入端口（管脚）直接相连的首级全局缓冲，所有从全局输入时钟端口输入的时钟信号必须经过IBUFG，否则在布局布线时会报错。
## IBUFGDS
  IBUFG的差分形式，当时钟信号从一对差分全局时钟输入端口输入时，必须使用IBUFGDS做为全局时钟输入缓冲。
## BUFG
  全局时钟缓冲，它的输入为IBUFG的输出，它的输出到达FPGA内部的CLB、IOB、选择性块RAM的时钟延迟和抖动最小。
## BUFGP
  相当于IBUFG和BUFG的组合。
## BUFGCE
  带有使能的全局时钟缓冲，它有一个输入端I、一个使能端CE和一个输出端O，只有当CE有效（高电平）时才有输出。
## BUFGMUX
  全局时钟选择缓冲，它有两个输入端I0和I1、一个控制端S和一个输出端O，当S为低电平时输出时钟I0，反之为I1。
## BUFGDLL
  全局缓冲延迟锁相环，相当于BUFG和DLL的组合，目前被DCM取代。
## DCM
  数字时钟管理单元，主要完成时钟的同步、移相、分频、倍频和去抖动等，和全局时钟缓冲配合使用以达到最小的延迟和抖动。

参考
===
* [Xilinx SRL16E 使用详解](http://xilinx.eetrend.com/blog/2019/100018715.html)
* [FPGA上电后IO的默认状态](https://cloud.tencent.com/developer/article/1888608)