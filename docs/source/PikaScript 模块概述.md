# PikaScript C module overview

We still use keil's simulation project as an example, if you don't get the simulation project yet, please refer to [1. Three minutes to get started quickly](https://pikadoc.readthedocs.io/en/latest/Keil%20%E4%BB%BF%E7%9C%9F%E5%B7%A5%E7%A8%8B.html)
### PikaScript module and module interface
We open the pikascript folder and find that in addition to main.py, there are Device.pyi, PikaObj.pyi and PikaStdLib.pyi in the root of the folder, which correspond to three PikaScript **C modules** (class package), each .pyi file itself is called the **module interface** (package interface). A C module can contain several classes that are more related.

![](assets/image-20220916120814065.png)

Each PikaScript **C module** consists of two parts: **module interface** and **module implementation** (package implement).
Let's start by opening Device.pyi to see the contents, we will call Device.pyi the **Device module interface** in the subsequent documentation.
Here is the entire contents of Device.pyi.

```python
# Device.pyi

class LED:
    def on(self):
        pass
    def off(self):
        pass

class Uart:
    def send(self, data:str):
        pass
    def setName(self, name:str):
        pass
    def printName(self):
        pass
```


As you can see, there are two classes defined in Device.pyi using pyhon standard syntax, the `LED` class and the `Uart` class.


The LED class defines two methods, the `on()` method and the `off()` method, while the `Uart` class defines the `send(data:str)` method, the `setName(name:str)` method, and the `printName()` method.


As you can see, all these methods have the feature that instead of being **definitions** of methods, they are **declarations** (annotations) of methods, because all method implementations are passed out and none of them are written for implementation. And the method's entry parameters are all with **type declarations**. For example, `data:str` means a `data` parameter with the type `str`, i.e. a string type.


This is because the module implementation of this module is written in C, i.e. the C modules of PikaScript are written with declarations in python syntax and implementations in C. PikaScript's module development is a **hybrid programming** technique for **interface-oriented** programming.


However, when using an existing module, it is not necessary to know the module implementation, but only the module interface, in order to use the module.


### Importing and calling modules


Let's see how to use this module.


Let's open main.py in the project, see the name, this file is the entry file for PikaScript.


The content of main.py is as follows


```python
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
```


Importing an already written C module is very simple, for example, to import the Device module, you just need to `import Device`, and note that all .py and .pyi files should be placed in the root directory of the pikascript fileshelf.


The call method uses the form `uart.setName('com')`, which is standard Python syntax and does not need much introduction.


After writing the module calls in main.py, double-click on rust-msc-v0.5.0.exe to pre-compile the PikaScript project, the pre-compiled output file is in the pikascrip-api folder.

![](assets/image-20220916121019138.png)

The pika pre-compiler generates .h declaration files for the imported modules. The filenames start with the module name and each class corresponds to one .h file.

![](assets/image-20220916121148778.png)

And PikaMain.h correspond to a special class that is the main PikaScript class, compiled from main.py.

![](https://user-images.githubusercontent.com/88232613/171088880-83247a92-2b1c-4d3f-a075-b4811132e54e.png)

pikaScript.c and pikaScript.h, on the other hand, are initialization functions compiled from main.py. When the initialization functions are run, the startup script is automatically executed.

![](assets/image-20220916121214655.png)

In the current main.py, the startup script is written in the outermost method call, which is:


```python
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
```


The compiled pikaScriptInit() initialization function corresponds to:


```c
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
```

