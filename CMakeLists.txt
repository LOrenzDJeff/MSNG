cmake_minimum_required(VERSION 3.10)
project(ZeroMQExample)

set(CMAKE_CSTANDART 99)

find_package(PkgConfig REQUIRED)
pkg_check_modules(ZeroMQ REQUIRED libzmq)

include_directories(${ZeroMQ_INCLUDE_DIRS})

add_executable(server tests/server.c)
add_executable(client tests/client.c)

target_link_libraries(server ${ZeroMQ_LIBRARIES})
target_link_libraries(client ${ZeroMQ_LIBRARIES})