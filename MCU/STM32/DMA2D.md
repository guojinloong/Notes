## 模式
### 寄存器到存储器
|寄存器|定义|
|---|---|
|DMA2D_CR|控制寄存器，设置工作模式和启动|
|DMA2D_OPFCCR|输出PFC控制寄存器，设置颜色模式|
|DMA2D_OCOLR|输出颜色寄存器|
|DMA2D_OMAR|输出存储器寄存器|
|DMA2D_NLR|行数寄存器，每行像素数和行数|
|DMA2D_OOR|输出偏移寄存器，输出行偏移（像素）|

### 存储器到存储器
|寄存器|定义|
|---|---|
|DMA2D_FGPFCCR|前景层PFC控制寄存器，设置颜色模式|
|DMA2D_FGMAR|前景层存储器地址寄存器|
|DMA2D_OMAR|输出存储器地址寄存器|
|DMA2D_NLR|行数寄存器，每行像素数和行数|
|DMA2D_FGOR|前景层偏移寄存器，前景层行偏移（像素）|
|DMA2D_OOR|输出偏移寄存器，输出行偏移（像素）|

## 操作步骤
```c
//使能DMA2D时钟
RCC->AHB1ENR &= ~(RCC_AHB1ENR_DMA2DEN)
//停止DMA2D
DMA2D->CR &= ~DMA2D_CR_START;
//设置DMA2D工作模式
DMA2D->CR = DMA2D_R2M;//寄存器到存储器模式
DMA2D->CR = DMA2D_M2M;//存储器到存储器模式
//设置DMA2D相关参数
...
//启动DMA2D
DMA2D->CR |= DMA2D_CR_START;
//等待DMA2D传输完成，清除相关标志
while(DMA2D->ISR & DMA2D_FLAG_TC == 0);//等待传输完成
DMA2D->IFCR |= DMA2D_FLAG_TC;//清除传输完成标志
```