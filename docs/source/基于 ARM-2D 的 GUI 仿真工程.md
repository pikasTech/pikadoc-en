# ARM-2D based GUI simulation project

## Preface


good news! The Arm2D module and simulation project of pikascript are preliminarily sorted out! pikaScript, ARM-2D, rt-thread work together, unlock new poses for python to play Arm2D! No hardware is needed, and it can be simulated directly, which is very convenient.


It is also very simple to deploy and run this simulation project on your own computer, just follow the steps below~
## Get the simulation project

Go to the PikaScript official website: http://pikascript.com, then select `sumulation-rtt-qemu-arm2d` for the platform, and then click Start to generate the project.

## Install the development environment


After you have the project, you also need to install the development environment. There are only two things that need to be installed. One is rt-thread studio, which is used as an IDE. rt-thread studio integrates qemu, which is very convenient for simulating mcu and gui. The other is the latest arm gcc toolchain.


### rt-thread studio installation package link


[https://download-sh-cmcc.rt-thread.org:9151/www/studio/download/RT-Thread Studio-v2.1.2-setup-x86_64_20210831-1200.exe](https://download-sh-cmcc.rt-thread.org:9151/www/studio/download/RT-Thread%20Studio-v2.1.2-setup-x86_64_20210831-1200.exe)


### arm gcc installation package link


[https://developer.arm.com/-/media/Files/downloads/gnu-rm/10.3-2021.10/gcc-arm-none-eabi-10.3-2021.10-win32.exe](https://developer.arm.com/-/media/Files/downloads/gnu-rm/10.3-2021.10/gcc-arm-none-eabi-10.3-2021.10-win32.exe)


You can install rt-thread studio where you like, arm gcc should be installed on the default c drive.


Once installed, you can start playing arm-2d with python.

## run

We open RT-Thread Studio and click Import


![](assets/139679061-2e3b2ea0-8e9a-44c9-9a0f-6f40d82a0208.png)


Then select the simulation-rtt-qemu-arm2d folder


![](assets/139679380-3a45f426-e575-4142-b5f1-76439c7efc38.png)


Select the project, then click the hammer to compile, and then click the bug to enter the simulation


![](assets/139679532-e19ed911-c7f4-4840-a5e3-f5b66905a62f.png)


A QEMU box will pop up, then click Run.


![](assets/139679756-cb099fc9-c3e9-4b76-9037-38392350530b.png)


If the operation is successful, you can see a small blue square on the white background. So far the deployment has been successful.


![](assets/139679797-3ce8f253-beb9-480f-90ee-1844500a77ab.png)


## Modify the python code and try


The source code of python is in simulation-rtt-qemu-arm2d/packages/pikascript/main.py, you can open it and see~


![](assets/139679915-45d1362e-7066-4829-ae83-b4bbc5d0aaa0.png)


The following is the content of main.py, create a new box object, and then set the color and position, you can try to change the color to 'white' or change the coordinates to see, you can also create another screen.elems.b2 try .


``` python

import PikaStdLib
import Arm2D

mem = PikaStdLib.MemChecker()

win = Arm2D.Window()
win.init()
win.background.setColor('white')

win.elems.b1 = Arm2D.Box()
win.elems.b1.init()
win.elems.b1.setColor('blue')
win.elems.b1.move(100, 100)
i = 0
x0 = 100
y0 = 100
sizeX0 = 50
sizeY0 = 50
alpha0 = 180
isIncrace = 1
loopTimes = 0

print('hello pikaScript')
print('mem used max:')
mem.max()
print('mem used now:')
mem.now()
while True:
    win.elems.b1.move(x0 + i * 2, y0 + i * 1)
    win.elems.b1.setAlpha(alpha0 - i * 1)
    win.elems.b1.setSize(sizeX0 + i * 2, sizeY0 + i * 1)
    win.update()
    if isIncrace > 0:
        i = i + 1
        if i > 160:
            isIncrace = 0
    if isIncrace < 1:
        i = i - 1
        if i < 0:
            isIncrace = 1
            loopTimes = loopTimes + 1

```

Remember to precompile after each modification to convert python to .c file in the project

![](https://user-images.githubusercontent.com/88232613/171087328-3ff8f8be-2415-4a24-9c5f-b4b4fe6e81e6.png)

Then compile again, enter the simulation, and you can see the effect. This time I changed the square to red.


![](assets/139680521-20f83ee3-2163-4649-ad23-ae73b77f482e.png)


## Conclusion


This is the Arm-2D warehouse~ Students who haven't starred remember to add a star~

[https://github.com/ARM-software/Arm-2D](https://github.com/ARM-software/Arm-2D)


![](assets/139681272-73a1a8c2-2889-4dab-bd05-7174cb14334c.png)


Thanks to liuduanfei for the rtt-Arm2d-qemu simulation project~ Here is the github homepage of liuduanfei


![](assets/139681543-99a64e9b-eb10-4c8e-bbe3-e8170c85385a.png)


[https://github.com/liuduanfei](https://github.com/liuduanfei)
