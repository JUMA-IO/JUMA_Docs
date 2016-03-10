SDK 中将对 iOS BLE 设备的操作封装成设备类(Device)，包括发送数据、接收数据、固件升级等操作。

SDK 为开源项目，本文涉及的代码参见：

* `BLE_SDK_iOS`：[github](https://github.com/JUMA-IO/BLE_SDK_iOS)



***
##发送蓝牙数据
###1. 函数声明
```
- (void)writeData:(NSData *)data type:(UInt8)typeCode;
```	

###2. 函数功能
向已连接的蓝牙设备发送数据.  
如果蓝牙设备会返回数据, 结果会通过 JumaDeviceDelegate 中的 device:didUpdateData:error: 给出.

###3. 函数参数
参数          | 类型                 | 说明
:-----       | :--------           | :------ 
*data*       | NSData *            | 要写入的数据
*typeCode*   | UInt8               | 要写入的数据的类型
*返回值*      | void                | 无

####关于 *data* 参数
*data* 不能为 nil，data 的长度必须小于 199.

####关于 *typeCode* 参数
*typeCode* 必须小于 128.


***
##发送蓝牙数据事件
###1. 函数声明
```
- (void)device:(JumaDevice *)device didWriteData:(NSError *)error;
```	

###2. 函数功能
向蓝牙设备发送数据后, 这个方法会被调用, 以此表示数据发送是否成功.


###3. 函数参数
参数              | 类型                   | 说明
:-----            | :--------             | :------ 
*device*          | JumaDevice * | 给出这个信息的对象
*error*           | NSError *    | 错误描述 
*返回值*           | void         | 无

###4. 函数举例
```
- (void)device:(JumaDevice *)device didWriteData:(NSError *)error {    
    if (error) {      
        NSLog(@"did fail to send data to %@, error : %@", device.name, error); 
    }
    else {        
        NSLog(@"did send data to %@", device.name);
    }    
}
```


***
##接收蓝牙数据事件
###1. 函数声明
```
- (void)device:(JumaDevice *)device didUpdateData:(NSData *)data type:(const char)typeCode error:(NSError *)error;
```	
	
###2. 函数功能
接收到蓝牙设备的数据时, 这个方法会被调用, 并给出相应的结果.

###3. 函数参数

参数              | 类型                   | 说明
:-----            | :--------             | :------ 
*device*      | JumaDevice * | 给出这个信息的对象
*data*        | NSData *     | 从蓝牙设备接收到的数据
*typeCode*    | const char   | 从蓝牙设备接收到的数据的类型
*error*       | NSError *    | 错误描述 
*返回值*       | void         | 无

####关于 *data* 参数 
如果接收数据时发生错误，*data* 为 nil.

####关于 *typeCode* 参数 
如果接收数据时发生错误，*typeCode* 为 -1.

###4. 函数举例
```
- (void)device:(JumaDevice *)device didUpdateData:(NSData *)data type:(const char)typeCode error:(NSError *)error {
    if (!error) {
        NSLog(@"did receive data %@ from %@, type code: %d", data, accessory.name, typeCode);
    }
    else {
        NSLog(@"did fail to  receive data from %@, error: %@", accessory.name, error);
    }
}
```


***
##读取设备信号强度
###1. 函数声明
```
typedef void (^JumaReadRssiBlock)(NSNumber *RSSI, NSError *error);
- (void)readRSSI;
- (void)readRSSI:(JumaReadRssiBlock)handler;
```	

###2. 函数功能
在 JumaDevice 已经和 JumaManager 连接的情况下, 读取连接的信号强度.   
 
调用 readRSSI  读取信号强度时, 结果会通过 JumaDeviceDelegate 的 device:didReadRSSI:error: 给出.  
调用 readRSSI: 读取信号强度时, 如果 handler 参数不为 nil, 结果会通过 hander 给出, 否则会通过 JumaDeviceDelegate 的 device:didReadRSSI:error: 给出.


###3. 函数参数
参数            | 类型                | 说明
:-----         | :--------           | :------  
*handler*      | JumaReadRssiBlock   | 读取结束时被调用的 block
*返回值*        | void                | 无

***
##读取设备信号强度事件
###1. 函数声明
```
- (void)device:(JumaDevice *)device didReadRSSI:(NSNumber *)RSSI error:(NSError *)error;
```	

###2. 函数功能
读取信号强度操作结束时, 这个方法会被调用, 并给出相应结果.

###3. 函数参数
参数              | 类型                   | 说明
:-----            | :--------             | :------ 
*device*          | JumaDevice * | 给出这个信息的对象
*RSSI*            | NSNumber *   | 读到的信号强度
*error*           | NSError *    | 错误描述 
*返回值*           | void         | 无

###4. 函数举例
```
- (void)device:(JumaDevice *)device didReadRSSI:(NSNumber *)RSSI error:(NSError *)error {    
    if (error) {      
        NSLog(@"can not read RSSI, error : %@", error); 
    }
    else {        
        NSLog(@"RSSI = %@", RSSI);
    }    
}
```


***
##设置设备固件更新(OTA)模式

###1. 函数声明
```
- (void)setOtaMode;
```	

###2. 函数功能
向蓝牙设备发送某些特定的数据, 使蓝牙设备在升级模式下工作.
在升级模式下, 可以通过无线传输的方式升级蓝牙设备的固件.


###3. 函数参数
参数            | 类型                | 说明
:-----         | :--------           | :------  
*返回值*        | void                | 无

***
##更新设备固件

###1. 函数声明
```
typedef void (^JumaUpdateFirmwareBlock)(NSError *error);
- (void)updateFirmware:(NSData *)firmwareData;
- (void)updateFirmware:(NSData *)firmwareData completionHandler:(JumaUpdateFirmwareBlock)handler;
```	
	
###2. 函数功能
更新蓝牙设备的固件.  

调用 updateFirmware: 进行更新操作时, 结果会通过 JumaDeviceDelegate 中的 device:didUpdateFirmware: 给出.  
调用 updateFirmware:completionHandler: 进行更新操作时, 如果 handler 参数不为 nil, 结果会通过 handler 给出, 否则会通过 JumaDeviceDelegate 中的 device:didUpdateFirmware: 给出.


###3. 函数参数

参数             | 类型                 | 说明
:-----          | :--------           | :------ 
*firmwareData*  | NSData *            | 要升级的固件数据
*handler*       | JumaUpdateFirmwareBlock   | 升级操作结束时被调用的 block
*返回值*         | void                | 无

***
####关于 *firmwareData* 参数
*firmwareData* 不能为 nil.


***
##更新设备固件事件

###1. 函数声明
```
- (void)device:(JumaDevice *)device didUpdateFirmware:(NSError *)error;
```	
	
###2. 函数功能
更新蓝牙设备的固件结束后, 这个方法会被调用, 并给出相应结果.


###3. 函数参数
参数              | 类型           | 说明
:-----            | :--------    | :------ 
*device*          | JumaDevice * | 给出这个信息的对象
*error*           | NSError *    | 错误描述 
*返回值*           | void         | 无


####关于 *error* 参数 
error 为 nil 说明升级成功，否则升级失败.

###4. 函数举例
```
- (void)device:(JumaDevice *)device didUpdateFirmware:(NSError *)error {
    if (!error) {
        NSLog(@"did update the firmware of %@", device.name);
    }
    else {
        NSLog(@"did fail to update the firmware, error: %@", error);
    }
}
```







