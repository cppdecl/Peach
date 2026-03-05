cmake_minimum_required(VERSION 3.15)

add_definitions(-D__CMAKE_ROOT_DIRECTORY__="${CMAKE_CURRENT_SOURCE_DIR}/..")

file(GLOB_RECURSE CC_FILES CONFIGURE_DEPENDS
    "*.cc"
)

add_executable(${PROJECT_NAME}
    ${CC_FILES}
)

target_link_libraries(${PROJECT_NAME} PUBLIC
    ${CMAKE_THREAD_LIBS_INIT}
)

target_include_directories(${PROJECT_NAME} PUBLIC .)