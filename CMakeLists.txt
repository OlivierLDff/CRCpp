#
#   CRCPP CMake script by Olivier Le Doeuff
#

# ┌──────────────────────────────────────────────────────────────────┐
# │                       CMAKE PROPERTIES                           │
# └──────────────────────────────────────────────────────────────────┘

cmake_minimum_required(VERSION 3.0.0 FATAL_ERROR)

# ┌──────────────────────────────────────────────────────────────────┐
# │                       PROJECT SETTINGS                           │
# └──────────────────────────────────────────────────────────────────┘

set(CRCPP_TARGET "CRCpp" CACHE STRING "Target Name")
set(CRCPP_USE_NAMESPACE ON CACHE BOOL "Should the library be compiled with a namespace. Default: ON")
set(CRCPP_USE_CPP11 ON CACHE BOOL "Define to enables C++11 features (move semantics, constexpr, static_assert, etc.). Not defined by default. Default: ON")
set(CRCPP_BUILD_TESTS OFF CACHE BOOL "Build test project or not")
set(CRCPP_FOLDER_PREFIX ${CRCPP_TARGET} CACHE STRING "Prefix folder for all CRCpp generated targets in generated project (only decorative)" )

# ┌──────────────────────────────────────────────────────────────────┐
# │                       STATUS                                     │
# └──────────────────────────────────────────────────────────────────┘

project(${CRCPP_TARGET} VERSION "1.0.0" LANGUAGES CXX)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

message(STATUS "------ ${CRCPP_TARGET} Configuration ------")
message(STATUS "CRCPP_USE_NAMESPACE       : ${CRCPP_USE_NAMESPACE}")
message(STATUS "CRCPP_USE_CPP11           : ${CRCPP_USE_CPP11}")
message(STATUS "CRCPP_BUILD_TESTS         : ${CRCPP_BUILD_TESTS}")
message(STATUS "CRCPP_FOLDER_PREFIX       : ${CRCPP_FOLDER_PREFIX}")
message(STATUS "------ ${CRCPP_TARGET} End Configuration ------")

# ┌──────────────────────────────────────────────────────────────────┐
# │                       LIBRARY                                    │
# └──────────────────────────────────────────────────────────────────┘

add_library(${CRCPP_TARGET} INTERFACE)
target_sources(${CRCPP_TARGET} INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}/inc/CRCpp.hpp")

if(CRCPP_USE_NAMESPACE)
    target_compile_definitions(${CRCPP_TARGET} INTERFACE -DCRCPP_USE_NAMESPACE)
endif() # CRCPP_USE_NAMESPACE

if(CRCPP_USE_CPP11)
    target_compile_definitions(${CRCPP_TARGET} INTERFACE -DCRCPP_USE_CPP11)
endif() # CRCPP_USE_CPP11

target_include_directories(${CRCPP_TARGET} INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/inc)

# ┌──────────────────────────────────────────────────────────────────┐
# │                       TESTS                                      │
# └──────────────────────────────────────────────────────────────────┘

if(CRCPP_BUILD_TESTS)

    enable_testing()

    set(CRCPP_TESTS ${CRCPP_TARGET}_Tests)
    add_executable(${CRCPP_TESTS} "${CMAKE_CURRENT_SOURCE_DIR}/test/src/main.cpp")
    target_link_libraries(${CRCPP_TESTS} ${CRCPP_TARGET})

    if(CRCPP_FOLDER_PREFIX)
        set_target_properties(${CRCPP_TESTS} PROPERTIES FOLDER ${CRCPP_FOLDER_PREFIX}/Tests)
    endif()

    message(STATUS "Add Test: ${CRCPP_TESTS}")
    add_test(NAME ${CRCPP_TESTS} COMMAND ${CRCPP_TESTS})

endif()
