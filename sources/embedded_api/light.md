本文档用于描述调光灯API的操作说明，API内部采用PWM信号来实现光的调节，支持RGB三种颜色。

> 本系列API目前仅支持nRF51平台。

对于nRF51平台的用户，使用时请引用相应的头文件：

* API头文件：[juma_sdk_api.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_api.h)
* 数据类型或宏定义参见：[juma_sdk_types.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_types.h)


***
## 调光灯设置
###1. 函数声明
```
void light_setup(uint8_t* pins, uint8_t is_active_high);
```


###2. 函数功能
配置调光灯。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*pins*|uint8_t * |用于控制调光灯的引脚
*is_active_high*|uint8_t|驱动调光灯变亮的电平
*返回值*  | 无    | 无

####关于 *pins* 的描述
用于指定与调光灯相连的引脚，在`juma_sdk_types.h`中有如下定义：

```
	typedef struct _light_config_t {
	
    	uint8_t pins[4]; // pins for RGBW
    	
	} light_config_t;
```

####关于 *is_active_high* 的描述
用于指定调光灯的电平，即，高电平有效亦或是低电平有效。

is_active_high的值| 对应的应用场合
:-----| :------
*0*|低电平驱动调光灯
*非零*|高电平驱动调光灯

###4. 函数举例

```	
	//当前函数
	void foo()
	{
		...
	
		light_config_t light_init_config;
		
		light_init_config.pins[0] = 21;		//R
		light_init_config.pins[1] = 22;		//G
		light_init_config.pins[2] = 23;		//B
	
		//配置调光灯 其驱动引脚为21,22,23 其驱动电平为低有效
		light_setup((uint8_t *)&light_init_config, 0);
	
		...
	}
```

***
## 打开调光灯
###1. 函数声明
```
void light_on(void);
```


###2. 函数功能
在配置好调光灯之后，调光灯并不会直接打开，需要调用本函数，才会打开调光灯。  
打开后默认为白色，即三种颜色全部点亮，建议在打开之前首先设置好要显示的颜色。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | 无    | 无



***
## 关闭调光灯
###1. 函数声明
```
void light_off(void);
```


###2. 函数功能
关闭调光灯。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | 无    | 无


***
## 调节颜色
###1. 函数声明
```
void light_set_color(const uint8_t* rgb_values);
```


###2. 函数功能
调节灯光颜色，有RGB三个通道的颜色可以设置。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*rgb_values*|const uint8_t * |颜色值数组
*返回值*  | 无    | 无

####关于 *rgb_values* 的描述
RGB的颜色设定，其通过一个数组一次性设定颜色值：

数组编号|对应的颜色值
:----|:----
rgb_values[0] | 红光数值，范围为[0~255]
rgb_values[1] | 绿光数值，范围为[0~255]
rgb_values[2] | 蓝光数值，范围为[0~255]



###4. 函数举例
```	
	//当前函数
	void foo()
	{
		...
		
		//配置调光灯 其驱动引脚为21,22,23 其驱动电平为低有效
		light_setup((uint8_t *)&light_init_config, 0);
	
		...
	
		//打开调光灯
		light_on();
		
		...
		//调节灯光颜色		
		uint8_t color[4] = {0, 100, 0, 0};
		light_set_color(color);		

		...
				
		//关闭调光灯
		light_off();
		
		...
	}
```






