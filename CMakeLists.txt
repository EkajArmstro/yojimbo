cmake_minimum_required(VERSION 3.28)

project(yojimbo)

include_directories(
    .
    include
    serialize
    sodium
    netcode
    reliable
    tlsf
)

file(GLOB sodium_SRC
    IF(WIN32)
        "sodium/*.c"
        "sodium/*.h"
    ELSE()
        "sodium/*.S"
    ENDIF()
)
add_library(sodium STATIC ${sodium_SRC})

add_library(netcode STATIC
    netcode/netcode.c
    netcode/netcode.h
)
target_include_directories(netcode PUBLIC netcode)

add_library(reliable STATIC
    reliable/reliable.c
    reliable/reliable.h
)
target_include_directories(reliable PUBLIC reliable)

add_library(tlsf STATIC
    tlsf/tlsf.c
    tlsf/tlsf.h
)

file(GLOB yojimbo_SRC
    "include/*.h"
    "source/*.cpp"
)
add_library(yojimbo STATIC ${yojimbo_SRC})
target_include_directories(yojimbo PUBLIC include PUBLIC serialize)
target_link_libraries(yojimbo PUBLIC sodium PUBLIC netcode PUBLIC reliable PUBLIC tlsf)
