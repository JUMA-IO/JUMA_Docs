本说明用于描述SPI总线的配置和使用。

> 本系列API目前仅支持nRF51平台。

对于nRF51平台的用户，使用时请引用相应的头文件：

* API头文件：[juma_sdk_api.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_api.h)
* 数据类型或宏定义参见：[juma_sdk_types.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_types.h)



***
## SPI总线配置
###1. 函数声明
```
void spi_setup(spi_init_struct_t * spi_struct);
```

###2. 函数功能
SPI系列API提供了一个3线的SPI总线接口，如果需要CS_N等使能信号，需要自行用GPIO来模拟。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
spi_struct| spi_init_struct_t *  | SPI的配置参数
*返回值*  | void     | 无

####关于 *spi_init_struct_t* 的描述
该参数定义了SPI总线使用的时候需要的必要参数：

```
typedef struct
{
	uint32_t MISO;			//miso pin
	uint32_t MOSI;			//mosi pin
	uint32_t SCK;				//sck	pin
	uint32_t FREQ;			//bus speed
	
	uint32_t ORDER;			//order msb lsb
	uint32_t CPOL;			//cpol
	uint32_t CPHA;			//cpha
}spi_init_struct_t;
```

其中，ORDER、CPOL、CPHA等支持如下定义：

```
ORDER：
SPI_ORDER_MSB
SPI_ORDER_LSB
```
```
CPOL:
SPI_CPOL_HIGH
SPI_CPOL_LOW
```
```
CPHA:
SPI_CPHA_1EDGE
SPI_CPHA_2EDGE
```


***
## SPI数据收发
###1. 函数声明
```
void spi_transmit_receive(uint8_t * tx_buff, uint8_t * rx_buff, uint32_t buff_len);
```

###2. 函数功能
利用SPI收发数据。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*tx_buff*   | uint8_t * | 要发送的数据的
*rx_buff* | uint8_t * | 接收数据的缓冲区
*buff_len* | uint32_t | 数据的长度
*返回值*  | void     | 无


###4. 函数举例
```
	void foo( void )
	{
		//config
		spi_init_struct_t spi_struct;
	
		spi_struct.MISO = 8;
		spi_struct.MOSI = 9;
		spi_struct.SCK  = 10;
		spi_struct.FREQ = SPI_FREQUENCY_FREQUENCY_M1;
	
		spi_struct.CPHA = SPI_CPHA_1EDGE;
		spi_struct.CPOL = SPI_CPOL_HIGH;
		spi_struct.ORDER = SPI_ORDER_MSB;
		
		spi_setup(&spi_struct);

		//send and receive
		uint8_t tx_buff[] = "juma sdk spi test";
		uint8_t rx_buff[20];
		
		spi_transmit_receive(tx_buff, rx_buff, sizeof(tx_buff));
		
		//ble send message
		ble_device_send(0x55, 16, rx_buff);
	}
```







