cmake_minimum_required(VERSION 3.0)

project(demo01)

aux_source_directory(. SRC)

set(CMAKE_CXX_STANDARD 11)



add_executable(demo01 ${SRC})