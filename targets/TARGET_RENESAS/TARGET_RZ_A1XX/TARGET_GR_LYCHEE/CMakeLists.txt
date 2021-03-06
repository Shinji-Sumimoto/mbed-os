# Copyright (c) 2020 ARM Limited. All rights reserved.
# SPDX-License-Identifier: Apache-2.0

if(${MBED_TOOLCHAIN} STREQUAL "ARM")
    set(LINKER_FILE device/TOOLCHAIN_ARM_STD/MBRZA1LU.sct)
    set(STARTUP_FILE device/TOOLCHAIN_ARM_STD/startup_RZ_A1LU.S)
    set(WEAK_HANDLER_FILE device/TOOLCHAIN_ARM_STD/weak_handler.S)
elseif(${MBED_TOOLCHAIN} STREQUAL "GCC_ARM")
    set(LINKER_FILE device/TOOLCHAIN_GCC_ARM/RZA1LU.ld)
    set(STARTUP_FILE device/TOOLCHAIN_GCC_ARM/startup_RZ1ALU.S)
    set(WEAK_HANDLER_FILE device/TOOLCHAIN_GCC_ARM/weak_handler.S)
endif()

set_property(GLOBAL PROPERTY MBED_TARGET_LINKER_FILE ${CMAKE_CURRENT_SOURCE_DIR}/${LINKER_FILE})


target_include_directories(mbed-core
    INTERFACE
        .
        device
        device/inc
        device/inc/iobitmasks
        device/inc/iodefines
)

target_sources(mbed-core
    INTERFACE
    trng_api_esp32.cpp
    PeripheralPins.c

    device/RZ_A1_Init.c
    device/cmsis_nvic.c
    device/mbed_sf_boot.c
    device/mmu_RZ_A1LU.c
    device/nvic_wrapper.c
    device/os_tick_ostm.c
    device/system_RZ_A1LU.c

    ${STARTUP_FILE}
    ${WEAK_HANDLER_FILE}
)

