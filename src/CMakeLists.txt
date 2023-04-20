cmake_minimum_required(VERSION 3.25)
project(nimble_client C)

set(CMAKE_C_STANDARD 99)
#set(CMAKE_C_FLAGS "-std=c99")

if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(isDebug TRUE)
else()
    message("no debug")
    set(isDebug FALSE)
endif()


if (isDebug)
    message("Debug build detected")
    add_compile_definitions(CONFIGURATION_DEBUG TYRAN_CONFIGURATION_DEBUG)
    set(CONFIGURATION_DEBUG 1)
endif()


set(lib ../temp/src/piot/)
set(include ../temp/include)

set(suffix "/src/lib/*.c")

file(GLOB lib_src FOLLOW_SYMLINKS
    "${lib}bit-array${suffix}"
    "${lib}blob-stream${suffix}"
    "${lib}clog${suffix}"
    "${lib}discoid-c${suffix}"
    "${lib}flood-c${suffix}"
    "${lib}imprint${suffix}"
    "${lib}monotonic-time-c${suffix}"
    "${lib}nimble-client-c${suffix}"
    "${lib}nimble-serialize-c${suffix}"
    "${lib}nimble-server-lib${suffix}"
    "${lib}nimble-steps-c${suffix}"
    "${lib}nimble-steps-serialize-c${suffix}"
    "${lib}ordered-datagram-c${suffix}"
    "${lib}stats-c${suffix}"
    "${lib}tiny-libc${suffix}"
    "${lib}udp-transport${suffix}"
)

add_library(nimble_lib STATIC
    ${lib_src}
)



target_compile_options(nimble_lib PRIVATE -Wall -Wextra -Wshadow -Wstrict-aliasing -ansi -pedantic -Wno-declaration-after-statement -Wno-extra-semi-stmt -Wno-undef -Wno-unused-variable -Wno-unused-parameter -Wno-padded -Werror=implicit-function-declaration -Werror=incompatible-pointer-types  -Werror=missing-prototypes -Werror=int-conversion -Werror=return-type -Werror=incompatible-function-pointer-types) # -Werror=incompatible-pointer-types-discards-qualifiers
if (COMPILER_CLANG)
target_compile_options(nimble_lib PRIVATE -Wmost -Weverything -Werror=missing-variable-declarations)
endif()


target_include_directories(nimble_lib PUBLIC ${include})
