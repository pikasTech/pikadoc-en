# Docking with IDE
## Overview
The toolset that PikaPython needs to interface with the IDE includes:

Package manager pikaPackage.exe

Refer to **package manager and module management** related documents

Precompiler rust-msc-latest-win10.exe

Refer to **module development** related documents

## calling method
### 1. Start path:

   1. [Bare metal project root directory]/pikascript path
   1. [rtthread project root directory]/packages/pikascript-latest path
### 2. Package Manager

   1. When pulling a module remotely from PikaSciprt for the first time, you need to run pikaPackge.exe
   1. After modifying request.txt, you need to run pikaPackage.exe
   1. If you use the latest version of the module, you need to run pikaPackage.exe when updating the module to the latest
### 3. Precompiler
a. run before each compilation
**[Note]:** When running for the first time, use pikaPackage.exe to pull the precompiler first.
## Project Files

   1. After executing the package manager or precompiler, you need to add **all (including subfolders)** .c files and include paths under **pikascript-lib, pikascript-core, pikascript-api** .
   1. Reset PikaPython project files: After deleting pikascript-lib, pikascript-core, and pikascript-api, re-run pikaPackage.exe and rust-msc-latest-win10.exe.

## example

Automatic precompile script pikaBeforeBuild-keil.bat written for keil:

```bash
cd ../pikascript

if not exist pikascript-core (
    pikaPackage.exe
)
rust-msc-latest-win10.exe
````
