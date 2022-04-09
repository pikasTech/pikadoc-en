# ARM-2D based GUI simulation project

## Preface


good news! The Arm2D module and simulation project of pikascript are preliminarily sorted out! pikaScript, ARM-2D, rt-thread work together, unlock new poses for python to play Arm2D! No hardware is needed, and it can be simulated directly, which is very convenient.


It is also very simple to deploy and run this simulation project on your own computer, just follow the steps below~
## Get the simulation project


First enter the code repository of PikaScript


[https://github.com/pikastech/pikascript](https://github.com/pikastech/pikascript) (requires scientific Internet access)


![](assets/139675132-739ec77b-db22-4ed9-a670-77ec7544d1b9.png)


Then click a Star


If you can't get in, go in here


[https://gitee.com/Lyon1998/pikascript](https://gitee.com/Lyon1998/pikascript) (also available in China)


![](assets/139675170-fe0ce449-872f-466e-8780-74465730178a.png)


Then also click a Star


Well, after you click on Star, we will start the next step.


We scroll down from the homepage of the code repository, see Get PikaScript, and then click PikaPackage.exe to download the package manager for pikascript.


![](assets/139675454-596829d1-0325-42ab-96c5-f3d3d369d7d4.png)


The same is true on Gitee, you can choose one.


![](assets/139675486-0f63e7b4-669d-4370-80ad-134c0f28f203.png)


Next, put PikaPackage.exe on the disk where you want to download PikaScript. In order to save your C drive, you can put PikaPackage.exe on the D drive, which can be any location on the D drive.


Double-click PikaPackage.exe, and the package manager will automatically download the latest PikaScript to the D:/tmp/pikascript folder for you. (If it is placed on the C drive, it will be downloaded to C:/tmp/pikascript)


The download uses domestic resources, does not require scientific Internet access, and the speed should be very good.


After downloading, you can delete this pikaPackage.exe.


If everything goes well, you can find the downloaded pikascript code repository in the /tmp/pikascript folder.


![](assets/139676635-c3f1c6ae-ab44-42a5-ab9a-9bedd2383f31.png)


We enter the bsp folder and copy a copy of simulation-rtt-qemu-arm2d out.


![](assets/139677151-33c1dbd0-c2f2-4ea3-a5ae-569e5a448cce.png)


So far, the project is ready to be ok.


## Install the development environment


After you have the project, you also need to install the development environment. There are only two things that need to be installed. One is rt-thread studio, which is used as an IDE. rt-thread studio integrates qemu, which is very convenient for simulating mcu and gui. The other is the latest arm gcc toolchain.


### rt-thread studio installation package link


[https://download-sh-cmcc.rt-thread.org:9151/www/studio/download/RT-Thread Studio-v2.1.2-setup-x86_64_20210831-1200.exe](https://download-sh -cmcc.rt-thread.org:9151/www/studio/download/RT-Thread%20Studio-v2.1.2-setup-x86_64_20210831-1200.exe)


### arm gcc installation package link


[https://developer.arm.com/-/media/Files/downloads/gnu-rm/10.3-2021.10/gcc-arm-none-eabi-10.3-2021.10-win32.exe](https://developer. arm.com/-/media/Files/downloads/gnu-rm/10.3-2021.10/gcc-arm-none-eabi-10.3-2021.10-win32.exe)


You can install rt-thread studio where you like, arm gcc should be installed on the default c drive.


Once installed, you can start playing arm-2d with python.


## Pull the module and precompile


We enter the simulation-rtt-qemu-arm2d/packages/pikascrpt directory, which is the pikascrit file.


![](assets/139678258-e2cdc50d-475b-435a-af8c-7c19cc3a218d.png)


For the convenience of version management, pikascript uses request.txt to manage the version of the kernel and modules, so there is no source code of pikascript in this folder, only a request.txt file. If you are familiar with pip, you will find this file and the version used by pip The description file is exactly the same.


![](assets/139678404-9b747c0a-6508-4f6d-b0ca-671560f31fbd.png)


We double-click to run pikaPackage.exe in this folder, and the kernel and modules of pikascript are pulled down.


![](assets/139678437-a77b7278-cafd-485e-b353-94a12302c8cb.png)


This is what it looks like after pulling it down.


![](assets/139678713-0cd86aef-2996-4898-931d-68c805534312.png)


Finally, we run rust-msc-latest-win10.exe to precompile, and it's OK.


![](assets/139678750-befc11e9-d812-4fcf-949e-64dd873d0211.png)


## run


We open RT-Thread Studio and click Import


![](assets/139679061-2e3b2ea0-8e9a-44c9-9a0f-6f40d82a0208.png)


Then select the simulation-rtt-qemu-arm2d folder


![](assets/139679380-3a45f426-e575-4142-b5f1-76439c7efc38.png)


Select the project, then click the hammer to compile, and then click the bug to enter the simulation


![](assets/139679532-e19ed911-c7f4-4840-a5e3-f5b66905a62f.png)


A QEMU box will pop up, then click Run.


![](assets/139679756-cb099fc9-c3e9-4b76-9037-38392350530b.png)


If the operation is successful, you can see a small blue square on the white background. So far the deployment has been successful.


![](assets/139679797-3ce8f253-beb9-480f-90ee-1844500a77ab.png)


## Modify the python code and try


The source code of python is in simulation-rtt-qemu-arm2d/packages/pikascript/main.py, you can open it and see~


![](assets/139679915-45d1362e-7066-4829-ae83-b4bbc5d0aaa0.png)


The following is the content of main.py, create a new box object, and then set the color and position, you can try to change the color to 'red' or change the coordinates to see, you can also create another screen.elems.b2 try .


![](assets/139680125-11ff47b3-e75e-47f4-8dd7-5b310c5be16c.png)


Remember to precompile after each modification to convert python to .c file in the project


![](assets/139680376-b9681759-971a-43f7-9282-ee0e35a367a5.png)


Then compile again, enter the simulation, and you can see the effect. This time I changed the square to red.


![](assets/139680521-20f83ee3-2163-4649-ad23-ae73b77f482e.png)


## Conclusion


This is the Arm-2D warehouse~ Students who haven't starred remember to add a star~

[https://github.com/ARM-software/EndpointAI](https://github.com/ARM-software/EndpointAI)


![](assets/139681272-73a1a8c2-2889-4dab-bd05-7174cb14334c.png)


Thanks to liuduanfei for the rtt-Arm2d-qemu simulation project~ Here is the github homepage of liuduanfei


![](assets/139681543-99a64e9b-eb10-4c8e-bbe3-e8170c85385a.png)


[https://github.com/liuduanfei](https://github.com/liuduanfei)