本文用以说明SDK中BLE扫描(Scan)API的操作方式。

SDK为开源项目，本文涉及的代码参见：

* `ScanHelper`：[ScanHelper.java](https://github.com/JUMA-IO/BLE_SDK_Android/blob/master/src/com/juma/sdk/ScanHelper.java)
* `JumaDevice `：[JumaDevice.java](https://github.com/JUMA-IO/BLE_SDK_Android/blob/master/src/com/juma/sdk/JumaDevice.java)

BLE扫描(Scan)通过ScanHelper类来实现，用户可以在自己的应用程序中对其实例化，并调用相关的如`startScan`、`stopScan`等API进行蓝牙扫描。蓝牙扫描的结果以`ScanCallback`回调接口通知应用程序。

> 以下API属于`ScanHelper`类成员。

***
## 数据结构
### 蓝牙设备的表示
SDK中通过`JumaDevice`的类来表示一个蓝牙设备，其中包含了该设备的蓝牙地址、服务、特征值等，详情可以查看[源码](https://github.com/JUMA-IO/BLE_SDK_Android/blob/master/src/com/juma/sdk/JumaDevice.java)。

如果SDK扫描到一个蓝牙设备，便会构建一个`JumaDevice`的对象，返回给应用程序。


***
## 启用蓝牙
###1. 函数声明
```
public boolen enable();
```

###2. 函数功能
开启手机的蓝牙功能。  
只有在开启手机蓝牙后，SDK才能进行有效的蓝牙操作。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
返回值  | boolean      | `true` - 启用蓝牙成功；`false` - 启用蓝牙失败


***
## 禁用蓝牙
###1. 函数声明
```
public boolen disable();
```

###2. 函数功能
禁用手机蓝牙。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
返回值  | boolean      | `true` - 禁用蓝牙成功；`false` - 禁用蓝牙失败

***
## 获取蓝牙状态
###1. 函数声明
```
public boolen isEnabled(); 
```

###2. 函数功能
获取蓝牙启用状态。 

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
返回值  | boolean      | `true` - 蓝牙已启用；`false` - 蓝牙已禁用

***
## 获取版本号
###1. 函数声明
```
public boolen getVersion();
```

###2. 函数功能
获取SDK的版本号。  

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
返回值  | String      | SDK版本号[TBD]

***
## 开始扫描
###1. 函数声明
```
public void startScan(String name);  
```

###2. 函数功能
手机开始蓝牙扫描。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
name    | String  | 设备名称，可为null
返回值  | boolean      | `true` - 开启扫描成功；`false` - 开始扫描失败

####关于 *name* 的描述
`name` 用来指定需要扫描的设备，如果传入了名字，将只扫描该名字的设备。如果传入的是空值，则扫描附近所有的设备。

***
## 停止扫描
###1. 函数声明
```
public void stopScan();      
```

###2. 函数功能
手机停止蓝牙扫描。 

###3. 函数参数
无。

***
## 获取扫描状态
###1. 函数声明
```
public boolen isScanning();
```

###2. 函数功能
查询当前是否正在蓝牙扫描。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
返回值  | boolean      | `true` - 正在扫描；`false` - 没有在扫描

***
## 扫描到蓝牙设备
本API属于`ScanCallback`。

###1. 函数声明
```
public void onDiscover(JumaDevice device, int rssi);
```

###2. 函数功能
当扫描到蓝牙设备时，会触发此回调。  

###3. 函数参数
参数	     | 数据类型  | 说明
:-----   | :----    | :------
device   | JumaDevice      | 扫描到的[蓝牙设备](#_1) 
rssi | int      | 扫描到的蓝牙设备的信号强度


***
## 扫描到蓝牙设备
本API属于`ScanCallback`。

###1. 函数声明
```
public void onConnectionStateChange(int status, int newState);
```

###2. 函数功能
扫描状态（开启扫描/停止扫描）发生改变时，会触发此回调。    

###3. 函数参数
参数	     | 数据类型  | 说明
:-----   | :----    | :------
newState | int      | 返回新的扫描状态，`STATE_START_SCAN` 或 `STATE_STOP_SCAN`


