mikrosdk_add_library(lib_platform MikroSDK.Conversions
    src/conversions.c

    include/conversions.h
)

target_link_libraries(lib_platform  PUBLIC
    MikroC.Core
)

set(conversions_def_list "")

if((${MCU_NAME} MATCHES "^PIC18.*") OR (${MCU_NAME} MATCHES "AT"))
    list(APPEND conversions_def_list __CONVERSIONS_CHIPS_PIC__)
else()
    list(APPEND conversions_def_list __CONVERSIONS_CHIPS_32BIT__)
endif()

if((${MCU_NAME} MATCHES "^PIC18.*") OR ((${MCU_NAME} MATCHES "^STM.*") AND (${CORE_NAME} MATCHES "M0")) OR (${MCU_NAME} MATCHES "AT") )
    list(APPEND conversions_def_list __CONVERSIONS_SUBSET__)
else()
    list(APPEND conversions_def_list __CONVERSIONS_SET__)
endif()

target_compile_definitions(lib_platform PUBLIC
    ${conversions_def_list}
)

target_include_directories(lib_platform
PRIVATE
    include
INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include/platform>
)

mikrosdk_install(MikroSDK.Conversions)

install(
    FILES
        include/conversions.h
    DESTINATION
        ${CMAKE_INSTALL_PREFIX}/include/platform/
)
