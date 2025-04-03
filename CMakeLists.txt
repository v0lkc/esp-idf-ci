# The following lines of boilerplate have to be in your project's
# CMakeLists in this exact order for cmake to work correctly
cmake_minimum_required(VERSION 3.16)

include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project(hello_world)

add_custom_command(TARGET app POST_BUILD
    COMMAND python -m esptool --chip ${chip_model} merge_bin -o ${CMAKE_PROJECT_NAME}-factory.bin @flash_args
)