cmake_minimum_required(VERSION 3.18.1)
project("lsplant_test")

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(test SHARED test.cpp elf_util.cpp)
find_package(dobby REQUIRED CONFIG)
find_package(lsplant REQUIRED CONFIG)
target_link_libraries(test log dobby::dobby lsplant::lsplant)
set_target_properties(test PROPERTIES CXX_SCAN_FOR_MODULES ON)

get_property(lsplant_INCLUDE TARGET lsplant::lsplant PROPERTY INTERFACE_INCLUDE_DIRECTORIES)
file(GLOB_RECURSE lsplant_MODULES "${lsplant_INCLUDE}/*.ixx")
target_sources(lsplant::lsplant INTERFACE FILE_SET CXX_MODULES BASE_DIRS ${lsplant_INCLUDE} FILES ${lsplant_MODULES})
set_target_properties(lsplant::lsplant PROPERTIES IMPORTED_CXX_MODULES_INCLUDE_DIRECTORIES "${lsplant_INCLUDE}")
