# How to contribute to PikaScript BSP
## Steps to make BSP:
### Make pikascript template project
- The BSP of pikascript is very simple, it is a pikascript template project that can be compiled independently.
- This project only needs to be able to run pikascript **basically**.
- You can refer to the **New Platform Porting Guide** to ensure that `print('hello PikaScript!')` in main.py can be run normally.
### Clean up project
- Clean up **compiled products**, leaving only project files and source code. (The compilation products include intermediate files .o .d , binary products .bin, .hex , executable files .exe , etc.).
- Clean up the **auto-pull** and **auto-generate** codes in the pikascript folder. Only the main.py, request.txt, and pikaPackage.exe files can be kept in the pikascript folder.
- Clean up unused source code and libraries, and control the size of the project to within **50MB**. **If the size of the project is still larger than 50MB after cleaning, you can create a new special warehouse to place the BSP, and only place a README.md containing a link to the special warehouse in pikscript/bsp. **
### Submit file
- Enter the pikascript code repository, either gitee or github, fork a pikascript repository, and then clone the forked repository locally.

![](assets/1638664526181-09b00c29-fc72-429a-bb99-3f009eae141e.png)

- Create a new folder in the [repository after fork]/bsp directory, then copy it into the template project, use the git command to add files, and push it to the pikascript repository after **fork.
  
```shell
cd pikascript/bsp
git add *
git commit -m 'add bsp'
git push
````


- (Optional) Update BSP information in pikascript/README.md and pikascript/README_zh.md.
- Open Pull Request and wait for merge.