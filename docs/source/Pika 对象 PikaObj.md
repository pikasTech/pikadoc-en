# Pika object PikaObj
## head File
```c
#include "PikaObj.h"
````
## Overview

- The object API is a series of functions prefixed with **obj_**.

- The Object API provides a series of interfaces for accessing Python objects in C. **Most frequently used in module development.**

- The object API itself is also designed using object-oriented ideas. The first entry parameters of these functions are pointers to the objects to be operated.

- An object consists of two parts: properties and methods, so the object API is also divided into two parts: properties and methods.

## type of data

The data type of the object itself is PikaObj, which is used by all Python objects when accessed in C.

```c
struct PikaObj_t {
    /* list */
    Args* list;
};
typedef struct PikaObj_t PikaObj;
```

PikaObj internally maintains a parameter table, which contains attribute information, class information, method information, etc.
**Be careful not to directly access the parameter table inside PikaObj**, please use the object API to access PikaObj. This is because the object API, as an external interface, is stable for a long time, and the internal implementation will change frequently with the iteration of the kernel code. Directly operating the interior of PikaObj will greatly lose backward compatibility.

## Object Properties API

This part of the API provides access to Python object properties.

### Attributes of primitive types

PikaObj supports **integer, floating point, pointer, string** four basic types of attributes. Use the set and get methods to read and write properties of an object.

| PikaObj attribute type |                         Read/Write API                          | Python type |     |
| :--------------------: | :-------------------------------------------------------------: | :---------: | --- |
|          int           |                   `obj_setInt() obj_getInt()`                   |     int     |     |
|         float          |                 `obj_setFloat() obj_getFloat()`                 |    float    |     |
|          str           |                   `obj_setStr() obj_getStr()`                   |   string    |     |
|        pointer         |                   `obj_setPtr() obj_getPtr()`                   |      -      |     |
|         bytes          |       `obj_setBytes() obj_getBytes() obj_getBytesSize()`        |    bytes    |     |

PikaObj objects are **dynamic**, so new properties can be added to the object at any time (the properties of static objects are determined at construction time).

The APIs for primitive type properties are as follows:

```c
/* set API */
int32_t obj_setInt(PikaObj* self, char* argPath, int64_t val);
int32_t obj_setPtr(PikaObj* self, char* argPath, void* pointer);
int32_t obj_setFloat(PikaObj* self, char* argPath, float value);
int32_t obj_setStr(PikaObj* self, char* argPath, char* str);
/* get API */
void* obj_getPtr(PikaObj* self, char* argPath);
float obj_getFloat(PikaObj* self, char* argPath);
char* obj_getStr(PikaObj* self, char* argPath);
int64_t obj_getInt(PikaObj* self, char* argPath);
```
Primitive type properties are named as obj_set[Type] and obj_get[Type].

The first input parameter is the object pointer to be manipulated.
The second input parameter is attribute name/attribute address.

PikaObj supports object nesting and can access properties of sub-objects. When accessing properties of sub-objects, the second parameter is the property address, and when accessing properties of this object, the second value is property name.

```c
// set an Int type arg, the arg name is "a".
obj_setInt(self, "a", 1);
// set an Int type arg for subObjcet , the arg path is "subObj.a".
obj_setInt(self, "subObj.a", 1);
```

The third input parameter of the set method is the written property value, and the return value of the get method is the read property value.
The return value of the set method is an error code, 0 means no error occurred.

### Generic properties

PikaObj supports generic properties and also provides set and get methods. Input parameters and return values ​​are similar to primitive types.
```c
int32_t obj_setArg(PikaObj* self, char* argPath, Arg* arg);
Arg* obj_getArg(PikaObj* self, char* argPath);
```
Generic properties need to be converted to primitive types when used.

Use the following API to determine the current type of a generic property.

```c
ArgType arg_getType(Arg* self);
```

Use the following API to convert generic properties to primitive types.

```c
int64_t arg_getInt(Arg* self);
float arg_getFloat(Arg* self);
void* arg_getPtr(Arg* self);
char* arg_getStr(Arg* self);
```

### Property management

- Determine whether an attribute exists, and the return value is 1 to indicate existence.

```c
int32_t obj_isArgExist(PikaObj* self, char* argPath);
```

- Delete an attribute

```c
int32_t obj_removeArg(PikaObj* self, char* argPath);
```
The return value is an error code, 0 means success.
## Object method API
The object method API is divided into two parts: method registration and method invocation. The **method registration part is proxied by the precompiler**, and the module developer only needs to use the method to call the API.

### Method call API

```c
void obj_run(PikaObj* self, char* cmd);
```

obj_run is a versatile API that can directly run Python scripts and supports multi-line scripts.
The first entry parameter is a pointer to the object, and the second entry parameter is the Python script as a string.
Note that when passing in a multi-line script, you should pass in a complete block of code.

## Throw an exception

An exception can be thrown using obj_setErrorCode in the module, and the user can customize the exception handling method (continue running or stop running).
Throwing an exception is usually used in the method of the C module, just pass in the self object pointer of the current method, and set errCode to non-zero to trigger the exception.
The obj_setSysOut method is often used in conjunction with the obj_setErrorCode method to provide debugging information, which will be displayed on the terminal when the exception is triggered.

```c
/* set Error Code, if the errCode is not 0, an exaption would be throw out */
void obj_setErrorCode(PikaObj* self, int32_t errCode);
/* print out exaption infomation */
void obj_setSysOut(PikaObj* self, char* str);
```
