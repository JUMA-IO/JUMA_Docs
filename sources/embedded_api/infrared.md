SDK中为用户提供了一个简易的红外发送功能，通过外接红外发射管，可以实现对常见家电的红外遥控。

> 本系列API目前仅支持nRF51平台。

对于nRF51平台的用户，使用时请引用相应的头文件：

* API头文件：[juma_sdk_api.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_api.h)
* 数据类型或宏定义参见：[juma_sdk_types.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_types.h)


***
## 红外配置
###1. 函数声明
```
uint32_t ble_infrared_config(ble_infrared_init_struct * Init_struct);

```

###2. 函数功能
红外发射的参数配置。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*Init_struct*|ble_infrared_init_struct * |红外配置的结构体
*返回值*  | 无    | 无

####关于 *Init_struct* 的描述

`Init_struct `参数是要用来对红外发送设备进行配置的全部内容，其完整定义参见`juma_sdk_types.h`，这里罗列部分重要参数如下：

```
typedef struct ble_infrared_pwm_config_struct_
{
	uint8_t infrared_signal_pin;          //pin num for infrared signal pin
	uint8_t infrared_shock_pin;           //pin num for shock signal pin
	
	//carrier
	uint16_t carrier_period;              //the time value of carrier period
	uint16_t carrier_plus;	              //the time value of carrier hight time
                
	//guidance              
	uint16_t guidance_burst;              //the carrier period number of guidance burst
	uint16_t guidance_space;              //the carrier period number of guidance space
                
	uint16_t logic_0_burst;	              //the carrier period number of logic 0 sending burst
	uint16_t logic_0_space;	              //the carrier period number of logic 0 sending space
                
	//logic 1             
	uint16_t logic_1_burst;	              //the carrier period number of logic 1 sending burst
	uint16_t logic_1_space;	              //the carrier period number of logic 1 sending space
	
	//end
	uint16_t end_burst;                   //the carrier period number of end gap burst 
	
}ble_infrared_pwm_config_struct;

```

###4. 函数举例

```	
	void foo()
	{
		...
		ble_infrared_init_struct init_struct;
		
		init_struct.Protocal_type = PWM_HEAD_END;
		
		init_struct.type_data.pwm.infrared_shock_pin = 6;
		init_struct.type_data.pwm.infrared_signal_pin = 5;
		
		init_struct.type_data.pwm.carrier_period = 214;
		init_struct.type_data.pwm.carrier_plus   = 150;
		
		init_struct.type_data.pwm.guidance_burst = 342;
		init_struct.type_data.pwm.guidance_space = 170;
		
		init_struct.type_data.pwm.logic_0_burst  = 21;
		init_struct.type_data.pwm.logic_0_space  = 21;
		
		init_struct.type_data.pwm.logic_1_burst  = 21;
		init_struct.type_data.pwm.logic_1_space  = 62;
		        
		init_struct.type_data.pwm.end_burst      = 21;
		
		ble_infrared_config(&init_struct);

		
		ble_infrared_send("MSB byte", 32);
		...
	}
```




***
## 红外发射
###1. 函数声明
```
uint32_t ble_infrared_send(const uint8_t * signal_data, uint8_t lenth);
```

###2. 函数功能
将数据通过红外信号发射出去。  



###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*signal_data*|const uint8_t * |红外发射的数据
*lenth*|uint8_t |红外数据的长度（bit）
*返回值*  | 无    | 无

> 红外发送过程中是按位发送的，这里的signal_data采用的是MSB的格式。

###4. 函数举例

```	
	//当前函数
	void foo()
	{
		...

		//发射红外数据
		uint8_t data[] = {0x55, 0xAA, 0xFF, 0x00};
		ble_infrared_send(data, 32);
		...
	}
```



***
## 获取红外状态
###1. 函数声明
```
uint32_t ble_infrared_get_statue(void);
```

###2. 函数功能

由于红外发射过程需要的时间比较长，因此采用了异步编程的方案。通过本函数来获知当前红外发送的状态。只有当红外发射处于闲置状态下，可以再次与红外发射有关的操作。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | uint32_t    | 当前红外发送机的状态，`0x0`或者`0x01`表示空闲状态，其他为繁忙状态。

###4. 函数举例

```	
	//当前函数
	void foo()
	{
		...
		//使用方法类似下面这个
		uint8_t data1[] = {0x55, 0xAA, 0xFF, 0x00};
		uint8_t data2[] = {0x55, 0xAA, 0x00, 0xFF};		
		ble_infrared_send(data1, 32);
		
		uint8_t inf_statue = ble_infrared_get_statue();
		while(inf_statue != 0 && inf_statue != 1)
		{
			inf_statue = ble_infrared_get_statue();
		}
		
		ble_infrared_send(data2, 32);

		...
	}
```






