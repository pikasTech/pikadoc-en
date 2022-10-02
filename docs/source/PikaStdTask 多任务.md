# PikaStdTask multitasking

The PikaStdTask multitasking library provides asynchronous multitasking capabilities of Task (task loop).
## Install

Add the dependency of PikaStdLib to requestment.txt. The version number of PikaStdLib should be the same as the version number of the kernel.

````
PikaStdLib
````

Run pikaPackage.exe

## class Task():

The Task class provides the task loop function, and a task loop can be created by creating an object of the Task class.

### Methods of the Task class

````python
import PikaStdData


class Task:
    calls = PikaStdData.List()

    def __init__(self):
        pass

    # regist a function to be called always
    def call_always(self, fun_todo: any):
        pass
    
    # regist a function to be called when fun_when() return 'True'
    def call_when(self, fun_todo: any, fun_when: any):
        pass

    # register a function to be called periodically
    def call_period_ms(self, fun_todo: any, period_ms: int):
        pass

    # run all registered function once
    def run_once(self):
        pass

    # run all registered function forever
    def run_forever(self):
        pass

    # run all registered function until time is up
    def run_until_ms(self, until_ms: int):
        pass

    # need be overried to supply the system tick
    def platformGetTick(self):
        pass

````

### Instructions:

Use the `call_xxx()` method to specify the calling method, and register the function to be executed in the task object.

Use the `run_xxx()` methods to specify how the task loops and execute all functions in the `task` object.

Time-related functions, such as `call_period_ms()` and `run_until_ms()`, need to provide the system clock by creating a new class that inherits from `PikaStdTask`, and then override the `platformGetTick()` method.

### Notice:

All registered functions should be **non-blocking**, otherwise the entire task loop will be blocked.

The task loop is not real-time.

### Example:

Create a new class that inherits from `PikaStdTask`.

````python
# STM32G0.py
class Task(PikaStdTask.Task):
    # override
    def platformGetTick():
        pass
````

Override the `platformGetTick()` method.

````c
/* STM32G0_Task.c */

void STM32G0_Task_platformGetTick(PikaObj* self) {
    obj_setInt(self, "tick", HAL_GetTick());
}
````

Python use cases

````python
import STM32G0
import PikaPiZero
import PikaStdLib

pin = STM32G0.GPIO()
rgb = PikaPiZero.RGB()
mem = PikaStdLib.MemChecker()

pin.setPin('PA8')
pin.setMode('out')
pin.enalbe()

rgb.init()
rgb.enable()

print('task demo')
print('mem used max:')
mem.max()


def rgb_task():
    rgb.flow()


def led_task():
    if pin.read():
        pin.low()
    else:
        pin.high()


task = STM32G0.Task()

task.call_period_ms(rgb_task, 50)
task.call_period_ms(led_task, 500)

task.run_forever()

````
