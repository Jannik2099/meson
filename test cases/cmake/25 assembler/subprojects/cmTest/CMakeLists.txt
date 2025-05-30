cmake_minimum_required(VERSION ${CMAKE_VERSION})

project(cmTest)

#Detect processor
if ("${CMAKE_SYSTEM_PROCESSOR}" MATCHES "amd64")
    SET(TEST_PROCESSOR "x86_64")
elseif ("${CMAKE_SYSTEM_PROCESSOR}" MATCHES "x86_64")
    SET(TEST_PROCESSOR "x86_64")
elseif ("${CMAKE_SYSTEM_PROCESSOR}" MATCHES "i386")
    SET(TEST_PROCESSOR "x86")
elseif ("${CMAKE_SYSTEM_PROCESSOR}" MATCHES "i686")
    SET(TEST_PROCESSOR "x86")
elseif ("${CMAKE_SYSTEM_PROCESSOR}" MATCHES "arm")
    SET(TEST_PROCESSOR "arm")
elseif ("${CMAKE_SYSTEM_PROCESSOR}" MATCHES "aarch64")
    SET(TEST_PROCESSOR "arm")
else ()
    message(FATAL_ERROR "MESON_SKIP_TEST: Unsupported Assembler Platform")
endif ()

#Detect ABI
if ("${CMAKE_SYSTEM_NAME}" MATCHES "Linux")
    SET(TEST_ABI "sysv")
elseif ("${CMAKE_SYSTEM_NAME}" MATCHES "FreeBSD")
    SET(TEST_ABI "sysv")
elseif ("${CMAKE_SYSTEM_NAME}" MATCHES "NetBSD")
    SET(TEST_ABI "sysv")
elseif ("${CMAKE_SYSTEM_NAME}" MATCHES "OpenBSD")
    SET(TEST_ABI "sysv")
else ()
    message(FATAL_ERROR "MESON_SKIP_TEST: Unsupported Assembler Platform")
endif ()

SET(TEST_PLATFORM "${TEST_PROCESSOR}-${TEST_ABI}")

if (    ("${TEST_PLATFORM}" MATCHES "x86_64-sysv")
     OR ("${TEST_PLATFORM}" MATCHES "x86-sysv")
     OR ("${TEST_PLATFORM}" MATCHES "arm-sysv"))
    SET(CMAKE_ASM_COMPILER ${CMAKE_C_COMPILER})
    enable_language(ASM)
    SET(TEST_SOURCE "cmTestAsm.s")
endif ()

add_library(cmTest STATIC cmTest.c ${TEST_SOURCE})
