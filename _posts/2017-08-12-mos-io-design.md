---
layout: post
title: MOS管，三极管基础知识总结
date: 2017-08-12
categories: 电子之路
tags: [MOS,三极管,推挽,开漏]
description: MOS
---

1.**MOS管符号箭头指向**

在所有半导体元件中, 箭头的意义表示p-n结的方向。

场效应管是电压控制型元器件，只要对栅极施加电压，DS就会导通。

三极管是电流控制型元器件，只要基极B有输入（或输出）电流就可以对这个晶体管进行控制。

![这里写图片描述](http://img.blog.csdn.net/20170808144524847?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

eg:

**N-channel-JFET:**

![这里写图片描述](http://img.blog.csdn.net/20170811115650677?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**P-channel JFET:**

![这里写图片描述](http://img.blog.csdn.net/20170811131810263?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**电路中的 MOS 管符号：**

![这里写图片描述](http://img.blog.csdn.net/20170811114527938?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**寄生二极管的方向：**

|MOS 管|指向|
|------:|------:|
|NMOS   |S极——>D极|
|PMOS   |D极——>S极|

寄生（体）二极管只在单个的MOS管中存在，在集成电路芯片内部通常是没有的。

**MOS管导通特性：**

**NMOS**：Vgs **大于** 一定的值就会导通，适合用于源极接地时的情况（低端驱动），只要栅极电压达到4V或10V就可以了。

**PMOS：**Vgs **小** 于一定的值就会导通，适合用于源极接VCC时的情况（高端驱动）。但是，虽然PMOS可以很方便地用作高端驱动，但由于**导通电阻大**，价格贵，替换种类少等原因，在高端驱动中，通常还是使用NMOS。

MOS 管做开关电路时的连接：**体二极管的负极输入，正极接地或者输出。**

**NMOS: D输入，S输出**
**PMOS: S输入，D输出**

![这里写图片描述](http://img.blog.csdn.net/20170811153241083?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**电路中的三极管符号：**

![这里写图片描述](http://img.blog.csdn.net/20170811132608215?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
_______

2.**推挽输出(Push-Pull)**

推挽输出是用两个晶体管或者场效应管构成的推挽电路，电路的特点就是输出电阻小。可以输出高,低电平,连接数字器件。push－pull 高低电平由IC的电源低定，不能简单的做逻辑操作等。push－pull是现在CMOS电路里面用得最多的输出级设计方式。

![这里写图片描述](http://img.blog.csdn.net/20170811141610014?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

在功率放大器电路中经常采用推挽放大器电路，这种电路中用两只三极管构成一级放大器电路，如图所示。两只三极管分别放大输入信号的正半周和负半周，即用一只三极管放大信号的正半周，用另一只三极管放大信号的负半周，两只三极管输出的半周信号在放大器负载上合并后得到一个完整周期的输出信号。​

**推：**当Vin电压为V+时，上面的N型三极管控制端有电流输入，Q1导通，于是电流从上往下通过，提供电流给负载，过上面的N型三极管提供电流给负载。

**挽：**当Vin电压为V-时，下面的三极管有电流流出，Q2导通，有电流从上往下流过，下面的P型三极管提供电流给负载。

推挽放大器电路中，一只三极管工作在导通、放大状态时，另一只三极管处于截止状态，当输入信号变化到另一个半周后，原先导通、放大的三极管进入截止，而原先截止的三极管进入导通、放大状态，两只三极管在不断地交替导通放大和截止变化，所以称为推挽放大器。输出既可以向负载灌电流，也可以从负载抽取电流.​

**3.开漏输出(Open-Drain)**

单片机I/O常用的输出方式的开漏输出（Open-Drain），漏极开路电路概念中提到的“漏”是指 MOSFET的漏极。同理，集电极开路电路中的“集”就是指三极管的集电极。在数字电路中，分别简称OD门和OC门。​

典型的集电极开路电路如图所示。电路中右侧的三极管集电极什么都不接，所以叫做集电极开路，左侧的三极管用于反相作用，即左侧输入“0”时左侧三极管截止，VCC通过电阻加到右侧三极管基极，右侧三极管导通，右侧输出端连接到地，输出“0”。​

![这里写图片描述](http://img.blog.csdn.net/20170811143310126?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

从图中电路可以看出集电极开路是无法输出高电平的，输出端悬空时变为高阻态，这时电平状态未知（**对于经典的51单片机的P0口而言，要想做输入输出功能必须加外部上拉电阻，否则无法输出高电平逻辑**），如果要想输出高电平可以在输出端加上上拉电阻。因此集电极开路输出可以用做电平转换，通过上拉电阻上拉至不同的电压，来实现不同的电平转换。

集电极开路输出除了可以**实现多门的线与逻辑**关系外，通过**使用大功率的三极管还可用于直接驱动较大电流的负载**，如继电器、脉冲变压器、指示灯等。由于现在MOS管用普遍，而且性能要比晶体管要好，所以很多开漏输出电路和推挽输出电路都用MOS管实现。

![这里写图片描述](http://img.blog.csdn.net/20170811145218680?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**完整的开漏电路应由开漏器件和开漏上拉电阻组成**。这里的上拉电阻R的阻值决定了**逻辑电平转换的上升/下降沿的速度。阻值越大，速度越低，功耗越小。**因此在选择上拉电阻时要兼顾功耗和速度。标准的开漏脚一般只有输出的能力。添加其它的判断电路，才能具备双向输入、输出的能力。​

开漏电路做驱动器时，由于OC门电路的输出管的集电极悬空，使用时需外接一个上拉电阻R到电源VCC。OC门使用上拉电阻以输出高电平，此外为了加大输出引脚的驱动能力。上拉电阻阻值的选择原则：**从降低功耗及芯片的灌电流能力考虑应当足够大；从确保足够的驱动电流考虑应当足够小。**

可以利用改变上拉电源的电压，改变传输电平。如上图， IC的逻辑电平由电源Vcc1决定，而输出高电平则由Vcc2（上拉电阻的电源电压）决定。这样我们就可以**用低电平逻辑控制输出高电平逻辑**了（这样就可以进行任意电平的转换）。（例如加上上拉电阻就可以提供TTL/CMOS电平输出等。）

将OC门输出连在一起时，再通过一个电阻接外电源，可以实现**“线与”**逻辑关系。只要电阻的阻值和外电源电压的数值选择得当，就能做到既保证输出的高、低电平符合要求，而且输出三极管的负载电流又不至于过大。**当这些引脚的任一路变为逻辑0后，开漏线上的逻辑就为0了。**在I2C等接口总线中就用此法判断总线占用状态。

![这里写图片描述](http://img.blog.csdn.net/20170811144704408?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

同集电极开路一样，利用外部电路的驱动能力(驱动能力相对集电极开路要强一点)，减少IC内部的驱动。当IC内部MOSFET导通时，驱动电流是从外部的VCC流经上拉电阻，再经MOSFET到GND。IC内部仅需很小的栅极驱动电流，因此漏极开路也常用于驱动电路中。

开漏输出提供了灵活的输出方式，**但是也有其弱点，就是带来上升沿的延时。**因为上升沿是通过外接上拉无源电阻对负载充电，所以当电阻选择小时延时就小，但功耗大；反之延时大功耗小。所以如果对延时有要求，则建议用下降沿输出。

**电阻小延时小**的前提条件是电阻选择的原则应在末级晶体管**功耗**允许范围内，有经验的设计者在使用逻辑芯片时，不会选择1欧姆的电阻作为上拉电阻。在脉冲的上升沿电源通过上拉无源电阻对负载充电，显然电阻越小上升时间越短，在脉冲的下降沿，除了负载通过有源晶体管放电外，电源也通过上拉电阻和导通的晶体管对地 形成通路，带来的问题是芯片的功耗和耗电问题。**电阻影响上升沿，不影响下降沿**。如果使用中不关心上升沿，上拉电阻就可选择尽可能的大点，以减少对地通路的 电流。如果对上升沿时间要求较高，电阻大小的选择应以芯片功耗为参考。

**参考：**

1.[MOS管符号箭头指向问题](https://www.zhihu.com/question/27955221/answer/38939126)

2.[如何正确的理解漏极开路输出跟推挽输出](https://www.zhihu.com/question/28512432/answer/41217074)

3.[详细解说开漏输出和推挽输出](http://blog.sina.com.cn/s/blog_14e0394720102vewa.html)

4.[MOS管原理_非常详细](https://wenku.baidu.com/view/677d0b56a300a6c30c229f5a.html?from=search)

5.[单片机I/O口推挽输出与开漏输出的区别（转）](http://blog.csdn.net/xiaoweiboy/article/details/6714199)

6.[MOS管驱动电路，看这里就啥都懂了！](http://www.eefocus.com/mygod12345/blog/16-08/389677_7db05.html)

7.[从头到脚带你了解COOL MOSFET的EMI设计](http://ledlight.eefocus.com/module/forum/thread-598781-1-1.html)