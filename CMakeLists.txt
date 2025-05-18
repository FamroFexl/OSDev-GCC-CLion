cmake_minimum_required(VERSION 3.15)
project(BaseOS)

## Setup cross-compilation
set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_PROCESSOR i686)

ENABLE_LANGUAGE(ASM)

set(TOOLS $ENV{HOME}/opt/cross)
set(CMAKE_C_COMPILER ${TOOLS}/bin/i686-elf-gcc)
set(CMAKE_CXX_COMPILER ${TOOLS}/bin/i686-elf-g++)
set(CMAKE_ASM_COMPILER ${TOOLS}/bin/i686-elf-as)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_FLAGS "-ffreestanding -O2 -Wall -Wextra -fno-exceptions -fno-rtti")
set(CMAKE_C_FLAGS "-ffreestanding -O2 -Wall -Wextra")

set(KERNEL_BIN ${CMAKE_PROJECT_NAME}.bin)
set(KERNEL_ISO ${CMAKE_PROJECT_NAME}.iso)

#Set the location for the linker file
set(LINKER "src/kernel/arch/x86/linker.ld")

# Building the kernel binary
add_executable(${KERNEL_BIN} src/kernel/arch/x86/boot.s src/kernel/init/kernel.c)

#Set linker flags
set_target_properties(${KERNEL_BIN} PROPERTIES LINK_FLAGS "-T ${CMAKE_SOURCE_DIR}/${LINKER} -ffreestanding -O2 -nostdlib -lgcc")

target_include_directories(${KERNEL_BIN} PUBLIC include PRIVATE src)

#Copy BaseOS.bin output to /bin
add_custom_command(TARGET ${KERNEL_BIN} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E make_directory ${CMAKE_SOURCE_DIR}/bin
        COMMAND ${CMAKE_COMMAND} -E copy ${KERNEL_BIN} ${CMAKE_SOURCE_DIR}/bin/)

#Config to generate BaseOS.iso
add_custom_target(${KERNEL_ISO}
        DEPENDS ${KERNEL_BIN}
        COMMAND ${CMAKE_SOURCE_DIR}/generate-iso.sh ${KERNEL_BIN})

#Copy BaseOS.iso output to /bin
add_custom_command(TARGET ${KERNEL_ISO} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E make_directory ${CMAKE_SOURCE_DIR}/bin
        COMMAND ${CMAKE_COMMAND} -E copy ${KERNEL_ISO} ${CMAKE_SOURCE_DIR}/bin/)
