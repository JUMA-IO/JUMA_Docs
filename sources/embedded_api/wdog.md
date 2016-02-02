SDK中提供看门狗的API，通过该系列API可以迅速方便的配置看门狗模块。

> 本系列API目前仅支持nRF51平台。

对于nRF51平台的用户，使用时请引用相应的头文件：

* API头文件：[juma_sdk_api.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_api.h)
* 数据类型或宏定义参见：[juma_sdk_types.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_types.h)

***
## 配置看门狗
###1. 函数声明
```
uint32_t watchDog_Config(uint32_t juma_wdt_en);
```

###2. 函数功能
配置看门狗，将某一个看门狗使能。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*juma_wdt_en*|uint32_t|使能相应的看门狗
*返回值*  | 无    | 无

####关于 *juma_wdt_en* 的描述
`juma_wdt_en`的取值范围如下:

```
#define JUMA_WDT_EN_USER_DOG1   0x01
#define JUMA_WDT_EN_USER_DOG2   0x02
#define JUMA_WDT_EN_USER_DOG3   0x04
#define JUMA_WDT_EN_USER_DOG4   0x08
              
#define JUMA_WDT_EN_SYS_SDK_DOG 0x80
```

其中，`JUMA_WDT_EN_USER_DOG1` ~ `JUMA_WDT_EN_USER_DOG4` 是提供给用户使用的，使能之后，需要用对应的喂狗函数来进行喂狗，否则会产生看门狗溢出事件。没有使能的看门狗不需要喂狗。

`JUMA_WDT_EN_SYS_SDK_DOG`对应的是SDK部分，如果使能了该看门狗，那么当SDK内部发生错误的时候，也会像用户的看门狗一样产生溢出事件。

>`JUMA_WDT_EN_SYS_SDK_DOG`看门狗不需要用户执行喂狗操作，由SDK内部完成喂狗操作。



***
## 看门狗开始工作
###1. 函数声明
```
uint32_t watchDog_Start(uint32_t juma_wdt_timer_out_value);
```

###2. 函数功能
让看门狗开始工作。  
如果需要停止开门狗，则将相应的看门狗使能位清除。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*juma_wdt_timer_out_value*|uint32_t|触发看门狗回调函数需要经过的周期数
*返回值*  | 无    | 无

####关于 *juma_wdt_timer_out_value* 的描述
关于 `juma_wdt_timer_out_value` 的取值，可参考：

```
timeout [s] = ( juma_wdt_timer_out_value + 1 ) / 32768
```


***
## 看门狗喂狗
###1. 函数声明
```
void watchDog_user_dogX_RR(void);
```

###2. 函数功能
将看门狗定时器内计数器的值清零，当计数器为满值时，会触发溢出事件。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | 无    | 无



***
## 看门狗溢出事件
###1. 函数声明
```
void watchDog_on_timerout(uint32_t juma_wdt_statue);
```

###2. 函数功能
当看门狗定时器内计数器的值满值时，触发该事件。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*juma_wdt_statue*|uint32_t|看门狗的状态
*返回值*  | 无    | 无

####关于 *juma_wdt_statue* 的描述
`juma_wdt_statue`的取值范围如下：

```
#define JUMA_WDT_USER_DOG1      0x01
#define JUMA_WDT_USER_DOG2      0x02  
#define JUMA_WDT_USER_DOG3      0x04
#define JUMA_WDT_USER_DOG4      0x08

#define JUMA_WDT_EN_SYS_SDK_DOG 0x80
```

对`juma_wdt_statue`做“&”操作，可得知是由于哪一个看门狗造成的溢出事件。  

###3. 函数举例

```	
	// 定时器时间到后的处理函数
	void watchDog_on_timerout(uint32_t juma_wdt_statue)
	{
		...
		NVIC_SystemReset();//伪指令，系统重启
		...
	}

	//当前函数
	void foo()
	{
		...
		//使能开门狗
		watchDog_Config(JUMA_WDT_EN_SYS_SDK_DOG | JUMA_WDT_EN_USER_DOG1);
		
		//看门狗开始运行
		//（计数参数的选取应该按照实际的使用情况选取，如果选的值太大，可能不能达到很好的效果，如果太小，可能造成太多溢出事件)	
		watchDog_Start(7000);
		
		//喂狗操作，由于使能了用户看门狗1所以需要执行喂狗操作，否则，系统将会回调void watchDog_on_timerout(uint32_t juma_wdt_statue);
		//JUMA_WDT_EN_SYS_SDK_DOG 使能的看门狗不需要用户执行喂狗操作，由JUMA SDK内部完成喂狗动作。
		watchDog_user_dog1_RR();
		...
	}
```