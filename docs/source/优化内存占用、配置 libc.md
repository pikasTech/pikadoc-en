# PikaScript configuration manual

## When to configure

PikaScript itself is **configuration-free**, so usually you don't need to know this part.

You can consider configuring PikaScript when you have the following requirements:

- faster speed
- Smaller memory footprint
- Replace dependencies (libc, pinrtf, etc.)
- Replace memory management algorithm ( malloc )
- Safer interruption protection
  
## Optimization

[Note]: For optimized configuration, the kernel version needs to be at least ```v1.5.4```

Similar to GCC, PikaScript also provides different optimization modes. The currently available optimization modes are:

- ```PIKA_OPTIMIZE_SIZE``` volume mode minimizes running memory

- ```PIKA_OPTIMIZE_SPEED``` performance mode maximizes running speed
  
### Enable user configuration

User configuration is not enabled by default. The way to enable user configuration is to add compile-time macro definition ``` PIKA_CONFIG_ENABLE ```. Then create the ```pika_config.h``` header file.

It should be noted that the ```PIKA_CONFIG_ENABLE ``` macro should be added to the compile options, such as keil:

![](assets/160849244-40fe7fa8-0e93-4791-8f14-bc044bbd0d59.png)

### Configuration items

Available configuration items and default configuration are in the ```pika_config_valid.h``` header file.

[https://github.com/pikastech/pikascript/blob/master/src/pika_config_valid.h](https://github.com/pikastech/pikascript/blob/master/src/pika_config_valid.h)

Intercept the important part for explanation:

```` c
/* default configuration */
   #define PIKA_LINE_BUFF_SIZE 128
   #define PIKA_SPRINTF_BUFF_SIZE 128
   #define PIKA_STACK_BUFF_SIZE 256
   #define PIKA_NAME_BUFF_SIZE 32
   #define PIKA_PATH_BUFF_SIZE 64
   #define PIKA_ARG_ALIGN_ENABLE 1
   #define PIKA_METHOD_CACHE_ENABLE 0
          
/* optimize options */
   #define PIKA_OPTIMIZE_SIZE 0
   #define PIKA_OPTIMIZE_SPEED 1
      
/* default optimize */
   #define PIKA_OPTIMIZE PIKA_OPTIMIZE_SIZE
      
/* use user config */
   #ifdef PIKA_CONFIG_ENABLE
      #include "pika_config.h"
   #endif
````

```default configuration``` is the default value of the configuration item. When the ```PIKA_CONFIG_ENABLE``` macro is defined, ```pika_config_valid.h``` will import ```pika_config.h```, so User can override the above default configuration in ```pika_config.h```.

For example, if you want to increase the runtime stack of the PikaScript virtual machine, you can write in ```pika_config.h```

```` c
#undef PIKA_STACK_BUFF_SIZE
#define PIKA_STACK_BUFF_SIZE 512
````

As can be seen from ```pika_config_valid.h```, the default optimization option of PikaScript ``` PIKA_OPTIMIZE ``` is ``` PIKA_OPTIMIZE_SIZE ```, if you need to switch to speed optimization, you can write in ```pika_config.h```

```` c
#undef PIKA_OPTIMIZE
#define PIKA_OPTIMIZE PIKA_OPTIMIZE_SPEED
````
### Sample code

[https://github.com/pikastech/pikascript/blob/master/bsp/stm32g070cb/Booter/pika_config.h](https://github.com/pikastech/pikascript/blob/master/bsp/stm32g070cb/Booter/pika_config.h)

## Dependency configuration

PikaScript can be configured by creating ``pika_config.c``, rewriting the weak functions in [PikaPlagform.h](https://github.com/pikastech/pikascript/blob/master/src/PikaPlatform.h) 's dependencies.
```` c
/* interrupt config */
void __platform_enable_irq_handle(void);
void __platform_disable_irq_handle(void);

/* printf family config */
#ifndef __platform_printf
void __platform_printf(char* fmt, ...);
#endif
int __platform_sprintf(char* buff, char* fmt, ...);
int __platform_vsprintf(char* buff, char* fmt, va_list args);
int __platform_vsnprintf(char* buff,
                         size_t size,
                         const char* fmt,
                         va_list args);

/* libc config */
void* __platform_malloc(size_t size);
void __platform_free(void* ptr);
void* __platform_memset(void* mem, int ch, size_t size);
void* __platform_memcpy(void* dir, const void* src, size_t size);

/* pika memory pool config */
void __platform_wait(void);
uint8_t __is_locked_pikaMemory(void);

/* support shell */
char __platform_getchar(void);

/* file API */
FILE* __platform_fopen(const char* filename, const char* modes);
int __platform_fclose(FILE* stream);
size_t __platform_fwrite(const void* ptr, size_t size, size_t n, FILE* stream);

/* error */
void __platform_error_handle(void);
````
### Configuration items:

- Interrupt Protection - Provides an interrupt master switch to protect PikaScript memory safety
  
- libC - select the implementation of libC
  
- Memory management - replace malloc free memory management algorithm
  
### Sample code:
- [https://github.com/pikastech/pikascript/blob/master/bsp/stm32g030c8/Booter/pika_config.c](https://github.com/pikastech/pikascript/blob/master/bsp/stm32g030c8/Booter/pika_config.c)
  
- [https://github.com/pikastech/pikascript/blob/master/package/pikaRTThread/pika_config.c](https://github.com/pikastech/pikascript/blob/master/package/pikaRTThread/pika_config.c )
