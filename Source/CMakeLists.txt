cmake_minimum_required(VERSION 3.15)

add_definitions(-D__CMAKE_ROOT_DIRECTORY__="${CMAKE_CURRENT_SOURCE_DIR}/..")

file(GLOB_RECURSE CC_FILES CONFIGURE_DEPENDS
    "*.cc"
)

file(GLOB OLD_PEACH_FILES
    "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/Peach*.exe"
    "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/Peach*.pdb"
)

add_custom_target(CLEANUP_PEACH
    COMMAND ${CMAKE_COMMAND} -E rm -f ${OLD_PEACH_FILES}
)

add_executable(${PROJECT_NAME}
    ${CC_FILES}
)

add_dependencies(${PROJECT_NAME} CLEANUP_PEACH)

set_target_properties(${PROJECT_NAME} PROPERTIES
    OUTPUT_NAME ${PEACH_OUTPUT_NAME}
)

target_link_libraries(${PROJECT_NAME} PUBLIC
    ${CMAKE_THREAD_LIBS_INIT}
)

target_include_directories(${PROJECT_NAME} PUBLIC .)