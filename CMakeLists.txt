cmake_minimum_required(VERSION 3.8)

project(cmake_test)

add_executable(test_exec main.cpp)

target_include_directories(test_exec PRIVATE ${CMAKE_SOURCE_DIR}/private_include)
target_include_directories(test_exec PUBLIC ${CMAKE_SOURCE_DIR}/public_include)
target_include_directories(test_exec INTERFACE ${CMAKE_SOURCE_DIR}/interface_include)

set(include_directories_generator "$<TARGET_PROPERTY:test_exec,INCLUDE_DIRECTORIES>")
set(INCLUDE_DIRS "$<$<BOOL:${include_directories_generator}>:-I$<JOIN:${include_directories_generator}, -I>>")

set(interface_include_directories_generator "$<TARGET_PROPERTY:test_exec,INTERFACE_INCLUDE_DIRECTORIES>")
set(INTERFACE_INCLUDE_DIRS "$<$<BOOL:${interface_include_directories_generator}>:-I$<JOIN:${interface_include_directories_generator}, -I>>")

configure_file(dump_include_dirs.cmake.in dump_include_dirs-pre.cmake)
file(GENERATE
     OUTPUT ${CMAKE_BINARY_DIR}/dump_include_dirs.cmake
     INPUT ${CMAKE_BINARY_DIR}/dump_include_dirs-pre.cmake)

add_custom_command(TARGET test_exec PRE_BUILD
                   COMMAND ${CMAKE_COMMAND} ARGS -P dump_include_dirs.cmake WORKING_DIRECTORY ${CMAKE_BINARY_DIR})
