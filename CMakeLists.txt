cmake_minimum_required(VERSION 3.8)

project(cmake_test)


set (CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR};${CMAKE_MODULE_PATH}")
find_package(HIP)
set_source_files_properties(main.cpp PROPERTIES HIP_SOURCE_PROPERTY_FORMAT 1)
hip_add_executable(test_exec main.cpp)

target_include_directories(test_exec PRIVATE ${CMAKE_SOURCE_DIR}/private_include)
target_include_directories(test_exec PUBLIC ${CMAKE_SOURCE_DIR}/public_include)
target_include_directories(test_exec INTERFACE ${CMAKE_SOURCE_DIR}/interface_include)
