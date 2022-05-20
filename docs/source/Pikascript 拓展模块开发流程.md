# PikaScript C module development process

We still use keil's simulation project as an example, if you haven't got the simulation project yet, please refer to [1. Three minutes to get started quickly](https://pikadoc.readthedocs.io/en/latest/Keil%20%E4%BB%BF%E7%9C%9F%E5%B7%A5%E7%A8%8B.html)

### New module interface


To write a new module, you first need to write a module interface file, for example, to write a math calculation module Math, the first step is to write Math.pyi.


If you want to create a new class from the PikaScript base class, you need to import PikaObj module, import PikaObj module should use `from PikaObj import *` way of introduction, actually Pika pre-compiler will not compile the module imported using `from` syntax, this is written just to get This is just to get smart syntax hints from the python editor, PikaObj is built into the Pika runtime kernel.


```python
# Math.pyi
from PikaObj import *
```


We can open the PikaObj.pyi file to see the class interfaces inside
```python
# PikaObj.pyi
class TinyObj:
    pass
class BaseObj(TinyObj):
    pass
def print(val: any):
    pass
def set(argPath: str, val: any):
    pass
```

You can see that there are two classes `TinyObj` and `BaseObj`, which are the basic classes implemented by the PikaScript kernel, and TinyObj is the most basic class without any function, with the least memory usage.

`print(val: any)` means that the input parameters are generic functions, `set(argPath:str, val:any)` are also generic functions, these two functions are implemented by the kernel.

### Writing class interfaces

Now we can create new classes inside Math.pyi. For example, if we want to create a new `Adder` class to implement the relevant addition operations, we can add the Adder class inside Math.py. To save memory, the Adder class inherits from the TinyObj base class.

Then we want Adder to provide addition operations for plastic and floating-point data, so we can add the byInt and byFloat methods.

```python
# Math.py
class Adder(TinyObj):
    def byInt(self, a:int, b:int)->int:
        pass
    def byFloat(self, a:float, b:float)->float:
        pass
```

The above code defines the `Adder` class and adds two method declarations, ```byInt(self, a:int, b:int)->int```, indicating that the method name is ``byInt``, the input parameters are ``a`` and ``b``, the type of ``a`` and ``b`` are both ``int``, and the return value is also ``int``. and the return value is determined by `->int`, which is the standard python syntax for writing with type annotations.

The first argument of a method of a class in python is `self`, which is required by python syntax.

We add a Multiplier class to math.py to implement multiplication, which is written as follows, also inheriting from the `TinyObj` base class.

```python
# Math.pyi
class Multiplier(TinyObj):
    def byInt(self, a:int, b:int)->int:
        pass
    def byFloat(self, a:float, b:float)->float:
        pass
```

This is the end of the interface. We introduce the Math module in main.py so that the Pika precompiler will go ahead and precompile the Math module.

```python
# main.py
import Math
```

Double-click to run the pika precompiler.

![](assets/131119247-ae25276e-f7c9-49ef-81e1-dbddcaffdf6c.png)

Opening the pikascript-api folder shows that our newly written module interface is ready to be compiled.


![](assets/131119310-99564d6a-d570-4375-9c01-c2d7cde74655.png)


### Writing the class implementation


Let's add the two newly compiled -api.c files we just made to the project and try compiling them.


![](assets/131119636-3c3d52ce-a7c2-48a4-beb4-5498dfd4f279.png)


found that the compilation reported an error, suggesting that there are four functions not found in the definition.


![](assets/131119786-823a96e3-7ab3-45f8-8c7c-282ba9b7b863.png)


This is normal because we did not write implementations for the classes of the Math module before, and we will write implementations for those classes below.


For the convenience of module management, we put all the implementation files in the pikascript-lib folder, the


![](assets/131120029-81c9b91f-2669-40cf-86da-78d72bce81c8.png)


Under the pikascript-lib folder, create a new Math folder to hold the implementation code for the Math module.


![](assets/131120240-a4001fa4-1fd2-4b6b-82a2-191834ed781b.png)


Then create a new .c file in the Math folder. It is recommended to use the naming scheme "module_class_name.c" to create a new .c file for each class to improve the clarity of the code.


![](assets/131120619-45ae3520-7b63-434b-8831-5b4d9f900cad.png)


Then we write the method implementation of the class inside these two .c files. So the question arises, how do we know which implementations should be written?


This is easy, we open Math_Multiplier.h and Math_Adder.h to find that the implementation functions we need to write have already been declared.


```c
/* Math_Multiplier.h */
/* ******************************** */
/* Warning! Don't modify this file!
/* ******************************** */
#ifndef __Math_Multiplier__H
#define __Math_Multiplier__H
#include <stdio.h>
#include <stdlib.h>
#include "PikaObj.h"

PikaObj *New_Math_Multiplier(Args *args);

float Math_Multiplier_byFloat(PikaObj *self, float a, float b);
int Math_Multiplier_byInt(PikaObj *self, int a, int b);

#endif
```


```c
/* Math_Adder.h */
/* ******************************** */
/* Warning! Don't modify this file!
/* ******************************** */
#ifndef __Math_Adder__H
#define __Math_Adder__H
#include <stdio.h>
#include <stdlib.h>
#include "PikaObj.h"

PikaObj *New_Math_Adder(Args *args);

float Math_Adder_byFloat(PikaObj *self, float a, float b);
int Math_Adder_byInt(PikaObj *self, int a, int b);

#endif
```


Then we directly implement these four functions in Math_Adder.c and Math_Multipler.c and we're good to go.


```c
/* Math_Adder.c */
#include "pikaScript.h"

float Math_Adder_byFloat(PikaObj *self, float a, float b)
{
	return a + b;
}

int Math_Adder_byInt(PikaObj *self, int a, int b)
{
	return a + b;
}
```


```c
/* Math_Multipler.c */
#include "pikaScript.h"

float Math_Multiplier_byFloat(PikaObj *self, float a, float b)
{
	return a * b;
}

int Math_Multiplier_byInt(PikaObj *self, int a, int b)
{
	return a * b;
}
````


At this point,compile the project again and it will pass.


### Test the effect


Let's test our newly written module with the following main.py


```python
# main.py
import Math

adder = Math.Adder()
muler = Math.Multiplier()

res1 = adder.byInt(1, 2)
print('1 + 2')
print(res1)

res2 = adder.byFloat(2.3, 4.2)
print('2.3 + 4.2')
print(res2)

res3 = muler.byInt(2, 3)
print('2 * 3')
print(res3)

res4 = muler.byFloat(2.3, 44.2)
print('2.3 * 44.2')
print(res4)
```


The result of the run is as follows.


![](assets/131123307-1d9564d1-8b99-4784-99ed-9756693781f1.png)


This shows that the module we wrote is working correctly.

### Available type annotations

The following table lists all the type declarations supported by PikaScript, and how they correspond to the native types of the C language.

| Python type annotations | C native types | description |
| --------------- | ----------- | -- |
| int | int | python basic types |
| float | float | python basic types |
| str | char * | python basic type |
| bytes | uint8_t * | python basic type |
| pointer | void * | PikaScript-specific type annotations |
| any | Arg* | PikaScript-provided generic containers |
| any class | PikaObj * |PikaScript-provided object container|


### Publishing modules


In the spirit of open source, it is very cool and exciting to publish your own modules.


All you need to do to publish a module is to publish the class interface and class implementation files.


For example, to publish the newly written Math module, you publish the Math.pyi file and the pikascript-lib/Math folder.


![](assets/131123704-403753d8-2ef1-488e-a02a-08fce33cd6de.png)


Please refer to the documentation in the **Participate in Community Contributions** section to distribute the modules you write.
