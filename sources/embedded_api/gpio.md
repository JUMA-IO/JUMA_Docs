本说明用于描述通用扩展口(GPIO)的操作方式。

> 本系列API目前仅支持nRF51平台。

对于nRF51平台的用户，使用时请引用相应的头文件：

* API头文件：[juma_sdk_api.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_api.h)
* 数据类型或宏定义参见：[juma_sdk_types.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_types.h)


***
## GPIO配置
###1. 函数声明
```
void gpio_setup (uint8_t pin, uint8_t mode);
```


###2. 函数功能
用于对一个 GPIO 引脚的工作模式进行配置。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*pin*    | uint8_t  | 引脚号
*mode*   | uint8_t  | 引脚的工作模式
*返回值*  | void      | 无


####关于 *mode* 参数
`mode`参数可以接受的数值范围：

*mode* 的数值|说明
:-------- | :------
GPIO_OUTPUT | 将GPIO配置成输出模式
GPIO_INPUT_NOPULL|将GPIO配置为输入悬空模式
GPIO_INPUT_PULLUP|将GPIO配置为输入上拉模式 
GPIO_INPUT_PULLDOWN|将GPIO配置为输入下拉模式


###4. 函数举例
```
	void foo( void )
	{
		...
		/*将偏移量为18的引脚，配置成GPIO_OUTPUT的工作模式*/
		gpio_setup(18, GPIO_OUTPUT);
		...
	}

```


***
## 读取GPIO
###1. 函数声明
```
uint8_t gpio_read (uint8_t pin);
```


###2. 函数功能
读取指定GPIO引脚上的状态。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*pin*    | uint8_t  | 引脚号
*返回值*  | uint8_t     | 当前引脚上的状态


####关于 *返回值* 

*返回值* 的数值|说明
:-------- | :------
非0 | 该GPIO引脚为高电平
0|该GPIO引脚为低电平

###4. 函数举例
```
	void foo( void )
	{
		...
		
		uint8_t statue;
		
		/*将偏移量为18的引脚配置成高电平*/
		statue = gpio_setup(18);
		
		//这里的statue就是新的状态
		
		...
	}
```



***
## 写入GPIO
###1. 函数声明
```
void gpio_write (uint8_t pin, uint8_t state);
```


###2. 函数功能
设置GPIO引脚上的状态，将其设置为高电平或者低电平。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*pin*    | uint8_t  | 引脚号
*state*   | uint8_t  | 引脚的状态
*返回值*  | void      | 无

####关于 *state* 参数

*state* 的数值|说明
:-------- | :------
非0 | GPIO输出高电平
0|GPIO输出低电平

###4. 函数举例
```
	void foo( void )
	{
		...
		
		/*将偏移量为18的引脚配置成高电平*/
		gpio_setup(18, 1);
		
		...
	}
```	


***
## GPIO状态监视
###1. 函数声明
```
void gpio_watch (uint8_t pin, uint8_t change_direction);
```


###2. 函数功能
向系统注册一个 GPIO 事件，当引脚状态变化满足指定条件时，系统会调用事件，即`gpio_on_change `的回调函数。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*pin*    | uint8_t  | 引脚号
*change_direction*|uint8_t|引脚的状态变化
*返回值*  | 无    | 无

###关于 *change_direction* 的描述
`change_direction `的取值如下：

change_direction 的数值|说明
:-------- | :------
GPIO_WATCH_LEVEL_LOW | 低电平
GPIO_WATCH_LEVEL_HIGHT| 高电平

***
## 解除GPIO状态监视
###1. 函数声明
```
void gpio_unwatch(uint8_t pin);
```


###2. 函数功能
解除已经注册的GPIO状态变化监视事件。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*pin*    | uint8_t  | 引脚号
*返回值*  | 无    | 无

***
## GIPO状态变化事件
###1. 函数声明
```
void gpio_on_change(uint32_t pins_state);
```


###2. 函数功能
在GPIO的引脚状态被改变后由系统触发该事件。    
该函数内部用于处理 GIPO 状态变化后的任务。  
> 由于监视的是GPIO的电平状态，而非触发边缘，如果同时监视多个引脚，应在内部加入辅助判断，来获知引脚的真实状态。

###3. 函数参数

参数 |数据类型 |说明
:-------- | :------ | :------
*pins_state*|uint8_t|当前引脚的状态
*返回值*  | void      | 无


###4. 函数举例
```
	//回调函数
	void gpio_on_change(uint32_t pins_state);
	{
		...
		
		if(new_state == 0)
		{
			red_led_blink();	//伪指令
		}
		else
		{
			green_led_blink();	//伪指令
		}
		
		...
	}
	
	//当前函数
	void foo()
	{
		...
		
		//设置要检测的引脚，当指定的状态到达后会自动调用上面的回调函数
		//当18脚变为低电平时，上述的回调函数会被调用
		gpio_watch(18, GPIO_WATCH_LEVEL_LOW);		
		...
	}

```




