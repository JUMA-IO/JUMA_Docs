本说明用于描述串口的配置和操作。

> 本系列API目前仅支持nRF51平台。

对于nRF51平台的用户，使用时请引用相应的头文件：

* API头文件：[juma_sdk_api.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_api.h)
* 数据类型或宏定义参见：[juma_sdk_types.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_types.h)




***
## 串口配置
###1. 函数声明
```
serial_setup(uint8_t rx_pin, uint8_t tx_pin, uint32_t baudrate);
```

###2. 函数功能
配置用于串口收发的引脚  


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*rx_pin* | uint8_t  | 接收数据的引脚
*tx_pin* | uint8_t  | 发送数据的引脚
*baudrate*|uint32_t | 波特率设置
*返回值*  | void     | 无

####关于 *baudrate* 参数
`baudrate `的取值范围如下：

```
#define UART_BAUDRATE_Baud1200    (0x0004F000UL) /*!< 1200 baud. */
#define UART_BAUDRATE_Baud2400    (0x0009D000UL) /*!< 2400 baud. */
#define UART_BAUDRATE_Baud4800    (0x0013B000UL) /*!< 4800 baud. */
#define UART_BAUDRATE_Baud9600    (0x00275000UL) /*!< 9600 baud. */
#define UART_BAUDRATE_Baud14400   (0x003B0000UL) /*!< 14400 baud. */
#define UART_BAUDRATE_Baud19200   (0x004EA000UL) /*!< 19200 baud. */
#define UART_BAUDRATE_Baud28800   (0x0075F000UL) /*!< 28800 baud. */
#define UART_BAUDRATE_Baud38400   (0x009D5000UL) /*!< 38400 baud. */
#define UART_BAUDRATE_Baud57600   (0x00EBF000UL) /*!< 57600 baud. */
#define UART_BAUDRATE_Baud76800   (0x013A9000UL) /*!< 76800 baud. */
#define UART_BAUDRATE_Baud115200  (0x01D7E000UL) /*!< 115200 baud. */
#define UART_BAUDRATE_Baud230400  (0x03AFB000UL) /*!< 230400 baud. */
#define UART_BAUDRATE_Baud250000  (0x04000000UL) /*!< 250000 baud. */
#define UART_BAUDRATE_Baud460800  (0x075F7000UL) /*!< 460800 baud. */
#define UART_BAUDRATE_Baud921600  (0x0EBED000UL) /*!< 921600 baud. */
#define UART_BAUDRATE_Baud1M      (0x10000000UL) /*!< 1M baud. */

```

###4. 函数举例
```
	#define RX_PIN 24
	#define TX_PIN 25
	
	void foo( void )
	{
		//配置串口收发引脚
		serial_setup( RX_PIN, TX_PIN, UART_BAUDRATE_Baud9600);	
	}
```


***
## 串口发送
###1. 函数声明
```
void serial_send(uint8_t* data, uint32_t length);      
```

###2. 函数功能
向串口发送数据。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*data*   | uint8_t * | 数据的起始地址
*length* | uint32_t | 数据的长度
*返回值*  | void     | 无


###4. 函数举例
```
	void foo( void )
	{
		uint8_t* data = "test data";
		/*在调用此函数前
		需先调用serial_setup配置收发引脚*/
		serial_send( data, strlen(data) );
	}
```



***
## 串口接收数据事件
###1. 函数声明
```
void serial_on_data(uint8_t data);      
```

###2. 函数功能
当串口上收到数据时，系统会触发该事件。  
每次接收一个字节的数据。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*data*   | uint8_t  | 串口收到的数据
*返回值*  | void      | 无


###4. 函数举例
```
	//回调函数
	void serial_on_data( uint8_t data )
	{
		//对收到的数据进行处理
	}

	//当前函数
	void foo( void )
	{
		...
		
		/*配置收发引脚
		  假设收发引脚已宏定义*/
		serial_setup( RX_PIN, TX_PIN );
		
		...
	}
```




