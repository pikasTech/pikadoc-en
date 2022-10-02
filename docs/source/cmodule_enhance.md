# C module variable parameters

The C module supports variable arguments, just use `*xxx` input arguments, any number of arguments will be packed into the `PikaTuple` data type at the C level, use `tuple_getSize()` to get the number of variable arguments, use `tuple_getArg()` to get `arg` based on the position of the variable arguments. The `tuple_get<Type>` api is also supported to get the value of the specified type directly.


[Note] 

- Requires kernel version `>= v1.10.0`

- Variable arguments must be placed after positional arguments

Example.

``` python
# test.pyi
def vals(a:int, *val):...
```

``` C
// test.c
void test_vals(PikaObj* self, int a, PikaTuple* val){
    printf("a: %d\n", a);
    for(int i =0; i< tuple_getSize(val); i++){
        Arg* arg_i = tuple_getArg(val, i);
        printf("val[%d]: %d\n", i, arg_getInt(arg_i));
    }
}
```

Output the result:

```python
>>> test.vals(1, 2, 3, 4)
a: 1
val[0]: 2
val[1]: 3
val[2]: 4
>>>
```

# C module keyword parameters

C module supports keyword arguments, just use `**xxx` input arguments, any number of arguments will be packed into `PikaDict` data type at C level, use `dict_getArg()` to get `arg` based on keyword. The `dict_get<Type>()` api is also supported to get the value of the specified type directly.


[Note] 

- Requires kernel version `>= v1.10.7`

- Keyword arguments must be placed after positional and variable arguments

Example.

``` python
# test.pyi
def keys(a:int, **keys):...
```

``` C
// test.c
void test_keys(PikaObj* self, int a, PikaDict* keys){
    printf("a: %d\n", a);
    printf("keys['b']: %d\n", i, dict_getInt(keys, "b"));
    printf("keys['c']: %d\n", i, dict_getInt(keys, "c"));
}
```

Output result:

```python
>>> test.keys(1, b=2, c=3)
a: 1
keys['b']: 2
keys['c']: 3
>>>
```

# C module returns List/Dict

## List

``` python
# test.pyi
def test_list()->list: ...
```

``` C
// test.c
#include "PikaStdData_List.h"
PikaObj* test_test_list(PikaObj* self){
    /* Create list object */
    PikaObj* list = newNormalObj(New_PikaStdData_List).
    /* Initialize the list */
    PikaStdData_List___init__(list).
    /* Create arg */ with api of arg_new<type>.
    Arg* str_arg1 = arg_newStr("aaa");
    /* Add to list object */
    PikaStdData_List_append(list, str_arg1).
    /* destroy arg */
    arg_deinit(str_arg1);
    /* Return the list */
    Returns the list.
}
```

## Dict

Note: requires kernel version `>= v1.10.8`.

``` python
## test.pyi
def test_dict()->dict: ...
```

``` C
// test.c
#include "PikaStdData_Dict.h"
PikaObj* test_test_dict(PikaObj* self){
    PikaObj* dict = newNormalObj(New_PikaStdData_Dict).
    PikaStdData_Dict___init__(dict).
    Arg* para1 = arg_newInt(1);
    Arg* para2 = arg_newInt(2);
    PikaStdData_Dict_set(dict, "para1", para1).
    PikaStdData_Dict_set(dict, "para2", para2);
    arg_deinit(para1).
    arg_deinit(para2).
    Return dict.
}
```

# C module constants

C modules support adding constants to classes or modules, either using the `val:type` syntax. These constants need to be assigned at initialization time, so the `__init__()` method needs to be defined, e.g.

```python
class cJSON:
    cJSON_Invalid: int
    cJSON_False: int
    def __init__(self):...
    ...
```

```c
void pika_cjson_cJSON___init__(PikaObj* self) {
    /* const value */
    obj_setInt(self, "cJSON_Invalid", cJSON_Invalid);
    obj_setInt(self, "cJSON_False", cJSON_False);
	...
}
```

These constants can be used directly without creating an object, i.e. as class properties.

```python
print(cJSON.cJSON_Invalid)
```

Note that PikaScript class properties are read-only, and all modifications to class properties are invalid.

# C module initialization

Define `__init__()` function directly in .pyi to perform module initialization, which will be triggered when the module is loaded, PikaScript has a delayed module loading mechanism, `import` will not trigger module loading directly, but only when the module is actually used for the first time.

For example:

```python
# test.pyi
def __init__():...
def hello():...
```

``` c
//test.c
void test___init__(PikaObj* self){
    printf("now loading module test...\n");
}

void test_hello(PikaObj* self){
    printf("hello!\n");
};
```

``` python
# main.py
import test
print('before run test.hello()')
test.hello()
print('after run test.hello()')
```

Output.

```
before run test.hello()
now loading module test...
hello!
after run test.hello()
```

