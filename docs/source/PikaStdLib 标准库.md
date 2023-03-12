# PikaStdLib standard library

PikaStdLib is a built-in library of PikaPython, which must be installed. It includes memory checking tools and system objects.
## Install

Add the dependency of PikaStdLib to requestment.txt. The version number of PikaStdLib should be the same as the version number of the kernel.

````
PikaStdLib
````

Run pikaPackage.exe

## import

Add in main.py
````python
#main.py
import PikaStdLib
````

## class MemChecker()

MemChecker provides PikaPython's memory monitoring capabilities. Can be used to view memory usage and check for memory leaks.
````python
def max(self):
````
Print the maximum memory footprint value.
````python
def now(self):
````
Print the current memory usage value.
````python
def getMax(self)->float:
````
Returns the largest memory footprint
````python
def getNow(self)->float
````
Returns the current memory usage value.
````python
def resetMax(self)
````
Reset the maximum memory usage value
**Example:**

````python
# main.py
import PikaStdLib
mem = PikaStdLib.MemChecker()
print('mem used max:')
mem.max()
print('mem used now:')
mem.resetMax()
print('mem used max:' + str(mem.getMax()))
print('mem used now:' + str(mem.getNow()))
````
## class SysObj()
SysObj is used to provide built-in functions, the scripts executed in main.py are executed by the root object, and the root object is created by the SysObj class, so the methods in the SysObj class are built-in functions.
````python
def type(arg: any):
````
print variable type
````python
def remove(argPath: str):
````
To remove a variable/object, use a string when removing, e.g. `remove('a')`.
````python
def int(arg: any) -> int:
def float(arg: any) -> float:
def str(arg: any) -> str:
````
for type conversion
````python
def print(arg:any):
````
Inherited from BaseObj, provides print output. Formatted output is not currently supported.
