---
layout: wiki
title: MPU6050的硬件解算四元素和软件解算四元素差异
---
# {{page.title}}

第一，关于MPU6050

   MPU-60X0 是全球首例9 轴运动处理传感器。它集成了3 轴MEMS 陀螺仪，3 轴MEMS
加速度计，以及一个可扩展的数字运动处理器DMP（Digital Motion Processor），可用I2C
接口连接一个第三方的数字传感器，比如磁力计。扩展之后就可以通过其I2C 或SPI 接口
输出一个9 轴的信号（SPI 接口仅在MPU-6000 可用）。MPU-60X0 也可以通过其I2C 接口
连接非惯性的数字传感器，比如压力传感器。
MPU-60X0 对陀螺仪和加速度计分别用了三个16 位的ADC，将其测量的模拟量转化
为可输出的数字量。为了精确跟踪快速和慢速的运动，传感器的测量范围都是用户可控的，
陀螺仪可测范围为±250，±500，±1000，±2000°/秒（dps），加速度计可测范围为±2，±4，
±8，±16g。
一个片上1024 字节的FIFO，有助于降低系统功耗。
和所有设备寄存器之间的通信采用400kHz 的I2C 接口或1MHz 的SPI 接口（SPI 仅
MPU-6000 可用）。对于需要高速传输的应用，对寄存器的读取和中断可用20MHz 的SPI。
另外，片上还内嵌了一个温度传感器和在工作环境下仅有±1%变动的振荡器。

第二，关于Crazepony-II的DMP应用 

   在Crazepony-II上，测试了软件解算四元素，然后通过四元素解算姿态角这种实现方式，其实总的来说，并没感觉36MHz的主控压力有多大，没有出现机身不稳，卡死的情况
   同时，本着务实他的态度，也测试了MPU6050的硬解四元素，即从IIC总线上读到的数据不再是MPU60x0的AD值，而是通过初始化对DMP引擎的配置，从IIC总线上读到的直接就是四元素的值，从而跳过了程序通过AD值计算四元素这个看起来繁琐的步骤。测试结果是，机身反应的确要比之前反应灵活，最关键的一点是，这样得出的偏航角（Yaw）很稳很稳，基本不会漂移或者说漂移小到了可以容忍的地步。
   最后，MPU60x0的强大之处不仅于此，它支持一个从IIC接口，可以外部接上一个磁力计，如HMC5883，这样一来，DMP引擎可以直接输出一个绝对的方向姿态，即能够输出一个带东西南北的姿态数据包，很厉害的样子。在Crazepony-II第四版将会加上这样一个磁力计，相信它再也不会迷路了~~
 