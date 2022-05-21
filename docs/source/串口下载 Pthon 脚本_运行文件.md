# Serial port download Python script run file

The serial port download Python script is very similar to the interactive running, and still uses the obj_run kernel API to run the script. Unlike interactive running, downloading a Python script also requires **storing** of the python script.
​

obj_run supports running Python scripts in the form of strings, so no matter how you store them, just pass the string of the Python script to obj_run at the end. So the possible storage methods are: **flash direct storage, file system, external storage** and so on.


PikaScript supports running Python script source code and parsed Pika bytecode.
## Store Python source code
Storing the Python source code is very simple, just write the Python script string received by the serial port into Flash completely. Instead of using the pikaScriptInit() function at startup, manually create the pikaMain root object, and then use obj_run(pikaMain, code) to run the script, where code represents the stored python source code.


For specific code examples, please refer to:

1. [https://gitee.com/Lyon1998/pikascript/blob/master/bsp/stm32g030c8/Booter/main.c](https://gitee.com/Lyon1998/pikascript/blob/master/bsp/stm32g030c8/Booter/main.c)
1. [https://gitee.com/Lyon1998/pikascript/blob/master/bsp/stm32g030c8/Booter/pika_config.c](https://gitee.com/Lyon1998/pikascript/blob/master/bsp/stm32g030c8/Booter/main.c)
1. [https://gitee.com/Lyon1998/pikascript/blob/master/bsp/stm32g030c8/Booter/pika_config.h](https://gitee.com/Lyon1998/pikascript/blob/master/bsp/stm32g030c8/Booter/main.c)
## Store Pika bytecode
(to be improved)
For specific code examples, please refer to:

1. bsp/stm32g030c8/Booter/main.c
1. bsp/stm32g030c8/Booter/pika_config.c
1. bsp/stm32g030c8/Booter/pika_config.h
