本系列API用于描述低功耗蓝牙**主设备**(BLE Master)的操作和事件。  
低功耗蓝牙协议规定：主设备可以监听从设备广播，主动发起连接。  
一般手机、电脑等是主设备或主从一体设备，蓝牙智能手环、蓝牙音箱等是从设备。

API声明和数据结构说明见头文件：

* STM32平台：[bluenrg_sdk_api.h](https://github.com/JUMA-IO/STM32_Platform/blob/master/system/juma/inc/bluenrg_sdk_api.h)

蓝牙主设备的API使用例程请参见：[host](https://github.com/JUMA-IO/STM32_Platform/blob/master/product/application/host/app.c)。

> 本系列API仅支持STM32平台。


***
## 数据结构
### API调用状态
`tBleStatus `表明了API的调用状态，它是一个`unit8`类型，在`ble_status.h`头文件中通过宏定义预置了一些状态，摘取部分如下：

宏定义    | 数值  | 说明
:----- | :-------- | :------
BLE_STATUS_SUCCESS | 0x00    | API调用成功
BLE_STATUS_FAILED | 0x41    | API调用失败
BLE_STATUS_BUSY | 0x43    | API状态繁忙
BLE_STATUS_TIMEOUT | 0xFF    | API调用超时

> 1. 其他宏定义请参阅：[ble_status.h](https://github.com/JUMA-IO/STM32_Platform/blob/master/system/drivers/bluenrg/inc/ble_status.h)。  
> 2. 本宏定义只适用于STM32平台。




***
## 设置扫描参数
###1. 函数声明
```
void ble_host_set_scan_param(uint8_t* own_address, uint8_t tx_power_level, uint16_t scan_interval);
```

###2. 函数功能
配置主设备的扫描参数。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*own_address*| uint8_t * |主设备自己的蓝牙地址
*tx_power_level*| uint8_t|主设备的发射功率等级，取值范围为[0~7]
*scan_interval*| uint8_t|主设备的扫描间隔，单位毫秒(ms)
*返回值*  | 无    | 无



***
## 开始蓝牙扫描
###1. 函数声明
```
tBleStatus ble_host_start_scan(void);
```

###2. 函数功能
开始扫描蓝牙设备。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | tBleStatus    | [status](#_1)


***
## 关闭蓝牙扫描
###1. 函数声明
```
tBleStatus ble_host_stop_scan(void);
```

###2. 函数功能
停止扫描蓝牙设备。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | tBleStatus    | [status](#_1)


***
## 扫描设备事件
###1. 函数声明
```
void ble_host_on_device_info(scan_device_found_info device_info)
```

###2. 函数功能
到扫描到蓝牙设备后，系统触发该事件，返回蓝牙设备的信息。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*device_info*| scan_device_found_info |蓝牙设备信息
*返回值*  | 无    | 无

####关于*scan_device_found_info*参数

`scan_device_found_info `数据类型的定义如下所示，它用于表示一个蓝牙设备的信息：

```
typedef struct scan_device_found {
    uint8_t		bdaddr_type;  	//设备地址类型
    tBDAddr	 	bdaddr;       	//设备地址
    uint8_t		local_name_len;  //设备名称长度
    uint8_t  	local_name[VARIABLE_SIZE]; //设备名称
    uint8_t		uuid_type; 		//UUID类型
    uint8_t  	uuid[VARIABLE_SIZE]; //UUID
    int8_t 		RSSI; 			//信号强度
} scan_device_found_info;
```

***
## 发起蓝牙连接
###1. 函数声明
```
void ble_host_connect(tBDAddr bdaddr);
```

###2. 函数功能
主设备发起对从设备的蓝牙连接。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*bdaddr*  | tBDAddr    | 蓝牙从设备地址，6个字节。
*返回值*  | 无    | 无


***
## 蓝牙连接事件
###1. 函数声明
```
void ble_host_on_connect( void );
```

###2. 函数功能
当成功建立起蓝牙连接后，系统触发该事件。

###3. 函数参数
无。


***
## 获取设备特性
###1. 函数声明
```
void ble_host_discover_char(void* arg);
```

###2. 函数功能
获取已连接设备的特性。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*arg*  | void *    | TBD
*返回值*  | 无    | 无





***
## 发送数据
###1. 函数声明
```
tBleStatus ble_host_send(uint8_t type, uint32_t length, uint8_t* value);
```

###2. 函数功能
发送蓝牙数据。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*type*| uint8_t * |数据类型
*length*| uint32_t |数据长度
*value*| uint8_t * |数据内容
*返回值*  | tBleStatus    | [status](#_1)



***
## 接收数据事件
###1. 函数声明
```
void ble_host_on_message(uint8_t type, uint16_t length, uint8_t* value);
```

###2. 函数功能
当接收到蓝牙数据时，系统会触发该事件。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*type*| uint8_t * |数据类型
*length*| uint32_t |数据长度
*value*| uint8_t * |数据内容
*返回值*  | tBleStatus    | [status](#_1)













