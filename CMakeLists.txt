cmake_minimum_required(VERSION 3.16)
cmake_policy(VERSION 3.16)

project(cpplearn)

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif (NOT CMAKE_BUILD_TYPE)

set(CMAKE_CXX_STANDARD 14)
list(APPEND CMAKE_CXX_FLAGS "-Wall")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

if(WIN32)
    message("Not tested on a Windows System proceed with caution")
endif()

set(prjs
        prj1
        prj2)

set(prj1
        subprj1
        subprj2)

set(prj2
        subprj1)

macro(makeLink src dest target)
    add_custom_command(TARGET ${target} POST_BUILD COMMAND ${CMAKE_COMMAND} -E create_symlink ${src} ${dest}  DEPENDS  ${dest} COMMENT "mklink ${src} -> ${dest}")
endmacro()

foreach(prj ${prjs})
    foreach(subprj ${${prj}})
        file(GLOB srcs
                "src/${prj}/${subprj}/*.h"
                "src/${prj}/${subprj}/*.cpp")
        set(name ${prj}__${subprj})
        add_executable(${name} ${srcs})
        set_target_properties(${name} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/bin/${prj}")
    endforeach(subprj)
endforeach(prj)

include_directories(${CMAKE_SOURCE_DIR}/inc)
