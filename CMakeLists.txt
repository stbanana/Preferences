cmake_minimum_required(VERSION 3.15)
project(Preferences C)

option(PREFERENCES_BUILD_TESTS "Build unit tests" ON)
option(PREFERENCES_BUILD_EXAMPLE "Build example" ON)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

add_library(preferences
    common/kvstore.c
)
target_include_directories(preferences PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/common
)

if (PREFERENCES_BUILD_EXAMPLE)
    add_executable(preferences_example
        Example/main.c
    )
    target_link_libraries(preferences_example preferences m)
endif()

if (PREFERENCES_BUILD_TESTS)
    enable_testing()
    add_executable(preferences_tests
        unitTest/test_kvstore.c
    )
    target_link_libraries(preferences_tests preferences m)
    add_test(NAME kvstore_basic COMMAND preferences_tests)
endif()