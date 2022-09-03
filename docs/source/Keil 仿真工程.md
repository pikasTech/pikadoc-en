# How to Get Started with PikaScript using KEIL Simulator

In this article, we introduce a new way of playing PikaScript without hardware, i.e. using simulation in MDK. 
The target board of simulation is stm32f103, and you can experience the fun of pikascript immediately after downloading the project.

## Create project
Open the pikascript official website [http://pikascript.com](http://pikascript.com)

Select simulation-keil and click "Start Generation"

![](https://user-images.githubusercontent.com/88232613/171085761-685500a7-dbbc-417c-979e-fefc34dd5c76.png)


Unzip the downloaded zip archive and open the project

![](assets/130745821-864038df-d8b0-41d2-97e8-199815d0d57d.png)

### Run the simulation project
Make sure you have select the simulator as the debugging target

![](https://user-images.githubusercontent.com/88232613/171085849-c2213242-5d88-4fda-a651-81f7187088b7.png)

Compile and debug:

![](https://user-images.githubusercontent.com/88232613/171086774-faf8a9b8-7ad8-4241-aad5-34e49ae9e01b.png)

Once entering the debug session, make sure you have opened the serial windows as shown below:

![](assets/130747952-42073ba1-c4c4-4acb-9495-766cd5731374.png)

run and check the output:

![](https://user-images.githubusercontent.com/88232613/171086180-ddeec7eb-39c6-47ec-bcd2-4d7380e8b703.png)

## REPL

Python scripts can be run interactively by typing them directly in the UART window.

**NOTE**: Please use With **4 white-spaces** for indentation.

![](assets/image-20220621093047893.png)

## How to Run a different python script

Open the `main.py` in any editor, e.g. vscode:

![](https://user-images.githubusercontent.com/88232613/171086868-3ac1b9f6-c59f-4306-9b43-edf45844a203.png)

In `main.py`, you might see something similar to:

````python
# main.py
import Device
import PikaStdLib

led = Device.LED()
uart = Device.Uart()
mem = PikaStdLib.MemChecker()

print('hello wrold')
uart.setName('com1')
uart.send('My name is:')
uart.printName()
print('mem used max:')
mem.max()
print('mem used now:')
mem.now()
````

This script uses standard python3 syntax. Suppose we have already modified this script, so how to run it on the device?

The interesting part is, pikascript uses a method similar to java, i.e. it is semi-compiled and semi-interpreted. For example, the pikascript compiler compiles classes and methods, while PikaVM interprets method-calls and object-creation/destruction at runtime.

The pikascript compilation is a two-step process:
1. Using pikascript compiler to compile the `.py` files into `.c` and `.h` files and store them in the `pikascript-api` folder.
2. Using the ordinary c compiler to compile all the c source files and generate an executable image for the target device.

Double-click `rust-msc-vxx.yy.zz.exe` to run the pika precompiler which is written in Rust.

**NOTE**: Here `xx.yy.zz` is the version number.

![](https://user-images.githubusercontent.com/88232613/171086391-7b6c5ee0-53b1-4800-bfe8-34f586974947.png)

If you want to ensure that the updated script is compiled as expected, please 
1) delete all files in the `pikascript-api` folder, 
2) run the precompiler 
and 
4) check whether the new `.c` and `.h` files have been generated or not. 


**IMPORTANT**: Please do **NOT** remove the `pikascript-api` folder but only the files inside.

Here is an example that shows the `*.c` and `*h` files generated in the `pikascript-api` folder

![](assets/130750476-eaffce03-caeb-40b3-9841-550034fa191a.png)


Now, let's modify main.py as a practice: 

````python
import Device
import PikaStdLib

led = Device.LED()
uart = Device.Uart()
mem = PikaStdLib.MemChecker()

print('hello wrold')
uart.setName('com1')
uart.send('My name is:')
uart.printName()
print('mem used max:')
mem.max()
print('mem used now:')
mem.now()

# new code start
print('add new code start')
uart.setName('com2')
uart.printName()
print('add new code end')
# new code end
````

As you can see, we have added 4 new lines to the `main.py`. Let's compile and run:

Compile `pikascript-api`

![](https://user-images.githubusercontent.com/88232613/171086456-c3ede545-3f94-422f-acb6-711e634b468e.png)

Compile the keil project and enter the debugging session:

![](assets/130751539-aa0bdb82-750f-4f98-8f6f-02d653dda64a.png)


run and observe the output

![](https://user-images.githubusercontent.com/88232613/171086558-7ee7aeca-eb2d-4cd1-ac5a-21aa6e6f9d7e.png)

As shown above, there are 3 new lines in the output, indicating that our modification works as expected.

That's all, enjoy!!
