本系列API用于支持获取环境传感器的数值（温湿度计HTS221，气压计LPS25HB）。
应用于一些需要环境数据的场合，例如：智能家居系统，智慧农场，户外运动设备等。

API声明和数据结构说明见头文件：
* STM32平台：[env_sensor.h]( https://github.com/JUMA-IO/STM32_Platform/blob/master/system/juma/inc/env_sensor.h)


> 本系列API仅支持STM32平台。


***
## 数据结构
### API调用状态
`env_status_t `表明了API的调用状态，它是一个`unit8`类型，在`env_sensor.h`头文件中通过枚举预置了一些状态，摘取部分如下：

宏定义    | 数值  | 说明
:----- | :-------- | :------
env_status_ok | 0x00    | API调用成功
env_status_error | 0x01    | API调用失败

> 1. 其他宏定义请参阅：[env_sensor.h](https://github.com/JUMA-IO/STM32_Platform/blob/master/system/juma/inc/env_sensor.h)。  
> 2. 本宏定义只适用于STM32平台。




***
## 选择sensor的功能进行使能
###1. 函数声明
```
env_status_t env_sensor_select_features(env_sensor_selsection_t features);
```

###2. 函数功能
可选择温湿度、气压功能的单个或者组合使能。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
features| env_sensor_selsection_t |可选择TEMP_HUMI_ENABLE/AIR_PRESSURE_ENABLE<br/>/ALL_ENABLE。
*返回值*  | env_status_t    | [status](#_1)



***
## 初始化sensor开始产生数据
###1. 函数声明
```
env_status_t env_sensor_start(void); 
```

###2. 函数功能
根据配置sensor的特定功能进行初始化与数据输出使能。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | env_status_t    | [status](#_1)



***
## 获取环境温度数据
###1. 函数声明
```
env_status_t env_sensor_get_temperature(float* temperature); 
```

###2. 函数功能
获取环境温度。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
temperature| float* |单位为摄氏度，eg：26.66℃，温度范围-40-120℃。
*返回值*  | env_status_t    | [status](#_1)



***
## 获取环境湿度数据
###1. 函数声明
```
env_status_t env_sensor_get_humidity(float* humidity);
```

###2. 函数功能
获取环境温度。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
humidity| float* |单位为相对湿度rH，eg：26.66%rH，湿度范围0-100%rH。
*返回值*  | env_status_t    | [status](#_1)



***
## 同时获取环境温度与湿度数据
###1. 函数声明
```
env_status_t env_sensor_get_temperature_humidity(float* temperature, float* humidity);
```

###2. 函数功能
同时获取环境温度与湿度数据。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
temperature| float* |单位为摄氏度，eg：26.66℃，温度范围-40-120℃。
humidity| float* |单位为相对湿度rH，eg：26.66%rH，湿度范围0-100%rH。
*返回值*  | env_status_t    | [status](#_1)



***
## 获取环境大气压数据
###1. 函数声明
```
env_status_t env_sensor_get_air_pressure(float* air_pressure);
```

###2. 函数功能
获取环境温度。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
air_pressure| float* |单位为绝对大气压hPa，eg：1000.00hPa，绝对大气压范围260-1260hPa。
*返回值*  | env_status_t    | [status](#_1)



***