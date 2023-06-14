cmake_minimum_required(VERSION 3.22)
project(nimble C)

set(CMAKE_C_STANDARD 99)
set(C_STANDARD_REQUIRED ON)

if (DEFINED PLATFORM)
    if (PLATFORM STREQUAL "OS64" )
        set(OS_IOS 1)
        set(OS_NAME "ios")
    endif()
elseif (APPLE)
    set(OS_MACOS 1)
    set(OS_NAME "macos")
    set(CMAKE_OSX_ARCHITECTURES "x86_64")
    set(CMAKE_OSX_ARCHITECTURES "x86_64" CACHE INTERNAL "" FORCE)
elseif (UNIX)
    set(OS_LINUX 1)
    set(OS_NAME "linux")
elseif (WIN32)
    set(OS_WINDOWS 1)
    set(OS_NAME "windows")
endif()


if (CMAKE_C_COMPILER_ID MATCHES "Clang")
    set(COMPILER_NAME "clang")
    set(COMPILER_CLANG 1)
elseif (CMAKE_C_COMPILER_ID STREQUAL "GNU")
    set(COMPILER_NAME "gcc")
    set(COMPILER_GCC 1)
elseif (CMAKE_C_COMPILER_ID STREQUAL "Intel")
elseif (CMAKE_C_COMPILER_ID STREQUAL "MSVC")
endif()


if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  set(isDebug TRUE)
else()
  message("Debug not detected")
  set(isDebug FALSE)
endif()



set(nimble_dir ./)

set(lib ${nimble_dir}src/piot/)
set(include ${nimble_dir}include)

set(suffix "/src/lib/*.c")

file(
  GLOB
  lib_src
  FOLLOW_SYMLINKS
  "${lib}assent-c${suffix}"
  "${lib}bit-array${suffix}"
  "${lib}blob-stream${suffix}"
  "${lib}clog${suffix}"
  "${lib}discoid-c${suffix}"
  "${lib}flood-c${suffix}"
  "${lib}hazy-c${suffix}"
  "${lib}imprint${suffix}"
  "${lib}monotonic-time-c${suffix}"
  "${lib}nimble-client-c${suffix}"
  "${lib}nimble-engine-client${suffix}"
  "${lib}nimble-serialize-c${suffix}"
  "${lib}nimble-server-lib${suffix}"
  "${lib}nimble-steps-c${suffix}"
  "${lib}nimble-steps-serialize-c${suffix}"
  "${lib}ordered-datagram-c${suffix}"
  "${lib}rectify-c${suffix}"
  "${lib}seer-c${suffix}"
  "${lib}stats-c${suffix}"
  "${lib}tiny-libc${suffix}"
  "${lib}transmute-c${suffix}"
  "${lib}udp-transport${suffix}")

file(GLOB platform_specific "${lib}monotonic-time-c/src/platform/posix/*.c"
     "${lib}clog/src/lib/posix/*.c")

add_library(nimble STATIC ${lib_src} ${platform_specific})
if(isDebug)
  message("Debug build detected")
  target_compile_definitions(nimble PRIVATE CONFIGURATION_DEBUG TYRAN_CONFIGURATION_DEBUG)
  set(CONFIGURATION_DEBUG 1)
endif()

set_target_properties(nimble PROPERTIES FRAMEWORK TRUE)

if(OS_WINDOWS)
  target_compile_options(
    nimble
    PRIVATE /F
            5000000
            /Wall
            /wd4668
            /wd4820
            /wd4100
            /wd4710
            /wd4711
            /wd5945
            /wd5105
            /wd5045
            /we4047
            /we4255
            /we4013
            /we4020
            /we4474
            /we4477
            /we4057
            /we4101
            /we4189) # /WX
else()
  target_compile_options(
    nimble
    PRIVATE -Wall
            -Wextra
            -Wshadow
            -Wstrict-aliasing
            -pedantic
            -Wno-declaration-after-statement
            -Wno-extra-semi-stmt
            -Wno-undef
            -Wno-unused-variable
            -Wno-unused-parameter
            -Wno-padded
            -Werror=implicit-function-declaration
            -Werror=incompatible-pointer-types
            -Werror=incompatible-function-pointer-types
            -Werror=missing-prototypes
            -Werror=int-conversion
            -Werror=return-type
  )

  if(COMPILER_CLANG)
    target_compile_options(nimble PRIVATE -Wmost -Weverything
                                          -Werror=missing-variable-declarations)
  endif()
endif()

target_include_directories(nimble PUBLIC ${include})
