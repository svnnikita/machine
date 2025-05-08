cmake_minimum_required(VERSION 3.20)
project(STM32_Project LANGUAGES C ASM)

# Настройка компилятора
set(CMAKE_C_COMPILER arm-none-eabi-gcc)

# Флаги компиляции
add_compile_options(
    -mcpu=cortex-m4
    -mthumb
    -Og
    -DUSE_HAL_DRIVER
    -DSTM32F407xx
)

# Пути к заголовкам
include_directories(
    Core/Inc
    Drivers/STM32F4xx_HAL_Driver/Inc
    Drivers/CMSIS/Device/ST/STM32F4xx/Include
    Drivers/CMSIS/Include
)

# Исходники
file(GLOB_RECURSE SOURCES 
    "Core/Src/*.c"
    "Drivers/STM32F4xx_HAL_Driver/Src/*.c"
)

# Сборка исполняемого файла
add_executable(${PROJECT_NAME} 
    ${SOURCES}
    ${STARTUP_ASM}
)

# Линковка
target_link_options(${PROJECT_NAME} PRIVATE 
    -T${CMAKE_SOURCE_DIR}/STM32F407VETx_FLASH.ld
    -Wl,--gc-sections
)