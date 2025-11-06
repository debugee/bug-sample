cmake_minimum_required(VERSION 4.1.2)

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

target_compile_options(sample PRIVATE -I${CMAKE_CURRENT_SOURCE_DIR}/test/D -iframework${CMAKE_CURRENT_SOURCE_DIR}/test/G -isystem${CMAKE_CURRENT_SOURCE_DIR}/test/F)

# build] clang -cc1 version 17.0.0 (clang-1700.3.19.1) default target x86_64-apple-darwin24.6.0
# [build] ignoring nonexistent directory "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/local/include"
# [build] ignoring nonexistent directory "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/Library/Frameworks"
# [build] #include "..." search starts here:
# [build] #include <...> search starts here:
# [build]  /Users/admin/Desktop/bug-sample/test/A
# [build]  /Users/admin/Desktop/bug-sample/test/B (framework directory)
# [build]  /Users/admin/Desktop/bug-sample/test/C
# [build]  /Users/admin/Desktop/bug-sample/test/D
# [build]  /usr/local/include
# [build]  /Users/admin/Desktop/bug-sample/test/E
# [build]  /Users/admin/Desktop/bug-sample/test/F
# [build]  /Users/admin/Desktop/bug-sample/build (framework directory)
# [build]  /Users/admin/Desktop/bug-sample/test/G (framework directory)
# [build]  /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/c++/v1
# [build]  /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/17/include
# [build]  /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include
# [build]  /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include
# [build]  /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks (framework directory)
# [build]  /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/SubFrameworks (framework directory)
# [build] End of search list.


# -------- Diagnostics - 2025/11/6 15:33:17
# Version: 1.29.0
# Current Configuration:
# {
#     "name": "Mac",
#     "includePath": [
#         "/Users/admin/Desktop/bug-sample/**"
#     ],
#     "defines": [],
#     "cStandard": "c17",
#     "cppStandard": "c++14",
#     "intelliSenseMode": "macos-clang-x64",
#     "intelliSenseModeIsExplicit": false,
#     "cStandardIsExplicit": false,
#     "cppStandardIsExplicit": false,
#     "mergeConfigurations": false,
#     "recursiveIncludes": {},
#     "recursiveIncludesReduceIsExplicit": false,
#     "recursiveIncludesPriorityIsExplicit": false,
#     "recursiveIncludesOrderIsExplicit": false,
#     "compilerPath": "/usr/bin/clang++",
#     "compilerPathIsExplicit": true,
#     "configurationProvider": "ms-vscode.cmake-tools",
#     "browse": {
#         "limitSymbolsToIncludedHeaders": true
#     }
# }
# Modified Settings:
# {
#     "C_Cpp.default.compilerPath": "/usr/bin/clang++"
# }
# Additional Tracked Settings:
# {
#     "editorTabSize": 4,
#     "editorInsertSpaces": true,
#     "editorAutoClosingBrackets": "languageDefined",
#     "filesEncoding": "utf8",
#     "filesAssociations": {
#         "*.cpp": "cpp",
#         "*.qrc": "xml"
#     },
#     "filesExclude": {
#         "**/.git": true,
#         "**/.svn": true,
#         "**/.hg": true,
#         "**/.DS_Store": true,
#         "**/Thumbs.db": true
#     },
#     "filesAutoSaveAfterDelay": false,
#     "editorInlayHintsEnabled": true,
#     "editorParameterHintsEnabled": true,
#     "searchExclude": {
#         "**/node_modules": true,
#         "**/bower_components": true,
#         "**/*.code-search": true
#     },
#     "workbenchSettingsEditor": "ui"
# }
# Custom browse configuration: 
# {
#     "browsePath": [
#         "/Users/admin/Desktop/bug-sample",
#         "/Users/admin/Desktop/bug-sample/test/A",
#         "/Users/admin/Desktop/bug-sample/test/C",
#         "/Users/admin/Desktop/bug-sample/test/E",
#         "/Users/admin/Desktop/bug-sample/test/B",
#         "/Users/admin/Desktop/bug-sample/build"
#     ],
#     "compilerPath": "/usr/bin/clang++",
#     "compilerArgs": [],
#     "compilerFragments": [
#         "-g",
#         "-v",
#         "-I/Users/admin/Desktop/bug-sample/test/D",
#         "-iframework/Users/admin/Desktop/bug-sample/test/G",
#         "-isystem/Users/admin/Desktop/bug-sample/test/F"
#     ]
# }
# cpptools version (native): 1.29.0.0
# Current database path: /Users/admin/Library/Caches/vscode-cpptools/685053b2341a791a38afa23bb515c719/.browse.VC.db
# Translation Unit Mappings:
# [ /Users/admin/Desktop/bug-sample/sample.cpp - source TU]:
# Translation Unit Configurations:
# [ /Users/admin/Desktop/bug-sample/sample.cpp ]
#     Process ID: 22269
#     Memory Usage: 168 MB
#     Compiler Path: /usr/bin/clang++
#     Include paths:
#         include: /Users/admin/Desktop/bug-sample/test/A
#         include: /Users/admin/Desktop/bug-sample/test/C
#         include: /Users/admin/Desktop/bug-sample/test/E
#         include: /Users/admin/Desktop/bug-sample/test/B
#         include: /Users/admin/Desktop/bug-sample/build
#         framework search path: /Users/admin/Desktop/bug-sample/build
#         include: /Users/admin/Desktop/bug-sample/test/D
#         system include (isystem): /Users/admin/Desktop/bug-sample/test/F
#         system include: /usr/local/include
#         system include: /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/c++/v1
#         system include: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/17/include
#         system include: /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include
#         system include: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include
#         system framework search path (default): /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks
#         system framework search path (default): /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/SubFrameworks
#     Standard Version: c++14
#     IntelliSense Mode: macos-clang-x64
#     Other Flags:
#         --clang
#         --clang_version=190104
# Total Memory Usage: 168 MB

# ------- Workspace parsing diagnostics -------
# Number of files discovered (not excluded): 12262
# Number of files parsed: 2342

