
cmake_minimum_required(VERSION 3.15.3)

# Path to toolchain file. This one has to be before 'project()' below
set(CMAKE_TOOLCHAIN_FILE ${CMAKE_SOURCE_DIR}/cmake/arm-none-eabi-gcc.cmake)

project(M480_Template)
set(EXECUTABLE ${PROJECT_NAME}.elf)
set(LINKER_FILE ${CMAKE_SOURCE_DIR}/BSP/Device/Nuvoton/M480/Source/GCC/gcc_arm.ld)

set(DEFINE_COMPILE_FLAGS
    -mcpu=cortex-m4
    -mthumb
    -mfpu=fpv4-sp-d16
    -mfloat-abi=hard

    -fdata-sections     # make data gc-able
    -ffunction-sections # make function gc-able

    -Wall
    -O0
    -g3
)

enable_language(C ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)

include(cmake/m480bsp.cmake)
# include(cmake/freertos.cmake)
# include(cmake/lwip.cmake)

set(SRC_FILES
    src/main.c
)

# Build the executable based on the source files
add_executable(${EXECUTABLE} ${SRC_FILES})

target_link_libraries(${EXECUTABLE}
        m480bsp
        # freertos
)

# List of include directories
target_include_directories(${EXECUTABLE} PRIVATE
        # inc
        m480bsp
)

# Compiler options
target_compile_options(${EXECUTABLE} PRIVATE
        ${DEFINE_COMPILE_FLAGS}
        )

# Linker options
target_link_options(${EXECUTABLE} PRIVATE
        -T${LINKER_FILE}
        -mcpu=cortex-m4
        -mthumb
        -mfpu=fpv4-sp-d16
        -mfloat-abi=hard
        -specs=nano.specs
        -specs=nosys.specs
        -lc     # C standard library
        -lm     # math library(math.h)
        -Wl,-Map=${PROJECT_NAME}.map,--cref
        -Wl,--gc-sections   # gc no-referenced code/data
        -Xlinker -print-memory-usage -Xlinker
        )

# Optional: Print executable size as part of the post build process
add_custom_command(TARGET ${EXECUTABLE}
        POST_BUILD
        COMMAND ${CMAKE_SIZE_UTIL} ${EXECUTABLE})

# Optional: Create hex, bin and S-Record files after the build
add_custom_command(TARGET ${EXECUTABLE}
        POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -O ihex ${EXECUTABLE} ${PROJECT_NAME}.hex
        COMMAND ${CMAKE_OBJCOPY} -O binary ${EXECUTABLE} ${PROJECT_NAME}.bin)