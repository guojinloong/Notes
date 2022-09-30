# SOEM移植

# SOEM使用
## 功能分析
* 自动检测并配置新的从站设备
* 设备热插拔

## 软件实现
* 初始化SOEM并绑定网卡：ECAT_Init
* 扫描从站设备并自动配置：ECAT_Scan
* 从站匹配：ECAT_SlaveMatch
* 从站MAP：ECAT_SlaveMap
* 从站状态机切换：ECAT_StateSet
* 从站连接检测和恢复

# SOEM实例
## 伺服
  驱动松下MINAS-A5B系列EtherCAT伺服MBDHT2510B01。

## 功能分析
* 伺服初始化并匹配：Servo_Init
* 设置伺服PDO映射：Servo_PDO
* 控制伺服PDS状态：Servo_PDS
* 设置伺服控制模式：Servo_ESM

## IO