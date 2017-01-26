cmake_minimum_required(VERSION 3.6)

project(cment)

set(SOURCE_FILES src/main.c)

add_executable(cment ${SOURCE_FILES})

include(CMent.cmake)


