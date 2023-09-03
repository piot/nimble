cmake_minimum_required(VERSION 3.16.3)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_EXTENSIONS false)


# --- Detect CMake build type, compiler and operating system ---

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  message("detected debug build")
  set(isDebug TRUE)
else()
  message("detected release build")
  set(isDebug FALSE)
endif()


if(CMAKE_C_COMPILER_ID MATCHES "Clang")
  set(COMPILER_NAME "clang")
  set(COMPILER_CLANG 1)
elseif(CMAKE_C_COMPILER_ID STREQUAL "GNU")
  set(COMPILER_NAME "gcc")
  set(COMPILER_GCC 1)
elseif(CMAKE_C_COMPILER_ID STREQUAL "MSVC")
  set(COMPILER_NAME "msvc")
  set(COMPILER_MSVC 1)
endif()

message("detected compiler: '${CMAKE_C_COMPILER_ID}' (${COMPILER_NAME})")

set(useSanitizers false)

if(useSanitizers)
  message("using sanitizers")
  set(sanitizers "-fsanitize=address")
endif()


# --- Library ---


file(GLOB lib_src FOLLOW_SYMLINKS "./*.c")


add_library(nimble STATIC ${lib_src})


if(APPLE)
  target_compile_definitions(nimble PRIVATE TORNADO_OS_MACOS)
  set(OS_MACOS 1)
elseif(UNIX)
  target_compile_definitions(nimble PRIVATE TORNADO_OS_LINUX)
  set(OS_LINUX 1)
elseif(WIN32)
  target_compile_definitions(nimble PRIVATE TORNADO_OS_WINDOWS)
  set(OS_WINDOWS 1)

endif()

if(isDebug)
  message("Setting definitions based on debug")
  target_compile_definitions(nimble PRIVATE CONFIGURATION_DEBUG)
endif()


function(unixlike)
endfunction()

if(OS_LINUX)
  message("Linux Detected!")
  unixlike()

elseif(OS_MACOS)
  message("MacOS detected!")
  unixlike()
endif()

if(COMPILER_CLANG)
  target_compile_options(
    nimble
    PRIVATE -Weverything
            -Werror
            -Wno-padded # the order of the fields in struct can matter (ABI)
            -Wno-declaration-after-statement # must be bug in clang? shows up
                                             # even if std=c99
            -Wno-unsafe-buffer-usage # unclear why it fails on clang-16
            -Wno-unknown-warning-option # support newer clang versions, e.g. clang-16
            -Wno-disabled-macro-expansion
            ${sanitizers})
elseif(COMPILER_GCC)
target_compile_options(
    nimble
    PRIVATE -Wall
            -Wextra
            -Wpedantic
            -Werror
            -Wno-padded # the order of the fields in struct can matter (ABI)
            ${sanitizers})
elseif(COMPILER_MSVC)
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
  target_compile_options(nimble PRIVATE -Wall)
endif()


target_link_libraries(nimble PUBLIC nimble-engine-client PUBLIC nimble-server-lib PUBLIC datagram-transport-local PUBLIC hazy)


if(useSanitizers)
  target_link_libraries(nimble PRIVATE ${sanitizers})
endif()

if(NOT isDebug)
  message("optimize!")
  target_compile_options(nimble PRIVATE -O3)
endif()

if (NOT MSVC)
target_link_libraries(nimble PRIVATE m)
endif()
