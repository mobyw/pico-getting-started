# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.12)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# initialize pico_sdk from installed location
# (note this can come from environment, CMake cache etc)
set(PICO_SDK_PATH "D:/Projects/MCU/Pico/pico-sdk")

# Pull in Pico SDK (must be before project)
include(pico_sdk_import.cmake)

project(blink C CXX)

# Initialise the Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_executable(blink blink.c )

pico_set_program_name(blink "blink")
pico_set_program_version(blink "0.1")

pico_enable_stdio_uart(blink 0)
pico_enable_stdio_usb(blink 1)

# Add the standard library to the build
target_link_libraries(blink pico_stdlib)

pico_add_extra_outputs(blink)

