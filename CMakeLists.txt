cmake_minimum_required(VERSION 3.19)
project(tiny_vm C)
set(CMAKE_C_STANDARD 11)

# Very simple JSON library for loader
find_package(cjson CONFIG REQUIRED)

add_custom_command(
        OUTPUT  ${CMAKE_SOURCE_DIR}/vm_code_table.c
        COMMAND python3 ${CMAKE_SOURCE_DIR}/build_bytecode_table.py
            ${CMAKE_SOURCE_DIR}/opdefs.txt
            ${CMAKE_SOURCE_DIR}/vm_code_table.c
        MAIN_DEPENDENCY ${CMAKE_SOURCE_DIR}/opdefs.txt
        DEPENDS ${CMAKE_SOURCE_DIR}/build_bytecode_table.py
        DEPENDS ${CMAKE_SOURCE_DIR}/vm_code_table.h
)

add_executable(tiny_vm
        main.c
        vm_state.c vm_state.h
        vm_ops.c vm_ops.h
        vm_code_table.h
        vm_code_table.c  # Generated
        builtins.c builtins.h
        vm_core.h
        vm_loader.c vm_loader.h
        logger.c logger.h)

target_include_directories(tiny_vm PRIVATE ${CJSON_INCLUDE_DIRS})
target_link_libraries(tiny_vm PRIVATE cjson)


