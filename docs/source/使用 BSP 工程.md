# Use BSP project
## create project
Enter pikascript official website [http://pikascript.com](http://pikascript.com)
Select the platform, module, and click "Start Build".
(The default module will be automatically selected after selecting the platform)

![](https://user-images.githubusercontent.com/88232613/171087433-75476a62-ab79-4517-a7be-81683d726d81.png)

## The source of the project
The transplanted bare metal MCU project is in the pikascript/bsp directory, and each folder in it is a transplanted bare metal project.

**Each project is independent and can be copied out of the pikascript repository for separate use.**

(simulation-keil-dev and pico-dev are not listed. These two bsp can only be used in the warehouse and are used to develop the kernel.)

[https://github.com/pikastech/pikascript/tree/master/bsp](https://github.com/pikastech/pikascript/tree/master/bsp)
![](assets/1638605947761-93b30636-099f-4c7c-a432-6aae5e2d8b53.png)

## Support list
In the README.md in the bsp folder, the current platform support and the usage of bsp are marked.

(The table below is not up-to-date)

[Click here for the latest form](https://github.com/pikastech/pikascript#mcu-support)

![](assets/1639629972025-ca8fdf74-5dc2-472e-8497-5bc163bccdf4.png)

![](assets/1639629981607-43c6b771-34bf-45ac-9a66-8604f705ddff.png)

You can help PikaPython extend this table by contributing **driver modules** or **bsp**, please refer to the **New Platform Porting Guide**, **Module Development** and **Package Management** in the documentation for details.

## Projcet structure
Taking CH32V103 as an example, a PikaPython project includes the following parts.

![](https://user-images.githubusercontent.com/88232613/171087652-22dfa35b-4b1c-4248-a5b8-f57bc11e3086.png)

1. The first is the part of the BSP folder except the PikaPython folder. This part is the real BSP, including the basic peripheral library provided by the manufacturer, CMSIS and other common libraries on some platforms. You can get it sorted.

2. The above part is the launcher of PikaPython, including the main.c entry file, the pika_config.c configuration file, and the *.s assembly startup file. The launcher is responsible for supporting printf, stack settings, the startup of PikaPython, as well as some functions such as interactive operation, serial port download of Python, etc.

**pika_config.c is used to support some advanced functions such as downloading Python through serial port. PikaPython can still run without this file.**

3. The above is the main part of PikaPython, which is divided into two parts: the kernel and the module. The kernel is the file in pikascript/src. You can choose a version and add it to compile. **No modification is required.**



4. Module part can be developed by yourself, or pulled from the warehouse, PikaStdLib standard library module is required. Other modules are optional.

For how to use modules and how to make modules, please refer to the **Module Development** section, and for how to contribute modules to the PikaPython reference, please refer to the How to contribute PikaPython modules section.



5. The top layer is the Python script that the PikaPython project can support. The Python script can be directly interpreted and run. There are various ways to load the script, including **pre-compiled into firmware, interactive operation, serial port download of Python scripts**, etc., pre-compiled For firmware, please refer to the **Module Development** section, and for interactive operation and serial port download, please refer to the **New Platform Porting** section.

Only modules imported in main.py will be compiled into the firmware, so main.py can also play the role of **trimming modules**.

## module management
**Launchers, kernels and modules can all be managed using the package manager.**

Therefore, the PikaPython folder in the BSP only contains the package manager **pikaPackage.exe** itself, the **requestment.txt** module description file and the **main.py** sample script three files.

requestment.txt uses the same module description syntax as general python. Running pikaPackage.exe directly can identify requestment.txt in the current folder and pull the corresponding module.


Taking requestment.txt in the bsp of stm32g030 as an example, the pulled modules are:

- Kernel: pikascript-core
- Standard library: PikaStdLib
- Peripheral module: STM32G0 PikaPiZero PikaStdDevice
````
pikascript-core==v1.10.0
PikaStdLib
PikaStdDevice==v1.6.0
STM32G0==v1.2.0
PikaPiZero==v1.1.3
````
The pulled module needs to be precompiled, just run rust-msc-latest-win10.exe directly.

## Precautions

1. Keil version **strongly recommended** not lower than **5.36**

![](assets/1641372084863-db6426eb-b3cc-454d-b14a-5338818d01aa.png)
