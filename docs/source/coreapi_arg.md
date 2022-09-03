# Generic parameters Arg

## Header file

```c
#include "dataArg.h"
```

## Overview

1. arg The generic argument API is a set of functions prefixed with arg_.
1. arg can hold a value of any type in it.
The types supported by arg are: int, float, pointer, string, null, bytes. 1.
1. arg can be put into an object and the value of arg can be accessed directly in the python script.

## Data types

The data type of a generic argument is Arg.

```c
typedef struct Arg Arg;
struct Arg {
    Arg* next;
    uint32_t size;
    uint8_t type;
    Hash name_hash;
    uint8_t content[];
};
```

The generic arguments internally include header information ( size, type, name_hash ), the data body (content), and a pointer (next) used to form the chain.

**Be careful not to access the internal members of arg directly**, use the arg API to access arg. for **maximum backward compatibility**.


## Creating and destroying generic arguments

- Creating a new generic argument

Creates a new generic argument from the heap and returns a pointer to the generic argument.

**Note that newly created generic parameters need to be manually destroyed to reclaim memory. Constantly creating new generic parameters but not destroying them can lead to memory leaks. **

[Note] The following api requires a kernel version of at least v1.9.2

```c
Arg* arg_newInt(int val);
Arg* arg_newFloat(double val);
Arg* arg_newPtr(ArgType type, void* pointer);
Arg* arg_newStr(char* val);
Arg* arg_newNull(void);
Arg* arg_newBytes(uint8_t* src, size_t size);
```

New arg The argument passed in is the value of arg.

- Destroy generic arguments.

```c
void arg_deinit(Arg* self);
```

- Copy generic arguments
```c
Arg* arg_copy(Arg* self);
```

Pass in a pointer to the generic argument and destroy the generic argument.

## Getting the value of a generic argument

Use the following API to determine the current type of a generic argument.

```c
ArgType arg_getType(Arg* self);
```

Use the following API to convert a generic argument to a basic type.

```c
int64_t arg_getInt(Arg* self);
float arg_getFloat(Arg* self);
void* arg_getPtr(Arg* self);
char* arg_getStr(Arg* self);
uint8_t* arg_getBytes(Arg* self);
size_t arg_getBytesSize(Arg* self);
```

## Important Notes

Direct use of the arg_new\<Type\>() api **is highly likely to cause **memory leaks or dangling references, resulting in **fatal flaws**.

Please develop under [docker development environment](https://pikadoc.readthedocs.io/zh/latest/get-start_linux.html) to ensure sufficient unit testing and memory checking.

## Case

Build a list of strings using arg

``` C
## Include "PikaStdData_List.h"
...
    /* Create a list object */
    PikaObj* list = newNormalObj(New_PikaStdData_List);
    /* initialize list */
    PikaStdData_List___init__(list);
    /* Create arg with api of arg_new<type> */
    Arg* str_arg1 = arg_newStr("aaa");
    /* add to list object */
    PikaStdData_List_append(list, str_arg1);
    /* destroy arg */
    arg_deinit(str_arg1);
...
```

