cmake_minimum_required(VERSION 3.5)
project(aubosdkdemo)

include_directories(${PROJECT_SOURCE_DIR}/dependents/robotSDK/inc)
include_directories(${PROJECT_SOURCE_DIR}/example)

#判断操作系统64还是32位 8*8=64bit
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
     message(STATUS "-------64bit------")
     include_directories(${PROJECT_SOURCE_DIR}/dependents/log4cplus/linux_x64/inc)     
     link_directories(${PROJECT_SOURCE_DIR}/dependents/log4cplus/linux_x64/lib)
     link_directories(${PROJECT_SOURCE_DIR}/dependents/robotSDK/lib/linux_x64)
else()
     message(STATUS "-------32bit------")
     include_directories(${PROJECT_SOURCE_DIR}/dependents/log4cplus/linux_x32/inc)
     link_directories(${PROJECT_SOURCE_DIR}/dependents/log4cplus/linux_x32/lib)
     link_directories(${PROJECT_SOURCE_DIR}/dependents/robotSDK/lib/linux_x32)
endif()

message(status "----------dir list begin---------")

get_property(dirs DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY INCLUDE_DIRECTORIES)
foreach(dir ${dirs})
  message(status " inc.dir='${dir}'")
endforeach(dir)

get_property(dirs DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY LINK_DIRECTORIES)
foreach(dir ${dirs})
  message(status " link.dir='${dir}'")
endforeach(dir)

message(status "----------dir list end-----------")

set(SRC_DIR ${PROJECT_SOURCE_DIR}  ${PROJECT_SOURCE_DIR}/example)

message(status "----------src list begin---------")

foreach(dir ${SRC_DIR})
     message(status, "src.dir='${dir}'")
     aux_source_directory(${dir} SRC_LIST)
endforeach(dir)

message(status "---------------------------------")

foreach(f ${SRC_LIST})
     message(status "sr.file='${f}'")     
     endforeach(f)

message(status "----------src list end----------")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")

add_executable(${PROJECT_NAME} ${SRC_LIST})

target_link_libraries(${PROJECT_NAME}  pthread log4cplus auborobotcontroller)

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/debug) 

message("------cmake completed------")
