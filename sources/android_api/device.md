本文用以说明BLE建立连接、断开连接、发送数据、接受数据等API的操作方式。

SDK为开源项目，本文涉及的代码参见：

* `JumaDevice`：[JumaDevice.java](https://github.com/JUMA-IO/BLE_SDK_Android/blob/master/src/com/juma/sdk/JumaDevice.java)
* `JumaDeviceCallback`：[JumaDeviceCallback.java](https://github.com/JUMA-IO/BLE_SDK_Android/blob/master/src/com/juma/sdk/JumaDeviceCallback.java) 

SDK中将一个蓝牙设备抽象为`JumaDevice`类对象，该类中有建立连接、断开连接、发送数据等API。接收数据、蓝牙状态变化等事件由系统回调给应用程序，SDK中将回调方式抽象为`JumaDeviceCallback`类。

> 以下API属于`JumaDevice`类成员，Callback属于`JumaDeviceCallback`类成员。



***
## 建立蓝牙连接
###1. 函数声明
```
public boolean connect(JumaDeviceCallback callback);      
```

###2. 函数功能
连接一个蓝牙设备。  

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
callback    | JumaDeviceCallback  | JumaDevice的回调函数
返回值  | boolean      | `true` - 连接操作成功；`false` - 连接操作未成功

###4. 函数举例
```
	boolean result = device.connect(new JumaDeviceCallback() {
		
		@Override
		public void onUpdateFirmware(int status) {
			// TODO Auto-generated method stub
			
		}
		
		@Override
		public void onSend(int status) {
			// TODO Auto-generated method stub
			
		}
		
		@Override
		public void onRemoteRssi(int status, int rssi) {
			// TODO Auto-generated method stub
			
		}
		
		@Override
		public void onReciver(byte type, byte[] message) {
			// TODO Auto-generated method stub
			
		}
		
		@Override
		public void onConnectionChange(int status, int newState) {
			// TODO Auto-generated method stub
			
		}
	});
```


***
## 断开蓝牙连接
###1. 函数声明
```
public boolean disconnect();    
```

###2. 函数功能
断开一个蓝牙设备的连接。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
返回值  | boolean      | `true` - 断开操作成功；`false` - 断开操作未成功


***
## 蓝牙连接状态变化事件
本API属于`JumaDeviceCallback`。

###1. 函数声明
```
public void onConnectionStateChange(int status, int newState);
```

###2. 函数功能
蓝牙设备的连接状态（蓝牙设备连接/蓝牙设备断开连接）发生改变时，会触发此回调。

###3. 函数参数
参数	     | 数据类型  | 说明
:-----   | :----    | :------
status   | int      | 连接/断开连接的状态：`SUCCESS` 或 `ERROR` 
newState | int      | 新的连接状态：`STATE_CONNECTED` 或 `STATE_DISCONNECTED`

***
## 获取设备名称
###1. 函数声明
```
public boolen getName();
```

###2. 函数功能
获取一个蓝牙设备的名称。  

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
返回值  | String      | 设备名称


***
## 获取设备信号强度
###1. 函数声明
```
public boolean getRemoteRssi();
```

###2. 函数功能
获取设备的信号强度。  

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
**返回值**  | boolean      | `true` - 获取信号强度成功；`false` - 获取信号强度未成功

***
## 获取设备信号强度事件
本API属于`JumaDeviceCallback`。

###1. 函数声明
```
public void onRemoteRssi(int status, int rssi);
```

###2. 函数功能
执行获取蓝牙设备信号强度操作时，会触发此回调。  

###3. 函数参数
参数	     | 数据类型  | 说明
:-----   | :----    | :------
status   | int      | 获取蓝牙设备信号强度的状态：`SUCCESS` 或 `ERROR` 
rssi | int      | 蓝牙设备的信号强度



***
## 获取设备的UUID
###1. 函数声明
```
public void getUuid();
```

###2. 函数功能
获取设备的UUID。 

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | String      | 设备的UUID

***
## 获取设备的SDK版本号
###1. 函数声明
```
public boolen getVersion();
```

###2. 函数功能
获取设备的SDK的版本号。  

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
返回值  | String      | SDK版本号

***
## 获取设备连接状态
###1. 函数声明
```
public boolen isConnected();
```

###2. 函数功能
获取设备的连接状态。 

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
返回值  | boolean      | `true` - 已连接设备；`false` - 未连接设备



***
## 发送蓝牙数据
###1. 函数声明
```
public boolean send(byte type, byte[] message);      
```

###2. 函数功能
向设备发送蓝牙数据。 

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*type*    | byte  | 数据类型
*message*    | byte[]  | 发送给设备的数据
*返回值*  | void      | `true` - 数据发送成功；`false` - 数据发送未成功

####关于**type**的参数描述
`type` 用户自定义数据类型，范围在0(含) ~ 127(含)之间。  

####关于**message**的参数描述
`message` 发送给远端设备的数据，数据长度控制在198个byte以内。

###4. 函数举例
```
	boolean result = device.send(0x00, new byte[]{0x01,0x02,0x03});
```

***
## 发送蓝牙数据事件
本API属于`JumaDeviceCallback`。

###1. 函数声明
```
public void onSend(int status);
```

###2. 函数功能
执行发送数据操作时，会触发此回调。   

###3. 函数参数
参数	     | 数据类型  | 说明
:-----   | :----    | :------
status   | int      |发送数据操作的状态：`SUCCESS` 或 `ERROR`

***
## 接收蓝牙数据事件
本API属于`JumaDeviceCallback`。

###1. 函数声明
```
public void onReceive(byte type, byte[] message);
```

###2. 函数功能
接收到蓝牙设备发送过来的数据时，会触发此回调。  

###3. 函数参数
参数	     | 数据类型  | 说明
:-----   | :----    | :------
type   | byte      | 数据类型
message | byte[]      | 数据数据




***
## 设置固件更新(OTA)模式
###1. 函数声明
```
public boolen setOTAMode();
```

###2. 函数功能
固件更新指通过蓝牙将固件发送到设备，实现设备固件空中升级、更新的功能，即OTA(Over-The-Air)。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
返回值  | boolean      | `true` - 设置OTA操作成功；`false` - 设置OTA操作未成功


***
## 设备更新固件
###1. 函数声明
```
public boolean updateFirmware(String url);  
```

###2. 函数功能
更新设备固件。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
**url**  | String  | 存放固件（xxx.bin）的路径
**返回值**  | boolean      | `true` - 更新固件成功；`false` - 更新固件未成功

###4. 函数举例
```
	boolean result = device.updateFirmware("http://127.0.0.1/firmware/app.bin");
```

***
## 获取设备固件更新状态
###1. 函数声明
```
public boolen isFirmwareUpdating();
```

###2. 函数功能
获取设备是否在更新固件。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*返回值*  | boolean      | `true` - 正在更新；`false` - 没有在更新


***
## 设备固件更新事件
本API属于`JumaDeviceCallback`。

###1. 函数声明
```
public void onUpdateFirmware(int status);

```

###2. 函数功能
执行更新蓝牙设备的固件时，会触发此回调。  

###3. 函数参数
参数	     | 数据类型  | 说明
:-----   | :----    | :------
status   | int      | 更新设备固件操作的状态：`SUCCESS` 或 `ERROR`





