# Play Python on Raspberry Pi Pico with MDK

It is well known that MicroPython supports the Raspberry Pi Pico, and we see there some room to improve, not only about the memory footprint, but also about the way to bind your own c modules. It's not rare to see people in the community complain about the complexity and debugging experience.

The resources and price of the Raspberry Pi Pico are really good, it is fun to play with, not to mention there is a big community behind it. One question for most of the MCU developer is that can we use MDK to develop Raspberry Pi Pico and play with PikaScript? Why not? Thanks to a open-source project called [Pico_Template](https://github.com/GorgonMeducer/Pico_Template), dream becomes reality. Please note that Pico_Template allows you to compile the latest pico-sdk using the Arm Compiler 6, debug without an extra pico and retarget printf to MDK without using any Serial2USB adapter. 

For details, see:

[I'm going to use MDK to develop Raspberry Pi Pico, how come!](Http://mp.weixin.qq.com/s?__biz=MzAxMzc2ODMzNg==&mid=2656103324&idx=1&sn=f1d3ece87c81eeaa7d402f3cba60dc8f&chksm=8039c863b74e4175edc806b4e329c25e75b6372df53f07565bd9a46cfbf13a3c4cd9e20c08cc#rd)

As we mentioned before, binding C modules in MicroPython is very complicated and difficult to debug. Is there a more convenient way to do it for python running on MCUs? 

YES! Our answer to this question is PikaScript. PikaScript is a completely rewritten ultra-lightweight python vitual machine, with zero dependency on toolchain, simple configuration, ultra-low memory footprint (i.e. you can use it with less than 4KB of SRAM). Using framework based C module development tools, your API calling written in Python can be automatically connect to the your C modules. Cannot be more simple or convenient, isn't it? No need to manually handle any global tables, macro functions, module registration, etc.

PikaScript provides MDK projects, hence you can debug C modules with python scripts.  

For details, see:
[I'm going to use the cheapest single-chip microcomputer to run python, and I also need to use MDK to develop it, what's the matter](Http://mp.weixin.qq.com/s?__biz=MzU4NzUzMDc1OA==&mid=2247484313&idx=1&sn=2749a27bba09b2fe9c7bc0ad4977c8a6&chksm=fdebd4f0ca9c5de6f9160d42c58aa5d5e072168752c826cbf82f700f1fc301b96a3aaf4cfcfd#rd)

In addition to pico, the portability of pikascript allows you to use it on a wide variety of platforms.
For example: stm32g0, stm32f1, ch32, apm32, cm32, as well as Pingtou's w801, Boliu's bl-706...

![](assets/1640497097904-f2b13577-44ee-4510-a7ce-e18dd01aaa20.webp)

The very popular ESP32C3, Godson architecture, and this time the Raspberry Pi Pico discussed in this document.

![](assets/1640497097922-8490fdc1-ba88-48a4-888b-3859384ca650.webp)

PikaScript supports both bare-metal but also RTOS enviroment, for example [RT-Thread](https://github.com/RT-Thread/rt-thread), [VSF](https://github.com/vsfteam/vsf), and Linux.

In fact, PikaScript is deeply integrated with rt-thread, it supports rt-thread full series of BSP via software packages. 
![](assets/1640497097898-69cdc136-7b7a-4a8c-b79c-0650ae3f5111.webp)


Let's see how to play PikaScript on Raspberry Pi pico using MDK:

[https://github.com/pikastech/pikascript/tree/master/bsp/pico#pikascript-in-pico](https://github.com/pikastech/pikascript/tree/master/bsp/pico#pikascript-in-pico)

If you can see the following information (or similar) on the Debug(printf) View, congrats!

![](assets/1640497099248-1358725f-072c-4810-a999-c9d372575f19.webp)

Enjoy!

More technical support please check in the forum or raising issues. Thank you.

https://whycan.com/f_55.html

![](assets/1640497099365-67930749-d3a2-4f70-9320-73da30f72659.webp)
