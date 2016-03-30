本系列API用于描述低功耗蓝牙**主设备**(BLE Master)的操作和事件。  
低功耗蓝牙协议规定：主设备可以监听从设备广播，主动发起连接。  
一般手机、电脑等是主设备或主从一体设备，蓝牙智能手环、蓝牙音箱等是从设备。

API声明和数据结构说明见头文件：

* STM32平台：[mesh.h](https://github.com/JUMA-IO/STM32_Platform/blob/master/system/juma/inc/mesh.h)

蓝牙主设备的API使用例程请参见：[host](https://github.com/JUMA-IO/STM32_Platform/blob/master/product/application/mesh/mesh.c)。

> 本系列API仅支持STM32平台。


***
## 数据结构
### API调用状态
`mesh_status_t `表明了API的调用状态，它是一个`unit8`类型，在`imu_sensor.h`头文件中通过枚举预置了一些状态，摘取部分如下：

宏定义    | 数值  | 说明
:----- | :-------- | :------
mesh_ok | 0x00    | API调用成功
mesh_failed | 0x01    | API调用失败


> 1. 其他宏定义请参阅：[mesh.h](https://github.com/JUMA-IO/STM32_Platform/blob/master/system/middlewares/juma/mesh/mesh.h)。  
> 2. 本宏定义只适用于STM32平台。




***
## 设置manufacturing data ，将设置到广播数据中
###1. 结构体声明
```
typedef struct _mesh_manuf_data_t{
 
    uint8_t   len;              //total data length
    uint8_t   data_type;        // advertise data type
    uint16_t  adv_info_number;  // advertising information number
    uint16_t  data;             // user data
}mesh_manuf_data_t;
```

###2. 结构体数据功能
配置mesh网络中的有效信息，adv_info_number消息版本号、data用户数据。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
len| uint8_t  |manufacturing data 总的数据长度(不包含len的1Byte)
data_type| uint8_t|加入到广播数据中的数据类型(mesh用到的是AD_TYPE_MANUFACTURER_SPECIFIC_DATA)
adv_info_number| uint16_t|mesh网络中消息的版本编号
data| uint16_t|用户想要发送的数据




***
## mesh网络扫描到广播的信息
###1. 函数声明
```
void mesh_rx_scan_data(void* data)
```

###2. 函数功能
接收到扫描到的mesh网络中的广播信息、判断广播信息版本是否为最新版本，给用户层提供最新版本的信息、
配置设备广播最新版本的消息

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
data  | void*    | void类型指针


***
## 在蓝牙连接状态下接收主机端更新mesh网络消息
###1. 函数声明
```
void mesh_rx_host_message(void* data)
```

###2. 函数功能
接收来自主机端的更新消息,断开与主机端的连接以及时将主机更新的消息在mesh网络中更新、将主机更新的消息提供给用户层进行处理、更新广播消息。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
data  | void*    | void类型指针


***
## mesh网络消息回调,将mesh网络中最新的消息提供给用户层
###1. 函数声明
```
void mesh_on_message(void* data)
```

###2. 函数功能
将mesh网络中最新的消息交给用户层进行处理

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
data  | void*    | void类型指针


***
## mesh demo 说明
###1. 在app.c中定义该设备在mesh网络中的ID

###2. 更改广播名字

###3.demo中设备的MESH_ID与mesh网络中的控制数据16bit的data是按位与的关系，
eg.data:1010 0000 1111 0001
   则mesh_id与上data不为0则亮，为0则灭
###4.因此MESH_ID要与移动端发送的数据配合起来


***