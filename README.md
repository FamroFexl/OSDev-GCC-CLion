# OSDev in Jetbrain's CLion with a GCC Toolchain
This project implements the [OSDev](https://wiki.osdev.org) [Bare Bones](https://wiki.osdev.org/Bare_Bones) project using a GCC toolchain. It uses GCC as the compiler and LD as the linker.

## Changes
- A toolchain file had to be added to stop CLion from complaining about the default C compiler not working properly.

## Using
The toolchain file should be automatically specified by the IDEA configuration. If not, manually specify it by adding `-DCMAKE_TOOLCHAIN_FILE=toolchain.cmake` to `Settings -> Build, Execution and Deployment -> CMake -> CMake Options:`
