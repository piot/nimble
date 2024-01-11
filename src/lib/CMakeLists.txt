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

add_library(nimble STATIC nothing.c)


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
elseif(COMPILER_GCC)
elseif(COMPILER_MSVC)
else()
  target_compile_options(nimble PRIVATE -Wall)
endif()

target_link_libraries(nimble PUBLIC
 nimble-engine-client
 nimble-server-lib
 datagram-transport-local
 hazy)


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
