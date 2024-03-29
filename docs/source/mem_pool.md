# Compact Memory Pools

## Overview

PikaPython has a built-in compact memory pool for small resource chips, which is not enabled by default.

Compact memory pooling can reduce memory fragmentation from the usual 20-30% to less than 5%.

Note] Compact memory pooling can slow down the operation speed.

## Enabling method

Note that the kernel version must be at least v1.9.0.

### Enable user configuration

Refer to the [configuration document](%E4%BC%98%E5%8C%96%E5%86%85%E5%AD%98%E5%8D%A0%E7%94%A8%E3%80%81%E9%85%8D%E7%BD%AE%20libc.html)

### Add configuration items

```
/* pika_config.h */
#define PIKA_POOL_ENABLE 1
#define PIKA_POOL_SIZE 0x1900
```

Where `PIKA_POOL_ENABLE` means open compact memory pool, `PIKA_POOL_SIZE` means the size of the memory pool, the memory pool pre-apply memory from heap, please make sure the heap can apply to that size.

Refer to [bsp/stm32g030c8/Booter/pika_config.h](https://gitee.com/Lyon1998/pikascript/blob/master/bsp/stm32g030c8/Booter/pika_config.h)

### Memory pool initialization

Initialize the memory pool before `pikaScriptInit()` or `newRootObj()`. 

``` C
mem_pool_init();
```

Reference: [bsp/stm32g030c8/Booter/main.c](https://gitee.com/Lyon1998/pikascript/blob/master/bsp/stm32g030c8/Booter/main.c)

## Freeing the memory pool

If the memory pool needs to be freed, call

``` C
mem_pool_deinit();
```

# Interrupting a running script

Calling `pks_vm_exit()` forces the interruption of a running script (also in a dead loop), which can be placed in the interrupt function.

> [Note]
> 
> Requires kernel version not lower than `v1.11.0`.
> 
> After interrupting a running script, only the VM is exited, the root object can still be used and will not be freed, if you need to free memory, you should execute `obj_deinit()` on the root object.
