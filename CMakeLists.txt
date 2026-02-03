cmake_minimum_required(VERSION 3.28 FATAL_ERROR)

project(
    LMDB
    VERSION 0.9.35
    LANGUAGES C
)

option(LMDB_SHARED "Build LMDB as a shared library." TRUE)

if(${LMDB_SHARED})
    set(lib_type SHARED)
else()
    set(lib_type STATIC)
endif()

add_library(
    liblmdb
    ${lib_type}
    "mdb.c"
    "midl.c"
)

set_target_properties(
    liblmdb PROPERTIES
    C_STANDARD 11
    C_STANDARD_REQUIRED TRUE
)

if(${MSVC})
    target_compile_definitions(liblmdb PRIVATE _CRT_SECURE_NO_WARNINGS)

    target_compile_options(
        liblmdb PRIVATE
        /W4
        /wd4100
        /wd4102
        /wd4127
        /wd4146
        /wd4172
        /wd4200
        /wd4244
        /wd4267
        /wd4333
        /wd4389
        /wd4456
        /wd4701
        /wd4703
    )
else()
    target_compile_options(
        liblmdb PRIVATE
        -W
        -Wall
        -Wno-unused-parameter
        -Wbad-function-cast
        -Wuninitialized
        -pthread
    )
endif()
