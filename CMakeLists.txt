cmake_minimum_required(VERSION 3.10)
project(ZeroMQExample)

set(CMAKE_CSTANDART 99)

execute_process(
    COMMAND sudo sh -c "echo 'deb http://download.opensuse.org/repositories/network:/messaging:/zeromq:/release-stable/xUbuntu_20.04/ /' > /etc/apt/sources.list.d/network:messaging:zeromq:release-stable.list"
    COMMAND curl -fsSL https://download.opensuse.org/repositories/network:messaging:zeromq:release-stable/xUbuntu_20.04/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/network_messaging_zeromq_release-stable.gpg > /dev/null
    COMMAND sudo apt update
    COMMAND sudo apt install -y libzmq3-dev
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)

execute_process(
    COMMAND git clone https://github.com/zeromq/czmq.git
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)
execute_process(
    COMMAND ./autogen.sh
    COMMAND ./configure
    COMMAND make check
    COMMAND sudo make install
    COMMAND sudo ldconfig
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/czmq
)

execute_process(
    COMMAND sudo apt install -y libtool
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)

find_package(PkgConfig REQUIRED)
pkg_check_modules(ZeroMQ REQUIRED libzmq)

include_directories(${ZeroMQ_INCLUDE_DIRS})

add_executable(server tests/server.c)
add_executable(client tests/client.c)

target_link_libraries(server ${ZeroMQ_LIBRARIES})
target_link_libraries(client ${ZeroMQ_LIBRARIES})