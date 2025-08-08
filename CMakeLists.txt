cmake_minimum_required(VERSION 3.90)

set(CMAKE_CONFIGURATION_TYPES "Debug" "Release")

set(PROJECT_NAME libhttp)
project(${PROJECT_NAME})

include("${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/CMakeLists.txt")

set(SOURCE_ROOT "${CMAKE_CURRENT_SOURCE_DIR}")

########################################################################
# Sources
set(HEADERS "libhttp.h")
set(SOURCES "libhttp.cpp")

source_group("include" FILES ${HEADERS})
source_group("src" FILES ${SOURCES})

set(FILES ${HEADERS} ${SOURCES})

set(OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/lib")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG "${OUTPUT_DIRECTORY}")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE "${OUTPUT_DIRECTORY}")

add_library(${PROJECT_NAME} STATIC ${FILES})
#add_executable(${PROJECT_NAME} ${FILES})

if(MSVC)
    set(DEFAULT_CXX_FLAGS "/DWIN32 /D_WINDOWS /D_MSBC /W4 /WX- /nologo /fp:precise /arch:AVX2 /Zc:wchar_t /TP /Gd /std:c++20")
    if("1800" VERSION_LESS MSVC_VERSION)
        set(DEFAULT_CXX_FLAGS "${DEFAULT_CXX_FLAGS} /EHsc")
    endif()

    set(CMAKE_CXX_FLAGS "${DEFAULT_CXX_FLAGS}")
    set(CMAKE_CXX_FLAGS_DEBUG "/D_DEBUG /MDd /Zi /Ob0 /Od /RTC1 /Gy /GR- /GS /Gm-")
    set(CMAKE_CXX_FLAGS_RELEASE "/MD /O2 /GL /GR- /DNDEBUG")

elseif(UNIX)
    set(DEFAULT_CXX_FLAGS "-Wall -O0 -g -std=c++20 -std=gnu++20 -march=native")
    set(CMAKE_CXX_FLAGS "${DEFAULT_CXX_FLAGS}")
elseif(APPLE)
endif()

set_property(DIRECTORY PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})
set_target_properties(${PROJECT_NAME}
    PROPERTIES
        OUTPUT_NAME_DEBUG "${PROJECT_NAME}d" OUTPUT_NAME_RELEASE "${PROJECT_NAME}")

# libhttp
set(LIBHTTP_ROOT "${CMAKE_CURRENT_SOURCE_DIR}")
set(LIBHTTP_LIB_DIR "${LIBHTTP_ROOT}/lib")
add_library(LIBHTTP MODULE IMPORTED)
set_target_properties(LIBHTTP PROPERTIES IMPORTED_LOCATION_DEBUG "${LIBHTTP_LIB_DIR}/libhttpd.lib" IMPORTED_LOCATION_RELEASE "${LIBHTTP_LIB_DIR}/libhttp.lib")
include_directories(AFTER "${LIBHTTP_ROOT}")
link_directories(AFTER "${LIBHTTP_LIB_DIR}")

add_subdirectory("${CMAKE_CURRENT_SOURCE_DIR}/test")

