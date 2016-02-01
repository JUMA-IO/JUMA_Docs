JSensor是JUMA为整合传感器开发的一系列简明API，目前，它支持[Cannon开发板](http://www.juma.io/doc/zh/boards/st/cannon/Resource/cannon_resource.html)上的所有传感器，如温湿度计、气压计、六轴陀螺仪、磁力计等。

```
// Application API/callback
void jsensor_app_setSensors(void);
void jsensor_app_setSensor(uint16_t sid);
JSensor_Status jsensor_app_read_sensor(uint16_t sid, void *data);

```

其中，jsensor_app_setSensors是传感器设置/初始化入口，jsensor_app_setSensor是设置/初始化某一传感器，jsensor_app_read_sensor是读取传感器数据。传感器通过*sid*编号来指定。

使用时，请引用SDK中的[`juma_sensor.h`](https://github.com/JUMA-IO/STM32_Platform/blob/master/system/juma/inc/juma_sensor.h)的头文件。

SDK中的[SensorTag](https://github.com/JUMA-IO/STM32_Platform/tree/master/product/application/sensor_tag)例程揭示了本API的典型使用方法。

> 本API只适用于STM32平台。

***
## 数据结构
### 1. 传感器编号
```
#define JSENSOR_TYPE_HUMITY_TEMP		0x01
#define JSENSOR_TYPE_MOTION_6AXIS		0x02
#define JSENSOR_TYPE_PRESSURE			0x03
#define JSENSOR_TYPE_MAGNET				0x04
```

### 2. 函数返回值
```
enum _JSensor_Status {
	JSENSOR_OK = 0,
	JSENSOR_FAIL = 1,
};
```

### 3. 传感器数据结构
```
//温湿度传感器
struct _JSensor_HUM_TEMP_data {
	int16_t *humidity;
	int16_t *temperature;
};

//气压传感器
struct _JSensor_Press_data {
	int32_t *pressure;
};

//六轴加速度/陀螺仪传感器
struct _JSensor_AXIS_data {
	int8_t *ACC;
	int8_t *GRO;
};

//磁力计传感器
struct _JSensor_MAG_data{
	int8_t *MAG;
};

```

***
## 传感器设置入口函数
###1. 函数声明
```
void jsensor_app_setSensors(void);
```  

###2. 函数功能
在函数内部通过调用 `jsensor_app_setSensor` 来设置/初始化每个单独的传感器。  
该函数会在初始化阶段被系统自动调用。 

###3. 函数参数
无。

###4. 函数举例
```
void jsensor_app_setSensors(void)
{
    jsensor_app_setSensor(JSENSOR_TYPE_HUMITY_TEMP);
    jsensor_app_setSensor(JSENSOR_TYPE_PRESSURE);
    jsensor_app_setSensor(JSENSOR_TYPE_MOTION_6AXIS);
    jsensor_app_setSensor(JSENSOR_TYPE_MAGNET);
}
```


***
## 设置传感器
###1. 函数声明
```
void jsensor_app_setSensor(uint16_t sid);
```  

###2. 函数功能
通过该函数设置/初始化单个传感器，不同传感器通过sid(编号)来区分。  
应在`jsensor_app_setSensors` 中使用该函数。



###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*sid*    | uint16_t  | [传感器编号](#_1)
*返回值*  | void      | 无


> 传感器的芯片手册请参见[这里](http://www.juma.io/doc/zh/boards/st/cannon/Resource/cannon_resource.html)。

***
## 读取传感器数据
###1. 函数声明
```
JSensor_Status jsensor_app_read_sensor(uint16_t sid, void *data);
```  

###2. 函数功能
该函数用于读取传感器采集到的数据，通过 `sid` 指定传感器，通过 `data` 存放数据。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*sid*    | uint16_t  | 传感器[编号](#_1)
*data*  | void *     | 存放传感器采集到的[数据](#_1)
*返回值*  | JSensor_Status      | 指示函数调用[状态](#_1)

###4. 函数举例

```	
    // sensor read temperature & humidity
    {
        int16_t humidity;
        int16_t temperature;
        JSensor_HUM_TEMP_Typedef tdef;

        tdef.humidity = &humidity;
        tdef.temperature = &temperature;

        if (JSENSOR_OK == jsensor_app_read_sensor(JSENSOR_TYPE_HUMITY_TEMP, (void *)&tdef)) {
            ble_device_send(0x00, 2, (uint8_t *)&temperature);
            ble_device_send(0x01, 2, (uint8_t *)&humidity);
        }
    }
    // sensor read pressure
    {
        JSensor_Press_Typedef tdef;
        int32_t pressure;

        tdef.pressure = &pressure;

        if (JSENSOR_OK == jsensor_app_read_sensor(JSENSOR_TYPE_PRESSURE, (void *)&tdef)) {
            ble_device_send(0x02, 3, (uint8_t *)&pressure);
        }
    }
    // sensor read magenetometer
    {
        JSensor_MAG_Typedef tdef;
        int8_t  MAG[6];
        tdef.MAG = MAG;

        if(JSENSOR_OK == jsensor_app_read_sensor(JSENSOR_TYPE_MAGNET, (void *)&tdef)) {
            ble_device_send(0x03, 6, (uint8_t*)MAG);
            //printf("%x,%x,%x,%x,%x,%x\n\r", MAG[0],MAG[1],MAG[2],MAG[3],MAG[4],MAG[5]);
        }
    }
    // sensor read motion 6 AXIS
    {
        JSensor_AXIS_Typedef tdef;
        int8_t ACC[6], GRO[6];
        tdef.ACC = ACC;
        tdef.GRO = GRO;

        if (JSENSOR_OK == jsensor_app_read_sensor(JSENSOR_TYPE_MOTION_6AXIS, (void *)&tdef)) {
            ble_device_send(0x04, 6, (uint8_t*)ACC);
            ble_device_send(0x05, 6, (uint8_t*)GRO);
        }
    }

```




