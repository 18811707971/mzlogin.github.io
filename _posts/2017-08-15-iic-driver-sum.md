---
layout: post
title: IIC专题（一）——基础知识准备
date: 2017-08-15
categories: IIC
tags: [IIC,DRIVER,BUS]
description: IIC STUDY AND APPLICATION
---

这几天看到原子哥 STM32 的 IIC 了，打算认真扎实的把 IIC 好好学一学，巩固加强一下。以前在学校也弄过，但很多地方理解的不够深刻，做事，学知识，不能浅尝辄止，而要扎扎实实，搞明白原理，不断实践，不断总结，才能有所提高，有所得。后续计划从51，STM32，iTop4412 额Linux下来实现 IIC 驱动，结合工作中的需求，从不同外设来实现。

**1.IIC简介：**      

**IIC** 即 Inter-Integrated Circuit(集成电路总线），这种总线类型是由飞利浦半导体公司在八十年代初设计出来的，主要是用来连接整体电路(ICS) ，IIC 是一种多向控制总线，也就是说多个芯片可以连接到同一总线结构下，同时每个芯片都可以作为实时数据传输的控制源。这种方式简化了信号传输总线接口。

IIC 最初为音频和视频设备开发，如今主要在服务器管理中使用，其中包括单个组件状态的通信。例如管理员可对各个组件进行查询，以管理系统的配置或掌握组件的功能状态，如电源和系统风扇。可随时监控内存、硬盘、网络、系统温度等多个参数，增加了系统的安全性，方便了管理。 

IIC 的主要构成只有两个双向的信号线，一个是数据线 SDA,一个是时钟线 SCL。

**2.IIC的 特点：**

1. 具有多机功能，该模块既可以做主设备也可以做为从设备；

2. IIC 主设备功能，主要产生时钟，产生起始信号和停止信号；

3. IIC 从设备功能，可编程的 IIC 地址检测，每个电路和模块都有唯一的地址，停止位检测；

4. 串行8位双向数据，支持不同速率的通讯速度，标准速度(最高速度 **100kHZ** ),快速（最高 **400kHZ** ）,和高速模式（3.4 Mbps）；

5.  连接到相同总线的IC 数量只受到总线的最大电容 400pF 限制；

6. 支持多主控模块，但同一时刻只允许有一个主控；

7. **SCL**：上升沿将数据输入到每个EEPROM器件中；下降沿驱动EEPROM器件输出数据。**(边沿触发)**；**SDA**：双向数据线，为OD门，与其它任意数量的OD与OC门成"线与"关系。

8.  总线的长度可高达25英尺（762cm），并且能够以10Kbps的最大传输速率支持40个组件；
9. IIC自动寻址、高低速设备同步和仲裁等功能的高性能串行总线，为半双工通信方式。

**3.IO输出结构**

![这里写图片描述](http://img.blog.csdn.net/20170814221325257?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

每一个I2C总线器件内部的 SDA、SCL 引脚电路结构都是一样的，引脚的输出驱动与输入缓冲连在一起。其中输出为**漏极开路的场效应管**，输入缓冲为一只**高输入阻抗的同相器**，这种电路具有两个特点： 

（1）由于 SDA、SCL 为**漏极开路结构(OD)**，只能输出“0”，不能输出“1”，因此它们**必须接有上拉电阻,阻值的大小常为 1k8, 4k7 and 10k ，但1k8 时性能最好**；当**总线空闲时，两根线均为高电平**。连到总线上的任一器件输出的低电平，都将使总线的信号变低，即各器件的SDA及SCL都是线"与"关系。

（2）引脚在**输出信号**的同时还将引脚上的电平进行检测，检测是否与刚才输出一致，为"时钟同步"和"总线仲裁"提供了硬件基础。

**SCL线的时钟同步**：SCL由于具有线“与”的逻辑功能，即只要有**一个节点**发送**低电平**时，总线上就表现为低电平。当**所有的节点**都发送**高电平**时，总线才能表现为高电平。正是由于线“与”逻辑功能的原理，当多个节点同时发送时钟信号时，在总线上表现的是统一的时钟信号。

**SDA线的仲裁**：SDA线的仲裁也是建立在总线具有线“与”逻辑功能的原理上的。节点在发送1位数据后，比较总线上所呈现的数据与自己发送的是否一致。是，继续发送；否则，退出竞争。SDA线的仲裁可以保证I2C总线系统在多个主节点同时企图控制总线时通信正常进行并且数据不丢失。总线系统通过仲裁只允许一个主节点可以继续占据总线。

**4.IIC总线上器件的连接**

系统中的所有外围器件都具有一个**7位的"从器件专用地址码"**，其中**高4位为器件类型**，由生产厂家制定，**低3位为器件引脚定义地址**，由使用者定义。

![这里写图片描述](http://img.blog.csdn.net/20170814221502097?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

对于 **10 位地址格式**，必须发送两个字节。前五位用来指定采用的是 10 位地址格式，发送的第一个字节中包括用于指定 10 位地址的 5 位以及地址的
两个 MSb 和 1 个 R/W 位。第二个字节是其余的 8 位地址。 

![这里写图片描述](http://img.blog.csdn.net/20170815211507007?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

主控器件通过地址码建立多机通信的机制，因此 I2C 总线省去了外围器件的片选线，这样无论总线上挂接多少个器件，其系统仍然为简约的二线结构。终端挂载在总线上，有主端和从端之分，主端必须是带有 CPU 的逻辑模块，在同一总线上同一时刻使能有一个主端，可以有多个从端，从端的数量受地址空间和总线的最大电容 400pF 的限制。

![这里写图片描述](http://img.blog.csdn.net/20170814210639298?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

当多个主机试图去控制总线时，通过仲裁可以使得只有一个主机获得总线控制权，并且它传输的信息不被破坏。

**相关概念介绍：**

|名称|解释|
|---|:---|
|主机|初始化发送，产生时钟信号和终止发送的器件|
|从机|被主机寻址的器件|
|发送器|发送数据到总线的器件|
|接收器|从总线接收数据的器件|
|仲裁|一个在有多个主机同时尝试控制总线，只允许其中一个控制总线并使报文不被破坏|
|同步|两个或多个器件同步时钟信号的过程|

**数据格式说明：**

|格式|表示|
|---|:---|
|无数据|SCL=1，SDA=1|
|开始位（Start）|当SCL=1时，SDA由1向0跳变|
|停止位（Stop）|当SCL=1时，SDA由0向1跳变|
|数据位|当SCL由0向1跳变时，由发送方控制SDA，此时SDA为有效数据，不可随意改变SDA；当SCL保持为0时，SDA上的数据可随意改变；|
|地址位|定义同数据位，但只由Master发给Slave|
|应答位（ACK）|当发送方传送完8位时，发送方释放SDA，由接收方控制SDA，且SDA=0|
|非应答位（NACK）|当发送方传送完8位时，发送方释放SDA，由接收方控制SDA，且SDA=1|
|单字节传送|开始位，8位地址位（含1位读写位），应答，8位数据，应答，停止位|
|一串字节传送|开始位，8位地址位（含1位读写位），应答，8位数据，应答，8位数据，应答，……，8位数据，应答，停止位|

**5.I2C协议**

（1）空闲状态

I2C总线总线的SDA和SCL**两条信号线同时处于高电平**时，规定为总线的空闲状态。此时**各个器件的输出级场效应管均处在截止状态**，即**释放总线，由两条信号线各自的上拉电阻把电平拉高。** 

（2）起始信号与停止信号

**起始信号：**当**SCL为高期间，SDA由高到低的跳变**；启动信号是一种电平跳变时序信号，而不是一个电平信号。

**停止信号：**当**SCL为高期间，SDA由低到高的跳变**；停止信号也是一种电平跳变时序信号，而不是一个电平信号。

![这里写图片描述](http://img.blog.csdn.net/20170814212330985?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（3）应答信号ACK

发送器每**发送一个字节**，就在**时钟脉冲9期间释放数据线**，由接收器反馈一个**应答信号**。 **应答信号为低电平时，规定为有效应答位**（ACK简称应答位），表示接收器已经成功地接收了该字节；应答信号为高电平时，规定为非应答位（NACK），一般表示接收器接收该字节没有成功。 

对于反馈有效应答位ACK的要求是，**接收器在第9个时钟脉冲之前的低电平期间将SDA线拉低，并且确保在该时钟的高电平期间为稳定的低电平。** 如果接收器是主控器，则在它收到最后一个字节后，发送一个NACK信号，以通知被控发送器结束数据发送，并释放SDA线，以便主控接收器发送一个停止信号P。

![这里写图片描述](http://img.blog.csdn.net/20170814213441713?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（4）数据有效性

I2C总线**进行数据传送**时，**时钟信号为高电平期间，数据线上的数据必须保持稳定**，只有**在时钟线上的信号为低电平期间，数据线上的高电平或低电平状态才允许变化。** 

即：数据在SCL的上升沿到来之前就需准备好。并在在下降沿到来之前必须稳定。

![这里写图片描述](http://img.blog.csdn.net/20170814213617296?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

数据传输：先发送**起始位**，再发一个**8bit数据**：前7bit表示从地址，第8bit表示读或者写**。0 write是处理器往IIC从设备发**，**1 read是IIC从设备往处理器发**。第9个时钟周期回复响应信号。

![这里写图片描述](http://img.blog.csdn.net/20170814220319431?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（5）数据的传送

IIC 数据从最高位（MSB）开始传输，发送到SDA线上的数据必须是8位的，每一个被传送的字节后面都必须跟随一位应答位（即一帧共有9位），发送器发送完 LSB 之后，应当释放 SDA 线（拉高 SDA，输出晶体管截止） ，以等待接收器产生应答位。在 IIC 总线上传送的每一位数据都有一个时钟脉冲相对应（或同步控制），即**在SCL串行时钟的配合下，在SDA上逐位地串行传送每一位数据**。**数据位的传输是边沿触发。**

**典型时序：**

![这里写图片描述](http://img.blog.csdn.net/20170815210413603?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**完整的数据传送：**

![这里写图片描述](http://img.blog.csdn.net/20170814214529138?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**eg:传输 1011 0110**

![这里写图片描述](http://img.blog.csdn.net/20170814221031748?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**应答位的时钟脉冲仍由主机产生，而应答位的数据状态则遵循“谁接收谁产生”的原则，即总是由接收器产生应答位。**主机向从机发送数据时，应答位由从机产生；主机从从机接收数据时，应答位由主机产生。

 在切换数据的传输方向时，可以不必先产生停止条件再开始下次传输，而是直接再一次产生开始条件。I2C 总线在已经处于忙的状态下，再一次直接产生起始条件的情况被称为重复起始条件。

______

**6.读写数据过程**

**写通讯过程:**

    1. 主控在检测到总线空闲的状况下，首先发送一个START信号掌管总线； 
    
    2. 发送一个地址字节(包括7位地址码和一位R/W)；
    
    3. 当被控器件检测到主控发送的地址与自己的地址相同时发送一个应答信号(ACK)；
    
    4. 主控收到ACK后开始发送第一个数据字节；
   
    5. 被控器收到数据字节后发送一个ACK表示继续传送数据，发送NACK表示传送数据结束；
    
    6. 主控发送完全部数据后，发送一个停止位STOP，结束整个通讯并且释放总线；
 
 当写数据的时候，Master每发送完8个数据位，Slave设备如果还有空间接受下一个字节应该回答“ACK”，Slave设备如果没有空间接受更多的字节应该回答“NACK”，Master当收到“NACK”或者一定时间之后没收到任何数据将视为超时，此时Master放弃数据传送，发送“Stop”。

**读通讯过程:**

    1. 主控在检测到总线空闲的状况下，首先发送一个START信号掌管总线；
    
    2. 发送一个地址字节(包括7位地址码和一位R/W)；

    3. 当被控器件检测到主控发送的地址与自己的地址相同时发送一个应答信号(ACK)；
   
    4. 主控收到ACK后释放数据总线，开始接收第一个数据字节；
     
    5. 主控收到数据后发送ACK表示继续传送数据，发送NACK表示传送数据结束；
    
    6. 主控发送完全部数据后，发送一个停止位STOP，结束整个通讯并且释放总线；

当读数据的时候，Slave 设备每发送完 8 个数据位，如果 Master 希望继续读下一个字节，Master 应该回答“ACK”以提示Slave准备下一个数据，如果Master不希望读取更多字节，Master应该回答“NACK”以提示Slave设备准备接收Stop信号。

**主控器向被控器发送的信息种类有**：启动信号、停止信号、7位地址码、读／写控制位、10位地址码、数据字节、重启动信号、应答信号、时钟脉冲。
 
**被控器向主控器发送的信息种类有**：应答信号、数据字节、时钟低电平。

注：SCL一直由Master控制，SDA依照数据传送的方向，读数据时由Slave控制SDA，写数据时由Master控制SDA。当 8 位数据传送完毕之后，应答位或者否应答位的SDA控制权与数据位传送时相反。


注明：以上大部分资料均来自网络总结，参考处注明。

__________

##参考：

1.[I2C总线之(一)---概述 ](http://www.cnblogs.com/BitArt/archive/2013/05/27/3101037.html)

2.[i2c总线（iic总线/ I square C）](http://www.cnblogs.com/zhangjiankun/archive/2011/12/27/2302720.html)

3.[PROTEUS仿真学习笔记04 （IIC 外设）——2014_7_8](http://bbs.elecfans.com/jishu_441030_1_1.html)

4.[IIC总线解析 ](http://www.cnblogs.com/zalebool/p/4214599.html)

5.[关于I2C和SPI总线协议](http://blog.csdn.net/ce123_zhouwei/article/details/6878547)

6.[IIC总线协议](http://blog.csdn.net/zailushangha/article/details/8233448)

7.[i2c 相关知识总结（转）](http://blog.csdn.net/hygzxf/article/details/17416725)

8.[IIC的一点理解](http://www.eefocus.com/stm3222/blog/16-10/393817_4e6f0.html)