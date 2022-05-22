# Start with RT-Thread package

PikaScript has been added to the [RT-Thread package](https://packages.rt-thread.org/detail.html?package=pikascript). Under the programming language category, you can quickly use PikaScript by directly adding packages.

The PikaScript package supports **full RT-Thread BSP**.
![](assets/1638840464842-02580253-48dc-4dcc-94a4-e62f1b596b38.png)
If you encounter compatibility problems during use, you can file an issue at [gitee](https://github.com/pikastech/pikascript), [github](https://github.com/pikasTech/pikascript) or [ Forum](https://whycan.com/f_55.html) to ask questions.

## Install

   Import the pikascript package

   ![](assets/159112436-d8814770-0e86-4016-a529-7053d7256df9.png)

   ![](assets/159112451-20335611-fb55-42da-b1ec-c6e9878333b3.png)

   ![](assets/159112459-36030f2a-69f7-4e8f-8b3f-57011eaff82b.png)

   ![](assets/159112482-378a549c-fb3b-4be6-b72e-a02b8e1217f3.png)

   Add RT_WEAK before rt_vsnprintf in rt-thread/src/kservice.c **(only for rt_thread version 4.1.0 and below)**

![](assets/1639103607485-f33b48f8-a127-4612-9c4a-e2094ec5d79e.png)

   Delete the static **(only for rt_thread version 4.1.0 and below)** of finsh_getchar in rt-thread/components/finsh/shell.c

![](assets/1639103788555-fcf1c31c-386f-4baf-b1d0-4f3016af32bc.png)

## start pikascript

**Option 1: Start with msh (default mode)**

   Use the pikaRTThread module in packages/pikascript-latest/requestment.txt (included by default).

The latest default [requestment.txt](https://github.com/pikastech/pikascript/blob/master/port/rt-thread/requestment.txt) can be viewed here.

   Type "pika" in msh to **start** PikaScript in a thread.

The initial startup will execute the /pikascript-latest/main.py initialization script. After execution, enter pika **interactive running** mode,
Enter "exit()" to return to msh, enter "pika" again to enter pikascript, and enter directly into interactive mode.
![](assets/1639058943232-9f0e0f78-0c8e-4b80-9283-6113c2450edf.png)
**Option 2: Automatically start at boot**

   Enter the package detailed configuration

![](assets/1639184483048-498f471e-cae7-4b6f-ad94-c1b5149d621c.png)

   Check Enable auto-running PikaScript

![](assets/1639184596044-a85902ac-601c-49b6-b2e5-3d20bd55ce81.png)

   3 After setting, it will automatically start PikaScript, run the main.py script, and then go back to msh

Enter **pika** in msh to run interactively.

**Option 3: Manual start**

If you need **custom start**, you can use the following methods to start manually.

   Import header file:
````c
#include "pikaScript.h"
````
 Start PikaScript:
````c
PikaObj * pikaMain = pikaScriptInit();
````

   run interactively

Refer to the **Support Interactive Run** section of the documentation.

   Serial download Python script

Refer to the **Support Serial Port Download Python** part of the document.

### Using the PikaScript module and package manager

   Modify pikascript-latest/requestment.txt, then right-click the project, Sconscripts Update, you can install the module/modify the module version, and precompile.

![](assets/1639531121038-abc40292-62fe-4a30-b074-7101714f6db7.jpeg)

For more usage, please refer to the **package manager**, **module usage, module development** part of the documentation.
