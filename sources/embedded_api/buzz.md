本文档用于说明蜂蜜器的使用。

> 本系列API目前仅支持nRF51平台。

对于nRF51平台的用户，使用时请引用相应的头文件：

* API头文件：[juma_sdk_api.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_api.h)
* 数据类型或宏定义参见：[juma_sdk_types.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_types.h)


***
## 蜂蜜器发声
###1. 函数声明
```
void play_sound(uint8_t pin);
```

###2. 函数功能
让蜂蜜器响一下。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*pin*|uint8_t|与蜂鸣器连接的引脚
*返回值*  | 无    | 无

###4. 函数举例
```
	//当前函数
	void foo()
	{
		play_sound(8);
	}
```






