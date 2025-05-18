# OSDev in Jetbrain's CLion with a GCC Toolchain
This project implements the [OSDev](https://wiki.osdev.org) [Bare Bones](https://wiki.osdev.org/Bare_Bones) project using a GCC toolchain. It uses GCC as the compiler and LD as the linker.

## Changes
- A toolchain file had to be added to stop CLion from complaining about the default C compiler not working properly. Because the GCC toolchain uses a cross-compiler, CLion is unable to compile a test file (which likely uses stdlib) and fails. CLion also needs to be stopped from compiling for the platform it is running on.

## Using
This setup has only been successfully tested on Ubuntu 24 Linux. I would recommend avoiding MacOS and Windows because attempting to bypass their target platform assumptions is a nightmare (especially MacOS).
<br /><br />
The toolchain file should be automatically specified by the IDEA configuration. If not, manually specify it by adding `-DCMAKE_TOOLCHAIN_FILE=toolchain.cmake` to `Settings -> Build, Execution and Deployment -> CMake -> CMake Options:`
