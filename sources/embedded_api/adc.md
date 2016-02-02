本文档用以描述模拟-数字转换(Analog - Digital Convertor)的操作方法。
> 本系列API目前仅支持nRF51平台。

对于nRF51平台的用户，使用时请引用相应的头文件：

* API头文件：[juma_sdk_api.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_api.h)
* 数据类型或宏定义参见：[juma_sdk_types.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_types.h)


***
## 测量模拟信号
###1. 函数声明
```
void adc_measure(uint8_t pin, uint8_t bits, function_t on_complete);
```

###2. 函数功能
获取指定引脚上的模拟信号，并在获取完成后自动调用指定函数处理模拟量。  

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*pin*|uint8_t|pin引脚号
*bits*|uint8_t|指定采集的精度
*on_complete*|function_t|模拟信号采集完毕后需要执行的函数(函数指针)
*返回值*  | 无    | 无

####关于 *pin* 的描述
可用于ADC的引脚号：
`1/2/3/4/5/26/27`

####关于 *bits* 的描述
*bits* 的数值|采样精度
:-------- | :------
8| 0-255
9|	0-511
10|	0-1023 

####关于 *on_complete* 参数
该参数是一个函数指针，是用户预定义用于处理模拟量的函数：
  
```
	typedef void (*function_t)(void* args);
```


###4. 函数举例
```	
	//用户自定义回调函数
	void on_adc_complete(void* args)
	{
		adc_result_t *result = (adc_result_t*)args; uint8_t buffer[2];
		//这里的result就是最终采集的到的数据
		...
		buffer[0] = result->value >> 8; buffer[1] = result->value & 0xff;
		ble_device_send(buffer, 2);
	}
	
	//当前函数
	void foo()
	{
		...
		//调用本函数向系统注册一个当ADC采集完成后会被执行的任务
		adc_measure(4, 8, on_adc_complete);
	
		...
	}
```


***
## 获取芯片温度
###1. 函数声明
```
int8_t get_temperature(void);
```

###2. 函数功能
获取当前芯片(nRF51822)内的温度。  
获取到的温度是利用芯片内部的温度传感器实现的，因此只能反映芯片内部的温度。如果需要测量外部温度，应使用相应的环境温度传感器。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  |int8_t| 采集到的温度

###4. 函数举例
```	
	//当前函数
	void foo()
	{
		...
		uint8_t temp;
		
		temp = get_temperature();
		...
	}
```

***
## 测量主芯片电压
###1. 函数声明
```
void vcc_measure(function_t on_complete);
```

###2. 函数功能
测量主芯片的电压。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*on_complete*|function_t|模拟信号采集完毕后需要执行的函数(函数指针)
*返回值*  | 无    | 无

####关于 *on_complete* 参数
该参数是一个函数指针，是用户预定义用于处理模拟量的函数：
  
```
	typedef void (*function_t)(void* args);
```

###4. 函数举例
```	
	//用户自定义回调函数
	void on_adc_complete(void* args)
	{
		adc_result_t *result = (adc_result_t*)args; uint8_t buffer[2];
		//这里的result就是最终采集的到的数据
		
		...
		buffer[0] = result->value >> 8; buffer[1] = result->value & 0xff;
		ble_device_send(buffer, 2);
	}
	
	//当前函数
	void foo()
	{
		...
	
		//调用本函数向系统注册内部的电压采集完成后会被执行的任务
		vcc_measure(on_adc_complete);
	
		...
	}
```







