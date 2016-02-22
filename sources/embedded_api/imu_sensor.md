本系列API用于支持以流模式获取9轴（6轴陀螺仪LSM303AGR+3轴磁力计LSM6DS3）的原始数据。
应用于一些需要对运动数据变化监视或控制的场景，例如：四周飞行器，自平衡小车。

API声明和数据结构说明见头文件：  
STM32平台的 [imu_sensor.h]( https://github.com/JUMA-IO/STM32_Platform/blob/master/system/juma/inc/imu_sensor.h)

其中，IMU是Inertial measurement unit的简称，即惯性测量单元。

IMU传感器API的使用例程请参见 ： [sensor_tag_fifo](https://github.com/JUMA-IO/STM32_Platform/blob/master/product/application/sensor_tag_fifo/app.c)

> 本系列API仅支持STM32平台。


***
## 数据结构
### API调用状态
`imu_status_t `表明了API的调用状态，它是一个`unit8`类型，在`imu_sensor.h`头文件中通过枚举预置了一些状态，摘取部分如下：

宏定义    | 数值  | 说明
:----- | :-------- | :------
imu_status_ok | 0x00    | API调用成功
imu_status_fail | 0x01    | API调用失败

> 1. 其他宏定义请参阅：[imu_sensor.h](https://github.com/JUMA-IO/STM32_Platform/blob/master/system/juma/inc/imu_sensor.h)。  
> 2. 本宏定义只适用于STM32平台。




***
## 初始化IMU传感器
###1. 函数声明
```
imu_status_t imu_sensor_reset(void);
```

###2. 函数功能
重新初始化传感器，包括LSM303AGR和LSM6DS3。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | imu_status_t    | [status](#_1)



***
## 使能IMU传感器
###1. 函数声明
```
imu_status_t imu_sensor_select_features(sensor_selsection_t features);
```

###2. 函数功能
可选择加速度计，陀螺仪，磁力计的单个或者组合使能。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
features| sensor_selsection_t |可选择`ACC_ENABLE`/`GYRO_ENABLE`/`MAG_ENABLE`/`ACC_AND_GYRO_ENABLE`<br/>/`ALL_ENABLE`。
*返回值*  | imu_status_t    | [status](#_1)



***
## 设置采样率
###1. 函数声明
```
imu_status_t imu_sensor_set_data_rate(uint32_t* p_data_rate, uint8_t mode)；
```

###2. 函数功能
设置加速度计，陀螺仪，磁力计的采样率。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*p_data_rate*| uint32_t * |FIFO采样率（0/10/25/50/100/200/400/800/1600Hz）,磁力计不采用FIFO mode采样率固定为100Hz。
*mode*| uint8_t |采集数据的模式(`FIFO MODE`/`CONTINUOUS_THEN_FIFO`/`BYPASS_THEN_CONTINUOUSE`/<br/>`CONTINUOUS_OVERWRITE MODE`)
*返回值*  | imu_status_t    | [status](#_1)



***
## 开始产生数据
###1. 函数声明
```
imu_status_t imu_sensor_start(void); 
```

###2. 函数功能
根据sensor的功能配置进行数据输出使能。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | imu_status_t    | [status](#_1)



***
## 停止产生数据
###1. 函数声明
```
imu_status_t imu_sensor_stop(void); 
```

###2. 函数功能
配置sensor的特定功能停止数据输出。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | imu_status_t    | [status](#_1)



***
## 数据回调事件
###1. 函数声明
```
void on_imu_sensor_data(imu_sensor_data_t* data); 
```

###2. 函数功能
从9轴读取数据后提供给用户的callback。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*data*  | imu_sensor_data_t*    | 9轴数据（加速度+陀螺仪+磁力计）
*返回值*  | imu_status_t    | [status](#_1)



