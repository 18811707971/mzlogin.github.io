---
layout: post
title: STM32中晶振的原理与作用
date: 2017-10-30
categories: STM32
tags: [STM32,XTAL,SIGNAL]
description: XIAT STUDY FOR STM32
---

转载地址:[STM32中晶振的原理与作用](http://www.stmcu.org/module/forum/thread-612187-1-8.html)
 
晶振在电气上可以等效成一个电容和一个电阻并联再串联一个电容的二端网络，电工学上这个网络有两个谐振点，以频率的高低分其中较低的频率为串联谐振，较高的频率为并联谐振。由于晶体自身的特性致使这两个频率的距离相当的接近，在这个极窄的频率范围内，晶振等效为一个电感，所以只要晶振的两端并联上合适的电容它就会组成并联谐振电路。这个并联谐振电路加到一个负反馈电路中就可以构成正弦波振荡电路，由于晶振等效为电感的频率范围很窄，所以即使其他元件的参数变化很大，这个振荡器的频率也不会有很大的变化。晶振有一个重要的参数，那就是负载电容值，选择与负载电容值相等的并联电容，就可以得到晶振标称的谐振频率。一般的晶振振荡电路都是在一个反相放大器（注意是放大器不是反相器）的两端接入晶振，再有两个电容分别接到晶振的两端，每个电容的另一端再接到地，这两个电容串联的容量值就应该等于负载电容，请注意一般IC的引脚都有等效输入电容，这个不能忽略。一般的晶振的负载电容为15p或12.5p，如果再考虑元件引脚的等效输入电容，则两个22p的电容构成晶振的振荡电路就是比较好的选择。我们具体看一下例子：

![这里写图片描述](http://img.blog.csdn.net/20171030145445572?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d0MTg4MTE3MDc5NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 Y1是晶体，相当于三点式里面的电感，C1和C2就是电容，5404非门和R1实现一个NPN的三极管，   5404必需要一个电阻，不然它处于饱和截止区，而不是放大区，R1相当于三极管的偏置作用，让5404处于放大区域，那么5404就是一个反相器，这个就实现了NPN三极管的作用，NPN三极管在共发射极接法时也是一个反相器。
 
 大家知道一个正弦振荡电路要振荡的条件是，系统放大倍数大于1，这个容易实现，相位满足360度，与晶振振荡频率相同的很小的振荡就被放大了。接下来主要讲解这个相位问题： 5404因为是反相器，也就是说实现了180°移相，那么就需要C1，C2和Y1实现180°移相就可以，恰好，当C1，C2，Y1形成谐振时，能够实现180移相，这个大家可以解方程等，把Y1当作一个电感来做。也可以用电容电感的特性，比如电容电压落后电流90°，电感电压超前电流90°来分析，都是可以的。当C1增大时,C2端的振幅增强，当C2降低时，振幅也增强。有些时候C1，C2不焊也能起振，这个不是说没有C1，C2，而是因为芯片引脚的分布电容引起的，因为本来这个C1，C2就不需要很大，所以这一点很重要。接下来分析这两个电容对振荡稳定性的影响。
 
因为5404的电压反馈是靠C2的，假设C2过大，反馈电压过低，这个也是不稳定，假设C2过小，反馈电压过高，储存能量过少，容易受外界干扰，也会辐射影响外界。C1的作用对C2恰好相反。因为我们布板的时候，假设双面板，比较厚的，那么分布电容的影响不是很大，假设在高密度多层板时，就需要考虑分布电容。如何得出晶振的负载电容：晶振线路两旁电容C1,C2.晶振匹配电容选择：[（C1*C2）/（C1+C2）]+(4~6PF)杂散电容.C1,C2是晶振旁边的两颗外接电容.

**在PCB布线中晶振的要求：**

1. 晶振内部存在石英晶体，受到外部撞击或跌落时易造成石英晶体断裂破损，进而造成晶振不起振，所以在设计电路时要考虑晶振的可靠安装，其位置靠近CPU 芯片优先放置，远离板边。

2. 在手工焊接或机器焊接时，要注意焊接温度。晶振对温度比较敏感，焊接时温度不能过高，并且加热时间尽量短｡

3. 耦合电容应尽量靠近晶振的电源引脚，位置摆放顺序：按电源流入方向，依容值从大到小依次摆放，容值最小的电容最靠近电源引脚。

4. 晶振的外壳必须接地，可以晶振的向外辐射，也可以屏蔽外来信号对晶振的干扰。

5. 晶振下面不要布线，保证完全铺地，同时在晶振的300mil范围内不要布线，这样可以防止晶振干扰其他布线、器件和层的性能。

6. 时钟信号的走线应尽量短，线宽大一些，在布线长度和远离发热源上寻找平衡。

7. 进行包地处理
 
 ___

**参考：**

[晶振电路的PCB设计](http://blog.csdn.net/peng_258/article/details/78373005)
 
 
 
 