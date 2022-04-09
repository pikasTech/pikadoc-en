# PikaScript extension module development process

We still take Keil's simulation project as an example. If you haven't obtained the simulation project, please refer to [1. Quick Start in Three Minutes](https://pikadoc.readthedocs.io/zh/latest/Keil%20%E4%BB% BF%E7%9C%9F%E5%B7%A5%E7%A8%8B.html)



### New module interface


To write a new module, you first need to write a module interface file. For example, to write a mathematical calculation module Math, the first step is to write Math.py.


If you want to create a new class from the PikaScript base class, you need to import the PikaObj module. Importing the PikaObj module should use the import method of `from PikaObj import *`. In fact, the Pika precompiler will not compile and import using the `from` syntax. module, written just to get smart syntax hints from the python editor, PikaObj is built into the Pika runtime kernel.


````python
# Math.py
from PikaObj import *
````


We can open the PikaObj.py file to view the class interface inside
````python
# PikaObj.py
class TinyObj:
    pass
class BaseObj(TinyObj):
    pass
def print(val: any):
    pass
def set(argPath: str, val: any):
    pass
````
It can be seen that there are `TinyObj` and `BaseObj` two classes, these two classes are the basic classes implemented by the PikaScript kernel, TinyObj is the most basic class without any function, and the memory usage is the least.

`print(val: any)` represents a function whose input parameter is a generic type, and `set(argPath:str, val:any)` is also a generic function, and these two functions are implemented by the kernel.

### Write the class interface

Now we can create a new class in Math.py. For example, if we want to create an `Adder` class to implement related addition operations, we can add an Adder class in Math.py. In order to save memory, the Adder class is derived from the TinyObj base class Inherited.

Then we hope that Adder can provide addition operations for integer and floating-point data, so we can add byInt method and byFloat method.

````python
# Math.py
class Adder(TinyObj):
    def byInt(self, a:int, b:int)->int:
        pass
    def byFloat(self, a:float, b:float)->float:
        pass
````

In the above code, we define the `Adder` class and add two method declarations, ```byInt(self, a:int, b:int)->int``` means the method name is ``` byInt ```, the input parameters are `a` and `b`, the types of `a` and `b` are both `int` type, and the return value is also `int` type, the return value is from `->int` Sure, this is the standard syntax of python, which is written with type declarations.

The first arguments to methods of classes in python are all `self` which is required by python's syntax.

Let's add a Multiplier class to math.py to implement multiplication. Multiplier is written as follows. The Multiplier class also inherits from the `TinyObj` base class:

````python
# Math.py
class Multiplier(TinyObj):
    def byInt(self, a:int, b:int)->int:
        pass
    def byFloat(self, a:float, b:float)->float:
        pass
````

To this type of interface, the writing is completed. We introduce the Math module in main.py, so that the Pika precompiler will precompile the Math module.

````python
# main.py
import Math
````

Double click to run the pika precompiler.
![](assets/131119247-ae25276e-f7c9-49ef-81e1-dbddcaffdf6c.png)
Open the pikascript-api folder to find that our newly written module interface can be compiled.


![](assets/131119310-99564d6a-d570-4375-9c01-c2d7cde74655.png)


### Write the implementation of the class


Let's add the two newly compiled -api.c files to the project, and then compile and try.


![](assets/131119636-3c3d52ce-a7c2-48a4-beb4-5498dfd4f279.png)


It was found that the compilation reported an error, and the prompt was that there were four functions that were not defined.


![](assets/131119786-823a96e3-7ab3-45f8-8c7c-282ba9b7b863.png)


This is normal because we didn't write implementations for the classes of the Math module before, so let's write the implementations of these classes.


For the convenience of module management, we put the implementation files in the pikascript-lib folder,


![](assets/131120029-81c9b91f-2669-40cf-86da-78d72bce81c8.png)


In the pikascript-lib folder, create a new Math folder to put the implementation code of the Math module.


![](assets/131120240-a4001fa4-1fd2-4b6b-82a2-191834ed781b.png)


Then create a new .c file in the Math folder. It is recommended to use the naming method of "module name_class name.c" to create a new .c file for each class to improve the clarity of the code.


![](assets/131120619-45ae3520-7b63-434b-8831-5b4d9f900cad.png)


Then we write the method implementation of the class in these two .c files. So the question is, how do we know which implementations to write?


This is very simple, we can open Math_Multiplier.h and Math_Adder.h to find that the implementation function we need to write has been declared.


````c
/* Math_Multiplier.h */
/* ******************************** */
/* Warning! Don't modify this file! */
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
````


````c
/* Math_Adder.h */
/* ******************************** */
/* Warning! Don't modify this file! */
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
````


Then we directly implement these four functions in Math_Adder.c and Math_Multipler.c and it's ok.


````c
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
````


````c
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


At this time, compile the project again, and you can pass.


### Test the effect


Let's test our newly written module with the following main.py


````python
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
````


The effect of running is as follows:


![](assets/131123307-1d9564d1-8b99-4784-99ed-9756693781f1.png)


This shows that the module we wrote is working properly.
### publish module


In the spirit of open source, publishing your own modules is a really cool and exciting thing to do.


Publishing a module only needs to publish the class interface and class implementation file.


For example, to publish the newly written Math module, it is to publish the Math.py file and the pikascript-lib/Math folder.


![](assets/131123704-403753d8-2ef1-488e-a02a-08fce33cd6de.png)


Please refer to the documentation in the **Participating in Community Contributions** section to publish your modules.