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

target_include_directories(sample PRIVATE test/A)

target_include_directories(sample PRIVATE test/B/b.framework)

target_include_directories(sample PRIVATE test/C)

target_include_directories(sample SYSTEM PRIVATE test/E)

target_compile_options(sample PRIVATE -I${CMAKE_CURRENT_SOURCE_DIR}/test/D)

# clang -v 
# [build]  /Users/admin/Desktop/bug-sample/test/A
# [build]  /Users/admin/Desktop/bug-sample/test/B (framework directory)
# [build]  /Users/admin/Desktop/bug-sample/test/C
# [build]  /Users/admin/Desktop/bug-sample/test/D
# [build]  /Users/admin/Desktop/bug-sample/test/E
# [build]  /Users/admin/Desktop/bug-sample/build (framework directory)
# [build]  /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/usr/include/c++/v1
# [build]  /Library/Developer/CommandLineTools/usr/lib/clang/16/include
# [build]  /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/usr/include
# [build]  /Library/Developer/CommandLineTools/usr/include
# [build]  /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/System/Library/Frameworks (framework directory)

# cmake fileapi
# {
#   "compileCommandFragments" : 
#   [
#     {
#       "fragment" : "-g -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk -mmacosx-version-min=14.6"
#     },
#     {
#       "backtrace" : 3,
#       "fragment" : "-v"
#     },
#     {
#       "backtrace" : 4,
#       "fragment" : "-I/Users/admin/Desktop/bug-sample/test/D"
#     }
#   ],
#   "frameworks" : 
#   [
#     {
#       "backtrace" : 6,
#       "path" : "/Users/admin/Desktop/bug-sample/test/B/b.framework"
#     },
#     {
#       "isSystem" : true,
#       "path" : "/Users/admin/Desktop/bug-sample/build/dynamicFramework.framework"
#     }
#   ],
#   "includes" : 
#   [
#     {
#       "backtrace" : 5,
#       "path" : "/Users/admin/Desktop/bug-sample/test/A"
#     },
#     {
#       "backtrace" : 7,
#       "path" : "/Users/admin/Desktop/bug-sample/test/C"
#     },
#     {
#       "backtrace" : 8,
#       "isSystem" : true,
#       "path" : "/Users/admin/Desktop/bug-sample/test/E"
#     }
#   ]
# }

#cmake-tools configurations
# {
#   "uri": "file:///Users/admin/Desktop/bug-sample/sample.cpp",
#   "configuration": {
#     "includePath": [
#       "/Users/admin/Desktop/bug-sample/test/A",
#       "/Users/admin/Desktop/bug-sample/test/C",
#       "/Users/admin/Desktop/bug-sample/test/E",
#       "/Users/admin/Desktop/bug-sample/test/B",
#       "/Users/admin/Desktop/bug-sample/build"
#     ],
#     "defines": [],
#     "compilerPath": "/usr/bin/clang++",
#     "compilerArgs": [],
#     "compilerFragments": [
#       "-g -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk -mmacosx-version-min=14.6",
#       "-v",
#       "-I/Users/admin/Desktop/bug-sample/test/D"
#     ]
#   }
# }

#cpptools configurations
# Translation Unit Mappings:
# [ /Users/admin/Desktop/bug-sample/sample.cpp - source TU]:
# Translation Unit Configurations:
# [ /Users/admin/Desktop/bug-sample/sample.cpp ]
#     Process ID: 15372
#     Memory Usage: 200 MB
#     Compiler Path: /usr/bin/clang++
#     Include paths:
#         include: /Users/admin/Desktop/bug-sample/test/A
#         include: /Users/admin/Desktop/bug-sample/test/C
#         include: /Users/admin/Desktop/bug-sample/test/E
#         include: /Users/admin/Desktop/bug-sample/test/B
#         include: /Users/admin/Desktop/bug-sample/build
#         include: /Users/admin/Desktop/bug-sample/test/D
#         system include: /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/usr/include/c++/v1
#         system include: /Library/Developer/CommandLineTools/usr/lib/clang/16/include
#         system include: /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/usr/include
#         system include: /Library/Developer/CommandLineTools/usr/include
#         system framework search path (default): /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/System/Library/Frameworks
#     Standard Version: c++17
#     IntelliSense Mode: macos-clang-x64
#     Other Flags:
#         --clang
#         --clang_version=170006
# Total Memory Usage: 200 MB



#1、priority information lost !!!
#2、framework directories are treated as normal include directories !!!