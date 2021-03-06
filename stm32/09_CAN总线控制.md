# CAN 是什么

CAN = controller area network解决汽车仪表网络。

以太网：线束过多。

CAN高性能和可靠性。

不同速度的网可以通过网关连接（中继器）。

CAN中继器：

1. 拓展传输距离
2. 将不同速度的网进行连接

stm32： 集成CPU与CAN controller， 外部外接CAN收发器。 

收发器：tja1050或者82c250。

如果CPU不存在can控制器，使用mcp2515， spi或者sja1000（速度更快）。

组网：

1. 环路模式：适合500kps，上下接电阻
2. 开环模式：适合125kps，两端接电阻，不形成闭环

## CAN特点

1. 多主控制。
		
		当总线空闲时，所有的单元都可以发送消息（多主控制）。最先访问总线的单元可以获得发送权。多个单元同时开始发送时，发送高优先级ID消息的单元可获得发送权，所有的消息都以固定的格式发送。
2. 系统的柔软性

        与总线相连的但愿没有类似于“地址”的信息，因此在总线上增加单元时，连接在总线上的其他单元的软硬件及应用层都不需要改变。
3. 通信速度
        
        根据整个网络的规模，可设定适合的通信速度。在同一网络中，所有单元必须设定成统一的通信速度。即使有一个单元的通信速度与其它的不一样，此单元也会输出错误信号，妨碍整个网络的通信。不同网络间则可以有不同的通信速度。
4. 远程数据请求
		
		可通过发送“遥控帧” 请求其它单元发送数据
5. 错误检测功能

		所有的单元都可以检测错误（错误检测功能）。

		检测出错误的单元会立即同时通知其他所有单元（错误通知功能）。

		正在发送消息的单元一旦检测出错误，会强制结束当前的发送。强制结束发送的单元会不断反复地重新发送此消息直到成功发送为止（错误恢复功能）。
6. 故障封闭
		
		CAN可以判断出错误的类型是总线上暂时的数据错误（如外部噪声）还是持续的数据错误（如单元内部故障、驱动器故障、断线等）。由此功能，当总线上发生持续数据错误时，可将引起故障的单元从总线上隔离出去。
7. 连接
		
		CAN总线是可同时连接多个单元的总线。可连接的单元总数理论上是没有限制的。但实际上可连接的单元数受总线上的时间延迟及电气负载的限制。降低通信速度，可连接的单元数增加；提高通信速度，则可连接的单元数减少。

## STM32 bxCAN主要特点

* 支持CAN协议2.0A和2.0B主动模式
* 波特率最高可达1Mb/s 50米， 5k - 10公里
* 支持时间触发通信功能

### 发送

* 3个发送邮箱：发送缓冲器
* 发送报文的优先级特性可软件配置：优先级指发送邮箱的优先级
* 记录发送SOF时刻的时间戳

### 接收
3级深度的2个接收FIFO

### 可变的过滤器组

* 在互联型产品中，CAN1和CAN2分享28个过滤器组
* 其它STM32F103xx系列产品中有14个过滤器组

### 标识符列表
### 记录接收SOF时刻的时间戳
### 时间触发通信模式

* 禁止自动重传模式
* 16位自由运行定时器
* 可在最后两个数据字节发送时间戳管理
* 中断可屏蔽

## 初学者需要关注的几个重点

### 隐性位与显性位

CAN总线为“隐形”（逻辑1）时，CAN_H和CAN_L的电平为2.5V（电位差为0）；

CAN总线为“显形”（逻辑0）时，CAN_H和CAN_L的电平分别是3.5V和1.5V（电位差为2V）；

## 数据帧类型

1. 标准数据帧

	首先12位仲裁字段，前11位id号，1位rtr决定了是数据帧还是远程帧

	然后6位控制字段，1位IDE是标准帧还是扩展帧，区别主要在仲裁字段，1位RB0，4位DLC3~DLC0，数据长度。

	数据字段8位，N大于等于0小于等于8。

	CRC字段16位，CRC校验15位，CRC定界符1位

	确认间隙位1，确认定界符1，结束位7，IFS 3。

2. 扩展数据帧

	仲裁字段32位

3. 标准远程帧

	12仲裁字段，

	6位控制字段，

	16位CRC字段，

	确认间隙位1，确认定界符1，帧结束7

4. 扩展远程帧

	仲裁字段32位

## STM32内部寄存器

32位寄存器，31~21位存的是标准标识符，扩展身份标识的高字节。

位20~3，EXID[10:0]:扩展标识符，扩展身份标识的低字节。

位2: IDE:标识符选择，该位决定发送邮箱中报文使用的标识符类型，0：使用标准标识符， 1：使用扩展标识符。

位1：RTR：远程发送请求，0：数据帧，1：远程帧。

位0：TXRQ：发送数据请求，由软件对其置1，来请求发送其邮箱的数据。当数据发送完成时，邮箱为空时，硬件对其清0.

与接收FIFO的邮箱的发送其邮箱的数据。当数据发送完成，邮箱为空时，硬件对其清0.

## 位时间特性

CAN总线中的所有期间都必须使用相同的比特率。然而，并非所有器件都要求具有相同的主振荡器时钟频率。对于采用不同时钟频率的器件，应通过适当设置波特率预分频比以及每一时间段中的时间份额的数量来对波特率进行调整。

* 同步段(SYNC_SEG):同步段为首段，用于同步CAN总线上的各个节点。输入信号的跳变沿就发生在同步段，该段持续时间为1TQ（一个时间份额）。
* 时间段1（BS1）：定义采样点的位置。其值可以编程为1到16个时间单元，但也可以被自动延长，以补偿因为网络中不同节点的频率差异所造成的相位的正向漂移。
* 时间段2（BS2）：定义发送点的位置。其值可以变成为1到8个时间单元，但也可以被自动缩短以补偿相位的负向漂移。

重新同步跳跃宽度(SJW)定义了，在每位中可以延长或缩短多少个时间单元的上限。其值可以编程为1到4个时间单元。

### 寄存器说明

位31： SLAM：静默模式（用于调试）（Silent mode(debug)）, 0: 正常状态， 1：静默模式。

位30： LBKM：环回模式（用于调试）（Loop back mode(debug)），0：禁止环回模式，1：允许环回模式。

位29：26 保留位，硬件强制为0.

位25：24 SJW[1:0]：重新同步跳跃宽度(Ewsynchronization jump width) 为了重新同步， 该位域定义了CAN硬件在每位中可延长或缩短多少个时间单元的上限。tRJW = tCAN x (SJW[1:0] + 1)

位23： 保留位，硬件强制为0

位22：20 TS1[3:0]:时间段2（Time segment 2） 该位域定义了时间段2占用了多少个时间单元 tBS2 = tCAN x (TS[2:0] + 1)

位19：16 TS1[3:0]:时间段1（Time segment 1） 该位域定义了时间段1占用了多少个时间单元 tBS1 = tCAN x (TS[3:0] + 1)

位15：10 保留位，硬件强制其值为0

位9：0 波特率分频器。

### CAN波特率计算公式

CAN波特率 = 系统时钟/分频数/(1*tq+tBS1+tBS2)

总体配置保持

tBS1大于等于tBS2， tBS2大于等于1个CAN时钟周期，tBS2大于等于2tSJW

可以按照CAN列表设置。

## 屏蔽滤波

1. 屏蔽位模式

屏蔽位模式下，标识符寄存器和屏蔽寄存器一起，指定报文标识符的任何一位，应当按照“必须匹配”或“不用关心”处理。

2. 标识符列表模式

在标识符列表模式下，屏蔽寄存器也被当做标识符寄存器用。因此，不是采用一个标识符加一个屏蔽位的方式，而是采用2个标识符寄存器。接收报文标识符的每一位都必须跟过滤器标识符相同。

为了过滤出一组标识符，因该设置过滤器组工作在屏蔽位模式。

为了过滤出一个标识符，应该设置过滤器组工作在标识符列表模式。

## bxCAN工作模式

bxCAN有三个主要的工作模式：初始化，正常和睡眠模式

还包括：测试模式、静默模式、环回模式、环回静默模式。

## 环回模式

在环回模式下，bxCAN把发送的报文当做接收的报文并保存（如果可以通过接收过滤）在接收邮箱里。也就是环回模式是一个自发自收的模式。环回模式用于自测试。为了避免外部的影响，在环回模式下CAN内核忽略确认错误（在数据/远程帧的确认位时刻，不检测是否有显性位。)在环回模式下,bxCAN在内部把Tx输出回馈到Rx输入上,而完全忽略CANRX引脚的实际状态。发送的报文可以在CANTx引脚上检测到。



## CAN发送流程

CAN发送流程为：程序选择一个空置的邮箱(TME=1)->设置标识符(ID),数据长度和发送数据->设置CAN_TIxR的TXRQ位为1，请求发送->邮箱挂号(等待成为最高优先级)->预订发送（等待总线空闲）->发送->邮箱空置

## CAN接收流程

CAN的接收流程为:FIFO 空->收到有效报文->挂号_1(存入FIFO的一个邮箱,这个由硬件 控制,我们不需要理会)->收到有效报文->挂号_2->收到有效报文->挂号_3->收到有效报文-> 溢出。

## 实验介绍

本章,我们通过KEY_UP按键选择CAN的工作模式(正常模式/环回模式),然后通过KEY0控制数据发送,并通过查询的办法,将接收到的数据显示在LCD模块上。如果是环回模式,我们用一个开发板即可测试。如果是正常模式,我们就需要2个阿波罗开发板,并且将他们的CAN接口对接起来,然后一个开发板发送数据,另外一个开发板将接收到的数据显示在LCD模块上。

（1）配置相关引脚的复用功能(AF9),使能 CAN 时钟。

我们要用CAN,第一步就要使能CAN的时钟,CAN的时钟通过APB1ENR的第25位来设置。其次要设置CAN的相关引脚为复用输出,这里我们需要设置PA11(CAN1_RX)和PA12
(CAN1_TX)为复用功能(AF9),并使能PA口的时钟。具体配置过程如下:

```
GPIO_InitTypeDef GPIO_Initure;
__HAL_RCC_CAN1_CLK_ENABLE(); //使能CAN1时钟
__HAL_RCC_GPIOA_CLK_ENABLE(); //开启GPIOA时钟

GPIO_Initure.Pin=GPIO_PIN_11|GPIO_PIN_12; //PA11,12
GPIO_Initure.Mode=GPIO_MODE_AF_PP; //推挽复用
GPIO_Initure.Pull=GPIO_PULLUP; //上拉
GPIO_Initure.Speed=GPIO_SPEED_FAST; //快速
GPIO_Initure.Alternate=GPIO_AF9_CAN1; //复用为 CAN1
HAL_GPIO_Init(GPIOA,&GPIO_Initure); //初始化

```

这里需要提醒一下,CAN 发送接受引脚是哪些口,可以在中文参考手册引脚表里面查找。

 (2) 设置 CAN 工作模式及波特率等。

这一步通过先设置CAN_MCR寄存器的INRQ位,让CAN进入初始化模式,然后设置
CAN_MCR的其他相关控制位。再通过CAN_BTR设置波特率和工作模式(正常模式/环回模式)等信息。最后设置INRQ为0,退出初始化模式。

这一步通过先设置CAN_MCR寄存器的INRQ位,让CAN进入初始化模式,然后设置CAN_MCR的其他相关控制位。再通过CAN_BTR设置波特率和工作模式(正常模式/环回模式)等信息。最后设置INRQ为0,退出初始化模式。

在库函数中,提供了函数HAL_CAN_Init用来初始化CAN的工作模式以及波特率,HAL_CAN_Init函数体中,在初始化之前,会设置CAN_MCR寄存器的INRQ为1让其进入初始化模式,然后初始化CAN_MCR寄存器和CRN_BTR寄存器之后,会设置CAN_MCR寄存器的INRQ为0让其退出初始化模式。所以我们在调用这个函数的前后不需要再进行初始化模式设置。下面我们来看看HAL_CAN_Init函数的声明:

```
HAL_StatusTypeDef HAL_CAN_Init(CAN_HandleTypeDef* hcan);
```

该函数入口参数只有 hcan 一个,为 CAN_HandleTypeDef 结构体指针类型,接下来我们看 看结构体CAN_HandleTypeDef的定义。



