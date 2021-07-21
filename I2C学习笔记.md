# I2C学习笔记

## 1、物理层

​		它是一个支持多设备的总线。“总线”指多个设备共用的信号线。
​		在一个l2C通讯总线中，可连接多个l2C通讯设备，支持多个通讯主机及多个通讯从机。



​		一个I2C总线只使用两条总线线路，一条双向串行数据线（SDA），一条串行时钟线（SCL）。数据线		即用来表示数据，时钟线用于数据收发同步。



​		每个连接到总线的设备都有一个独立的地址，主机可以利用这个地址进行不同设备之间的访问。

![image-20210721110447435](https://gitee.com/hgchshs/markdown-table/raw/master/img/20210721110457.png)



​		总线通过上拉电阻接到电源。当l2C设备空闲时，会输出高阻态，而当所有设备都空闲，都输出高阻态时，由上拉电阻把总线拉成高电平。多个主机同时使用总线时，为了防止数据冲突，会利用仲裁方式决定由哪个设备占用总线。

​		

​		具有三种传输模式：标准模式传输速率为100kbit/s，快速模式为400kbit/s，高速模式下可达			3.4Mbit/s，但目前大多IPC设备尚不支持高速模式。



​		连接到相同总线的IC数量受到总线的最大电容400pF限制。

## 2、协议层

I2C的读写过程：

主机寻址（7位地址）+（读/写）->从机应答->（主或从机）传输数据->从或主机(应答)->........->

（从或主机）非应答->（从或主机）停止发送



![image-20210721113519846](https://gitee.com/hgchshs/markdown-table/raw/master/img/20210721113521.png)

当SCL线是高电平时SDA线从高电平向低电平切换，这个情况表示通讯的起始。



当SCL是高电平时SDA线由低电平向高电平切换，表示通讯的停止。



起始和停止信号一般由主机产生。

### 3、数据有效性

​				I2C使用SDA信号线来传输数据，使用SCL信号线进行数据同步。SDA数据线在SCL的每个时钟周期传输一位数据。



![image-20210721114657904](https://gitee.com/hgchshs/markdown-table/raw/master/img/20210721114659.png)

​		SCL为高电平的时候SDA表示的数据有效，即此时的SDA为高电平时表示数据“1”，为低电平时表示数据“0"。
​		当SCL为低电平时，SDA的数据无效，一般在这个时候SDA进行电平切换，为下一次表示数据做好准备。

## 4、响应

​		

​		



![image-20210721115431209](https://gitee.com/hgchshs/markdown-table/raw/master/img/20210721115432.png)

​		传输时主机产生时钟，在第9个时钟时，数据发送端会释放SDA的控制权，由数据接收端控制SDA，若SDA为高电平，表示非应答信号（NACK），低电平表示应答信号（ACK）。

### 5、时钟控制逻辑

SCL线的时钟信号，由lPC接口根据时钟控制寄存器（CCR）控制，控制的参数主要为时钟频率。







**·可选择I2C通讯的“标准/快速”模式，这两个模式分别I2C对应100/400Kbit/s的通讯速率。**



**·在快速模式下可选择SCL时钟的占空比，可选Tow/Thigh-2或Tow/Thigh-16/9模式。**



**·CCR寄存器中12位的配置因子CCR，它与12C外设的输入时钟源共同作用，产生SCL时钟。STM32的l2C外设输入时钟源为PCLK1。**

ps：

PCLK1是指APB1时钟；PCLK2是指APB2时钟，以此类推。

### 6、计算时钟频率

计算时钟频率：
标准模式：
Thgh = CCR * TpCKL1          Tow = CCR * TPCLK1；

快速模式中Tow/Thioh=2时：
Thigh = CCR * TPCKL1           Tow = 2 * CCR * TPCKL1；

快速模式中Tow/Thioh=16/9时：
Thigh = 9 * CCR * TPCKL1      Tow = 16 * CCR * TPCKL1；



例如，我们的PCLK1=36MHz，想要配置400Kbit/s的速率，计算方式如下：

**PCLK时钟周期**：TPCLK1 = 1/36000000

**目标SCL时钟周期**：TSCL = 1/400000
**SCL时钟周期内的高电平时间**：THIGH = TSCL/3
**SCL时钟周期内的低电平时间**：TLOW = 2*TSCL/3

**计算CCR的值**：CCR = THIGH/TPCLK1 = 30

计算出来的CCR值写入到寄存器即可。

## 二、I2C编程部分

~~~c
typedef struct{
uint32t I2C ClockSpeed；/*！<设置SCL时钟频率，此值要低于400000*/
uint16tI2C Mode；/*！<指定工作模式，可选I2C模式及SMBUS模式*/
uint16t I2C DutyCycle；/*指定时钟占空比，可选1ow/high=2：1及16：9模式*
uint16t I2C OwnAddress1；/*！<指定自身的I2C设备地址*/
uint16t I2C Ack；/*！<使能或关闭响应（一般都要使能）*/
uint16_t I2C_AcknowledgedAddress；/*！<指定地址的长度，可为7位及10位*/
}I2C InitTypeDef；
    
~~~

I2C结构体

**uint16tI2C Mode**

​		选择I2C的使用方式，有12C模式（12C_Mode_l2C）和SMBus主、从模式（12C_Mode_SMBusHost、12C_Mode_SMBusDevice）。
​	

​	I2C不需要在此处区分主从模式，直接设置l2CModel2C即可。



**uint16t I2C OwnAddress1**

​		配置STM32的I2C设备自己的地址，每个连接到I2C总线上的设备都要有一个自己的地址，作为主机也不例外。地址可设置为7位或10位（受下面12CAcknowledgeAddress成员决定），只要该地址是I2C总线上唯一的即可。
​	

​	STM32的I2C外设可同时使用两个地址，即同时对两个地址作出响应，这个结构成员l2C_OwnAddress1配置的是默认的、OAR1寄存器存储的地址，若需要设置第二个地址寄存器OAR2，可使用**I2C_OwnAddress2Config**函数来配置，OAR2不支持10位地址。

