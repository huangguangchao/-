### 中断编程的顺序

1-使能中断请求

2-配置中断优先级分组

3-配置NVIC寄存器，初始化NVIC_InitTypeDef;

4-编写中断服务函数

### 固件库中EXTI初始化结构体

~~~ c
EXTI_InitTypeDef
    1-EXTI_LINE:用于产生中断/事件线
    2-EXTI_Mode：EXTI模式（中断/事件）
    3-EXTI_Trigger:触发（上/下/上下）
    4-EXTI_LineCmd:使能或者失能（IMR/EMR）
~~~

### 编程要点

~~~c
#define LEDG TOGGLE {LED_G_GPIO_PORT->0DR^=GPTO_Pin_0;}//中断服务函数会用到

static void EXIT_NVIC_Config(void)//其他文件无法调用
{
    NVIC_InitTypeDef NVIC_InitStruct;//定义结构体变量；NVIC是嵌套向量中断控制寄存器
    NVIC_ PriorityGroupConfig(NVIC_PriorityGroup_1);//优先级分组
  
    //配置结构体成员
   NVIC InitStruct.NVIC IRQChannel=EXTIO IRQn; //选择通道
   NVIC_InitStruct.NVIC_IRQChannelPreemptionPriority=1；//配置主优先级，优先级数字越小优先级越高
   NVIC_InitStruct.NVIC_IRQChannelSubPriority=1;//配置子优先级
   NVIC InitStruct.NVIC IRQChannelCmd =ENABLE;//使能寄存器
    
   // 调用NVIC的初始化函数
     NVIC_Init(&NVIC_InitStruct);
         
}




void EXIT_Key_Config(void)
{
    GPIO_InitTypeDef GPI0_InitStruct；
    GPIO_InitTypeDef GPI0_InitStruct；//定义结构体变量
     //配置中断优先级
     EXIT_NVIC_Config();
    
   // 1-初始化要连接到EXTI的GPIO
   //初始化gpio的函数在库函数中有，但是接口要自己选择合适的
	RCC_APB2PeriphClockCmd(KEY1_INT_GPIO_CLK, ENABLE)； //首先开时钟
	GPIO_InitStruct.GPIO_Pin=KEY1_INT_GPIO PIN； //然后选择gpio的引脚
	GPIOInitStruct.GPIO_Mode=GPIO_Mode_IN_FLOATING； //然后选择gpio端口
	GPIO_Init(KY1_INT_GPIO_PORT,&GPIO_InitStruct)；//然后配置输出模式，这里使用浮空输入（gpio的默认电平由外部决定）
   // 2-初始化EXTI用于产生中断/事件（库函数也有）
    RCC APB2PeriphClockCmd(KEY1 INT GPIO CLK,ENABLE); //打开AFIO时钟，AFIO控制外部中断
    GP1O_EXTILineCofig(GPIO_PortSourceGPIOA,GPIO PinSource0);//选择输入线，形参是端口和引脚
   EXTI_InitStruct.EXTI_Line = EXTI_Line0;//选择线
   EXTI_InitStruct.EXTI_Mode = EXTI_Mode_Interruppt;//配置模式为中断
   EXTI_InitStruct.EXTI_Trigger = EXTI_Trigger_Rising;//配置成上升沿触发
   EXTI_InitStruct.EXTI_LineCmd =ENABLE//打开中断使能控制寄存器
   EXTI_Init(&EXTI_InitStruct);//调用固件库函数初始化EXTI
} 

~~~

### 编写中断服务函数

~~~c  

//函数名称可以在hd.s里面的相量表（包含了所有中断服务函数的地址）里找
void EXTIO_IRQHandler(void)
{
    //判断中断标志位有没有置位的函数在exti.h里
    // ITStatus EXTI_GetITStatus(uint32_t EXTI_Line)
    if(EXTI_GetITStatus(EXTI_Line0)!=RESET)//如果中断线被触发
    {
       LEDG TOGGLE //执行相应的动作，这里以电平翻转一次为例
    }
    EXTI_ClearITPendingBit(EXTI_Line0)；//执行完之后把中断标志位清除
}
~~~

### 编写main函数

~~~c
int main (void)//以按下按键灯泡电平变化为例
{
    LED_GPIO_Config();
    EXIT_Key_Config();
    
    while(1)
    {  
    }
}

~~~





