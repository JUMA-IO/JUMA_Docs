SDK在封装了Flash存储媒介的操作接口，用于数据的掉电后保存。

> 本系列API目前仅支持nRF51平台。

对于nRF51平台的用户，使用时请引用相应的头文件：

* API头文件：[juma_sdk_api.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_api.h)
* 数据类型或宏定义参见：[juma_sdk_types.h](https://github.com/JUMA-IO/nRF51_Platform/blob/master/Interface/Include/juma_sdk_types.h)



***
## 读取数据
###1. 函数声明
```
uint32_t data_storage_read(uint8_t data_id, uint8_t * data_len, uint8_t * data);
```

###2. 函数功能
从Flash空间读取数据。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*data_id*    | uint8_t  | 数据的索引值
*data_len*   | uint8_t * | 读取数据的长度
*data*		|uint8_t * | 读取数据的缓存
*返回值*  | void      | 无

####关于 *data_id* 的说明
`data_id `是数据的索引值，在存储和读取的时候都要使用，在Flash存储空间内部作为数据的唯一标识。

###4. 函数举例
```
	void foo( void )
	{
		...
		
		uint32_t err_code;
		uint8_t buff[28];
		uint8_t len;
		
		err_code = data_storage_read(0, &len, buff);
		
		if(err_code > 0)
		{
			switch(err_code)
			{
				case 1: 
					//black is error
				break;
	
			}
		}
		
		...
	}
```	

***
## 写入数据
###1. 函数声明
```
uint32_t data_storage_write(uint8_t data_id, uint8_t data_len, uint8_t * data);
```

###2. 函数功能
向Flash空间写入数据。

###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*data_id*    | uint8_t  | 数据的索引值
*data_len*   | uint8_t  | 要写入的数据的长度
*data*		|uint8_t * |要写入的数据实体
*返回值*  | void      | 无

####关于 *data_id* 的说明
`data_id `是数据的索引值，在存储和读取的时候都要使用，在Flash存储空间内部作为数据的唯一标识。


####关于 *data_len* 的说明
Flash存储空间采用的是按照数据库(Block)来存储的的方式，每一个块的大小为32字节，其中有4个字节用来存储了SDK内部的块说明信息，所以每一个存储块的实际可用大小为28字节。如果超过这个大小，会返回一个错误码`2`。

###4. 函数举例
```
	void foo( void )
	{
		...
		
		uint32_t err_code;
		
		err_code = data_storage_write(0, 16, "juma_sdk_storage");
		
		if(err_code > 0)
		{
			switch(err_code)
			{
				case 1: 
					//data_id is too many
				break;
				case 2: 
					//data_len is too many
				break;
			}
		}
		
		...
	}
```	
	



***
## 写入数据完成事件
###1. 函数声明
```
void data_storage_on_finish(uint8_t op_code);
```

###2. 函数功能

由于Flash存储空间操作具有异步性，当写入数据的时候并不能立刻写入完成，因此设计该函数，当写入数据完成后，由系统触发该事件。


###3. 函数参数
参数    | 数据类型   | 说明
:----- | :-------- | :------
*op_code*|uint8_t|预留，未定义
*返回值*  | void      | 无

###4. 函数举例
```
	void data_storage_on_finish(uint8_t op_code)
	{
	
	}
	
	void fun()
	{
		//当该操作完成后上述的函数会被调用
		data_storage_write(0, 16, "juma_sdk_storage";
	}
	
```


