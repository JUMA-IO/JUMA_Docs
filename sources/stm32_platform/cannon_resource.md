

##板子简介
Cannon是基于STM32平台和BlueNRG蓝牙芯片设计的高性能开发板，它的板载资源丰富、处理能力强大，并能够远距离传输蓝牙信号。  

![](./images/cannon_poster.jpg)

***
##板载资源
####1. 处理器：STM32F401  
- ARM Cortex-M4，最高可达84MHz frequency；
- 512KB Flash / 96KB SRAM；
- 1.7V-3.6V供电电压；

####2. 低功耗蓝牙芯片：BlueNRG
- 发射功率最高可达+8dBm；
- 发射峰值电流8.2mA @0dBm, 3V；
- 待机电流1.7uA；
- 支持蓝牙Master与Slave模式；
- 支持蓝牙GAP、GATT、SM、L2CAP、LL、RF_PHY层；
- MCU接口：HCI(Host-Controller Interface) over SPI；
- 附有巴伦天线匹配芯片；

####3. 传感器阵列
- 加速度计/陀螺仪：LSM6DS3
- 磁力计：LSM303AGR
- 温湿度计：HTS221
- 气压计：LPS25H(B)

####4. 其他
- 3组I2C、4组SPI、3组UART、USB、TF卡槽等、按键、LED等；
- 可选择的Cable天线和陶瓷天线；
- 可选择的USB供电、电池组供电；

***
##规格手册
请下载：[Cannon.pdf](http://www.juma.io/spec/cannon.pdf)




