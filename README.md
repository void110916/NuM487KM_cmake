# NuM487KM_cmake
A  CMake build template project with GNU GCC toolchain for nuvoton M480 series MCUs.
## Project Architecture
```
NuM487_cmake
|    CMakeLists.txt
|    flash.cmd
|    README.md
|    LICENSE
|    .gitignore
|
|────src
|    |────main.c
|
|────inc   
|
|────device
|    └────M481_v1.svd
|
|────BSP
|    |────CMSIS
|    |────Device
|    |────StdDriver
|    └────BSP here
|────cmake
|    |────arm-none-eabi-gcc.cmake
|    |────freertos.cmake
|    └────m480bsp.cmake
|
|────.vscode
|    |────.cortex-debug.peripherals.state.json
|    |────.cortex-debug.registers.state.json
|    |────c_cpp_properties.json
|    |────cmake-kits.json
|    |────launch.json
|    |────settings.json
|    └────taskss.json
```
## Requirement
* CMake 3.15.3 minimum
* arm-none-eabi-gcc
* nuvoton M480 BSP
* vscode extension
  * C/C++
  * CMake Tools
  * IntelliCode
## Using this project
1. check the following path in the files with `//* PATH *//` behind.
   * c_cpp_properties.json
   * launch.json
   * cmake/arm-none-eabi-gcc.cmake
2. download BSP on nuvoton website and unzip into directory `BSP`.
3. select active project and kit then click Build button on the button.
