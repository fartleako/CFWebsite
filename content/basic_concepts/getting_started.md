---
title: "Installation"
date: 2022-10-22T18:53:54+02:00
draft: false
weight: 10
---

## Basic Setup

The following steps have to be performed, no matter what OS you are running.

1. If you haven't done already, install git https://git-scm.com/book/en/v2/Getting-Started-Installing-Git \
and clone this repository with `git clone https://github.com/Tachikoma87/CrossForge.git` 
2. CrossForge relies on vcpkg as package manager on Windows and Linux https://github.com/microsoft/vcpkg. Follow the installation instructions for your system: https://github.com/microsoft/vcpkg#getting-started

### **Windows**

The prefered way to compile Crossforge under Windows is with the help of [Visual Studio](https://visualstudio.microsoft.com/) (A guide to install Visual Studio can be found [here](https://learn.microsoft.com/en-us/visualstudio/install/)). 

Due to the fact that Crossforge uses CMake and not the native Visual Studio Solutions, you need to select **C++Cmake Tools for Windows** during the installation or update your Visual Studio installer (Here you can find more about [CMake](https://cmake.org/)).

**How to update the installer**
1. Open the Visual Studio Installer
1. click on "Change" 
1. click on "Single components"
1. search for "CMake" and tick the box
![Install CMake](/CMake_install.JPG)

Now we need vcpkg to work with Visual Studio. Open a command prompt and type `[path-to-vcpkg]\vcpkg integrate install`

Start Visual Studio and select Open a local folder. Move to the directory where you executed the first git command and open the CrossForge folder. If the CMake configuration process does not start automatically, right click on CMakeLists.txt and select configure cache. Wait for the configuration to complete, then select Build->Build All (F7) from the menu bar. After completion select CForgeSandbox.exe as starting item and select Debug->Start Debugging (F5) from the menu bar or click on the small green right facing arrow.

## **Linux**:
CrossForge uses vcpkg in manifest mode, which means that dependencies should be installed automatically. Run the following commands from the top directory to build the software:

```
cmake -B out/build-lin -S . "-DCMAKE_TOOLCHAIN_FILE=[path-to-vcpkg]/scripts/buildsystems/vcpkg.cmake" 

cmake --build out/build-lin
```

And run the sandbox with

```
cd out/build-lin

./CForgeSandbox
```
