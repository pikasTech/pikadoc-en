# Overview of PikaScript modules

We still take Keil's simulation project as an example. If you haven't obtained the simulation project, please refer to [1. Quick Start in Three Minutes](https://pikadoc.readthedocs.io/en/latest/Keil%20%E4%BB%BF%E7%9C%9F%E5%B7%A5%E7%A8%8B.html)
### PikaScript modules and module interface
We open the pikascript folder and find that in addition to main.py, there are Device.py, PikaObj.py and PikaStdLib.py in the root of the folder, which correspond to three PikaScript modules (class packages), or packages for short. py file itself is called package interface. A module can contain several classes that are more related.

![](assets/1638582993068-0a8afe28-baa2-41ad-bac1-6626d50192ad.png)

 Each PikaScript module consists of two parts: the module interface and the package implement. Let's open Device.py to see the contents, and we will refer to Device.py as the Device module interface in the subsequent documentation. Here is the entire contents of Device.py.

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


As you can see, Device.py uses pyhon standard syntax to define two classes, LED class and Uart class, both of which inherit from TinyObj.

The LED class defines two methods, the on() method and the off() method, while the Uart class defines the send(data:str) method, the setName(name:str) method and the printName() method.

As you can see, all these methods have one feature: instead of being method definitions, they are method declarations (annotations), because all method implementations are passed out and not written. Also, the method entry parameters are all type-declared. For example, data:str means a data parameter of type str, which is a string type.

This is because the module implementation is written in C, i.e. all PikaScript modules are written in python syntax for declarations and in C. PikaScript module development is a hybrid programming technique with interface-oriented programming.

However, when you use an existing module, you don't need to know the module implementation, you only need to know the module interface to use the module.


### Import and call the module


Let's see how to use this module.

We open main.py in our project, see the name, this file is the entry file for PikaScript.

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


Importing a module that has already been written is very simple, for example, to import the Device module, you just need to import Device, it is important to note that all .py files should be placed in the root of the pikascript file shelf.

The call method uses the form uart.setName('com'), which is standard Python syntax and does not need much introduction.

After writing the module calls in main.py, double click on rust-msc-v0.5.0.exe to pre-compile the PikaScript project, the pre-compiled output file is in the pikascrip-api folder.

![](assets/1638582989556-feafe97a-037f-44b2-8f2c-55ddf8f041ea.png)


The pika pre-compiler generates .h declaration files and -api.c constructor files for the imported modules. The filenames start with the module name and each class corresponds to one .h file and one -api.c file.

![](assets/1638582990457-2540db61-f185-4100-8b63-4d6d599c3b0e.png)


And PikaMain-api.c and PikaMain.h correspond to a special class that is the main PikaScript class, compiled from main.py.


![](assets/1638582990858-10783588-5ff0-469e-b64d-50e56e2357bc.png)


pikaScript.c and pikaScript.h, on the other hand, are initialization functions compiled from main.py. When the initialization functions are run, the startup script is automatically executed.

![](assets/1638582992822-6c4a7f39-a379-4c66-991a-1935ec3bfa7a.png)

In the current main.py, the startup script is written in the outermost method call, which is :

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

The compiled pikaScriptInit() initialization function corresponds to the :

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
