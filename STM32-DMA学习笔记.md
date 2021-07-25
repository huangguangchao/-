**DMA:**

Data Memory Access，直接存储器访问。
主要功能是可以把数据从一个地方搬到另外一个地方，而且不占用CPU。

### DMA仲裁器

1、软件阶段，DMA_CCRx:PL[1:0].

2、硬件阶段，通道编号小的优先级大，DM1的优先级高于DMA2的优先级

### DMA初始化结构体

~~~c
typedef struct
{
	uint32t DMA PeripheralBaseAddr；//外设地址
    uint32t DMA MemoryBaseAddr；//存储器地址
    uint32t DMA DIR；//传输方向
    uint32 t DMA Buffersize；//传输数目
    uint32t DMA peripheralInc；//外设地址增量模式
    uint32t DMA MemoryInc；//存储器地址增量模式
    uint32t DMA Peripheralpatasize；//外设数据宽度
    uint32t DMA MemoryDataSize；//存储器数据宽度
    uint32t DMA Mode；//模式选择
    uint32t DMA Priority；//通道优先级
    uint32t DMA M2M；//存储器到存储器模式
}DMA InitTypeDef；
~~~

编程的时候往结构体成员里面写数据，然后再调用一下初始化函数就可以把成员的值写到寄存器里面

### 数据从哪里来，要到哪里去

####  从哪里来

*1、外设地址，DMA_CPAR*

*2、存储器地址，DMA_CMAR*

#### 到哪里去

3、传输方向，DMA_CCR:DIR

### 数据要传多少，传的单位是什么

1、传输数目，控制DMA_CNDTR寄存器

2、外设地址是否递增，控制DMA_CCRX:PINC寄存器

3、存储器地址是否递增，控制DMA_CCRX:MINC寄存器

4、外设数据宽度，控制DMA_CCRX:PSIZE寄存器

5、存储器数据宽度，控制DMA_CCRX:MSIZE寄存器

### 什么时候传输结束

1、模式选择，DMALCCRx:CIRC
2、传输过半，传输完成，传输出错，DMA_ISR

## M TO M编程要点

1-在FLASH中定义好要传输的数据，在SRAM中定仅好用来接收FLASH数据的变量。

//FLASH中存放常量；SRAM存放变量

2-初始化DMA，要是配置DMA初始化结构体。
3-编写比较函数。
4-编写main函数。

~~~c
void MtM_DMA_Config(void)//配置DMA的函数
{
    DMA_InitTypeDef DMA_InitStruct;//定义结构体
    RCC_AHBPeriphClockCmd(MTM_DMA_CLK,ENABLE);//使能DMA外设的时钟
    
}
~~~

### M TO P编程要点

1-初始化串口（从现有的例程移植过来）
2-配置DMA初始化结构体。
3-编写主函数（开启串口发送DMA请求）。
