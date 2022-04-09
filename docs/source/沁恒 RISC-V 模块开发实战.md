# Qinheng RISC-V module development practice

Foreword:
This is an entry project of the RTT design competition, which realizes the deployment of the PikaScript Python engine based on rt-thread on the CH32V103 RISC-V platform.
Project webpage:
[http://www.elecfans.com/project/938](http://www.elecfans.com/project/938)​
(The page also includes a video demonstration)

The following is the text of the project introduction:
​

Project Description
PikaScript is a completely rewritten ultra-lightweight python engine with a full interpreter, bytecode and virtual machine architecture that can run on less than 4KB of RAM for small resource embedded systems. Compared with similar products, such as MicroPython, LuaOS, etc., the resource consumption is reduced by more than 85%. Selected as the most valuable open source project of Gitee in 2021, adding RT-Thread embedded real-time operating system programming language software package. Completed the deployment of PikaScript on the CH32V103 RISC-V development board, and submitted the PikaSciprt standard BSP and driver module package for CH32V103, and completed the interactive running driver.

**RT-Thread usage overview:**
The technology stack involved in the whole scheme includes: RT-Thread thread and timer, compilation principle, bytecode design, virtual machine design, PikaScript deployment technology and driver module development technology, etc. Through this work, the BSP support list of PikaScript is expanded, the compatibility of PikaScript and rt-thread is verified, and the deployment capability and compatibility of PikaScript in the small-capacity (64Kb) RISC-V architecture are verified.
Kernel part: threads and timers are used.
**Package:**

PikaScript package
​

The hardware uses the CH32V103 development board provided by the RTT competition, and uses the LED resources on the board to indicate the running status of the script. A Python script module is developed for the GPIO hardware to test the script-driven expansion function.
![](assets/1638495380404-1b88e98c-4325-48fa-824d-18a45c153b85.webp)

**Software Description**
**0.Summary**
​

PikaScript is a completely rewritten ultra-lightweight python engine with a full interpreter, bytecode and virtual machine architecture that can run on less than 4KB of RAM for small resource embedded systems. Compared with similar products, such as MicroPython, LuaOS, etc., the resource consumption is reduced by more than 85%.
Selected as the most valuable open source project of Gitee in 2021, adding RT-Thread embedded real-time operating system programming language software package.

This project completed the deployment of PikaScript on the CH32V103 RISC-V development board, submitted the PikaSciprt standard BSP and driver module package for CH32V103, and completed the driver for interactive operation.

**1. Scheme selection - CH32V103 runs Python script, which is not easy to handle**
First we need to choose an embedded Python interpreter capable of running on CH32.

To be able to deploy the Python interpreter on a RISC-V MCU with a flash of 64Kb, a very small compilation volume is required, and it cannot rely on the exclusive technology of the ARM architecture.
First, exclude the general-purpose Python interpreter CPython, not to mention that CPython needs to rely on linux, but the size can be excluded.

Secondly, the MicroPython technology that is popular in the embedded field is a possible alternative, but MicroPython requires a minimum volume of 128Kb on the ARM platform, and the optimization maturity of the GCC compiler of the RISC-V platform is not as good as the ARM platform, so the compilation volume will only be larger. Big will not be smaller, so MicroPython cannot be deployed on this CH32V103 platform.
​

Well, let’s not talk about it. The only Python interpreter that can be deployed on the CH32V103 platform is the PikaScript ultra-lightweight Python interpreter that I am currently developing (if there are other solutions, please criticize and correct me, I will modify it). Although PikaScript does not have such complete standard library support compared to MicroPython, basic runtime objects, control flow, and interactive operation are all achievable, and PikaScript's cross-platform capability is very good. Under the extreme dependency management strategy, PikaScript only depends on LibC, there is almost no dependency missing problem on any platform, and it may be able to run in the FPGA soft core (theoretically feasible, not verified).

In addition, thanks to the open source platform provided by Gitee, PikaScript has just been selected by the Gitee judges as GVP - the most valuable open source project, so if you open the Gitee homepage now, there is a high probability that you can see the gold medal of PikaScript.
![](assets/1638495380222-9af9955c-4a80-4325-9d75-2edd40d42320.webp)

PikaScript is also included in the rt-thread package, which is really a very dynamic open source community
![](assets/1638495380395-47352427-5fd7-4a70-b8b4-bf57f6290a49.webp)

PikaScript's strict dependency management strategy makes deployment very easy, which is cross-platform and easy to deploy. But simple deployment is useless. If it is difficult to expand the function, it is just a vase. We know that in the field of MCU development, C language has always dominated the world. The ecology of C language accounts for more than 80% of MCU development. Most MCUs have C language development kits provided by manufacturers. Therefore, the Python interpreter of the MCU platform is the most important The expansion method is to bind the native library of the C language, and bind the C language library to a Python module, which is usually called a Python C module.
​

Binding C language modules for MicroPython is similar to general CPython. The C library needs to be compiled into a static library and then linked. Many global tables need to be manually registered during linking, and a large number of Linux platform-specific tools need to be used in the process of making C modules. Tools, this is a high threshold for MCU engineers who mainly develop on the Windows platform.
​

PikaScript can complete the development of C modules on the Windos platform that MCU engineers are familiar with. Through the self-developed module precompiler, the registration of modules can be automatically completed. The developers of C modules need to provide only a module written in Python. Just calling the API, the precompiler will automatically precompile the Python file into a C file to complete the linking and registration of the module. As long as the correct naming is used, the native C functions can be automatically registered into the module for the interpreter to call, and there is no need to compile the static library.
​

Let PikaScript run on CH32V103, which means developing a PikaScript firmware that can run on CH32V103.
​

Let's first look at what parts a PikaScript firmware has.
![](assets/1638495380538-858d703b-9406-499b-b1e6-8473e2a17b60.webp)

The yellow part in the figure is what we need to make, and the green part is cross-platform. We only need to pull the source code to compile it without modification.

Looking from the bottom up, the first thing is to need a BSP of PikaScript. BSP is the board-level support package, which can usually be obtained by sorting out the standard library of the MCU provided by the manufacturer. Then there's the PikaScript launcher, which contains the firmware entry main.c, and basic device initialization code, including support for printf.

With the BSP and the launcher, the firmware of PikaScript can already be run, but only the standard library functions provided by PikaScript and the basic syntax of Python can be used, and the peripheral resources mounted on the MCU cannot be used.

In order to use the peripheral resources of CH32V103, we also need to develop the driver module of CH32V103. In this project, we developed the driver module of GPIO and the delay module based on rt-thread tick timer.
​

The top layer is the Python script we want to run. The module precompiler can also process Python scripts, and automatically trim the firmware according to the modules imported in the script. The firmware that is not imported in the script will be automatically trimmed. Select the module to be added to the firmware in .py, and write the first Python script to run after the system is initialized, and burn it into the firmware.

**2. Make BSP and launcher - run first and then **
BSP is usually made with the routine provided by the original factory of the chip. In this project, we use the uart_printf in the official routine of CH32V103 and the rt-thread template generated by MounRiver River Studio to make it. After completing some tailoring of the rt-thread template, add the initialization function of printf, tidy up the project a little, and the BSP part is completed.
​

The creation of the PikaScript launcher is also relatively simple. Add #include "pikaScript.h" to main.c and call the pikaScriptInit() function to start PikaScript. Both pikaScript.h and pikaScriptInit() are automatically generated by the precompiler. Before making the launcher, you need to pull the source code of PikaScript.

PikaScript officially (actually, myself) provides a package management tool, just need to write request.txt, you can automatically pull the corresponding version of the source code and modules from gitee. When pulling the kernel source code, the precompiler will also be pulled down automatically. We write import PikaStdLib in main.py, and then precompile with the pulled precompiler to get pikaScriptInit() function.
​

The package management tool can not only pull the kernel, but also pull the module, that is to say, the driver module of CH32V103 made by ourselves can also be linked to the PikaScript module library for automatic pulling.
​

I recorded a video tutorial for the production of BSP and launcher. Those who want to know the details or want to make BSP by themselves can watch the video to understand.
https://www.bilibili.com/video/BV1Cq4y1G7Tj
![](assets/1638495380269-bb341d5a-d901-4b3e-9b67-843a97f3c27d.webp)



**3. Make the driver module of CH32V103**
​

Next, we make the driver module of CH32V103, so that the peripheral resources on CH32V103 can be called by Python script.
​

In this project, we made a standard device driver for PikaScript. What is a standard device driver? Let's start with other scripting technologies, such as MicroPython, there is no unified peripheral calling API, which makes users need to re-learn the API when using different platforms. For example, the following is the code for MicroPython to drive GPIO on the STM32F4 platform .
![](assets/1638495380966-02a52d33-9986-401c-a7e1-136ce71ad53e.webp)

This is for ESP8266
![](assets/1638495381179-e6afcca5-7f32-4a2f-a531-10f6b106db15.webp)

It can be clearly seen that when selecting the pin of the pin, one uses a string, while the other uses an integer number, and the API standard of the driver is very confusing.

Is there any way to unify the APIs of peripherals, so that users only need to be familiar with one set of APIs and can be used on any platform?

There is a method, that is, the PikaStdDevice standard device driver module!
![](assets/1638495380938-60679f63-cb15-4366-ac59-4efd6ff85d87.webp)

PikaStdDevice is an abstract device driver module that defines all user APIs, and the driver modules of each platform can obtain the same user API as long as they inherit from PikaStdDevice, and PikaStdDevice will indirectly call the platform driver and rewrite it through polymorphism. The underlying platform driver can work on different platforms!
​

Taking the GPIO module as an example, the following is the user API defined by PikaStdDevice
​

![](assets/1638495381064-51409a36-812a-48ea-a6ae-23b3c177582d.webp)



The following are the platform drivers that PikaStdDevice needs to rewrite
​

![](assets/1638495381214-1189c3eb-28ef-408e-a21e-e6bbc594d6fb.webp)



And the GPIO module of CH32V103 we want to make is inherited from the standard driver module.
​

![](assets/1638495381703-8e227dcc-97d7-4069-8754-4c118deea3fb.webp)



Through this method, we can make the STM32 driver module, CH32 driver module, and ESP32 driver module have exactly the same user API! As long as users are familiar with a set of APIs, they can easily use all platforms that support the PikaScript standard driver module! This is true cross-platform!
​

The following are some of the C native driver functions that are registered in the driver module
​

![](assets/1638495381557-21aaad62-bd63-40bc-b818-257e16992780.webp)



For the development of the driver module, I also made two videos for the reference of the big guys who want to know the details.
https://www.bilibili.com/video/BV1aP4y1L7pi
https://www.bilibili.com/video/BV1Jr4y117Z8
![](assets/1638495381957-6bc5f6f6-f3aa-4913-a06c-a0065a7d2cba.webp)![](assets/1638495382088-c0b1d1e6-746c-4f15-9775-924aa225829b.webp)



**4. Support interactive operation**

PikaScript does not depend on the file system, it can run as long as a string is passed in, so as long as you make a serial port driver that supports string reading, you can support interactive running!
The following is the driver code that supports interactive operation in this project.
![](assets/1638495382112-7d45db4b-c1d5-4573-a06e-7b72140a3abf.webp)



**5.main.py initialization script**​
![](assets/1638495382306-0f817e57-9526-44a0-99ec-c82a13cd45e5.webp)

Finally, we write an initialization script written in Python, which runs after the firmware starts, initializes the GPIO, and obtains a system object to provide the delay function. After initialization, the led blinks 10 times and hello pikascript is printed!

After writing the initialization script, it can be integrated in the firmware using the precompiler.

The following is the initialization function generated by the precompiler
​



![](assets/1638495382514-ee59c198-0557-434b-96d3-f2a21f596d2b.webp)

project address:

PikaScript-CH32V103 entry project warehouse:

https://gitee.com/lyon1998/ch32v103-pika

PikaScript main repository:

https://gitee.com/lyon1998/pikascript

https://github.com/pikastech/pikascript