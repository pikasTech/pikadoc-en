# String pool Strs
## head File
````c
#include "dataStrs.h"
````
## Overview

1. The Strs string pool API is a series of functions prefixed with strs.
1. The string pool provides **dynamic memory space** for strings, supports **any length** strings, and a string pool can store **any number of** strings.
1. Provide **convenient memory management**, when destroying the string pool, all the string memory in the pool will be automatically **batch destroyed**.
1. Provide a **safe operation method**, when using the strs API, the quoted string **will not be modified**. All modifications are made in the **newly allocated memory area**. Therefore, there will be no serious security problems such as dangling pointers and tampered strings.
1. The Strs string pool API is designed using object-oriented ideas. The first entry parameters of these functions are pointers to the operated string pool.
## type of data
The data type of Strs is Args, and a parameter table is maintained internally.
````c
typedef Link Args;
````
**Be careful not to directly access the string pool's parameter table**, use the Strs API to access Strs for **memory safety** and **maximum backward compatibility**.