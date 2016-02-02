本系列API用于描述低功耗蓝牙**从设备**(BLE Slave)的操作和事件。  
低功耗蓝牙协议规定：从设备只能发送广播，不能主动发起连接，必须等待主设备来连接。
一般蓝牙设备（如蓝牙智能手环）都是从设备，手机、电脑等是主设备。

API声明和数据结构说明见头文件：

* nRF51平台：[juma_sdk_api.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_api.h)
* STM32平台：[bluenrg_sdk_api.h](https://github.com/JUMA-IO/STM32_Platform/blob/master/system/juma/inc/bluenrg_sdk_api.h)

> 如无特殊说明，本系列API同时兼容nRF51平台和STM32平台。

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

## 设置发射功率
###1. 函数声明
**nRF51平台**

```
void ble_device_set_tx_power(int8_t tx_power);
```

**STM32平台**

```
tBleStatus ble_device_set_tx_power(uint8_t level)；
```


###2. 函数功能
设置BLE设备的发射功率或者发射等级。
> 请在`ble_device_start_advertising()`之前调用。  

###3. 函数参数
**nRF51平台**

参数    | 数据类型   | 说明
:----- | :-------- | :------
*tx_power*|int8_t|-40, -30, -20, -16, -12, -8, -4, 0, 4，数值的单位为dBm。缺省值为0。
*返回值*  | 无    | 无

**STM32平台**

参数    | 数据类型   | 说明
:----- | :-------- | :------
*level*|uint8_t|数值范围为0~7，对应8个等级的发射功率。缺省值为7，表示设置为最大发射功率(8dBm)。
*返回值*  | tBleStatus    | [status](#_1)


***
## 设置广播名称
###1. 函数声明
```
void ble_device_set_name(const char* device_name);
```


###2. 函数功能
设置当前BLE设备的广播名称。  
> 请在`ble_device_start_advertising()`之前调用。  


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*device_name*|const char * |设备名称
*返回值*  | 无    | 无


***
## 设置广播间隔
###1. 函数声明
```
void ble_device_set_advertising_interval(uint16_t interval);
```

###2. 函数功能
设置BLE的广播间隔。 
> 请在`ble_device_start_advertising()`之前调用。  

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*interval*|uint16_t |广播包的发送间隔，单位为ms。
*返回值*  | 无    | 无


***
## 设置广播地址
###1. 函数声明
**nRF51平台**

```
void ble_device_select_address(uint8_t index);
```

**STM32平台**

```
tBleStatus ble_address(uint8_t* advaddress);
```


###2. 函数功能
设置BLE设备的广播地址。
> 请在`ble_device_start_advertising()`之前调用。  


###3. 函数参数
**nRF51平台**

参数    | 数据类型   | 说明
:----- | :-------- | :------
*index*|uint8_t|index代表的时内置的地址选择的索引，其范围为 0-255。JUMA SDK的模块其内部都已经内置了256个地址，通过该函数可以选择设备要使用的地址。
*返回值*  | 无    | 无

**STM32平台**

参数    | 数据类型   | 说明
:----- | :-------- | :------
*advaddress*|uint8_t * |广播地址数组，长度为6个字节
*返回值*  | tBleStatus   | 设置是否成功

***
## 开始广播
###1. 函数声明
**nRF51平台**

```
void ble_device_start_advertising(void);
```

**STM32平台**

```
tBleStatus ble_device_start_advertising(void);
```


###2. 函数功能
BLE设备开始广播。


###3. 函数参数
**nRF51平台**

参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | 无    | 无

**STM32平台**

参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | tBleStatus    | [status](#_1)

***
## 停止广播
###1. 函数声明
**nRF51平台**

```
void ble_device_stop_advertising(void);
```

**STM32平台**

```
tBleStatus ble_device_stop_advertising(void);
```


###2. 函数功能
BLE设备停止广播。


###3. 函数参数
**nRF51平台**

参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | 无    | 无

**STM32平台**

参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | tBleStatus    | [status](#_1)


***
## 建立连接事件
###1. 函数声明

```
void ble_device_on_connect(void);
```



###2. 函数功能
当有BLE设备(主设备)连接本设备后，系统会触发该事件。  
该函数内部用于处理BLE设备连接后需要执行的任务。 

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | void      | 无

###4. 函数举例
```
	//当前函数
	void ble_device_on_connect()
	{
		...
		red_led_blink();	//伪指令，点亮一盏LED灯
		...
	}
```	


***
## 断开连接事件
###1. 函数声明

```
void ble_device_on_disconnect(uint8_t  reason);
```


###2. 函数功能
当原来连接的BLE设备断开后，系统会触发该事件。  
该函数内部用于处理BLE设备断开后需要执行的任务。 

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*reason*|uint8_t|断开的原因(TBD)
*返回值*  | void      | 无

###4. 函数举例
```
	//当前函数
	void ble_device_on_disconnect(uint8_t reason)
	{
		...
		
		switch(reason)
		{
			case 0: ...break;
			case 1: ...break;
			case 2: ...break;
			
			default: ... break;
		}
		
		...
	}
	
```

***
## 断开连接
###1. 函数声明
**nRF51平台**

```
void ble_device_disconnect(void);
```

**STM32平台**

```
tBleStatus ble_user_disconnect_device(void);
```


###2. 函数功能
断开与之连接的BLE设备。


###3. 函数参数
**nRF51平台**

参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | 无    | 无

**STM32平台**

参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | tBleStatus| [status](#_1)


***
## 查询连接状态
###1. 函数声明

```
uint8_t ble_device_is_connected(void);
```

> 本API只适用于nRF51平台。


###2. 函数功能
查询当前是否有BLE设备与之连接。  


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | uint8_t  | 连接状态，0表示没有连接设备，非0表示有连接设备。

***
## 发送数据
###1. 函数声明

```
void ble_device_send(uint8_t type, uint32_t length, uint8_t* value);
```



###2. 函数功能
向与之相连的BLE设备发送数据。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*type*   | uint8_t | 数据类型，数值范围（0x00~0x7F）
*length* | uint32_t | 数据长度
*value*  | uint8_t * | 数据内容
*返回值*  | void     | 无

####关于 *type* 参数
JUMA SDK对`type`参数有如下定义：

*type* 的数值|说明
:-------- | :------
0x00~0x7F | 用户自定义，用于区分自己的数据类型
0x80~0xFF | JUMA SDK定义的数据类型


###4. 函数举例
```
	#define TYPE 0x01

	void foo()
	{
		...
		//向与之相连的BLE设备发送数据
		ble_device_send( TYPE, strlen("JUMA_BLE"），"JUMA_BLE");
		...
	}
	
```

***
## 接收数据事件
###1. 函数声明

```
void ble_device_on_message(uint8_t type, uint16_t length, uint8_t* value);
```


###2. 函数功能
当与之相连的BLE设备向本设备发送消息时，系统会触发该事件。  
该函数内部用于处理其他BLE设备发过来的数据。  

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*type*   | uint8_t | 数据类型，数值范围（0x00~0x7F）
*length* | uint16_t | 数据长度
*value*  | uint8_t * | 数据内容
*返回值*  | void     | 无

####关于 *type* 参数
JUMA SDK对`type`参数有如下定义：

*type* 的数值|说明
:-------- | :------
0x00~0x7F | 用户自定义，用于区分自己的数据类型
0x80~0xFF | JUMA SDK定义的数据类型


***
###4. 函数举例
```
	void ble_device_on_message(uint8_t type, uint16_t length, uint8_t* value)
	{
		...		
		ble_device_send( type, length, value );//将收到的数据发送回去
		...
	}
```	


***
## 获取设备身份
###1. 函数声明
```
void ble_device_get_id(uint8_t* id, uint8_t len);
```
> 本API只适用于nRF51平台。


###2. 函数功能
获取用来设备的唯一身份编号。  
ID是由BLE设备的MAC地址，经过特殊的计算方式得出的一个能够唯一表示当前设备的字节序列。 


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*id*|uint8_t * |用来存放设备ID的数组地址
*len*|uint8_t|用来指定要获取的标示序列长度（最大为16字节）
*返回值*  | 无    | 无









