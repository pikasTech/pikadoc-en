# PikaDebug debugger

The PikaDebug debugger module provides features such as breakpoint debugging.

## Install

Add the dependency of PikaStdLib to requestment.txt. The version number of PikaStdLib should be the same as the version number of the kernel.

````
PikaStdLib==v1.6.1
````

Run pikaPackage.exe

## class Debuger():

The Debuger class provides the debugger function. By creating an object of the Debuger class, a debugger can be created.

### Debuger class methods

````c
class Debuger(TinyObj):
    def __init__(self):
        pass

    def set_trace(self):
        pass
    
````

The `__init__()` method is the method executed when the object is created, and the user does not need to know about it. The `set_trace()` method can place a breakpoint in the code. When the code execution reaches the breakpoint, it will stop and open the `(pika-debug)` terminal. The user can enter commands in the terminal (c : continue running, q : to end debugging), or a python interactive call ( `printf(i)`, `i = 10`).


Example:

````python
import PikaDebug

pkdb = PikaDebug.Debuger()

i = 0
while i < 10:
    i = i + 1
    print('i:' + str(i))
    # set a breakpoint here
    pkdb.set_trace()
````

Command example:

n: (next) continue to run to the next breakpoint.

q: (quit) to exit debug mode and continue running.

p: (print) print variable, `p i` means print variable `i`.

Interactive run: Directly execute interactive commands, such as `print(i)`, `i = 2`, etc.

```bash
# Debug logging example
i : 1
(pika-debug) n
i : 2
(pika-debug) n
i : 3
(pika-debug) n
i : 4
(pika-debug) p i
4
(pika-debug) print(i)
4
(pika-debug) i = 2
(pika-debug) n
i : 3
(pika-debug) n
i : 4
(pika-debug) i = 9
(pika-debug) n
i : 10
(pika-debug) i = 2
(pika-debug) n
i : 3
(pika-debug) q
i : 4
i : 5
i:6
i : 7
i :8
i :9
i : 10
````
