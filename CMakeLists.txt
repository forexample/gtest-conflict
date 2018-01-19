cmake_minimum_required(VERSION 3.0)
project(foo)

option(WITH_EXTRA_TESTS "Enable extra tests" OFF)

set(sources basic_tests.cpp)
if(WITH_EXTRA_TESTS)
  list(APPEND sources extra_tests.cpp)
endif()

add_executable(boo ${sources})

find_package(GTest CONFIG REQUIRED)
target_link_libraries(boo PUBLIC GTest::gtest_main)

if(WITH_EXTRA_TESTS)
  find_package(GMock CONFIG REQUIRED)
  target_link_libraries(boo PUBLIC GMock::gmock)
endif()

enable_testing()
add_test(NAME boo COMMAND boo)

set_tests_properties(
    boo
    PROPERTIES
    ENVIRONMENT "PATH=$<TARGET_FILE_DIR:GTest::gtest_main>;$ENV{PATH}"
)
