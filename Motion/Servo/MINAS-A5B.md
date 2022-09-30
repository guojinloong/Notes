## 控制模式
|Mode of operation|说明|
|---|---|
|Profile位置控制模式（Profile position mode，pp）|
|Cyclic位置控制模式（Cyclic synchronous position mode，csp）|
|补偿位置控制模式（Interpolated position mode，ip）|
|原点复位位置控制模式（Homing mode，hm）|
|速度控制模式（Velocity mode，vl）|
|Profile速度控制模式（Profile velocity mode，pv）|
|Cyclic速度控制模式（Cyclic synchronous velocity mode，csv）|
|Profile转矩控制模式（Torque profile mode，tq）|
|Cyclic转矩控制模式（Cyclic synchronous torque mode，cst）|

特点：对于周期同步模式，每个EtherCAT周期发送一次目标位置，假如通信周期是1s，要转10圈，那我第一秒发1，第二秒发2。对于轮廓位置模式，直接发送最终目标位置，伺服自己规划每个时间点需要到达的位置。针对上述，周期同步模式更灵活，我可以发送s型波，梯形波，甚至各种各样形状的波形，最终到达目标位置。
