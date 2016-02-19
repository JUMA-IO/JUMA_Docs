SDK 中将对 iOS BLE 的操作封装成管理类(Manager)，包括扫描、建立连接、断开连接等操作。

SDK 为开源项目，本文涉及的代码参见：

* `BLE_SDK_iOS`：[github](https://github.com/JUMA-IO/BLE_SDK_iOS)


***
##数据结构
###常量: JumaManagerOptionRestoreIdentifierKey  
```
FOUNDATION_EXPORT NSString * const JumaManagerOptionRestoreIdentifierKey NS_AVAILABLE(NA, 7_0);
```

在初始化 JumaManager 时的可选参数之一, 该参数对应的值的类型是 NSString *.  

如果使用了该参数, 那么 app 就可以支持"蓝牙状态保存和恢复"功能, 即当 app 被系统杀死时, 系统会保存那一刻蓝牙连接的状态，如果蓝牙连接的状态发生了变化, 系统会在后台唤醒 app, 并给出状态更新的回调.  

通常用于以下两种情况:  
1. app 被系统杀死时, 手机已经连接了蓝牙设备, 但是在 app 被系统杀死后, 连接断开了. 这时系统就会唤醒 app, 并给出连接断开的回调.  
2. app 被系统杀死时, 手机正在连接蓝牙设备, 在 app 被系统杀死后, 连接成功. 这时系统就会唤醒 app, 并给出连接成功的回调.

该功能的实现, 需要先为 app 指定“Uses Bluetooth LE accessories”类型的后台模式.

该功能有以下限制：  
1.每次初始化 JumaManager 时, 该参数的值必须保持一致.  
2.只支持 app 被系统杀死这一种情况.  
3.app 被系统杀死后, 手机不可关机或重启, 手机的蓝牙必须一直打开.  
4.在 app 被杀死的时候, 手机必须正在连接蓝牙设备, 或者已经连接蓝牙设备.

###常量: JumaManagerOptionShowPowerAlertKey  

```
FOUNDATION_EXPORT NSString * const JumaManagerOptionShowPowerAlertKey NS_AVAILABLE(NA, 7_0);
```

在初始化 JumaManager 时的可选参数之一, 该参数对应的值的类型是 NSNumber * (Boolean).  
如果为该参数指定的值为 @YES, 在初始化 JumaManager 时, 如果手机上的蓝牙处于关闭状态, 系统会向用户显示一个警告对话框.  

###常量: JumaManagerScanOptionAllowDuplicatesKey  
```
FOUNDATION_EXPORT NSString * const JumaManagerScanOptionAllowDuplicatesKey;  
```

该参数是启动扫描时的可选参数, 该参数对应的值的类型是 NSNumber * (Boolean).  

蓝牙设备会持续的发出信号来显示自己的存在, 手机每收到一次信号, JumaManagerDelegate 中的 manager:didDiscoverDevice:RSSI: 就可能会被调用一次.  
如果为该参数指定的值是 @YES, 那么在启动扫描之后, 手机每收到一次信号, manager:didDiscoverDevice:RSSI 就会被调用一次.  
如果为该参数指定的值是 @NO, 那么在启动扫描之后, 不管手机收到多少次信号, manager:didDiscoverDevice:RSSI 都只会被调用一次.


***
##初始化
###1. 函数声明
```
- (instancetype)initWithDelegate:(id<JumaManagerDelegate>)delegate queue:(dispatch_queue_t)queue options:(NSDictionary *)options  NS_AVAILABLE(NA, 7_0);
```

###2. 函数功能
初始化一个 JumaManager 类型的对象。  
初始化之后，JumaManagerDelegate 中的 managerDidUpdateState: 会第一个被调用，以此表明 JumaManager 的状态已经改变。


###3. 函数参数
参数          | 类型                           | 说明
:-----       | :--------                      | :------ 
*delegate*   | id<JumaManagerDelegate>        | 接收 JumaManager 的事件的对象 
*queue*      | dispatch_queue_t               | JumaManager 的事件将被发送到这个 queue 上
*options*    | NSDictionary *                 | 初始化 JumaManager 对象的时候的可选参数
*返回值*      | instancetype                   | 由这个方法生成的 JumaManager 类型的对象实例


####关于 *queue* 参数 
如果 queue 为 nil, 则 JumaManager 会使用 main queue.


####关于 *options* 参数 
可选参数，见 `JumaManagerOptionShowPowerAlertKey` 和 `JumaManagerOptionRestoreIdentifierKey`.


###4. 函数举例
```
NSDictionary *options = @{ JumaManagerOptionShowPowerAlertKey    : @YES,
                           JumaManagerOptionRestoreIdentifierKey : @"io.juma.restoreID" };
self.manager = [[JumaManager alloc] initWithDelegate:self queue:nil options:options];
```


***
##开始扫描
###1. 函数声明
``` 
- (void)scanForDeviceWithOptions:(NSDictionary *)options;
```	

###2. 函数功能
扫描周围的蓝牙设备.  

###3. 函数参数
参数            | 类型              | 说明
:-----         | :--------        | :------ 
*options*      | NSDictionary *   | 启动扫描时的参数(可选)
*返回值*        | void             | 无

####关于 *options* 参数
可选参数，见 `JumaManagerScanOptionAllowDuplicatesKey`.

###4. 函数举例
```
NSDictionary *options = @{ JumaManagerScanOptionAllowDuplicatesKey : @NO };
[self.manager scanForDeviceWithOptions:options];
```



***
##停止扫描
###1. 函数声明
```
- (void)stopScan;
```	
	
###2. 函数功能
停止扫描.

###3. 函数参数
参数            | 类型          | 说明
:-----         | :--------    | :------  
*返回值*        | void         | 无


***
##检索扫描记录
###1. 函数声明
``` 
- (JumaDevice *)retrieveDeviceWithUUID:(NSString *)UUID NS_AVAILABLE(NA, 7_0);
```	
	
###2. 函数功能
检查系统中是否保存有和 UUID 参数相关的蓝牙设备的信息. 如果有, 就返回相应的 JumaDevice 对象.

###3. 函数参数
参数       | 类型               | 说明
:-----    | :--------          | :------ 
*UUID*    | NSString *         | 蓝牙设备的 UUID
*返回值*   | JumaDevice *       | JumaDevice 类型的对象。



***
##发现蓝牙设备事件

###1. 函数声明
```
- (void)manager:(JumaManager *)manager didDiscoverDevice:(JumaDevice *)device RSSI:(NSNumber *)RSSI;
```

###2. 函数功能
在扫描过程中, 发现蓝牙设备时, 这个方法会被调用, 并且给出被发现的蓝牙设备的信息和当前信号强度.


###3. 函数参数
参数              | 类型                   | 说明
:-----            | :--------             | :------ 
*manager*         | JumaManager * | 给出这个信息的对象
*device*          | JumaDevice * | 被发现的蓝牙设备
*RSSI*            | NSNumber *   | 当前信号强度
*返回值*           | void         | 无


####关于 *RSSI* 参数 
如果 *RSSI* 的值为 127，则说明无法获取当前信号强度。


***
##停止扫描事件
###1. 函数声明
```
- (void)managerDidStopScan:(JumaManager *)manager;
```
	
###2. 函数功能
通过 stopScan 停止扫描后, 这个方法会被调用

###3. 函数参数
参数              | 类型                   | 说明
:-----            | :--------            | :------ 
*manager*         | JumaManager *        | 给出这个信息的对象 
*返回值*           | void                 | 无





***
##建立连接
###1. 函数声明
```
- (void)connectDevice:(JumaDevice *)device;
```	
	
###2. 函数功能
连接蓝牙设备.

###3. 函数参数
参数         | 类型          | 说明
:-----      | :--------    | :------ 
*device*    | JumaDevice * | 被连接的 JumaDevice 对象
*返回值*     | void         | 无



***
##断开连接
###1. 函数声明
```
- (void)disconnectDevice:(JumaDevice *)device;
```	

###2. 函数功能
断开和蓝牙设备的连接, 或者取消一个等待结果的连接请求

###3. 函数参数
参数            | 类型          | 说明
:-----         | :--------    | :------  
*device*       | JumaDevice * | 被断开链接的 JumaDevice 对象
*返回值*        | void         | 无




***
##连接成功事件
###1. 函数声明
```
- (void)manager:(JumaManager *)manager didConnectDevice:(JumaDevice *)device;
```	
	
###2. 函数功能
通过 connectDevice: 方法成功连接到某个蓝牙设备时, 这个方法会被调用.

###3. 函数参数
参数              | 类型                   | 说明
:-----            | :--------             | :------ 
*manager*         | JumaManager *         | 给出这个信息的对象
*device*          | JumaDevice *          | 已经连接的蓝牙设备 
*返回值*           | void                  | 无




***
##连接断开事件
###1. 函数声明
```
- (void)manager:(JumaManager *)manager didDisconnectDevice:(JumaDevice *)device error:(NSError *)error;
```	
	
###2. 函数功能
和蓝牙设备的连接断开时, 这个方法会被调用.  

###3. 函数参数
参数              | 类型                   | 说明
:-----            | :--------             | :------ 
*manager*         | JumaManager *   | 给出这个信息的对象
*device*          | JumaDevice *   | 和手机的连接断开的那个蓝牙设备
*error*           | NSError *    | 断开原因 
*返回值*           | void         | 无


####关于 *error* 参数 
如果连接断开是由调用 disconnectDevice: 导致的, *error* 为 nil.




***
##连接失败事件
###1. 函数声明
```
- (void)manager:(JumaManager *)manager didFailToConnectDevice:(JumaDevice *)device error:(NSError *)error;
```	

###2. 函数功能
通过 connectDevice: 连接蓝牙设备失败时, 这个方法会被调用.


###3. 函数参数
参数              | 类型                   | 说明
:-----            | :--------             | :------ 
*manager*         | JumaManager *         | 给出这个信息的对象
*device*          | JumaDevice *          | 连接失败的那个蓝牙设备
*error*           | NSError *             | 连接失败的原因 
*返回值*           | void                  | 无




***
##蓝牙状态更新事件
###1. 函数声明
```
- (void)managerDidUpdateState:(JumaManager *)manager;
```
	

###2. 函数功能
当 JumaManager 的 state 属性值发生变化时, 这个方法会被调用, state 属性每改变一次, 这个方法就会被调用一次.  
只有 state 属性的值是 JumaManagerStatePoweredOn 时, 才可以执行扫描和连接等方法.


###3. 函数参数
参数              | 类型                      | 说明
:-----            | :--------                | :------ 
*deviceManager*   | JumaManager *            | 状态发生变化的那个 JumaManager 对象
*返回值*           | void                     | 无


####关于 *manager* 参数 
*manager* 的状态可以由 JumaManagerState state = manager.state 得到  
在 *JumaBluetoothSDK/JumaManager.h* 文件中对 *JumaManagerState* 进行了定义：

枚举值                       |说明
:--------                        | :------
JumaManagerStateUnknown       | 当前设备中蓝牙的状态未知
JumaManagerStateResetting     | 当前设备中的蓝牙正在重置
JumaManagerStateUnsupported   | 当前设备中蓝牙无法支持的状态 
JumaManagerStateUnauthorized  | 对蓝牙的使用没有获得用户授权
JumaManagerStatePoweredOff    | 蓝牙已经关闭
JumaManagerStatePoweredOn     | 蓝牙已经打开, 可以进行扫描和连接操作












