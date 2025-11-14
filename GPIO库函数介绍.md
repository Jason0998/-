# GPIO库函数介绍

### 一.1 个初始化函数：

```
void GPIO_Init(GPIO_TypeDef* GPIOx, GPIO_InitTypeDef* GPIO_InitStruct);
```

第一个入口参数：配置哪一组io

第二个入口参数：配置第一入口中的哪一个

配置代码例子

```plaintext
GPIO_InitTypeDef GPIO_InitStructure;

RCC_AHB1PeriphClockCmd(RCC_AHB1Periph_GPIOF, ENABLE);//使能GPIOF时钟

//GPIOF9,F10初始化设置
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_9 | GPIO_Pin_10;//LED0和LED1对应IO口
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_OUT;//普通输出模式
GPIO_InitStructure.GPIO_OType = GPIO_OType_PP;//推挽输出
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_100MHz;//100MHz
GPIO_InitStructure.GPIO_PuPd = GPIO_PuPd_UP;//上拉
GPIO_Init(GPIOF, &GPIO_InitStructure);//初始化GPIOF9,F1
```

### 二.2 个读取输入电平函数：

```
uint8_t GPIO_ReadInputDataBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
```

第一个入口参数：配置哪一组io

第二个入口参数：配置第一入口中的哪一个

```
uint16_t GPIO_ReadInputData(GPIO_TypeDef* GPIOx);
```

一次读一组io

### 三.2 个读取输出电平函数：

```
uint8_t GPIO_ReadOutputDataBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
```

```
uint16_t GPIO_ReadOutputData(GPIO_TypeDef* GPIOx);
```

### 四.4 个设置输出电平函数：

```
void GPIO_SetBits(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
```

第一个入口参数：配置哪一组io

第二个入口参数：配置第一入口中的哪一个

作用：设置某个IO口输出为高电平（1）

实际操作BSRRL寄存器    

```
void GPIO_ResetBits(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
```

作用：设置某个IO口输出为低电平（0）

实际操作BSRRH寄存器

```
void GPIO_WriteBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin, BitAction BitVal);
```

```
void GPIO_Write(GPIO_TypeDef* GPIOx, uint16_t PortVal);
```

不常用

***



# 各种输出模式

### 1.**输入浮空**

- 用途：引脚作为输入，无内部上拉 / 下拉电阻，电平由外部电路决定。常用于检测外部不确定电平的场景，如某些传感器的输出（需外部电路保证电平稳定）、按键（需额外消抖和电平驱动电路）。

### 2.**输入上拉**

- 用途：内部上拉电阻使引脚默认电平为高，外部电路拉低时电平变低。典型应用是**按键检测**（按下按键时引脚接地，电平由高变低，可触发检测），也用于需要默认高电平的输入接口。

### 3.**输入下拉**

- 用途：内部下拉电阻使引脚默认电平为低，外部电路拉高时电平变高。适用于需要默认低电平的输入场景，如某些模块的使能信号检测。

### 4.**模拟输入**

- 用途：引脚用于传输模拟信号，如 **ADC（模数转换）采集电压**、**DAC（数模转换）输出模拟电压**。此时数字电路不参与，仅作为模拟信号的通路。

### 5.**开漏输出**

- 用途：引脚只能输出低电平或高阻态（需外部上拉电阻才能输出高电平）。常用于**I2C 总线通信**（多设备并联，开漏模式可实现 “线与” 逻辑）、需要多设备共享总线的场景，或驱动外部上拉的负载（如 LED 灯，通过拉低引脚使其点亮）。

### 6.**推挽输出**

- 用途：可**主动**输出高电平和低电平，驱动能力强。常用于**普通数字信号输出**，如控制 LED 灯（直接输出高低电平点亮 / 熄灭）、继电器、数码管等，或需要强驱动能力的负载。

### 7.**推挽式复用功能**

- 用途：引脚复用为外设功能（如 USART 串口、SPI 总线等），并以推挽方式工作。例如，USART 串口的 TX 引脚复用为推挽输出，可稳定输出串口通信的高低电平，保障数据传输可靠性。

### 8.**开漏式复用功能**

- 用途：引脚复用为外设功能，以开漏方式工作。例如，I2C 总线的 SDA、SCL 引脚复用为开漏模式，实现多设备间的 I2C 通信（兼容开漏的 “线与” 逻辑）。

  ***

  

# 实验室学习收获

1.开漏输出与推挽输出的区别





