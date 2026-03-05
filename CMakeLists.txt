cmake_minimum_required(VERSION 3.15)

project(Peach)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_C_STANDARD 11)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

set(PEACH_VERSION "0.0")
set(PEACH_BUILD "0")

find_package(Git QUIET)

if(GIT_FOUND)
    execute_process(
        COMMAND ${GIT_EXECUTABLE} rev-parse --abbrev-ref HEAD
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
        OUTPUT_VARIABLE GIT_BRANCH
        OUTPUT_STRIP_TRAILING_WHITESPACE
    )

    set(GIT_REMOTE_REF "origin/${GIT_BRANCH}")

    execute_process(
        COMMAND ${GIT_EXECUTABLE} rev-parse --verify ${GIT_REMOTE_REF}
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
        RESULT_VARIABLE REMOTE_EXISTS
        OUTPUT_QUIET ERROR_QUIET
    )

    if(REMOTE_EXISTS EQUAL 0)
        execute_process(
            COMMAND ${GIT_EXECUTABLE} tag --list
            WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
            OUTPUT_VARIABLE GIT_TAG_LIST
            OUTPUT_STRIP_TRAILING_WHITESPACE
        )
        if(GIT_TAG_LIST)
            execute_process(
                COMMAND ${GIT_EXECUTABLE} describe --tags --abbrev=0 ${GIT_REMOTE_REF}
                WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
                OUTPUT_VARIABLE GIT_TAG
                OUTPUT_STRIP_TRAILING_WHITESPACE
                ERROR_QUIET
            )
            if(GIT_TAG)
                set(PEACH_VERSION ${GIT_TAG})
                execute_process(
                    COMMAND ${GIT_EXECUTABLE} rev-list ${GIT_TAG}..${GIT_REMOTE_REF} --count
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
                COMMAND ${GIT_EXECUTABLE} rev-list ${GIT_REMOTE_REF} --count
                WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
                OUTPUT_VARIABLE GIT_COMMITS
                OUTPUT_STRIP_TRAILING_WHITESPACE
            )
            if(GIT_COMMITS)
                set(PEACH_BUILD ${GIT_COMMITS})
            endif()
        endif()
    endif()
endif()

# Peach-${PEACH_VERSION}-${PEACH_BUILD} 
# Use temporary naming for beta testing. Don't include version for now. 
set(PEACH_OUTPUT_NAME "Peach-${PEACH_BUILD}")

add_subdirectory(Source)