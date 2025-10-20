cmake_minimum_required(VERSION 3.30)

set(CMAKE_VERBOSE_MAKEFILE ON)

project(sample VERSION 0.1 LANGUAGES C CXX)

add_executable(sample
    sample.cpp
)

add_library(dynamicFramework SHARED
            dynamicFramework.cpp
            dynamicFramework.h
)

set_target_properties(dynamicFramework PROPERTIES
  FRAMEWORK TRUE
  SYSTEM TRUE
  PUBLIC_HEADER dynamicFramework.h
)

#when dynamicFramework is framework target, cmake will add -F<path_to_dynamicFramework> to sample CXX_INCLUDES
#when SYSTEM is TRUE, cmake will add -iframework <path_to_dynamicFramework> to sample CXX_INCLUDES
target_link_libraries(sample PRIVATE dynamicFramework)

target_compile_options(sample PRIVATE -v)

target_include_directories(sample PRIVATE /Users)

target_compile_options(sample PRIVATE -I${CMAKE_CURRENT_SOURCE_DIR}/abc)