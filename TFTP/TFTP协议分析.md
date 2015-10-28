# TFPT协议分析
***
TFTP协议采用UDP，端口为69。主要功能：在文件服务器端进行读写文件操作。
### 报文格式
TFTP有5种报文格式：
* | IP首部 | UDP首部 | 操作码 | 文件名 | 0 | 模式 | 0 | <br>
                         |————1=RRQ,2-WRQ（分别对应读写操作）
* | IP首部 | UDP首部 | 操作码 | 块编号 | 数据 | <br>
                         |————3=data（数据）
* | IP首部 | UDP首部 | 操作码 | 块编号 | <br>
                         |————4=ACK（回应请求）	
* | IP首部 | UDP首部 | 操作码 | 差错码 | 差错信息 | 0 | <br>
                         |————5=error（错误）
						 
***
所以，我们可以得到：
'''C
	#define TFTP_RRQ	1
	#define TFTP_WRQ	2
	#define TFTP_DATA	3
	#define TFTP_ACK	4
	#define TFTP_ERROR	5
'''
### 头文件结构
'''C
	typedef struct tftpheader
	{
		unsigned short tftp_opcode; //操作码
		union
		{
			unsigned short tftp_block; //块号
			unsigned short tftp_error_code; //错误码
			char tftp_stuff[1]; //数据包填充物
		}tftp_message;
		char tftp_data[1]; //数据或错误字符串
	}
'''	
					 				   	
