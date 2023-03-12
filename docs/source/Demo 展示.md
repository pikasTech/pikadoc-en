# Demo show

I want to run a Python with a microcontroller, I have to use linux virtual machine + cross-compilation tool chain + command line compile micropython firmware, but also have to use the DfuSe tool to burn the firmware, burned also can not use the C debugger to debug.

I want to expand a C module of my own, but I have to learn to use some completely unintelligible macro functions, and I have to register them manually, and I have to write makeFile, and I can't debug C after compilation.

I am poor, can not afford to buy STM32F4, want to buy a STM32F103C8T6 micropython development board, Taobao a search, seems not.

Now the C8T6 is also expensive, I still want to use F0, use G0, with domestic chips, can it work?

It seems that it is not very easy to port micropython to G0.

So? Is there another way to play?

In other words, I want to develop with Keil, debug with Keil, and I want to use the cheapest microcontroller, and it's very easy to develop C modules.

How about trying PikaPython?

What is PikaPython?

PikaPython provides extremely easy to deploy and extend Python scripting support for resource-constrained mcu. It doesn't require an OS, it runs bare metal, and it doesn't require a filesystem.

PikaPython supports bare-metal operation, at least for mcu with RAM ≥ 4kB and FLASH ≥ 32kB, the recommended configuration is RAM ≥ 10kB and FLASH ≥ 64kB, such as stm32f103c8t6 and stm32g070RBT6, which have no pressure at all and even meet the recommended configuration.

And support Keil, IAR, RT-Thread studio, segger embedded studio and other IDE development, zero dependencies, zero configuration, out-of-the-box, extremely easy to integrate into the existing C project.

These are all demos of STM32G070RBT6.

## Demo 01 Light up

``` python
import PikaStdLib
import machine

mem = PikaStdLib.MemChecker()
io1 = machine.GPIO()
time = machine.Time()

io1.setPin('PA8')
io1.setMode('out')
io1.enable()
io1.low()

print('hello pikascript')
print('mem.max:')
mem.max()
print('mem.now:')
mem.now()

while True:
    io1.low()
    time.sleep_ms(500)
    io1.high()
    time.sleep_ms(500)
    
````

Look at the script, it's all Python3 standard syntax.

The light is flashing.

![](assets/132943428-f2b365ca-140e-42f4-936c-db6a7d9f8dee.gif)

## Demo 02 Serial port test

``` python
import PikaStdLib
import machine

time = machine.Time()
uart = machine.UART()
uart.setId(1)
uart.setBaudRate(115200)
uart.enable()

while True:
    time.sleep_ms(500)
    readBuff = uart.read(2)
    print('read 2 char:')
    print(readBuff)
    
````

Open a serial port and try to read two characters

![](assets/132943365-0f7059b3-4f9d-4989-a5ec-2cce72b0cc96.gif)

very smooth

## Demo 03 Try reading an ADC

``` python
import PikaStdLib
import machine

time = machine.Time()
adc1 = machine.ADC()

adc1.setPin('PA1')
adc1.enable()

while True:
    val = adc1.read()
    print('adc1 value:')
    print(val)
    time.sleep_ms(500)
    
````

Again a few lines of script fixes it.

![](assets/132944185-0a01b1ba-8cf7-4f9f-9d73-fe9cbcd52f0b.png)

This is the output result.

The maximum value of RAM occupied by these demos is only 3.56K, including the 1K stack is also 4.56K, the maximum Flash occupation is 30.4K, using the STM32F103C8T6's 20K RAM and 64K Flash as the standard, RAM is only used up less than 25%, Flash is only used up less than 50%, simply more resources do not know how to spend. This is a lot of resources.

Also running Python, we can briefly compare the common chip STM32F405RG for micropython and the chip STM32G070CB for PikaPython.

## RAM resource comparison

![](assets/132944731-a55ece1d-061f-4b91-ba87-bd6547be96a7.png)

## Flash resource comparison

![](assets/132944745-e9cf598d-e75f-40bb-873e-911819d535b7.png)
## Reference price comparison (take the price of 10 pieces of Lichuang Mall on September 11, 2021 as a reference)

![image](https://user-images.githubusercontent.com/88232613/171085508-e788518b-ec2e-48f3-8fc8-9327b54a2dbb.png)

## How about the expansion ability?

In addition to device drivers, developing custom python script bindings for mcu is very easy with the pikascript development framework. The following two demos are custom C module extensions that develop some python script interfaces based on the ARM-2D image driver library.

## A few small squares~

![](assets/132945282-bfd310df-8063-456d-b90c-6b798a2c8ed5.gif)
## Several rotating suns~

![](assets/132945107-e473a2cc-9fbc-47f9-aaed-a28d3ad1048c.gif)
## So, is PikaPython open source?
Of course, this is the github home page of PikaPython:
[https://github.com/pikasTech/pikascript](https://github.com/pikasTech/pikascript)

## Is it difficult to develop?
PikaPython has prepared rich demos and development guides from shallow to deep for developers, and the guides will continue to be improved and maintained.

## Can it be commercialized?
Of course! PikaPython uses the MIT protocol and allows modifications and commercialization, but be careful to keep the original author's byline.
