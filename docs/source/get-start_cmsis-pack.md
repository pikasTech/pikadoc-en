# Start with CMSIS-PACK

Users developing with Keil can use CMSIS-PACK to install PikaScript with one click.

## Install PikaTech.PikaScript.x.x.x.pack

[ Click to download ](https://gitee.com/Lyon1998/pikascript/attach_files/1106948/download)

Just go all the way to Next and install

![](assets/image-20220624090014867.png)

## Set in the project

![](assets/image-20220624090340868.png)

Check PikaScript, including Core and PikaStdLib

![](assets/image-20220624090401713.png)

Here you can see that PikaScript has been added

![](assets/image-20220624090444608.png)

In Before Build add

```
.\RTE\PikaScript\pikaBeforBuild-keil.bat
```

![](assets/image-20220624090543736.png)

Then introduce in main.c

``` c
#include "pikaScript.h"
```

Start PikaScript after initializing the system and printf

``` c
PikaObj *pikaMain = pikaScriptInit();
```

Compile successfully.

![](assets/image-20220624091046123.png)

Run successfully !

![](assets/image-20220624091137190.png)

For more usage, please refer to [porting guide](https://pikadoc.readthedocs.io/en/latest/index_porting.html)
