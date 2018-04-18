---
layout: post
title: 13.56MHz天线绘制
date: 2018-03-22
categories: 电子之路
tags: [antenna,PCB,RFID]
description: 13.56MHz读卡天线的设计
---


转载自[13.56MHz天线绘制](https://wenku.baidu.com/view/314c64cea45177232e60a20b.html)

_____

## **1.13.56Mhz天线简介**


![这里写图片描述](https://img-blog.csdn.net/20180322223158726?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d3dDE4ODExNzA3OTcx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

                                                           图1 天线电路


如图1所示，13.56Mhz读卡器天线电路包括两大部分，其中黄色区域是信号接收电路；下面的蓝、绿、土黄色区域是信号发射电路。下面分别介绍两部分电路。

____

**发射电路：**

信号发射部分可细分为**EMC滤波电路**、**谐振与阻抗匹配电路**、**线圈**三部分。其中：

**EMC滤波电路：**主要是由**LC低通滤波电路组成低通滤波器**，读卡芯片经由TX1和TX2送出的天线信号主要是13.56Mhz，但是不可避免也会有高次谐波存在。所以该部分的低通滤波器主要作用就是滤除高于13.56Mhz的无用信号。这样即有利于读卡器与卡片之间的正常通信，也能减少天线部分对空间或者附近电路的电磁干扰。

**匹配电路：**

匹配电路形式上图中土黄色区域，此部分主要是**调整整个天线发射部分的谐振频率点13.56Mhz
附近**，这样可以使得线圈上的信号幅度增加有利于磁场辐射。另外匹配电路还要**将发射部分电路的电阻匹配到与读卡芯片的输出电阻附件**，典型的是50欧姆（不同芯片不一样）。这样可以使得天线部分获得最大功率有利于读卡距离提升。

 **线圈：**线圈可以是**PCB线圈或者铜线绕制线圈**。
____

**接收电路：**信号接收电路比较简单，由四个元器件构成，图中黄色区域Cmin电容可稳定读卡芯片内部提供的固定参考电压Vmin，R1则将此参考电压引入到RX引脚，为芯片的接收信号添加固定直流电平，CRx则从发生电路引入反馈信号与Vmin叠加后送入芯片内部。通过调节R2和R1的比值可以调节Rx脚信号的幅度，使得芯片的读卡距离最佳。

## **2.硬件电路设计**

以图1中电路为例天线部分电路设计应该遵循以下原则：

电路形式要做一些改进：在电感L0左侧串联0Ω电阻，将来断开电阻接入分析仪进行观察调试会比较方便，否则就只能把L0翘起来调试，这是比较麻烦的。应把C0、C2都分成2 只并联的容，便于将来调整参数。

 **PCB设计注意：**

整个发射部分电路一定要注意对称设计，从Tx1和Tx2出来的两路信号的走线长度尽量做到一致。两只电感应该用0805以上的封装，以保证有足够的电流通过（680nH/0805），否则不利于远距离读卡。

 PCB线圈的长宽视具体情况而定，如果电路板不受模具限制可设计为与普通卡片长宽一致，如果线圈大小受限制也可减小线圈面积。一般原则是设计为与所读的卡片大小一致最好；

再说线圈的圈数问题，长宽在3x3cm以上4圈即可，过多反而不好调整参数。如果是在3x3cm以下可以将圈数增加至6圈或者更多；PCB线圈走线宽度在0.5mm~1mm之间，间距与线宽相同即可，另外线圈拐角以圆弧过渡最好。

 **线圈部分:**

不可大面积敷铜，否则会引起磁场涡流效应造成能量严重损耗要注意线圈作用范围内（线圈周围全部空间）不可有大面积的金属元件、金属物体、金属镀膜等。整个发射电路所有器件的地必须连接到同一根地线上并且返回芯片的TVSS脚，且天线电路器件附近不可大面积覆铜，器件之间以导线连接。

 下面是天线电路设计实例：
 

![这里写图片描述](https://img-blog.csdn.net/20180322224941939?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d3dDE4ODExNzA3OTcx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

				图2 天线部分设计实例

_____


 如图2所示，L0、C01、C02构成两路发射信号的EMC滤波电路，这两部分是上下对称的。同理后面的匹配电路以及线圈都是对称设计，这样可以使得整个发射电路的性能最佳。

 **器件选型:**

 由于天线电路的性能与每个器件的参数都有关，所以选用器件时就要考虑器件的精度以及温度系数等问题。如图1所示电路选型建议如下：

 L0要选用封装在0805及以上的器件，且电感额定电流要在150mA以上。
 C0、C1、C2要选用精度为±5%或者更高等级，COG材质（温度系数小）的电容。这样才能保证批量生产时产品的性能一致性。


_____

文章地址：(https://wenku.baidu.com/view/314c64cea45177232e60a20b.html)
 

 


