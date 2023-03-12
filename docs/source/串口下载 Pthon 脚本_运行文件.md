# Serial port download Python script

The serial port download Python script is very similar to the interactive running, and still uses the obj_run kernel API to run the script. Unlike interactive running, downloading a Python script also requires **storing** of the python script.
​

obj_run supports running Python scripts in the form of strings, so no matter how you store them, just pass the string of the Python script to obj_run at the end. So the possible storage methods are: **flash direct storage, file system, external storage** and so on.


PikaPython supports running Python script source code and parsed Pika bytecode.
## Store Python source code
Storing the Python source code is very simple, just write the Python script string received by the serial port into Flash completely. Instead of using the pikaScriptInit() function at startup, manually create the pikaMain root object, and then use obj_run(pikaMain, code) to run the script, where code represents the stored python source code.


For specific code examples, please refer to:

1. [https://github.com/pikastech/pikascript/blob/master/bsp/stm32g030c8/Booter/main.c](https://github.com/pikastech/pikascript/blob/master/bsp/stm32g030c8/Booter/main.c)
1. [https://github.com/pikastech/pikascript/blob/master/bsp/stm32g030c8/Booter/pika_config.c](https://github.com/pikastech/pikascript/blob/master/bsp/stm32g030c8/Booter/main.c)
1. [https://github.com/pikastech/pikascript/blob/master/bsp/stm32g030c8/Booter/pika_config.h](https://github.com/pikastech/pikascript/blob/master/bsp/stm32g030c8/Booter/main.c)
## Store Pika bytecode
(to be improved)
For specific code examples, please refer to:

1. bsp/stm32g030c8/Booter/main.c
1. bsp/stm32g030c8/Booter/pika_config.c
1. bsp/stm32g030c8/Booter/pika_config.h


# Running Files Using the File System

When the MCU has a filesystem ported, you can use the file API to run Python script files directly.

[Note: requires kernel version `>= v1.10.0`.

The file API needs to be interfaced to the following file systems by overriding the WEAK function.

``` C
/* fopen */
PIKA_WEAK FILE* __platform_fopen(const char* filename, const char* modes);
/* fclose */
PIKA_WEAK int __platform_fclose(FILE* stream);
/* fwrite */
PIKA_WEAK size_t __platform_fwrite(const void* ptr,
                                   size_t size,
                                   size_t n,
                                   FILE* stream);
/* fread */
PIKA_WEAK size_t __platform_fread(void* ptr,
                                  size_t size,
                                  size_t n,
                                  FILE* stream);
```

Use `pikaVM_runSingleFile` to run a single Python file (no other files can be imported).

Function prototype.

``` C
VMParameters* pikaVM_runSingleFile(PikaObj* self, char* filename);
```

Use `pikaVM_runFile` to run Python files and their `import` files. A new `pikascript-api` folder needs to be created in the same level path as the running Python file to hold the intermediate files.

Function prototype.

``` C
VMParameters* pikaVM_runFile(PikaObj* self, char* file_name);
```
