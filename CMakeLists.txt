cmake_minimum_required(VERSION 3.22)

# Setup project
project(LwLibPROJECT)

# -------------------------------------------------
# This CMakeLists.txt is used only if it is a top-level file.
# Purpose of it is to be able to compile project in standalone way only
#
# When library sources are to be included in another project
# user shall use /lwesp/CMakeLists.txt instead
if (NOT PROJECT_IS_TOP_LEVEL)
    message(FATAL_ERROR "This CMakeLists.txt can only be used as top-level. Use /lwesp/CMakeLists.txt for library include purpose")
endif()

# Set as executable
add_executable(${PROJECT_NAME})

# Add key executable block
target_sources(${PROJECT_NAME} PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}/dev/main.c

    # Development additional files
    ${CMAKE_CURRENT_LIST_DIR}/lwesp/src/system/lwesp_mem_lwmem.c
    ${CMAKE_CURRENT_LIST_DIR}/../lwmem/lwmem/src/lwmem/lwmem.c
    ${CMAKE_CURRENT_LIST_DIR}/../lwmem/lwmem/src/system/lwmem_sys_win32.c

    # Http server file system
    ${CMAKE_CURRENT_LIST_DIR}/lwesp/src/apps/http_server/lwesp_http_server_fs_win32.c

    # Port specific
    ${CMAKE_CURRENT_LIST_DIR}/lwesp/src/system/lwesp_sys_win32.c
    ${CMAKE_CURRENT_LIST_DIR}/lwesp/src/system/lwesp_ll_win32.c
    )

# Add key include paths
target_include_directories(${PROJECT_NAME} PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}
    ${CMAKE_CURRENT_LIST_DIR}/dev
    ${CMAKE_CURRENT_LIST_DIR}/../lwmem/lwmem/src/include

    # Port specifics
    ${CMAKE_CURRENT_LIST_DIR}/lwesp/src/include/system/port/win32
    )

# Compilation definition information
target_compile_definitions(${PROJECT_NAME} PUBLIC
    WIN32
    _DEBUG
    CONSOLE
    LWESP_DEV
    )

# Add subdir with lwesp and link to the project
add_subdirectory("lwesp")
target_link_libraries(${PROJECT_NAME} lwesp)
target_link_libraries(${PROJECT_NAME} lwesp_api)
target_link_libraries(${PROJECT_NAME} lwesp_apps)
add_subdirectory("snippets")
target_link_libraries(${PROJECT_NAME} lwesp_snippets)
