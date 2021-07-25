## STM32 ADC学习笔记

### 电压输入范围

ADC输入电压输入范围是0~3.3V

超出范围的电压可用下图电路转换使得能测-10V~10V的电压

![image-20210723234751921](https://gitee.com/hgchshs/markdown-table/raw/master/img/20210723234754.png)

原理是基尔霍夫定律

根据基尔霍夫定律（KCL），节点流入的电流等于流出的电流
（Vint-Vout）/R2+（3V3-Vout）/R1=Vout/R3
Vout =（Vint +10）/6

### 输入通道

外部通道在转换的时候又分为规则通道和注入通道，其中规则通道最多有16路，注入通道最多有4路



**规则通道**：

顾名思意，规则通道就是很规矩的意思，我们平时一般使用的就是这个通道。



**注入通道**：

注入，可以理解为插入，插队的意思，是一种不安分的通道。它是一种在规则通道转换的时候强行插入要转换的一种。这点跟中断程序很像，都是不安分的主。所以，注入通道只有在规则通道存在时才会出现。

**通道转换顺序**

由**规则序列寄存器SQRx，x（1，2，3**）控制：

寄存器位SQ1~16设置第1~16个要转换的通道，寄存器的取值可以是1~16代表通道1~16；SQL位代表要转换多少个通道。

具体内容可以看参考手册，这里简单概括一下。

**注入序列寄存器：**

注入序列寄存器JSQR只有一个，最多支持4个通道，具体多少个由JSQR的JL[2：0]决定。

如果JL的值小于4的话，则JSQR跟SQR决定转换顺序的设置不一样，第一次转换的不是JSQRl[4：0]，而是JCQRx[4：0]，x=（4-L），跟SQR刚好相反。如果JL=00（1个转换），那么转换的顺序是从JSQR4[4：0]开始，而不是从JSQR1[4：0]开始，这个要注意，编程的时候不要搞错。当几等于4时，跟SQR一样。

![image-20210724115400486](https://gitee.com/hgchshs/markdown-table/raw/master/img/20210724115401.png)

### 触发源

**1、软件触发**：ADC_CR2：ADON/SWST
ART/JSWSTART
**2、外部事件触发**：内部定时器/外部IO
选择：ADC_CR2：EXTSEL[2：0]和JEXTSEL[2：0]
激活：ADC_CR2：EXTEN和JEXTEN

### 转换时间

**转换时间**：Tconv=采样时间+12.5个周期

**ADC_CLK:ADC模拟电路时钟**，最大值为14M，由PCLK2提供，还可分频，2/4/6/8，RCC_CFGR的ADCPRE[1：0]设置。PCLK2=72M。

**数字时钟**：RCC_APB2ENR，用于访问寄存器

**数据寄存器**

规则组的数据放在ADC_DR寄存器，注入组的数据放在JDRx。

规则组：

1-16位有效，（用于存放独立模式转换完成数据

2-ADC_CR2：ALIGN
3-多通道采集的是最好使用DMA

注入组：

1-16位有效，用于存放注入通道转换完成数据

2-ADC_CR2：ALIGN
3-有4个这样的寄存器

### 中断

1-ADC_SR,ADC_CR1

2-ADC HTR,ADC_LTR

![image-20210724123216538](https://gitee.com/hgchshs/markdown-table/raw/master/img/20210724123217.png)

### 电压转换

**怎么根据数据量算出模拟量？**

1-电压输入范围为：0~3.3V
2-分辨率为12位
3-最小精度为：3.3/2^124-设数字量为X，则有模拟量Y=（3.3/212）*X

## ADC代码部分

### 初始化结构体

~~~c
typedef struct
{
	uint32_t ADC_Mode；//ADC工作模式选择
    Functionalstate ADC_ScanconvMode；/*ADC扫描（多通道）或者单次（单通道）模式选择*/
	Functionalstate ADC_ContinuousconvMode；//ADC单次转换或者连续转换选择
        uint32_t ADC_ExternalTrigconv；//ADc 转换触发信号选择
        uint32_t ADC_DataAlign；//ADC 数据寄存器对齐格式
        uint8_t ADC_NbrOfchanne1；//aDC采集通道数
）}ADC Initrypepef；
~~~

常用固件库函数

~~~c
ADC_Init();429
RCC_ADCCEKConfig();680
ADC_RegularChannelConfig();442
ADC_Cmd();431
ADC_ SoftwareStartConvCmd();438
ADC_ExternalTrigConvCmd();443
ADC_DMACmd();432
    //后面的数字代表在固件库中的行数
~~~

### 独立模式-单通道-中断读取   编程要点

1-初始化ADC用到的GPIO
2-初始化ADC初始化结构体
3-配置ADC时钟，配置通道的转换顺序和采样时间//RCC
4-使能ADC转换完成中断，配置中断优先级

5-使能ADC，准备开始转换

6-校准ADC
7-软件触发ADC，真正开始转换

8-编写中断服务函数，读取ADC转换数据

9-编写main函数，把转换的数据打印出来

### 独立模式-单通道-DMA读取   编程要点

和中断读取类似，详情请查看例程代码

### 独立模式-多通道-DMA读取   编程要点

详情请查看例程代码

### 同步规则模式-单通道-DMA读取   编程要点

详情请查看例程代码

