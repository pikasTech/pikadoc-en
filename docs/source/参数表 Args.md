# Parameter list Args
## head File
````c
#include "dataArgs.h"
````
## Overview

1. The Args parameter table API is a series of functions prefixed with args_.
1. The Args parameter table API is designed using object-oriented ideas. The first entry parameters of these functions are pointers to the parameter table to be manipulated.
1. The Args parameter table uses the **key-value pair (Map)** data model, or **dictionary (Dist)**.
1. A parameter table can contain **any number of** parameters, each parameter is indexed by **parameter name (key)**.
1. The parameters obtained by indexing can be **basic data types (int, float, pointer, string)** or **generic** parameters (Arg).
1. The Args parameter table supports adding, deleting, modifying and searching parameters dynamically.
1. Args parameter table **does not support nesting** (the main difference from the PikaObj attribute).
## type of data
The data type of the parameter table is Args.
````c
typedef Link Args;
````
The parameter table is internally implemented based on a linked list (Link).
**Be careful not to directly access the linked list inside Args**, please use the Args API to access Args. For **maximum backward compatibility**.
​

## Create and destroy the parameter table

1. Create a new parameter table, create a new parameter table from the heap, and return the pointer of the parameter table. **Note that the newly created parameter table needs to be destroyed manually to reclaim the memory. Constantly creating new parameter tables without destroying them can lead to memory leaks.**

[Note] To avoid memory leaks, please develop under [docker development environment](get-start_linux.html) and ensure sufficient unit tests and memory checks.


````c
Args* New_args(Args* args);
````
The parameter passed in when creating a new parameter table is a reserved auxiliary parameter table, which is usually filled with NULL.
​


2. Destroy the parameter table. When a parameter table is destroyed, all parameters inside the parameter table will also be automatically destroyed.
````c
void args_deinit(Args* self);
````
The pointer to the parameter table is passed in, and the parameter table is destroyed.
## CRUD API
This part of the API provides the addition, deletion, modification and query of the parameter table.
### Basic types of additions, deletions, modifications and inspections
Args parameter table supports **integer, floating point, pointer, string** four basic types of parameters. Use the set and get methods to read and write parameters in a parameter table.
​

The Args parameter table is **dynamic**, so new parameters can be added to the parameter table at any time.
​

The API for primitive type properties is as follows, which is similar to the parameter API for objects, but **does not support nesting**:
````c
/* set API */
int32_t args_setInt(Args* self, char* name, int64_t int64In);
int32_t args_setFloat(Args* self, char* name, float argFloat);
int32_t args_setPtr(Args* self, char* name, void* argPointer);
int32_t args_setStr(Args* self, char* name, char* strIn);

/* get API */
int64_t args_getInt(Args* self, char* name);
float args_getFloat(Args* self, char* name);
void* args_getPtr(Args* self, char* name);
char* args_getStr(Args* self, char* name);
````
Primitive type attributes are named args_set[Type] and args_get[Type].
​


1. The first input parameter is the pointer to the parameter table to be manipulated.
1. The second input parameter is the parameter name
1. The third input parameter of the set method is the written parameter value, and the return value of the get method is the read parameter value.
1. The return value of the set method is an error code, 0 means no error occurred.
### Generic parameters
args supports generic parameters and also provides set and get methods. Input parameters and return values ​​are similar to primitive types.
args_getType can get the type of the argument.
````c
int32_t args_setArg(Args* self, Arg* arg);
Arg* args_getArg(Args* self, char* name);
ArgType args_getType(Args* self, char* name);
````
Generic parameters need to be converted to primitive types when used.
​

Use the following API to determine the current type of a generic parameter.
````c
ArgType arg_getType(Arg* self);
````
Generic parameters can be converted to primitive types using the following API.
````c
int64_t arg_getInt(Arg* self);
float arg_getFloat(Arg* self);
void* arg_getPtr(Arg* self);
char* arg_getStr(Arg* self);
````
### Parameter management

1. Use the parameter name hash or parameter name to determine whether a parameter exists. The return value is 1 to indicate that it exists, and the times33 algorithm is used to obtain the parameter name hash.
````c
int32_t args_isArgExist_hash(Args* self, Hash nameHash);
int32_t args_isArgExist(Args* self, char* name);
Hash hash_time33(char* str);
````

2. Delete a parameter using a pointer to a generic parameter
````c
int32_t args_removeArg(Args* self, Arg* argNow);
````
The return value is an error code, and 0 indicates success.
## traversal of parameter list
A parameter table can be traversed using the following API.

1. The first entry parameter is a pointer to the parameter table.
1. The second parameter is the function pointer of the callback function when traversing the parameters
1. The third parameter is an auxiliary parameter table, which is used to pass auxiliary parameters. When auxiliary parameters are not used, the third input parameter can be filled with NULL.
````c
int32_t args_foreach(Args* self,
                     int32_t (*eachHandle)(Arg* argEach, Args* handleArgs),
                     Args* handleArgs);
````