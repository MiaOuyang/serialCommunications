# 1 USART介绍
## 1.1 串口的基本概念
在STM32的参考手册中，串口被描述成通用同步异步收发器(USART)，它提供了一种灵活的方法与使用工业标准NRZ异步串行数据格式的外部设备之间进行全双工数据交换。USART利用分数波特率发生器提供宽范围的波特率选择。它支持同步单向通信和半双工单线通信，也支持LIN（局部互联网），智能卡协议和IrDA（红外数据组织）SIR ENDEC规范，以及调制解调器(CTS/RTS)操作。它还允许多处理器通信。还可以使用DMA方式，实现高速数据通信。
USART通过3个引脚与其他设备连接在一起，任何USART双向通信至少需要2个引脚：**接受数据输入(RX)和发送数据输出(TX)**。

**RX**: 接受数据串行输入。通过过采样技术来区别数据和噪音，从而恢复数据。

**TX**: 发送数据输出。当发送器被禁止时，输出引脚恢复到它的I/O端口配置。当发送器被激活，并且不发送数据时，TX引脚处处于高电平。在单线和智能卡模式里，此I/O口被同时用于数据的发送和接收。

## 1.2 串口的工作方式
**一般有两种工作方式：查询与中断**

 1. 查询
串口程序不断地循环查询，看看当前有没有数据要它传送。如果有，就帮助传送（可以从PC到STM32板子，也可以从STM32板子到PC）
2. 中断
平时串口只要打开中断即可。如果发现有一个中断来，则意味着要它帮助传输数据——它就马上进行数据的传送。同样，可以从 PC到STM32板子，也可以从STM32板子到PC

## 1.3 USART介绍
USART/UART通信是STM32的一个非常重要的外设，是一种通用串行数据总线，可实现全双工通信。
**UART**：通用异步收发器，
**USART**:通用同步/异步收发器，
可以看出USART比UART多了一个同步模式。

1. **异步通信**：
异步通信是按字符传输的。每传输一个字符就用起始位来进来收、发双方的同步。不会因收发双方的时钟频率的小的偏差导致错误。
这种传输方式利用每一帧的起、止信号来建立发送与接收之间的同步。特点是：每帧内部各位均采用固定的时间间隔，而帧与帧之间的间隔时随即的。接收机完全靠每一帧的起始位和停止位来识别字符时正在进行传输还是传输结束。
2. **同步通信**：
进行数据传输时，发送和接收双方要保持完全的同步，因此，要求接收和发送设备必须使用同一时钟。
优点是可以实现高速度、大容量的数据传送；缺点是要求发生时钟和接收时钟保持严格同步，同时硬件复杂。

本文采用异步通信

# 2 运行结果
## 2.1 仿真
编译后通过仿真查看端口输出，调试打开UART1窗口

![在这里插入图片描述](https://img-blog.csdnimg.cn/d70a54e890f44cf78541502bf45986a8.png)

打开逻辑分析仪，观察A4管脚的波形图，运行结果如下

![在这里插入图片描述](https://img-blog.csdnimg.cn/ce48784d9cbb4f82afc484422ef5c872.png)

观察可知，PA4端口在每次发送信息后，灯亮大约0.1s后熄灭，等待0.9s后循环上述步骤

![在这里插入图片描述](https://img-blog.csdnimg.cn/ac8e2e708cec4d1bb9d7ecafa6c6c929.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/76ff2239b78d4632acae4e04f3bf7ff8.png)

结果与编程预期想达到的效果一致

## 2.2 实际运行结果
打开串口调试助手，打开串口，得到结果如下

![在这里插入图片描述](https://img-blog.csdnimg.cn/ec731340740b48febbd76321c593bde3.png)
