# Overview of PikaScript modules

We still take Keil's simulation project as an example. If you haven't obtained the simulation project, please refer to [1. Quick Start in Three Minutes](https://pikadoc.readthedocs.io/zh/latest/Keil%20%E4%BB% BF%E7%9C%9F%E5%B7%A5%E7%A8%8B.html)
### PikaScript modules and module interface
We opened the pikascript folder and found that in addition to main.py, there are Device.py, PikaObj.py and PikaStdLib.py in the root directory of the folder. These three .py files correspond to three PikaScript **modules** (class package ), referred to as **package** (package), each .py file itself is called **module interface** (package interface). A module can contain several related classes.

![](assets/1638582993068-0a8afe28-baa2-41ad-bac1-6626d50192ad.png)
Each PikaScript **module** consists of **module interface** and **module implementation** (package implement) two parts.
Let's open Device.py first to see the content. In subsequent documents, we will call Device.py **Device module interface**.
The following is the entire content of Device.py.

````python
# Device.py
from PikaObj import *

class LED(TinyObj):
    def on(self):
        pass
    def off(self):
        pass

class Uart(TinyObj):
    def send(self, data:str):
        pass
    def setName(self, name:str):
        pass
    def printName(self):
        pass
````


As you can see, two classes are defined in Device.py using python standard syntax, namely `LED` class and `Uart` class, both of which inherit from `TinyObj`.


The LED class defines two methods, the `on()` method and the `off()` method, while the `Uart` class defines the `send(data:str)` method, `setName(name:str) ` method and the `printName()` method.


It can be seen that these methods have a characteristic. It is not so much the **definition** of the method, but the **declaration** (annotation) of the method, because all the method implementations have been passed and not written. accomplish. And the entry parameters of the method are all with **type declaration**. For example, `data:str` represents a `data` parameter, and the parameter type is `str`, which is a string type.


This is because the module implementation of this module is written in C language, that is, all modules of PikaScript use python syntax to write declarations, and use C language to write implementations. Module development in PikaScript is a **hybrid programming** technique for **interface-oriented** programming.


However, when using an existing module, you do not need to understand the implementation of the module. You only need to understand the interface of the module to use this module.


### Import and call the module


Let's take a look at how to use this module.


We open the main.py in the project, as the name implies, this file is the entry file of PikaScript.


The content of main.py is as follows


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


Importing a module that has already been written is very simple, such as importing the Device module, just `import Device`, it should be noted that all .py files should be placed in the root directory of the pikascript file shelf.


The calling method uses the form of `uart.setName('com')`, which is the standard syntax of Python and does not need too much introduction.


After writing the module call in main.py, double-click rust-msc-v0.5.0.exe to precompile the PikaScript project, and the precompiled output file is in the pikascript-api folder.


![](assets/1638582989556-feafe97a-037f-44b2-8f2c-55ddf8f041ea.png)


The pika precompiler generates .h declaration files and -api.c constructor files for imported modules. The file names start with the module name, and each class corresponds to a .h file and an -api.c file.


![](assets/1638582990457-2540db61-f185-4100-8b63-4d6d599c3b0e.png)


PikaMain-api.c and PikaMain.h correspond to a special class, which is the main class of PikaScript and is compiled from main.py.


![](assets/1638582990858-10783588-5ff0-469e-b64d-50e56e2357bc.png)


pikaScript.c and pikaScript.h are initialization functions compiled according to main.py. When the initialization function is run, the startup script will be automatically executed.


![](assets/1638582992822-6c4a7f39-a379-4c66-991a-1935ec3bfa7a.png)


In the current main.py, the startup script is written in the outermost method call, that is:


````python
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


The compiled pikaScriptInit() initialization function corresponds to:


````c
PikaObj * pikaScriptInit(){
    PikaObj * pikaMain = newRootObj("pikaMain", New_PikaMain);
    obj_run(pikaMain,
            "\n"
            "led = Device.LED()\n"
            "uart = Device.Uart()\n"
            "mem = PikaStdLib.MemChecker()\n"
            "\n"
            "print('hello wrold')\n"
            "uart.setName('com1')\n"
            "uart.send('My name is:')\n"
            "uart.printName()\n"
            "print('mem used max:')\n"
            "mem.max()\n"
            "print('mem used now:')\n"
            "mem.now()\n"
            "\n"
            "\n");
    return pikaMain;
}
````