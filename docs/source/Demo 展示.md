# Demo show

I just want to use a microcontroller to run Python, I have to use a linux virtual machine + cross-compilation toolchain + command line to compile the micropython firmware, and I have to use the DfuSe tool to burn the firmware. After burning, I cannot use the C debugger to debug.

I want to expand my own C module, and I have to learn to use some macro functions that I don’t understand at all. I have to register it manually, and I have to write makeFile . After compiling, I still can’t debug C .

I'm poor and can't afford STM32F4. I want to buy a **STM32F103C8T6** micropython development board. I searched Taobao and it seems that there is no one.

Now C8T6 is also expensive, I still want to use **F0, use G0, and use domestic chip**, can I do it?

It seems that porting micropython to G0 is not very easy.

That? Is there another way to play?

In other words, I want to **develop with Keil, debug with Keil, **I also want to use **the cheapest microcontroller, **and **developing C modules is very easy**.

Can you play Python?

![](assets/132941900-985ebc9e-fb65-48f6-8677-d3ebc65422ee.gif)

Or, try PikaScript?

What is PikaScript?

PikaScript provides **Python** scripting support for **resource-constrained** MCUs that are **extremely easy to deploy and extend**. **Does not require an operating system**, can run **bare metal**, and **doesn't need a file system**.

PikaScript supports bare metal operation, at least can run in **RAM ≥ 4kB**, **FLASH ≥ 32kB** mcu, the recommended configuration is RAM ≥ 10kB, FLASH ≥ 64kB, such as stm32f103c8t6, ​​stm32g070RBT6 These are completely stress-free, even The recommended configuration has been met.

And supports IDE development such as **Keil, IAR, RT-Thread studio, segger embedded studio**, zero dependencies, zero configuration, out of the box, and easy to integrate into existing C projects.

Having said so much, Liu Huaqiang has doubts. Do you think this script is familiar?

![](assets/1638666543673-423aafcb-0c29-49b3-8221-22fdc3c65199.png)

I have a ~~fruit~~ script stall, can I buy your raw script eggs?

Let's pick some demos for my brother to see.

These are all demos of STM32G070RBT6.

## Demo 01 Light up

``` python
import PikaStdLib
import machine

mem = PikaStdLib.MemChecker()
io1 = machine.GPIO()
time = machine.Time()

io1.init()
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

Take a look at this script, but it's all Python3 standard syntax.

Isn't this light on?

![](assets/132943428-f2b365ca-140e-42f4-936c-db6a7d9f8dee.gif)

## Demo 02 Serial port test

``` python
import PikaStdLib
import machine

time = machine.Time()
uart = machine.UART()
uart.init()
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

adc1.init()
adc1.setPin('PA1')
adc1.enable()

while True:
    val = adc1.read()
    print('adc1 value:')
    print(val)
    time.sleep_ms(500)
    
````

The same few lines of script to get.

![](assets/132944185-0a01b1ba-8cf7-4f9f-9d73-fe9cbcd52f0b.png)

This is the result of the output.

The maximum RAM occupied by these demos is only **3.56K**, the 1K stack is also 4.56K, and the maximum Flash occupancy is 30.4K. Taking the 20K RAM and 64K Flash of STM32F103C8T6 as the standard, the RAM is only ** Using less than 25%**, Flash only uses less than 50%**, it is simply too many resources to know what to do.

Also running Python, we can simply compare the common chip STM32F405RG of micropython and the chip STM32G070CB running PikaScript this time

## RAM resource comparison

![](assets/132944731-a55ece1d-061f-4b91-ba87-bd6547be96a7.png)

## Flash resource comparison

![](assets/132944745-e9cf598d-e75f-40bb-873e-911819d535b7.png)
## Reference price comparison (take the price of 10 pieces of Lichuang Mall on September 11, 2021 as a reference)

![](assets/132944757-2b5cfda8-f93f-4456-8d7f-4e4767954056.png)

## How about the expansion ability?

In addition to device drivers, it is very easy to develop custom python script bindings for mcu under the development framework of pikascript. The following two demos are custom C module extensions. This demo is based on ARM- The 2D image driver library develops some python scripting interfaces.
## A few small squares~

![](assets/132945282-bfd310df-8063-456d-b90c-6b798a2c8ed5.gif)
## Several rotating suns~

![](assets/132945107-e473a2cc-9fbc-47f9-aaed-a28d3ad1048c.gif)
## So, is PikaScript open source?
Of course, this is the github home page of PikaScript:
[https://github.com/pikasTech/pikascript](https://github.com/pikasTech/pikascript)

## Is it difficult to develop?
PikaScript has prepared rich demos and development guides from shallow to deep for developers, and the guides will continue to be improved and maintained.

## Can it be commercialized?
certainly! PikaScript adopts the MIT license, allowing modification and commercial use, but pay attention to retain the original author's signature.