# Play Python on Raspberry Pi Pico with MDK

We know that the Raspberry Pi Pico can directly use the MicroPython firmware to play Python, but the Micropython firmware is very difficult to develop. If you want to bind the C module yourself, it is very troublesome, you need to rely on a lot of tools in the Linux ecosystem, and it is difficult to debug.

However, the hardware resources and price of the Raspberry Pi Pico are really good, and the features such as pio are also very playable. So, can we use a down-to-earth development method on a platform that we are familiar with, such as in MDK, to develop a tree What about the Raspberry Pi Pico? Of course, please invite the Pico_template of the "silly boy" boss, which allows you to develop Raspberry Pi pico in the familiar MDK, and also supports debugger-free single-step debugging (use one core to debug another).

For details, see:

[I'm going to use MDK to develop Raspberry Pi Pico, how come!](Http://mp.weixin.qq.com/s?__biz=MzAxMzc2ODMzNg==&mid=2656103324&idx=1&sn=f1d3ece87c81eeaa7d402f3cba60dc8f&chksm=8039c863b74e4175edc806b4e329c25e75b6372df53f07565bd9a46cfbf13a3c4cd9e20c08cc#rd)

Binding C modules in MicroPython is very complicated and difficult to debug. Is there a more convenient technology that can easily bind C modules?

Yes, that is PikaScript, PikaScript is a completely rewritten super lightweight python engine, zero dependencies, zero configuration, can run under less than 4KB of RAM, with framework-style C module development tools, as long as it is written in Python Calling the API can automatically connect to the C module, which is very convenient and fast. No need to manually deal with any global tables, macro functions, module registration, etc.

And PikaScript also supports MDK development, which can easily debug C modules.

For details, see:
[I'm going to use the cheapest single-chip microcomputer to run python, and I also need to use MDK to develop it, what's the matter](Http://mp.weixin.qq.com/s?__biz=MzU4NzUzMDc1OA==&mid=2247484313&idx=1&sn=2749a27bba09b2fe9c7bc0ad4977c8a6&chksm=fdebd4f0ca9c5de6f9160d42c58aa5d5e072168752c826cbf82f700f1fc301b96a3aaf4cfcfd#rd)

In addition to pico, the portability of pikascript allows it to run on a wide variety of platforms.

From stm32g0, stm32f1, to domestic ch32, apm32, cm32, as well as Pingtou's w801, Boliu's bl-706, all are supported.

![](assets/1640497097904-f2b13577-44ee-4510-a7ce-e18dd01aaa20.webp)

The very popular ESP32C3, Godson architecture, and this time the protagonist Raspberry Pi Pico.

![](assets/1640497097922-8490fdc1-ba88-48a4-888b-3859384ca650.webp)

In addition to supporting bare metal, it also supports rt-thread, vsf operating systems, and linux operating systems.

And deeply integrated with rt-thread, it can support rt-thread full series of BSP based on software package
![](assets/1640497097898-69cdc136-7b7a-4a8c-b79c-0650ae3f5111.webp)

Let's get to the point and see how to develop with MDK on the Raspberry Pi pico and play with Python.
Instructions for use:

[https://gitee.com/Lyon1998/pikascript/tree/master/bsp/pico#pikascript-in-pico](https://gitee.com/Lyon1998/pikascript/tree/master/bsp/pico#pikascript-in-pico)

If you can see the following information on the serial port, it means the operation is successful!

![](assets/1640497099248-1358725f-072c-4810-a999-c9d372575f19.webp)

Enjoy!

More technical support can be discussed in the forum~

https://whycan.com/f_55.html

![](assets/1640497099365-67930749-d3a2-4f70-9320-73da30f72659.webp)
