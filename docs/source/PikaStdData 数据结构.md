# PikaStdData data structure

PikaStdData data structure library provides List (list), Dict (dictionary) data structure.

## Install

Add the dependency of PikaStdLib to requestment.txt. The version number of PikaStdLib should be the same as the version number of the kernel.
````
PikaStdLib==v1.6.1
````

Run pikaPackage.exe

## import
Add in main.py
````python
#main.py
import PikaStdData
````

## class List():

The List class provides the List list function. By creating an object of the List class, a list can be created.
Such as:
````python
import PikaStdData
list = PikaStdData.List()
````

### Methods of the List class

````python
    # add an arg after the end of list
    def append(self, arg: any):
        pass

    # get an arg by the index
    def get(self, i: int) -> any:
        pass

    # set an arg by the index
    def set(self, i: int, arg: any):
        pass

    # get the length of list
    def len(self) -> int:
        pass
````

Note that the index of the `set()` method cannot exceed the length of the List. If you want to add members of the list, you need to use the `append()` method.

### Use '[]' brackets to index the list

List objects can be indexed using '[]'. `list[1] = a` is equivalent to `list.set(1, a)`, and `a = list[1]` is equivalent to `a = list.get(1)`.

### Use for loop to iterate over List

List objects support for loop traversal

example:
````python
import PikaStdData
list = PikaStdData.List()
list.append(1)
list.append('eee')
list.append(23.44)
for item in list:
    print(item)

````

## class Dict():

The Dict class provides the Dict dictionary function, and a dictionary can be created by creating an object of the Dict class.
Such as:

````python
import PikaStdData
dict = PikaStdData.Dict()
````

### Dict class methods

````python
    # get an arg by the key
    def get(self, key: str) -> any:
        pass

    # set an arg by the key
    def set(self, key: str, arg: any):
        pass

    # remove an arg by the key
    def remove(self, key: str):
        pass
````

### Index dictionary using '[]' brackets

Dict objects can be indexed using '[]'. `dict['x'] = a` is equivalent to `dict.set('x', a)` and `a = dict['x']` is equivalent to `a = dict.get('x') `.

### Using a for loop to iterate over a Dict

Dict objects support for loop traversal

example:
````python
import PikaStdData
dict = PikaStdData.Dict()
dict['a'] = 1
dict['b'] = 'eee'
dict['c'] = 23.44
for item in dict:
    print(item)

````

## class ByteArray(List)

[Note]: The version of PikaStdData requires at least v1.5.3

The ByteArray class provides the ByteArray byte array function. By creating an object of the ByteArray class, a byte array can be created.

Such as:
````python
import PikaStdData
bytes = PikaStdData.ByteArray()
````

The ByteArray class inherits from the List class and can use the methods of the List class.

### Methods of the ByteArray class

``` python
    # convert a string to ByteArray
    def fromString(self, s:str):
        pass
````
Example:
``` python
>>> bytes = PikaStdData.ByteArray()
>>> bytes.fromString('test')
>>> for byte in bytes:
...     print(byte)
...
116
101
115
116
>>> bytes.append(0xff)
>>> bytes.append(0x0f)
>>> print(bytes[4])
255
>>> print(bytes[5])
15
````
