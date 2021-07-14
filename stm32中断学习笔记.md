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

1-初始化要连接到EXTI的GPIO

2-初始化EXTI用于产生中断/事件

3-初始化NVIC，用于处理中断

4-编写中断服务函数

5-main函数