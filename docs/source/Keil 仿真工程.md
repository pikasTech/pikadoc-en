# How to Get Started with PikaScript using KEIL Simulator

In this article, we introduce a way of getting started without hardware, i.e. using simulation in MDK. 
The target board of simulation is stm32f103, and you experience the fun of pikascript immediately after by downloading the project.
### Create project
Enter pikascript official website [http://pikascript.com](http://pikascript.com)

Select simulation-keil and click "Start Generation"

![](assets/1644128841425-378e4391-426d-4dc3-bb2d-934e8facd22e.png)



Unzip the downloaded zip archive and open the project

![](assets/130745821-864038df-d8b0-41d2-97e8-199815d0d57d.png)




### Run the simulation project
Make sure you have select the simulator as the debugging target

![](assets/130747706-b912e09f-3f68-495a-a69f-f8f7500b1e4e.png)



Compile and debug:

![](assets/130747350-70ffa319-f04d-4f26-a75b-61864a19b8d8.png)



Once entering the debug session, make sure you have opened the serial windows as shown below:

![](assets/130747952-42073ba1-c4c4-4acb-9495-766cd5731374.png)



run and check the output:

![](assets/130748221-53fff9f6-6427-417d-b95a-3fa52a57eeaf.png)



### How to Change a different python script
Open main.py in any editor, e.g. vscode:

![](assets/130748847-477facfb-e16e-4e0e-8876-d66efd0ae48c.png)



In main.py, you might see something similar:

````python
# main.py
from PikaObj import *
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

This script uses standard python3 syntax. Suppose we have updated this script, so how to make it run in the microcontroller?
​

The interesting part is, pikascript uses a method similar to java, i.e. it is semi-compiled and semi-interpreted. For example, classes and methods are to be compiled, while method-calls and object-creation/destruction are to be interpreted at runtime. .
​

The pikascript compilation is a two-step process:
1. Use the pikascript precompiler to compile the .py files into .c and .h files in pikascript-api.
2. Use the c compiler to compile all the c source files, and then download the generated image into the microcontroller.

Double-click `rust-msc-vxx.yy.zz.exe` to run the pika precompiler which is written in Rust.

**NOTE**: Here `xx.yy.zz` is the version number.

![](assets/130749341-d12b7985-3685-419c-b9b8-8a09ae6f73d3.png)


If you want to make sure the updated script is compiled as required, please 1) delete all files in the `pikascript-api` folder, 2) run the precompiler and 3) check whether the .c and .h files have been generated or not. 


**IMPORTANT**: Please do NOT remove the `pikascript-api` folder but the files inside.

Here is an example shows the \*.c \*h files generated in `pikascript-api`

![](assets/130750476-eaffce03-caeb-40b3-9841-550034fa191a.png)


Now, let's modify main.py as a practice: 

````python
from PikaObj import *
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

As you can see, we have added 4 new lines to main.py. Let's compile and run.

Compile `pikascript-api`

![](assets/130751195-40944d60-7d56-48a9-9f47-cab87d77d5a8.png)


Compile the keil project and enter the debugging session:

![](assets/130751539-aa0bdb82-750f-4f98-8f6f-02d653dda64a.png)


run and watch the output

![](assets/130751653-cad627c2-367c-45a6-8c5f-686c7514df3c.png)


As shown above, there are 3 new lines in the output, indicating that our modification works as expected.

That's all, enjoy!!
