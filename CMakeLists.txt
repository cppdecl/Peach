cmake_minimum_required(VERSION 3.15)

project(Peach)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_C_STANDARD 11)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -gdwarf-4")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -gdwarf-4")

set(PEACH_VERSION "0.0")
set(PEACH_BUILD "0")

find_package(Git QUIET)

if(GIT_FOUND)
    execute_process(
        COMMAND ${GIT_EXECUTABLE} tag --list
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
        OUTPUT_VARIABLE GIT_TAG_LIST
        OUTPUT_STRIP_TRAILING_WHITESPACE
    )
    if(GIT_TAG_LIST)
        execute_process(
            COMMAND ${GIT_EXECUTABLE} describe --tags --abbrev=0
            WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
            OUTPUT_VARIABLE GIT_TAG
            OUTPUT_STRIP_TRAILING_WHITESPACE
        )
        if(GIT_TAG)
            set(PEACH_VERSION ${GIT_TAG})
            execute_process(
                COMMAND ${GIT_EXECUTABLE} rev-list ${GIT_TAG}..HEAD --count
                WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
                OUTPUT_VARIABLE GIT_COMMITS
                OUTPUT_STRIP_TRAILING_WHITESPACE
            )
            if(GIT_COMMITS)
                set(PEACH_BUILD ${GIT_COMMITS})
            endif()
        endif()
    else()
        execute_process(
            COMMAND ${GIT_EXECUTABLE} rev-list HEAD --count
            WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
            OUTPUT_VARIABLE GIT_COMMITS
            OUTPUT_STRIP_TRAILING_WHITESPACE
        )
        if(GIT_COMMITS)
            set(PEACH_BUILD ${GIT_COMMITS})
        endif()
    endif()

endif()

# Peach-${PEACH_VERSION}-${PEACH_BUILD}
# Use temporary naming for beta testing. Don't include version for now.
set(PEACH_OUTPUT_NAME "Peach-${PEACH_BUILD}")

add_subdirectory(Source)