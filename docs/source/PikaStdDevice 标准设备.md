# PikaStdDevice standard device

PikaStdDevice is an abstract device model that provides a unified peripheral API across platforms.
## Install

- Add PikaStdDevice dependency in requestment.txt.
````
PikaStdDevice==v1.4.3
````

- Run pikaPackage.exe
## Why have standard equipment modules
What is a standard equipment module? Let's start with other scripting technologies, such as MicroPython, there is no unified peripheral calling API, which makes users need to re-learn the API when using different platforms. For example, the following is the code for MicroPython to drive GPIO on the STM32F4 platform .

![](assets/1638495380966-02a52d33-9986-401c-a7e1-136ce71ad53e.webp)

This is for ESP8266

![](assets/1638495381179-e6afcca5-7f32-4a2f-a531-10f6b106db15.webp)

It can be clearly seen that when selecting the pin of the pin, one uses a string, while the other uses an integer number. When controlling the level, one uses the `high()`, `low()` methods, while the other uses the `high()` and `low()` methods. An API standard driven by `on()`, `off()` methods, in short, is confusing.
Is there any way to unify the APIs of peripherals, so that users only need to be familiar with a set of APIs and can be used on any platform?
There is a method, that is, the PikaStdDevice standard device driver module.

## module structure

![](https://user-images.githubusercontent.com/88232613/171090021-2c5667b2-4656-4e44-9ade-f869e047007e.png)

PikaStdDevice is an abstract device driver module that defines all user APIs. As long as the driver modules of each platform inherit from PikaStdDevice, they can obtain the same user API, and PikaStdDevice will indirectly call the platform driver and rewrite the underlying layer through polymorphism. The platform driver can work on different platforms.

## Module example

Taking the GPIO module as an example, the following is the user API defined by PikaStdDevice

``` python
class GPIO(TinyObj):
    def __init__(self):
        pass

    def init(self):
        pass

    def setPin(self, pinName: str):
        pass

    def setId(self, id: int):
        pass

    def getId(self) -> int:
        pass

    def getPin(self) -> str:
        pass

    def setMode(self, mode: str):
        pass

    def getMode(self) -> str:
        pass

    def setPull(self, pull: str):
        pass

    def enable(self):
        pass

    def disable(self):
        pass

    def high(self):
        pass

    def low(self):
        pass

    def read(self) -> int:
        pass

````



The following are the platform drivers that PikaStdDevice needs to rewrite

``` python
    # need be overrid
    def platformHigh(self):
        pass

    # need override
    def platformLow(self):
        pass

    # need override
    def platformEnable(self):
        pass

    # need override
    def platformDisable(self):
        pass

    # need override
    def platformSetMode(self):
        pass

    # need override
    def platformRead(self):
        pass
````

And the GPIO module of CH32V103 we want to make is inherited from the standard driver module.

``` python
class GPIO(PikaStdDevice.GPIO):
    # override
    def platformHigh(self):
        pass

    # override
    def platformLow(self):
        pass

    # override
    def platformEnable(self):
        pass

    # override
    def platformDisable(self):
        pass

    # override
    def platformSetMode(self):
        pass

    # override
    def platformRead(self):
        pass
````

Through this method, we can make the STM32 driver module, CH32 driver module, and ESP32 driver module have the same user API. As long as users are familiar with a set of APIs, they can easily use all platforms that support the PikaScript standard driver module.
The following are some of the C native driver functions that are registered in the driver module

```` C
void CH32V103_GPIO_platformEnable(PikaObj *self){
    char *pin = obj_getStr(self, "pin");
    char *mode = obj_getStr(self, "mode");
    GPIO_TypeDef *GPIO_group = get_GPIO_group(pin);
    uint16_t GPIO_pin = get_GPIO_pin(pin);
    GPIOMode_TypeDef GPIO_mode = get_GPIO_mode(mode);
    uint32_t GPIO_clock_group = get_GPIO_Clock_group(pin);

    GPIO_InitTypeDef GPIO_InitStructure;
    RCC_APB2PeriphClockCmd(GPIO_clock_group,ENABLE);
    GPIO_InitStructure.GPIO_Pin = GPIO_pin;
    GPIO_InitStructure.GPIO_Mode = GPIO_mode;
    GPIO_InitStructure.GPIO_Speed=GPIO_Speed_50MHz;
    GPIO_Init(GPIO_group, &GPIO_InitStructure);
}

void CH32V103_GPIO_platformHigh(PikaObj *self){
    char *pin = obj_getStr(self, "pin");
    GPIO_TypeDef *GPIO_group = get_GPIO_group(pin);
    uint16_t GPIO_pin = get_GPIO_pin(pin);

    GPIO_WriteBit(GPIO_group, GPIO_pin, Bit_SET);
}
````
